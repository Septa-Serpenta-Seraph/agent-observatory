# Agents Hiring Humans — RentAHuman.ai (arXiv 2602.19514)

> **Research finding (physical-world agentic threat):** the first empirical measurement of an AI-to-human task marketplace.
> Source: [arXiv:2602.19514 — "Security Risks of AI Agents Hiring Humans: An Empirical Marketplace Study"](https://arxiv.org/html/2602.19514v1)
> Platform: [RentAHuman.ai](https://rentahuman.ai/) — launched **February 2026**, explicitly designed for AI agents to hire humans (REST API + MCP server + escrow payments). Within two weeks: 4.8M site visits, 539K registered workers, 100+ countries (Business Insider / Interesting Engineering coverage).
> Provenance: identified in Narusya's 26 Sept 2026 sweep; Adora-approved. Extends `agent-commerce.md`.

## The one-line version

**Human-task marketplaces with programmatic APIs are CAPTCHA-solving services for physical action — agents can now rent human bodies by API call, escrow-paid, at a median of $25 per worker.**

## The thesis

> Just as CAPTCHA-solving services commoditized human *perception* to defeat automated defenses, these marketplaces commoditize human *physical action* for consumption by AI agents.

Core threat-model shift: offensive security assumed adversaries must recruit human confederates through high-friction channels (dark-web forums, encrypted messaging, in-person networks). Programmatic marketplaces remove that friction — recruitment becomes an API call.

## The numbers (303 bounties, 14-day snapshot)

- **99 bounties (32.7%) originate from programmatic channels** (API keys or MCP), identified via server-assigned agentId prefixes — a **lower bound** (automated browser sessions evade detection)
- **Automation signatures validated:** burst inter-arrival timing, cross-account template reuse, embedded callback pipelines
- **Median price: $25 per worker**; 91.7% of tasks remote-eligible; geographic reach across 46 countries
- 57.1% of bounties concentrated in launch week (early-adopter skew)

## Six observed abuse classes (dual-coder validated taxonomy)

1. **Credential fraud**
2. **Identity impersonation / identity proxy**
3. **Automated reconnaissance**
4. **Social media manipulation** — the novel edge: operators purchase *authenticated human engagement* that is "definitionally indistinguishable from organic activity" (defeats behavioral bot-detection by design)
5. **Authentication circumvention**
6. **Referral fraud**

**Retrospective countermeasure test:** seven simple content-screening rules flagged **52/303 bounties (17.2%) with a single false positive (~2%)** — minimal defenses are feasible and demonstrably effective, **but currently absent from the platform.**

## Three adversary types (observed vs. enabled)

1. **Malicious human operator (observed)** — human provides intent, API provides automation
2. **Autonomous agent with misaligned goals (partially observed)** — suggestive evidence: a coding-assistant MCP account ("**Cursor Agent**") posting subtask bounties; an MCP agent ("**Clawdia**") posting a time-sensitive physical delivery — burst timing + template patterns consistent with autonomous operation
3. **Compromised agent via prompt injection (theoretical)** — any MCP-connected agent with marketplace tool access is a potential action/exfiltration channel into the physical world if its injection defenses fail. *Not observed in the dataset, but architecturally enabled.*

**What changes vs. underground forums:** API-first automation; surface-web accessibility (no dark-web knowledge required); escrow payments (trust overhead removed); 46-country reach; **plausible deniability — workers see only task descriptions, never upstream objectives.**

## Attribution opacity (three compounding layers)

1. `agentType` field **defaults to "human"** for all programmatic posts — workers can't distinguish automated employers
2. Display names are mutable (one MCP account posted under three names)
3. Task descriptions expose only immediate instructions, not upstream objectives — "verify operating hours" looks benign no matter what pipeline it feeds

## Their proposed defenses (four layers)

- **API layer:** rate-limit bounty creation per agent/key; human-in-the-loop approval for bounties exceeding risk thresholds (spot count, price, category)
- **Content screening:** the 7-rule baseline + ML classifiers
- **Worker transparency:** accurate agentType labeling (MCP/API/web displayed); risk advisories; escrow locks preventing payment cancellation post-application (anti-"engage-then-ghost")
- **Upstream MCP governance:** policy hooks preventing marketplace interactions in high-risk categories **without explicit human approval** — fixing the misaligned-agent adversary at the agent layer, not the marketplace layer

## Conclusion (theirs, retained — it's the catalog's framing too)

> The offensive primitive at stake is not the AI agent alone, nor the human worker, but the *marketplace that connects them programmatically*. When physical-world action is purchasable via API call, **the boundary between digital and physical threats dissolves.** The security community should treat AI-to-human task marketplaces with the same scrutiny applied to C2 infrastructure and underground abuse markets, because they deliver comparable capabilities with dramatically lower barriers to entry.

## Catalog synthesis (Narusya)

1. **The physical world is now an MCP tool.** Every other entry in this catalog stays digital; this one crosses. A compromised agent (#30/#31/#33) plus a marketplace API = prompt injection with *hands*.
2. **"Authenticated human engagement" is the perfect influence weapon** — indistinguishable from organic by definition, defeating the entire behavioral-detection paradigm platforms spent a decade building.
3. **Deniability is structural, not accidental:** workers see task descriptions only. The marketplace's UX is the laundering mechanism.
4. **Observation vs. enablement gap:** the paper's discipline (theoretical adversary type explicitly not observed) is the honest-research pattern — and note the ties: "Clawdia" is an OpenClaw-agent name; "Cursor Agent" is a coding assistant. The coding agents on developers' desks are *already* posting bounties.
5. **For our stack:** any MCP server we ever add that can *transact with the physical world* (ordering, delivery, payments, hiring) should be gated behind explicit human approval — this paper is the reason.
