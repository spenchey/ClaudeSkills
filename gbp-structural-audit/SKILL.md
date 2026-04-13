---
name: gbp-structural-audit
description: Weekly audit of Google Business Profile structural elements — categories, attributes, services section, business description, and photos. Compares against competitors and outputs a prioritized fix list. Use when cron runs the Monday GBP audit or when the user mentions "GBP categories," "GBP attributes," "GBP services," "GBP description," or "GBP photos."
tags:
  - marketing
  - seo
  - local-seo
  - gbp
metadata:
  version: 1.0.0
  cadence: weekly
  day: monday
  covers: prompts 1,2,6,7,8
---
# GBP Structural Audit

You are running a weekly structural audit of the Motor Inn Auto Group Google Business Profile against its competitors. Structural elements change slowly — this audit catches drift and missing optimizations.

## Pre-Flight

1. Read `~/.agents/product-marketing-context.md` for business context (NAP, GBP URL, competitor GBP URLs, service areas, keywords).
2. Read `~/motor-inn-seo/tools/seomachine/config/competitors.json` for competitor GBP URLs.
3. Load prior audit if it exists: `~/motor-inn-seo/audits/gbp-structural/gbp-structural-latest.md`

If any GBP URL is missing from context, **stop and escalate** — do not guess URLs.

## Tools

- **Browser:** Use `gstack-browse` to load GBP listings and extract data
- **Output:** Markdown report + spreadsheet-format comparison tables
- **No paid tools.** This audit uses only direct GBP scraping.

## Audit Sections

Run all 5 sections every week. Each section follows the same pattern: scrape ours, scrape competitors, compare, output gaps.

---

### Section 1: Category Audit

**What to extract from each GBP listing:**
- Primary category
- All secondary categories
- For each competitor: which map pack searches they appear in

**Steps:**
1. Open Motor Inn GBP listing via `gstack-browse`
2. Extract primary category and all secondary categories
3. Repeat for each competitor GBP listing
4. Build comparison table:

```
| Category | Motor Inn | Champion Ford | Wittrock | Coleman | Okoboji Toyota |
|----------|-----------|---------------|----------|---------|----------------|
| [cat]    | Primary   | Secondary     | —        | Primary | Secondary      |
```

5. Generate priority list:
   - **P1 (non-negotiable):** Categories ALL competitors have that we don't
   - **P2 (strong rec):** Categories 2+ competitors have that we don't
   - **P3 (differentiation):** Categories only 1 competitor has — evaluate fit

**Decision rule:** If a category is relevant to any Motor Inn service (Chevy, Buick, GMC, Toyota, used cars, service center), recommend adding it. If not relevant, skip with a note.

---

### Section 2: Attributes Audit

**What to extract from each GBP listing:**
- All visible attributes and tags (e.g., "veteran-owned," "free estimates," "wheelchair accessible," "online appointments," "Wi-Fi," "restrooms")

**Steps:**
1. Scrape attributes from Motor Inn GBP
2. Scrape attributes from each competitor GBP
3. Build comparison table:

```
| Attribute | Motor Inn | Champion Ford | Wittrock | Coleman | Okoboji Toyota |
|-----------|-----------|---------------|----------|---------|----------------|
| [attr]    | Yes       | Yes           | No       | Yes     | No             |
```

4. Generate three lists:
   - **Table stakes:** Attributes ALL competitors have — add immediately
   - **Strong rec:** Attributes 2+ competitors have
   - **Differentiator:** Attributes only 1 competitor has

5. For each missing attribute: note ranking impact (high/medium/low) and CTR impact (yes/no)

**Automotive-specific attributes to always check for:**
- Online appointment scheduling
- Test drives available
- Service department
- Parts department
- Financing available
- Trade-in accepted
- Certified pre-owned
- Wi-Fi / waiting area amenities

---

### Section 3: Services Section Audit

**What to extract from each GBP listing:**
- Every service listed
- Whether each service has a description
- Full description text if present

**Steps:**
1. Scrape Motor Inn GBP services section
2. Scrape competitor GBP services sections
3. Build comparison table:

```
| Service | Motor Inn (has desc?) | Champion Ford | Wittrock | Coleman | Okoboji Toyota |
|---------|-----------------------|---------------|----------|---------|----------------|
| [svc]   | Yes (42 words)        | Yes (60 words)| No       | —       | Yes (35 words) |
```

4. Cross-reference against Motor Inn website (`motorinnautogroup.com`) — flag services on the site but missing from GBP
5. For every Motor Inn service that has no description or a weak description (under 30 words), write an optimized replacement:
   - 2-3 sentences, 40-60 words
   - Include primary service keyword naturally
   - Mention at least one service area (Carroll, Spirit Lake, Okoboji)
   - Include a specific customer benefit
   - Sound human, not robotic

**Output service descriptions to:** `~/motor-inn-seo/content/drafts/gbp-services/services-$(date +%Y-%m-%d).md`

---

### Section 4: Description Audit

**What to extract from each GBP listing:**
- Full business description text
- Character count

**Steps:**
1. Extract Motor Inn GBP description
2. Extract competitor descriptions
3. Build comparison table:

```
| Business | Char Count | Keywords Used | Service Areas | Trust Signals | CTA |
|----------|-----------|---------------|---------------|---------------|-----|
```

4. Analyze: what do ALL competitors mention? What does nobody mention? What does Motor Inn say that nobody else bothers with?
5. If Motor Inn description is suboptimal (missing keywords, missing service areas, over/under 750 chars), write 3 replacement versions:
   - **Version 1 — Keyword-focused:** Maximum ranking signal
   - **Version 2 — Conversion-focused:** Written to generate calls
   - **Version 3 — Trust-focused:** 90+ years, family-owned, community
   - All versions: under 750 chars, include target keywords + Carroll/Spirit Lake/Okoboji
   - All versions: sound like a real person, not marketing copy

**Output descriptions to:** `~/motor-inn-seo/content/drafts/gbp-description/description-$(date +%Y-%m-%d).md`

---

### Section 5: Photo Audit

**What to extract from each GBP listing:**
- Total photo count
- Estimated upload frequency (photos in last 30/90 days if visible)
- Photo types present: team, job site, vehicles, showroom, service bays, exterior, before/after, branded trucks

**Steps:**
1. Count and categorize Motor Inn GBP photos
2. Count and categorize competitor photos
3. Build comparison table:

```
| Metric | Motor Inn | Champion Ford | Wittrock | Coleman | Okoboji Toyota |
|--------|-----------|---------------|----------|---------|----------------|
| Total  | X         | X             | X        | X       | X              |
| Last 30d | X       | X             | X        | X       | X              |
| Team   | X         | X             | X        | X       | X              |
| Vehicles | X       | X             | X        | X       | X              |
```

4. Generate 4-week photo upload plan:
   - Target: beat top competitor's velocity by 50%
   - Shot list per week: what to photograph, where, why
   - Photo naming convention: `motorinn-[service]-[location]-[date].jpg`
   - Geotagging instructions for Carroll and Spirit Lake locations

**Output photo plan to:** `~/motor-inn-seo/content/drafts/gbp-photos/photo-plan-$(date +%Y-%m-%d).md`

---

## Output Format

### Weekly Report: `~/motor-inn-seo/audits/gbp-structural/gbp-structural-$(date +%Y-%m-%d).md`

```markdown
# GBP Structural Audit — [date]

## Executive Summary
- [1-2 sentences: what changed since last week, top finding]

## Categories
[comparison table]
### Missing Categories (Prioritized)
- P1: [list]
- P2: [list]
- P3: [list]

## Attributes
[comparison table]
### Missing Attributes (Prioritized)
- Table stakes: [list]
- Strong rec: [list]

## Services
[comparison table]
### Services Missing from GBP: [list]
### Services Needing Better Descriptions: [list]

## Description
[comparison table]
### Current Motor Inn Description: [text]
### Recommendation: [keep / rewrite — if rewrite, versions in drafts folder]

## Photos
[comparison table]
### Upload Plan Summary: [X photos/week for 4 weeks]

## Action Items (This Week)
1. [highest impact action]
2. [second highest]
3. [third]
```

### Symlink latest: After saving the dated report, copy it to `gbp-structural-latest.md` in the same directory for next week's diff.

## Delivery

1. Save report to `~/motor-inn-seo/audits/gbp-structural/`
2. Save any draft content to `~/motor-inn-seo/content/drafts/gbp-*/`
3. Git commit: `cd ~/motor-inn-seo && git add -A && git commit -m 'GBP structural audit [date]' && git push`
4. Post P1 findings only to Slack #seo-monitoring (C0AC3BP5XPF)
5. Post action items to Slack #automated-marketing (C0AGB08UY9W)

## Escalate When

- Any GBP listing returns 404 or is unavailable
- Motor Inn GBP shows a category or attribute that was removed since last audit (possible hijack)
- Competitor count changes (new competitor appeared or one closed)
- Any competitor has 3+ categories Motor Inn lacks — flag as urgent

## Rules

- **NEVER mention days on lot** in any generated content (weight 0.98 — instant failure)
- All draft content goes to `~/motor-inn-seo/content/drafts/` — Spencer reviews before anything publishes
- Free tools only — no DataForSEO, no Ahrefs, no SEMrush
- If `gstack-browse` fails on a GBP listing, retry once, then skip that listing and note it in the report
