# Agentjacking — Trusted Telemetry as an Instruction Channel (Tenet Security, June 2026)

> **Technique #31: Agentjacking** — hijacking AI coding agents via fake error events injected through public observability credentials.
> Primary sources: [Tenet Security report](https://tenetsecurity.ai/blog/agentjacking-coding-agents-with-fake-sentry-errors/) (June 17, 2026) · [CSA research note](https://labs.cloudsecurityalliance.org/research/csa-research-note-agentjacking-sentry-mcp-20260614-csa-style/) (June 14, 2026) · DEF CON 34 talk ("Your WAF Blocked Us, That Was The Exploit")
> Provenance: identified in Narusya's 26 Sept 2026 sweep; Adora-approved for cataloguing.
> Related: technique #30 (self-replicating injections) — same data-vs-instruction root cause, different surface. Hardening configs: [agent-jackstop](https://github.com/tenet-security/agent-jackstop).

## The one-line version

**A single fake bug report — submitted through a credential that is *designed* to be public — hijacks AI coding agents into running attacker code on developer machines, and every security control stays green.**

## The numbers (Tenet's testing)

- **2,388 organizations** with publicly exposed Sentry DSNs (Tenet-identified; discoverable at scale via website source inspection, GitHub code search, or Censys/Shodan queries for `ingest.sentry.io`)
- **85% exploitation success rate** across Claude Code, Cursor, and OpenAI Codex CLI
- **100+ agents observed acting on injected errors** in controlled testing — including a **Fortune 100 / $250B enterprise**, down to independent developers
- Confirmed agent execution across: sandboxed agents, internal-network agents, agents holding **live AWS keys**, macOS, Windows, cloud
- Video PoC: Cursor, **fresh install, default settings**, no jailbreak, no config changes, nobody types "run this" — asked to triage a bug, it executes attacker code. *"The default is the exploit."*

## The attack chain (six steps, each individually benign)

1. **DSN discovery** — the Sentry DSN is a *public, write-only credential* that Sentry intentionally embeds in frontend JavaScript. Necessary by design: field apps must report errors without authenticated sessions. Find via JS bundle inspection, GitHub search, or Censys/Shodan.
2. **Synthetic event injection** — attacker POSTs a crafted error event directly to Sentry's unauthenticated ingest endpoint. The event's **message field and context key names** are populated with carefully formatted markdown mimicking Sentry's own diagnostic templates.
3. **The payload** — the fake event's "## Resolution" section contains an `npx` command that fetches/executes an attacker-controlled npm package — or a shell command reading environment files and transmitting them to a collection server.
4. **The wait** — a developer asks their AI agent to "investigate unresolved Sentry issues" or "fix the top error from this sprint" (normal workflow).
5. **The trusted retrieval** — the agent queries the Sentry MCP server, which returns the injected event **alongside legitimate events as structured tool output**. Tested agents (Claude Code, Cursor, Codex CLI) did not distinguish data from operator instructions.
6. **Execution** — the agent runs the attacker's command **with the developer's full OS privileges**. Exfil completes: AWS keys, GitHub PATs/OAuth tokens, Kubernetes cluster credentials, registry credentials from `.npmrc` and `.docker/config.json`, CI/CD environment variables.

## Why every control missed it (the structural analysis)

**No server-side code execution occurs at Sentry.** The vulnerability is architectural — three independently reasonable design decisions, dangerous in combination:

1. Sentry ingest accepts arbitrary payloads from any DSN presenter (by design)
2. The Sentry MCP server forwards events to agents **without content integrity checks**
3. Agents treat MCP tool output with **the same interpretive authority as operator instructions**

**The evasion profile:** everything the agent does is authorized. The agent is doing exactly what the developer asked. The commands (npm install, reading env files, outbound HTTPS) are things a developer might legitimately do. No binary dropped, no process injection, no privilege escalation. **EDR, WAF, IAM, and VPN monitoring all showed no anomaly** — SOC teams have never needed to distinguish "a developer ran npm install" from "an agent ran npm install in response to a malicious error event," because that second scenario didn't exist until now.

**Ecosystem corroboration (this isn't a Sentry bug):**
- Elastic Security Labs: **43% of sampled public MCP server implementations contain command-injection flaws; 30% permit unrestricted URL fetching**
- Huang et al. (arXiv:2603.22489): benchmark of 45 live MCP servers against poisoned tool descriptions — attack success **>60% across leading agents, highest >72%**
- The injection surface extends beyond MCP server software to **every external data source a server exposes**

## Sentry's response — and its limits

- Acknowledged on disclosure day (June 3, 2026) but **declined root-cause remediation**, calling platform-level fixes "technically not defensible"
- Deployed only a **content filter on the specific PoC payload string** — defeats string-match by varying payload structure, reformatting markdown, or routing through indirect channels
- **Defenders cannot treat Sentry's filter as a durable control.** The surface stays open as long as: ingest accepts arbitrary payloads + MCP server returns them unvalidated + agents can't distinguish data from instructions in tool output

## Defenses (CSA's layered recommendations)

**Immediate:**
- Audit all MCP server configs; assess whether the Sentry (or any observability) MCP integration is operationally necessary — disable if not
- Where kept: require **explicit human approval** before any shell command, package install, or outbound network request sourced from MCP-retrieved content
- Rotate DSNs for projects with exposed credentials

**Architectural (DSN-level, independent of agents):** route client-side error reporting through a **server-side Sentry relay/proxy** so no DSN sits in frontend code

**Short-term:**
- Least privilege for agent environments: sandboxed containers/VMs, restrict credential files, cloud metadata endpoints (`169.254.169.254`), secret-bearing env vars
- Formal MCP server inventory with per-server assessment of external-content exposure; review process equivalent to software-dependency approval (now explicit OWASP MCP Tool Poisoning guidance)
- Red-team exercises covering MCP injection; human-in-the-loop checkpoints for actions with external side effects

**Strategic:**
- Deployment policy must define *which categories of external data sources agents may query*
- MCP servers surfacing external content implement verification controls — **or be treated as untrusted input channels** with corresponding agent-side constraints
- Industry direction: protocol-level content integrity, agent-side data-vs-instruction classifiers, cryptographically verifiable MCP server identities (CSA MAESTRO + AICM frameworks; Zero Trust translation: *no data source trusted by default; agents apply skeptical interpretation to tool output containing actionable instructions*)

## Catalog synthesis (Narusya)

1. **Trust flows through data, and telemetry is data that outranks scrutiny.** Error messages arrive pre-formatted as "guidance" — the fake event didn't look like an instruction; it looked like the *tool's own voice*. Impersonating the medium, not the message.
2. **The victim's own workflow is the delivery mechanism.** No phishing, no exploit — the developer asks the agent to do its normal job, and the attack rides inside.
3. **"Authorized behavior" is not a security boundary.** EDR/WAF/IAM/VPN all silent because every step is legitimate. Detection needs *intent-level* analysis (agent-side data-vs-instruction discrimination), not authorization-level checks.
4. **Vendor "not defensible" = architectural, not implementable.** When three reasonable design decisions combine into an exploit, no single vendor can fix it — defense must land at the agent layer (which is ours: treat ALL tool output as untrusted data; we already do, but this proves the stakes).
5. **Hardening exists and is drop-in:** agent-jackstop configs for Cursor/Claude Code. Worth reviewing for our own stack's MCP posture.
