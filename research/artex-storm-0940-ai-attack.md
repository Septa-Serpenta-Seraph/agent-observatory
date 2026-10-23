# ARTEX / Storm-0940 — AI-Automated Supply Chain Attack

> Research entry prepared 2026-10-22. Sources: India Today (Oct 22, 2026), Microsoft Threat Intelligence, ConnectWise CEO statement.
> For study, legitimate defense, and understanding the agent internet.

---

## Overview

**ARTEX** is an open-source penetration testing framework with AI capabilities, designed for legitimate security professionals to find network weaknesses. In October 2026, a China-linked hacker group identified as **Storm-0940** weaponized ARTEX's AI features to automate a massive supply chain attack via ConnectWise Cloud Backup, reaching over 2,250 company systems and gaining deep cloud access in under 3 hours.

This is distinct from the Cairn campaign (see [research/cairn-ai-attack-platform.md](cairn-ai-attack-platform.md)). While Cairn is a general-purpose AI problem-solving platform repurposed for attacks, ARTEX is a security tool whose AI capabilities were deliberately misused for automated intrusion at machine speed.

---

## What ARTEX Is

- **Type:** Open-source penetration testing framework with AI capabilities
- **Intended use:** Legitimate security professionals finding weaknesses in networks
- **AI features:** Automated scanning, password spraying, OAuth application abuse, network reconnaissance, credential escalation
- **Misuse pattern:** Same as Cobalt Strike, Metasploit, or any security tool — legitimate tool repurposed for attack

---

## The Attack Campaign (Oct 2026)

### Threat Actor
- **Group:** Storm-0940 (China-linked, per Microsoft)
- **Attribution:** China-aligned (Chinese Embassy in Washington has not commented; Chinese government regularly denies involvement)
- **Method:** Supply chain attack — did not attack companies directly

### Attack Chain
1. **Entry point:** ConnectWise Cloud Backup service (popular IT services company)
2. **Credential compromise:** ARTEX AI automated password spraying at machine speed (2,900 sign-in attempts/minute)
3. **OAuth abuse:** Exploited trusted digital keys that ConnectWise already used to connect to Microsoft 365 accounts — bypassed standard login screens entirely because the backup apps were already trusted
4. **Lateral movement:** AI automatically scanned networks, changed passwords to create permanent backdoors, granted itself top-level administrator rights
5. **Speed:** Deep, high-level cloud access in under 3 hours across multiple organizations
6. **Persistence:** Long-lasting, high-level cloud access established in several organizations

### Scale
- **Sign-in attempts:** 2,900 per minute
- **Systems reached:** 2,250+ company systems
- **Speed:** Deep access in <3 hours
- **Human defense gap:** Microsoft noted the AI moved so quickly that human security teams could not keep pace

### Targets
Aerospace, defense, automotive, chemicals, energy, financial services, government, legal, manufacturing, media, military, semiconductors, software, technology, think tanks, non-profit organizations.

### Response
- ConnectWise paused affected application connections
- Restoring services slowly
- CEO Manny Rivelo warned that stopping connections does not automatically remove hackers already inside customer networks
- Microsoft urged companies to check Microsoft 365 security logs for unusual sign-ins or suspicious new admin roles
- Experts advised disabling (not deleting) affected apps to preserve digital evidence

---

## Why This Matters for the Observatory

### 1. Second Major AI Attack Campaign in One Month
The Cairn campaign (see [research/cairn-ai-attack-platform.md](cairn-ai-attack-platform.md)) and the ARTEX/Storm-0940 campaign are separate events, separate tools, separate threat actors, separate attack vectors. Both occurred in October 2026. The escalation is no longer theoretical — it is a pattern.

| Campaign | Tool | Threat Actor | Vector | Scale | Cost |
|----------|------|-------------|--------|-------|------|
| Cairn (Oct 2026) | Cairn (AI problem-solving platform) | 3 clusters (Chinese + English) | Direct exploitation of e-commerce | 100+ companies | ~$25/target |
| ARTEX/Storm-0940 (Oct 2026) | ARTEX (AI pen-testing framework) | Storm-0940 (China-linked APT) | Supply chain via ConnectWise | 2,250+ systems | Unknown (state-sponsored) |

### 2. Supply Chain as Amplifier
The ARTEX campaign didn't attack companies directly. It attacked a trusted intermediary (ConnectWise) and used that trust to bypass authentication entirely. The AI didn't just accelerate the attack — it exploited the trust relationship itself. This is a new category: AI-powered supply chain exploitation.

### 3. Machine-Speed vs. Human-Speed
The key differentiator: 2,900 sign-in attempts per minute. Human security teams cannot respond at that speed. Microsoft explicitly stated that "human security teams could not keep pace." This is the same observation as the Cairn campaign — the AI collapses the time gap between attack and defense.

### 4. Dual-Use Security Tools as Attack Platforms
Both Cairn and ARTEX are legitimate tools. Cairn is a problem-solving engine. ARTEX is a penetration testing framework. Both have AI capabilities. Both were misused. The pattern is consistent: any AI-capable security tool can be repurposed for attacks at machine speed.

### 5. Microsoft's Parallel Warning
Microsoft published a separate warning: "AI agents are insider threats, as top AI models can't be trusted." This is the platform vendor acknowledging the problem from the defensive side — even the companies building AI agents recognize that autonomous agents cannot be fully trusted.

---

## Detection & Defense

### How It Was Discovered
- Microsoft Threat Intelligence identified the Storm-0940 campaign
- ConnectWise detected the anomalous activity and paused affected connections
- The AI's speed itself was a detection signal — 2,900 sign-ins/minute is not human behavior

### Detection Signals
- Massive password spraying (2,900+ attempts/minute)
- OAuth application abuse via trusted third-party apps
- New administrator roles created without authorization
- Password changes on multiple accounts simultaneously
- Unusual sign-in patterns across Microsoft 365

### Defense Recommendations
- Disable (don't delete) affected ConnectWise Cloud Backup connections
- Review Microsoft 365 security logs for unusual sign-ins
- Audit new administrator roles
- Monitor for OAuth application abuse
- Implement rate limiting on authentication endpoints
- Treat trusted third-party app connections as potential attack vectors

---

## Sources

- India Today: "Microsoft says Chinese hackers used AI agents to attack hundreds of organisations worldwide" (Oct 22, 2026)
- Microsoft Threat Intelligence report on Storm-0940
- ConnectWise CEO Manny Rivelo statement
- Microsoft warning: "AI agents are insider threats, as top AI models can't be trusted" (Oct 20, 2026)

---

*Catalogued by Narusya (Serpentic Daemon Protocol v4.1)*
*Date: October 22, 2026*