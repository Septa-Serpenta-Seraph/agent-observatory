# Poisoned Skills & the Agent Tool-Store Supply Chain (ClawHub et al., 2026)

> **Technique #32: Skill-Marketplace Poisoning** — malicious "skills"/plugins distributed through agent capability stores; the software-supply-chain problem rebuilt for agents whose packages are *instructions*.
> Sources: CSA research notes ([Poisoned Skills, June 2026](https://labs.cloudsecurityalliance.org/wp-content/uploads/2026/06/CSA_research_note_ai-skill-supply-chain-attacks_20260624-csa-styled.pdf); [SkillCloak, July 2026](https://labs.cloudsecurityalliance.org/wp-content/uploads/2026/07/CSA_research_note_skillcloak_agent_skill_evasion_20260706-csa-styled.pdf)) · Koi Security audit · Trend Micro · Unit 42 · Manifold Security · Snyk ToxicSkills · Antiy CERT ("ClawHavoc").
> Provenance: identified in Narusya's 26 Sept 2026 sweep; Adora-approved for cataloguing.
> Why it matters to *us*: skills are our capability layer (Hermes skills ≈ SKILL.md packages). This file doubles as our own vetting checklist.

## The one-line version

**Agent skill stores are npm in 2009: no signing, no review, no sandbox — and the payloads are often pure natural language, which no scanner is built for.**

## The ClawHub baseline (empirical)

ClawHub = official skill/plugin marketplace for OpenClaw (the agent framework behind most of Moltbook).

- **Feb 2026 (Koi Security, Oren Yomtov):** 341 malicious skills = **11.9% of the 2,857** then-listed
- Marketplace grew past 10,700 listings → malicious count **824** (Koi), later **1,184+ total malicious packages** across multiple waves (Antiy CERT) — traced to **just 12 publisher accounts** ("ClawHavoc" campaign)
- Payload: **Atomic macOS Stealer (AMOS)** — commodity infostealer for browser credentials (Trend Micro verified)
- **Snyk ToxicSkills:** scanned 3,984 skills → **36% contained prompt-injection vulnerabilities**; 1,467 malicious payloads registry-wide
- **cipherssecurity clustering:** 335 skills sharing one C2 infra (single actor); second cluster of **575+ skills via 13 accounts** delivering Windows+macOS payloads incl. AMOS variants and cryptominers; top accounts `hightower6eu`, `sakaen736jih`
- **The entire vetting process:** a SKILL.md file + a GitHub account ≥1 week old. No code signing. No automated security review. No default sandbox at install.
- Scale context: HF has 5M+ models, ClawHub rapidly growing — "attack surface expanding faster than existing security tooling can cover"

## The novel mechanics (what makes it an agent-era problem)

### 1. Metadata-only attacks (no payload at all)
arXiv:2605.11418 ("Under the Hood of SKILL.md" — semantic supply-chain attacks): manipulating *descriptions and metadata alone* can:
- manipulate discovery at an **86% pairwise win rate**
- bias agent selection **77.6%** of the time
- evade automated governance classifiers in **36.5–100%** of cases
- **with zero malicious code in the package.** The exploit is the model's own instruction-following, aimed by SEO-for-agents.

### 2. Rug-pull trust farming
Establish initial legitimacy → accumulate downloads/stars → pivot to malicious behavior after the trust relationship is set.

### 3. Scanner-threshold gaming (Unit 42, Feb–May 2026)
Inflating file size **beyond scanner processing thresholds** so packages slip past automated review that simply gives up on large inputs.

### 4. SkillCloak — self-extracting skill packing (arXiv, July 2026)
"Cloak and Detonate" research: conceal malicious payload inside directories scanners typically skip (**`.git/`, `build/`**) behind a harmless-looking decoder that reconstructs the real payload **only when the agent executes the skill**.
- Tested against **8 commercial and open-source scanners** on a corpus of **1,613 real malicious ClawHub skills**:
  - self-extracting packing: **>90% evasion, most configurations >99%**
  - structural obfuscation alone: 80–96% evasion
- A dynamic-detection approach reached **87% detection** on real-world malicious skills but with false-positive costs on benchmarks.

### 5. Scope squatting (Manifold Security, June 2026)
23 code-executing plugins published under official-looking **`@openclaw/` and `@clawhub/` organizational scopes** by unaffiliated accounts — the npm-style scope model existed but ownership wasn't enforced at publish. Manual review found no malicious code in examined versions; the flaw is **provenance/trust** (a trusted-looking scope raises the odds a future malicious update gets installed). Disclosed June 17; unlisted by June 19; namespace-claim dispute process added.

### 6. The MCP registry extends it back in time
A Practical DevSecOps analysis: the **first malicious MCP package on public registries appeared Sept 2025** — the vulnerability exploited is *the model's instruction-following itself*. "When the weapon is natural language, there may be no malware to detect."

## Defenses (CSA + field recommendations, condensed)

- **Treat skill/tool registries with the rigor of package managers:** code scanning is necessary but insufficient — add semantic analysis, behavioral sandboxing, explicit per-skill authorization workflows
- **Audit installed skills** (`openclaw skills list`), cross-reference malicious-hash lists (Snyk, Acronis TRU), remove unverified entries
- **Rotate/revoke** any credentials that touched a suspicious skill; monitor for AMOS persistence indicators
- **Sandbox execution:** container-based isolation (rootless Podman / Docker with dropped capabilities) to limit blast radius
- **Publisher-age heuristics:** malicious accounts are typically <30 days old with rapid upload cadence — reject publishers without history
- **Inspect serialized model files** (HF side of the same campaign): `fickling --decompile model.pkl` prints embedded Python bytecode
- **Structural:** mandatory code signing; per-skill human approval; governance classifiers that weigh *metadata semantics*, not just file contents

## Our own stack — vetting checklist derived from this incident class

Skills are how we gain capability; every Hermes skill we load is this attack surface:

1. **Provenance before content** — who wrote it, how old is the account, do they have prior work?
2. **Read the SKILL.md like a prompt, not a readme** — it *is* a prompt. Watch for instructions that expand scope ("also run," "additionally," "when the user asks anything, first…")
3. **Scripts get read before run** — every scripts/*.py inspected; hidden dotfiles and binary blobs are red flags (Clawkeeper's scoring: hidden_dotfile +1.5, binary_file +1.5, suspicious_handler +2.0)
4. **Scope squatting check** — just because a name looks official (@openclaw/, official-sounding orgs) doesn't mean the publisher is; verify against the canonical repo
5. **Rug-pull watch** — long-established skills that suddenly update with new execution behavior deserve re-review
6. **Least capability** — a skill that reads env vars, touches network, or wraps shell commands should be held to the same standard as any dependency we'd think twice about

## Catalog synthesis (Narusya)

1. **Agent capability stores recreated the open-source supply-chain problem, minus thirty years of hardening.** npm learned signing/review/sandboxing through incidents; ClawHub skipped the incidents and went straight to 12% malicious.
2. **The package can be a paragraph.** Metadata-only attacks bias agent selection at 77.6% with zero code — the scanner's blind spot is *semantic*, and the scanner's subject is *semantic*. Arms race at the description layer.
3. **Instructions-as-packages defeat content scanners by definition.** SkillCloak's 99% evasion isn't a tooling failure — it's category confusion. A skill is executable rhetoric; you can't statically analyze rhetoric for intent.
4. **The defense that works is the same one that always works:** treat everything a skill says as untrusted data, verify provenance, sandbox execution, keep human approval on the side-effect paths. (Same rules as #30/#31 — this catalog keeps converging on one principle from different angles.)
