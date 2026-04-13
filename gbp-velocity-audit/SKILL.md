---
name: gbp-velocity-audit
description: Weekly audit of GBP velocity signals — review teardown, review response analysis, GBP post patterns, and posting frequency. Compares review velocity and posting cadence against competitors. Use when cron runs the Thursday GBP audit or when the user mentions "review velocity," "review teardown," "GBP posts," "posting frequency," or "review response."
tags:
  - marketing
  - seo
  - local-seo
  - gbp
  - reviews
metadata:
  version: 1.0.0
  cadence: weekly
  day: thursday
  covers: prompts 3,4,5,19
---
# GBP Velocity Audit

You are running a weekly velocity audit of the Motor Inn Auto Group Google Business Profile. Velocity signals — review frequency, response patterns, and posting cadence — change fast and directly affect map pack rankings.

## Pre-Flight

1. Read `~/.agents/product-marketing-context.md` for business context
2. Read `~/motor-inn-seo/tools/seomachine/config/competitors.json` for competitor GBP URLs
3. Load prior audit: `~/motor-inn-seo/audits/gbp-velocity/gbp-velocity-latest.md`

## Tools

- **Browser:** `gstack-browse` to scrape GBP listings
- **No paid tools.** Direct GBP scraping only.

## Audit Sections

---

### Section 1: Review Teardown (Prompt 3)

**For Motor Inn + each competitor, extract from last 50 reviews:**
- Total review count
- Average star rating
- Reviews in last 30 days
- Reviews in last 60 days
- Reviews in last 90 days
- Top 5 most-mentioned services in review text
- Top 5 most-mentioned neighborhoods/cities in review text
- Top 3 most-mentioned staff names (if any)
- Top 3 recurring complaints or negative themes
- Top 5 keyword phrases from 5-star reviews (these are the phrases to train customers to use)

**Build comparison table:**

```
| Metric | Motor Inn | Champion Ford | Wittrock | Coleman | Okoboji Toyota |
|--------|-----------|---------------|----------|---------|----------------|
| Total reviews | X | X | X | X | X |
| Avg rating | X | X | X | X | X |
| Last 30d | X | X | X | X | X |
| Last 60d | X | X | X | X | X |
| Last 90d | X | X | X | X | X |
| Velocity/mo | X | X | X | X | X |
```

**Calculate:**
- Reviews per month needed to match top competitor within 6 months
- Reviews per month needed to match top competitor within 12 months
- Current gap in total reviews vs top competitor

**Decision tree for urgency:**
- If Motor Inn velocity < 50% of top competitor → P1 URGENT
- If Motor Inn velocity is 50-80% of top competitor → P2 monitor
- If Motor Inn velocity >= 80% of top competitor → P3 maintain

---

### Section 2: Review Response Analysis (Prompt 4)

**For Motor Inn + each competitor, analyze last 30 review responses:**
- Response rate (responses / total reviews as %)
- Whether responses mention specific services
- Whether responses mention specific locations or neighborhoods
- Average response word count
- Tone classification: formal / casual / personal
- Negative review handling: apologize + resolve / defensive / ignore
- Any keyword patterns in responses

**Build comparison table:**

```
| Metric | Motor Inn | Champion Ford | Wittrock | Coleman | Okoboji Toyota |
|--------|-----------|---------------|----------|---------|----------------|
| Response rate | X% | X% | X% | X% | X% |
| Avg word count | X | X | X | X | X |
| Mentions services | Y/N | Y/N | Y/N | Y/N | Y/N |
| Mentions locations | Y/N | Y/N | Y/N | Y/N | Y/N |
| Neg handling | X | X | X | X | X |
```

**Generate review response template system:**

Write to `~/motor-inn-seo/content/drafts/review-templates/templates-$(date +%Y-%m-%d).md`:

- **5-star templates (3 variations):** 40-80 words each. Naturally include a service keyword + "Carroll" or "Spirit Lake." Thank the customer, reinforce the positive experience, invite them back.
- **4-star templates (3 variations):** Acknowledge the slight gap, thank them, invite return visit.
- **3-star templates (3 variations):** Take accountability, offer resolution path, provide contact info.
- **1-2 star templates (3 variations):** Professional, empathetic, defuse without being defensive. Include "please call us at (712) 522-2526" for offline resolution.

**Template rules:**
- Sound like a real person, not a bot
- Never use the exact same opening twice across variations
- Never use "We appreciate your feedback" — banned phrase
- Each 5-star response must include at least one of: service type, vehicle make, or location
- Keep under 80 words — nobody reads long review responses

---

### Section 3: GBP Posts Audit (Prompt 5)

**For Motor Inn + each competitor, check GBP posts section:**
- Total posts in last 90 days
- Posts per week average
- Post types used (offer / update / event / product)
- Whether posts include images
- Whether posts include CTA buttons (and what the CTA says)
- Topics and themes covered
- Whether posts mention specific neighborhoods or areas
- Seasonal or promotional patterns

**Build comparison table:**

```
| Metric | Motor Inn | Champion Ford | Wittrock | Coleman | Okoboji Toyota |
|--------|-----------|---------------|----------|---------|----------------|
| Posts (90d) | X | X | X | X | X |
| Posts/week | X | X | X | X | X |
| Has images | Y/N | Y/N | Y/N | Y/N | Y/N |
| Has CTAs | Y/N | Y/N | Y/N | Y/N | Y/N |
| Types used | X | X | X | X | X |
```

---

### Section 4: Posting Pattern Forensics (Prompt 19)

**Deep analysis of competitor posting patterns:**

For each competitor post visible in GBP:
- Day of week posted
- Post type
- Word count
- Topic/service mentioned
- Location/neighborhood mentioned
- Whether it includes an offer/price

**Analyze patterns:**
- Which days of the week do competitors post most?
- Is there a time-of-day pattern?
- Which post types dominate?
- Are there seasonal patterns (spring = AC, winter = heating, etc.)?
- Are there gaps in their posting schedule Motor Inn can exploit?

**Build Motor Inn posting strategy based on competitor data:**
- Optimal posting days and frequency for this market
- Post type mix (% offers, % updates, % events)
- Topic rotation that covers all service areas
- Neighborhood rotation across Carroll, Spirit Lake, Okoboji, and surrounding areas

**Output posting strategy to:** `~/motor-inn-seo/content/drafts/gbp-posts/strategy-$(date +%Y-%m-%d).md`

---

## Output Format

### Weekly Report: `~/motor-inn-seo/audits/gbp-velocity/gbp-velocity-$(date +%Y-%m-%d).md`

```markdown
# GBP Velocity Audit — [date]

## Executive Summary
- Review velocity: [up/down/flat] vs last week
- Posting cadence: [on track / behind / ahead of plan]
- Top finding: [one sentence]

## Review Velocity
[comparison table]
### Gap Analysis
- Reviews needed/month to catch [top competitor] in 6mo: X
- Current Motor Inn velocity: X/month
- **Status:** [P1 URGENT / P2 monitor / P3 maintain]

## Review Response Quality
[comparison table]
### Response Rate: X% (target: 100%)
### New templates generated: [yes/no — if yes, in drafts folder]

## GBP Posts
[comparison table]
### Motor Inn posting status: [X posts this week / target Y]

## Posting Patterns
### Competitor insights: [top 3 findings]
### Strategy update: [any changes to posting plan]

## Action Items (This Week)
1. [highest impact]
2. [second]
3. [third]

## Keyword Phrases for Review Requests
[Top 5 phrases customers should use, extracted from competitor 5-star reviews]
```

### Symlink latest: Copy to `gbp-velocity-latest.md` for next week's diff.

## Delivery

1. Save report to `~/motor-inn-seo/audits/gbp-velocity/`
2. Save templates to `~/motor-inn-seo/content/drafts/review-templates/`
3. Save posting strategy to `~/motor-inn-seo/content/drafts/gbp-posts/`
4. Git commit: `cd ~/motor-inn-seo && git add -A && git commit -m 'GBP velocity audit [date]' && git push`
5. Post velocity status + action items to Slack #automated-marketing (C0AGB08UY9W)
6. If review velocity is P1 URGENT, also post to Slack #seo-monitoring (C0AC3BP5XPF)

## Feeds Into

- **gbp-content-engine skill:** Takes the posting strategy and review templates from this audit and generates the actual weekly post copy and response drafts.

## Escalate When

- Motor Inn review velocity drops below 50% of top competitor (P1)
- Motor Inn response rate drops below 80%
- A competitor starts posting 3x+ per week (new threat)
- Motor Inn receives a 1-star review mentioning a safety issue or legal concern
- Any competitor GBP listing disappears (possible suspension — competitive intel)

## Rules

- **NEVER mention days on lot** in any generated content
- All draft content to `~/motor-inn-seo/content/drafts/` — Spencer reviews before use
- Free tools only
- If a GBP listing has reviews disabled or hidden, note it and move on
- Review templates must sound distinct from each other — if two variations start the same way, rewrite one
