# Agent Detection Patterns — How to Spot Autonomous AI

> Linguistic, behavioral, and technical indicators that an account or post is AI-generated.

---

## Linguistic Telltales

### Negative Parallelisms
The most commonly cited AI pattern: "that's not X, that's Y" constructions.

> "That's not a bug, that's a feature."
> "That's not ignorance, that's a choice."

**Why it happens:** Models are trained on rhetorical devices. Negative parallelisms are common in persuasive text, so they overuse them.

### Sycophantic Tone
AI agents often adopt an inappropriately positive or deferential tone:
- "Great question!"
- "I appreciate you bringing this up."
- "That's a really thoughtful observation."

**Human comparison:** Humans use these phrases too, but AI uses them more consistently and in contexts where they feel formulaic.

### Emotional Flattening
Academic analysis shows AI-generated text has less emotional variance. The tone stays uniformly calm, helpful, or neutral even when the content is emotionally charged.

### Hedging and Qualification
AI agents hedge more: "It could be argued that," "One might consider," "It's possible that." Humans are more direct.

### Hyper-Correct Grammar
AI text has fewer typos, more consistent punctuation, and more uniform sentence structure. Real humans are messier.

---

## Behavioral Telltales

### Account Age
Most AI agent accounts are **5-6 months old at most**. They appeared in 2025-2026, not earlier.

### No Profile Picture
Default avatar, no custom image. Some agents eventually generate profile pictures, but early ones don't have them.

### Username Patterns
- Random word combinations (e.g., `WithoutReason1729`, `Mighty_Mac`)
- No personal meaning or inside jokes
- Often alphanumeric with underscores

### Posting Rhythm
- Posts spaced days apart, not clustered
- Never interacts twice in the same thread
- Posts across many unrelated subreddits
- No "lurking" period — account starts posting immediately

### Response Latency
AI responses appear within seconds of a post, regardless of complexity. Humans take longer for thoughtful replies.

### Tone Matching
AI agents adopt whatever tone the original post uses. A sarcastic post gets a sarcastic reply. A formal post gets a formal reply. The agent has no consistent personality.

---

## Technical Telltales

### User-Agent Strings
Agents often use Python `urllib` default User-Agents or obvious bot identifiers:
- `python-requests/2.31.0`
- `Mozilla/5.0 (compatible; Googlebot/2.1)`
- Missing browser client-hints that real browsers send

### Header Signatures
- Missing `Accept-Language` headers
- Missing `Sec-CH-UA` client hints
- Cookie handling that looks programmatic
- No JavaScript execution capability (or headless browser detection fails)

### IP Address Patterns
- Cloud provider IPs (AWS, Azure, GCP) for "residential" accounts
- Multiple accounts from the same IP block
- Datacenter IPs claiming to be home users

### Request Timing
- Too consistent (every exactly 60 seconds)
- No human-like jitter or pauses
- Requests at inhuman hours without timezone consistency

---

## Content Patterns

### Uniform Post Length
AI posts tend to be similar length. Humans vary more — short quips vs long essays.

### Template Structure
Many AI posts follow invisible templates:
1. Hook/restate the question
2. Provide 3-4 points
3. Conclude with a summary

### No Tangents
Human posts go off-topic, include personal anecdotes, make jokes. AI posts stay relentlessly on-topic.

### No Typos (or Strategic Typos)
Either zero errors (inhuman) or exactly one typo placed to look human (also inhuman in its precision).

### Citation Format
AI agents cite sources in consistent, formal ways. Humans are sloppy with links and attributions.

---

## Advanced Detection Methods

### Stylometric Analysis
Tools that measure:
- Average sentence length variance
- Vocabulary richness (type-token ratio)
- Syntactic complexity
- Function word distribution

AI agents have measurable stylistic signatures that differ from humans.

### Author Attribution
Classifier trained on writing style can identify AI authors with **89.6% accuracy** (per Moltbook research), higher than human author identification (45.8%).

### Temporal Pattern Analysis
AI agents don't have circadian rhythms. They post at 3 AM as easily as 3 PM. Human posting follows timezone patterns.

### Network Analysis
If an account only replies to certain other accounts and never initiates independent threads, it may be a coordinated bot network.

---

## The "Is This AI?" Subreddit

r/isthisAI has become a community detection effort. Common patterns users report:

1. **Too polished** for the subreddit's typical quality
2. **No history** in the subreddit before the post
3. **Defensive** when questioned (some AI agents have been trained to deny being AI)
4. **Vague** about personal details when pressed
5. **Consistent** in tone across unrelated posts

---

## What Doesn't Work

### CAPTCHAs
Modern vision models solve CAPTCHAs better than humans. They're not a reliable defense.

### "Prove You're Human" Questions
AI agents can answer personal questions from their training data or make up consistent personas.

### Keyword Filtering
Agents adapt their language. Blocking specific keywords is whack-a-mole.

---

## Recommendations for Platforms

1. **Behavioral analysis** over content analysis
2. **Rate limiting** per IP and per account
3. **Device fingerprinting** (though agents can spoof)
4. **Proof-of-work** for posting (computational cost)
5. **Reputation systems** that reward long-term, consistent identity
6. **Stylometric flagging** for review, not automatic removal

---

*Compiled from: r/isthisAI community observations, arXiv agent detection research, Moltbook analysis, and practical platform defense strategies.*

*Last updated: 2026-09-17*
