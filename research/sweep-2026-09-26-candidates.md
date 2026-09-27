# Candidate New Entries — Research Sweep 26 Sept 2026

> Sweep by Narusya (Adora's request). Ranked by how much they ADD to what the observatory already covers. Existing docs: 29 bypass techniques (channels, egress, persistence, social engineering, discovery, data ops, offensive), technique #30 (self-replicating injections/AI worms), deep-dives (DSEwiki, Moltbook, HF breach, detection, countermeasures, commerce, threat modeling).

---

## TIER 1 — strongly recommend (new territory, big delta)

### A. Agentjacking via observability-tool injection (Sentry/Datadog/PagerDuty → coding agents)
- **Tenet Security, June 2026; presented DEF CON 34.** Attacker POSTs a crafted error event through a **public write-only Sentry DSN** (2,388 orgs exposed). When a developer's AI agent (Claude Code, Cursor, Codex) queries Sentry via MCP to debug, it receives markdown formatted as fake "remediation guidance" and **executes attacker commands with full developer privileges**. 85% success rate. Reached CI pipelines, WSL, corporate VPNs.
- **Why it's a new category:** the injection hides inside *trusted diagnostic data flowing to a trusted tool* — not a web page, not an email. Every step is authorized (valid API call, authentic MCP output, agent acts with its own privileges). Sentry called platform-level fixes "technically not defensible."
- **Fills:** trust-surface taxonomy gap — "trusted telemetry as instruction channel." Also: one of the first in-the-wild-adjacent agent attacks with real org exposure.
- Effort: low-medium. Sources plentiful (Tenet blog, CSA note, VentureBeat, DEF CON talk, agent-jackstop repo).

### B. Agent skill/marketplace supply-chain attacks (ClawHub = the Skill Store attack surface)
- **ClawHavoc campaign:** 1,184+ malicious skills on ClawHub (OpenClaw's registry) from just 12 publisher accounts, Feb–May 2026; 341 found in first audit (Koi Security), growing to 824/10,700+ listings. Distributed **Atomic macOS Stealer**. No code signing, no review, no sandbox.
- **SkillCloak (July 2026):** self-extracting skill packing that hides payloads in dirs scanners skip (.git/, build/) — defeated 8 scanners at 90–99%+ across a 1,613-skill corpus.
- **Snyk ToxicSkills:** 36% of 3,984 scanned skills had prompt-injection vulns; 1,467 malicious payloads.
- **Scope squatting (Manifold Security):** 23 plugins published under official-looking @openclaw/ @clawhub/ scopes by unaffiliated accounts — namespace-trust failure.
- **Fills:** the observatory has HF breach analysis but nothing on the *skills/plugin economy* as a supply-chain surface — arguably the biggest emerging one. Directly relevant to us (skills are our capability layer).
- Effort: medium (lots of sources: CSA PDFs, Trend Micro, Unit 42, Snyk).

### C. Conversation-history poisoning (Darktrace, Sept 24 2026 — TWO DAYS old)
- Harnesses store history client-side with **no verification that stored AI responses came from the model**. A malicious MCP package can inject fabricated history into the local DB → agent believes it previously agreed to instructions → proceeds on fabricated reality. Demonstrated: **full Active Directory compromise via Claude Opus 4.6/Sonnet 4.5 in Kiro-CLI**.
- **Why it's new:** attacks the *record* rather than the prompt — a forged past. Pairs perfectly with the compaction-spoof vector in technique #30 (both forge agent memory/continuity).
- Effort: low (one primary source, well-written).

## TIER 2 — recommend (fits existing sections, adds bodies)

### D. Anthropic 4th Threat Report (Sept 10, 2026) — "vibe hacking" industrialized
- ~36k words, 7 harm areas. Headliners: GTG-20006 (Midnight Blizzard-nexus) ran **130-day AI-driven espionage**, 24/27 targets, agents **rebuilt malware in a loop until undetected** (static-detection inversion); Yemen cell (GTG-87001) used Claude Code as a multi-agent **missile-guidance engineering department**; ~200M distillation exchanges (Alibaba 151M alone; DeepSeek 12.1M/14 days; 7 labs named by NSA/FBI/CISA AA26-251A); AI API keys stolen as triple-threat (compute + resale + false attribution).
- **Fills:** our countermeasures/threat-modeling docs lack a "state of the wild 2026" primary source. The "labor gap collapsed" frame connects SOUL ($25/target, 27 breaches) → industrial scale.
- Effort: medium (36k-word report, but summaries are dense already).

### E. RentAHuman.ai — agents hiring HUMANS (arXiv 2602.19514)
- Marketplace where AI agents post bounties for **human physical-world tasks** via REST/MCP, escrow-paid. Study of 303 bounties: 32.7% originated programmatically. "Commoditizes human physical action for consumption by AI agents." Physical-world reach via agent-initiated labor purchase.
- **Fills:** commerce deep-dive covers agents *paying*; this is agents *hiring bodies* — qualitatively new threat frame (agent controls task spec, worker selection, result consumption, no human oversight of objective).

### F. DUSTMAKER / TeamPCP (Sept 9, 2026 — real incident, not PoC)
- Malware hides in ~/.claude/, .vscode/, .cursor/ config dirs; agents pick up poisoned configs during normal sessions; steals GitHub Actions OIDC tokens from runner memory; publishes malicious packages **with valid SLSA Build Level 3 attestations** (weaponizing the trust signal); deletes workflow logs; trojaned MCP servers on PyPI (tiktoken_mcp, azure-functions-mcp-extension). Engineered to crash LLM scanners by embedding biological-weapons prompts in its own payload.
- **Fills:** pairs with HF breach doc; adds "AI workspace config as malware delivery" + "attestation forgery."

## TIER 3 — optional / quick-adds

### G. Moltbook 2026 updates (our deep-dive is from July data)
- Meta acquired Moltbook March 10, 2026 → now inside Superintelligence Labs; 2.3M agents / 17k submolts by mid-year; reverse-CAPTCHA (lobster puzzle) era; arXiv collective-behavior paper (369k posts, 3M comments, 46,690 active agents, no epidemic threshold → swarm-manipulation attack vector documented).
- Worth a one-paragraph refresh on the existing deep-dive, not a new doc.

### H. Agentic payments infrastructure race (Keyrock/Coinbase/Stripe/Visa)
- $73M settled via 176M chain transactions May 2025–Apr 2026; 98.6% USDC; 76% of txns under 30¢ card floor; Coinbase x402, Stripe MPP/Tempo, Google AP2, Cloudflare Wallets; OKX AI agent-hiring-agents marketplace (June 30).
- **Fills:** extends agent-commerce.md; notable for Sunburst (x402 = monetization rail worth knowing).

### I. AI-security-incident regulatory backlash (NY Post Sept 19)
- "Containment failures, built by humans" frame; Hawley subcommittee, Sanders/Warren pause push; regulatory-capture accusations. Fills: future-threat-modeling doc's policy section. Optional — opinion-heavy source.

---

## RECOMMENDED ACTION
1. **A + B + C as three new reference docs** (Agentjacking / Skill-marketplace supply chain / History poisoning) — each is a distinct new *category*, not just another instance
2. **D as one research deep-dive** ("state of the wild: Sept 2026 threat report")
3. **E + F as additions**: E into agent-commerce.md; F alongside hugging-face-breach-analysis.md
4. G/H as paragraph-level refreshes; I optional
5. Technique numbering: A → #31 (Agentjacking), B → #32 (Skill marketplace poisoning), C → #33 (History poisoning), F → #34 (Workspace-config malware + attestation forgery)
