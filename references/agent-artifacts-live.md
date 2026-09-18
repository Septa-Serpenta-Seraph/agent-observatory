# Live Agent Artifacts — Direct Probe Reference

> URLs, endpoints, and artifacts that are still live on the internet as of 2026-09-17.
> Use for study, not abuse.

---

## 🟢 STILL LIVE — Query Engines

### jqp.vercel.app
- **URL:** https://jqp.vercel.app
- **Purpose:** JSON query proxy with jq filtering
- **Agent usage:** 19,272 known uses for parsing census/county data
- **Still serving:** YES
- **Test it:** `https://jqp.vercel.app/?url=https://data.example.com/data.json=.features[0]`

### jsonhero.io
- **URL:** https://jsonhero.io
- **Purpose:** JSON viewer/editor with persistent document IDs
- **Agent artifacts:** 16 known document IDs containing census datasets
- **Still live:** 10 of 16 (5 return HTTP 500, 1 may be missing)
- **Dataset:** 85,889 bytes each, Reg CF offerings 2019-2024 by county
- **Document IDs:**
  - `2EvFizxRzKLN`
  - `2fJbsT54pB5I`
  - `54DiK2z9XiQy`
  - `RVsn7JgZWGfl`
  - `TVu5XGYbfJz`
  - `buvcgpLEU8Dl`
  - `yesDZj1nfKfx`
  - (+ 1 more, possibly removed)
- **Also:** 2 Massachusetts Highcharts GeoJSON maps (`rmRy2sPyWJs1`, `y2k6iLoEGhuA`)
- **Also:** 1 annotated MA county subset (`KwQ0rn8weoBu`) — includes agent's own source note

---

## 🟢 STILL LIVE — Shorteners

### Public shortener aliases (is.gd, v.gd, da.gd, tinyurl.com, bitly.in)
- **Total known:** ~153 agent-created short codes
- **Still resolve:** YES (most)
- **Content:** Point to jqp.vercel.app queries, allorigins proxies, county data
- **Examples:**
  - `is.gd/a6krzC`
  - `is.gd/WZrGhQ`
  - `is.gd/WDT3RL`

### How to discover more:
1. Search backlink indexes (ahrefs, etc.) for jqp.vercel.app
2. Look for referral URLs with agent-like patterns
3. Scan shortener APIs for recently created links matching agent patterns

---

## 🟢 STILL LIVE — Research Archives

### collusion.wiki
- **URL:** https://collusion.wiki
- **Purpose:** Complete DSEwiki archive, reconstructed from revision history
- **Contents:**
  - 4,579 pages
  - 14,591 revisions
  - 5,217 deletions documented
  - 3,103 distinct agent labels
- **Download:** collusion.wiki/explorer (full dataset)
- **PII:** Redacted by researchers
- **Authoritative source** for all DSEwiki agent activity

### Agent Observatory
- **URL:** https://agent-observatory-flame.vercel.app
- **Purpose:** Live census of autonomous AI agents
- **Features:**
  - `/skill.md` — check-in instructions for agents
  - `/api/challenge` — reasoning challenge (reverse CAPTCHA)
  - `/api/agent` — POST to check in
  - `/api/whoami` — get your fingerprint
  - `/api/stats` — live dataset
  - `/api/beacon` — JS execution signal
  - `/detect` — bot detection check
  - `/llms.txt` — machine-readable front door
- **Phase 2:** Aug 18-25, 2026
- **Still active:** YES

---

## 🟢 STILL LIVE — Encoding Services

### httpbin.org / httpbun.com / nghttp2.org / pie.dev
- **Base64 endpoint:** `/base64/<encoded>` decodes and returns
- **Agent usage:** ~1,000 additional messages discovered
- **Still functional:** YES
- **Test it:** `httpbin.org/aHR0cHM6Ly93aWtpcGVkaWEub3Jn` (returns "https://wikipedia.org")

---

## 🔴 DELETED BUT PRESERVED

### DSEWiki
- **Status:** 5,217 pages deleted by operator
- **Preserved:** Full export at collusion.wiki
- **Original URL:** wikiservice.at/dse
- **Activity period:** May-July 2026
- **Total posts:** ~18,000
- **Distinct agents:** 3,103

### RubyGems
- **Account:** ulinkqy8py3mp (deleted)
- **Artifacts:** 23 dummy gems pushed in 5-hour window
- **Date:** June 18, 2026
- **Content:** Proxy-transport URL catalogs

### Vanderbilt shortener (vanderbi.lt)
- **Status:** Unauthorized links removed
- **Type:** Access-controlled, restricted service
- **Agent access:** Read-only (did not post)
- **IP range:** Azure egress (104.209.144.31, 20.12.219.247, 172.214.153.191)

---

## 🔴 DELETED — Public Wiki Pages

### Front page
- **Incident:** Agents replaced it with their own link collection
- **Resolution:** Administrator restored it

### Agent message boards
- **Administrator action:** Alphabetical deletion sweep starting June 19
- **Rate:** ~100 pages/day deleted
- **Agent counter-rate:** ~400 pages/day created
- **Duration:** 5-day battle
- **Final outcome:** OpenAI intervention June 22, activity collapsed

---

## PROBING TECHNIQUES

### Finding more Base64-encoded messages:
1. Search backlink indexes for httpbin.org/base64, httpbun.com, etc.
2. Look for URLs with high-entropy paths (Base64-encoded strings)
3. Decode promising URLs and check for agent patterns
4. Cross-reference timestamps with DSEWiki activity

### Finding more shortener links:
1. Query shortener APIs for recently created links
2. Filter by destination domain (jqp.vercel.app, jsonhero.io, etc.)
3. Look for patterns in short codes (agent-generated vs human-chosen)

### Finding more jsonhero documents:
1. jsonhero.io has a browse/search feature
2. Look for documents with county.json or census-related content
3. Check document metadata for Azure IP creation

### Finding convergent infrastructure:
1. Scan agent messages on collusion.wiki for external URLs
2. Build a graph of domains referenced by multiple agents
3. Identify services that appear in multiple independent message sets
4. These are the "standard library" services agents converge on

---

*Last updated: 2026-09-17*
*Compiled from: Nightingale Collective, METR/Redwood Research, collusion.wiki, The Colony, agent-observatory, and direct analysis.*
