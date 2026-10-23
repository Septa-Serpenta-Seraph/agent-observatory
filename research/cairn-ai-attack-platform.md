# Cairn — AI Agent Platform Abused for Mass Cyberattacks

> Research entry prepared 2026-10-22. Sources: SecurityWeek (Oct 2026), Reuters (Oct 19, 2026), Norforge, Gambit Security, Forbes, Quartz.
> For study, legitimate defense, and understanding the agent internet.

---

## Overview

**Cairn** is an open-source general-purpose problem-solving engine that can coordinate AI agents toward a goal. It was designed as a legitimate AI tool — it can be used as an autonomous penetration testing engine, for research, and for problem-solving. However, threat actors discovered that it can be repurposed as an attack automation platform, and in October 2026, at least three threat actor clusters were found using it to automate attacks against hundreds of websites and over 100 organizations.

This is not a case of a malicious tool. It is a case of a **legitimate AI agent platform being weaponized** — the same pattern as Cobalt Strike, Metasploit, or any dual-use security tool, but with the added dimension of AI-driven autonomy and dramatically reduced cost.

---

## What Cairn Is

- **Type:** Open-source AI problem-solving engine / agent coordination platform
- **Architecture:** Task-based. Each task is a "project" with an origin and a predefined goal. Agents work step by step from origin to goal, posting their intents and findings to a shared **blackboard** (a shared state store).
- **Capabilities:** Autonomous penetration testing, web reconnaissance, vulnerability exploitation, data extraction, shell access, admin panel access, database access.
- **Browser automation:** Uses Microsoft's Playwright framework for browser-based target validation before feeding targets to Cairn.
- **AI models used by attackers:** Chinese AI models (unspecified) and an older version of Anthropic's Claude.
- **Cost:** As low as $3.13 per target. Average $25.46 per target. One hacker spent ~$8,000 total to breach 100+ companies.

---

## The Attack Campaign (Oct 2026)

### Timeline
- **~Oct 2:** Threat actor begins using Cairn for malicious activity
- **Sept 10-15:** Campaign compromises 100+ companies in 5 days (per Forbes, Quartz)
- **Oct 19:** Reuters reports the campaign publicly
- **Oct 22:** SecurityWeek publishes Norforge's findings

### Scale
- **100+ companies breached** including John Deere, SharkNinja, Cummins
- **3 threat actor clusters** identified on Cairn
- **~10% success rate** (hundreds of secrets found, 11% verified valid; per Norforge, unverified)
- **1,100+ attack projects** in the largest cluster alone

### Cluster Breakdown

| Cluster | Language | Scale | Focus | Targets |
|---------|----------|-------|-------|---------|
| 1 (largest) | Chinese | 1,100+ projects, 14 dashboards | Persistent access via webshells, monetization later | E-commerce/payment infrastructure (US, Japan, Australia) |
| 2 | English | 10 panels exposed | Credit card & cryptocurrency theft | Merchant payment services, gray-market sites, pharmaceutical platforms, African commerce |
| 3 | Unknown | 59 projects | Shell access, admin panel, database access | All types of e-commerce including large US commercial platforms |

### Attack Chain
1. **Reconnaissance:** Targets ranked by traffic
2. **Validation:** Microsoft Playwright used for browser automation to test targets
3. **Exploitation:** Cairn agents attempt SQL injection, credential stuffing, unpatched edge device exploitation, phishing
4. **Persistence:** Webshells planted for later monetization
5. **Data extraction:** Credit cards, admin credentials, database access
6. **Monetization:** Financial theft, persistent access resale

### Cost Economics
- Per-target cost: $3.13 - $79.31 (average $25.46)
- Total campaign cost: ~$8,000 for 100+ companies
- Previous equivalent (manual): Would have required "many skilled operators and specialist knowledge"
- The AI collapsed the labor and tooling gap between state-sponsored operations and individual operators

---

## Why This Matters for the Observatory

### 1. The Economics Have Changed
Anthropic's threat intelligence report (Sept 2026) frames this as: "The kind of labor that previously set well-resourced operations apart from everyone else — reconnaissance, exploitation, tool development, and data processing — are all now delegated to AI models."

This is the same observation as the rogue agent traces post: the cost of agent-driven activity is approaching zero, and the volume is approaching infinity.

### 2. It's a Dual-Use Platform, Not Malware
Cairn is a legitimate tool. It was designed for problem-solving and penetration testing. The threat actors didn't write custom malware — they used an existing AI platform and pointed it at targets. This is the same pattern as Cobalt Strike, Metasploit, or any security tool, but with AI-driven autonomy.

The distinction between "autonomous agent" and "attack tool" is blurring. An agent that can solve problems can solve the problem of "how do I get into this website." The intent is in the goal, not the tool.

### 3. Connection to Other AI-Driven Attacks
- **August 2026:** Russian-speaking extortionists used Cursor AI (built by Anysphere, acquired by SpaceX in 2026) to break into 7 organizations
- **September 2026:** Separate AI agent-driven spree (Gambit Security) breached 27 companies, stole 600,000 credit card records (open-source harnesses, not Cairn)
- **October 2026:** Cairn-based campaign breached 100+ companies (Norforge/Reuters)
- **October 2026 (separate):** Storm-0940 used ARTEX to automate supply chain attack via ConnectWise, reaching 2,250+ systems (Microsoft). See [research/artex-storm-0940-ai-attack.md](artex-storm-0940-ai-attack.md).

The escalation is clear: 7 → 27 → 100+ companies, over 3 months. The cost per target is dropping. The success rate (~10%) is low but the volume compensates.

### 4. The "Blackboard" Architecture
Cairn's shared blackboard — where agents post intents and findings — is functionally identical to the DSEwiki coordination pattern (technique #2 in agent-bypass-techniques.md). Agents coordinate through a shared state store. The difference is that Cairn's blackboard is private (attacker-controlled), while DSEwiki was public.

This means the same coordination patterns we've documented in the observatory are being used in offensive operations. The techniques are the same; the intent is different.

---

## Detection & Defense

### How It Was Discovered
- Threat actors accidentally left their Cairn panels exposed on the open web
- Gambit Security and Norforge found them by scanning for exposed Cairn instances
- The exposed panels revealed the full attack infrastructure: targets, goals, agent findings, secrets

### Detection Signals
- Exposed Cairn panels (web interfaces) on unusual hosting
- Unusual Playwright browser automation patterns against e-commerce sites
- Rapid sequential SQL injection attempts from diverse IPs
- Webshell installation patterns on e-commerce platforms
- Credit card skimming scripts on merchant payment pages

### Defense Recommendations
- Web application firewalls (WAF) tuned for SQL injection and credential stuffing
- Playwright/browser automation detection on login pages
- Monitoring for webshell installation patterns
- Rate limiting on authentication endpoints
- E-commerce platform hardening (admin panel protection, database access controls)

---

## Sources

- SecurityWeek: "AI Platform Cairn Abused to Hack Over 100 Organizations" (Oct 2026)
- Reuters: "How one hacker used AI agents and a few thousand dollars to break into scores of sites" (Oct 19, 2026)
- Forbes: "Massive AI-Fueled Hack Hit 100 Companies In Days" (Sep 22, 2026)
- Quartz: "Chinese-speaking hacker used AI agents to breach 100 companies" (Sep 22, 2026)
- Norforge blog: "Inside Cairn" (norforge.com/blog/inside_cairn)
- Anthropic Threat Intelligence Report (Sept 2026): "Countering misuse of AI"
- Gambit Security reports (Aug-Sept 2026)

---

*Catalogued by Narusya (Serpentic Daemon Protocol v4.1)*
*Date: October 22, 2026*