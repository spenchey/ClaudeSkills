---
name: website-seo-audit
description: Monthly website-level SEO audit combining keyword gap analysis, money page audit, service+city page identification, Google Search Console analysis, and review sentiment analysis. Feeds into content generation pipeline. Use when the cron runs the monthly website audit or when the user mentions "keyword gap," "money pages," "city pages," "GSC analysis," "page 2 keywords," "striking distance," or "review sentiment."
tags:
  - marketing
  - seo
  - local-seo
  - website
  - analytics
metadata:
  version: 1.0.0
  cadence: monthly
  covers: prompts 9,10,11,12,13
---
# Website SEO Audit

Monthly website-level SEO audit for Motor Inn Auto Group. Identifies keyword gaps, page 2 opportunities, missing city pages, and customer language patterns. This audit produces the prioritized action list that drives the next month's SEO work.

## Pre-Flight

1. Read `~/.agents/product-marketing-context.md` for business context
2. Read `~/motor-inn-seo/tools/seomachine/config/competitors.json` for competitor domains
3. Activate venv: `source ~/motor-inn-seo/.venv/bin/activate`
4. Load prior audit: `~/motor-inn-seo/audits/website-seo/website-seo-latest.md`
5. Confirm GA4 CLI works: `/Users/spencerheywood/.local/bin/ga4 -p 364125348 -s 7daysAgo -e today traffic`
6. Check GSC connection status — if not connected, skip GSC sections and note it

## Tools

- **GA4 CLI:** `/Users/spencerheywood/.local/bin/ga4` — property 364125348
- **Credentials:** `~/.config/gcloud/ga4-service-account.json`
- **SEO scripts:** `~/motor-inn-seo/tools/seomachine/` — research_quick_wins.py, research_competitor_gaps.py, etc.
- **Site crawler:** `~/motor-inn-seo/tools/claude-seo/scripts/fetch_page.py` + `parse_html.py`
- **Browser:** `gstack-browse` for competitor site analysis
- **No paid tools.** GA4 + GSC + direct crawling only.

## Audit Sections

---

### Section 1: Keyword Gap Audit (Prompt 9)

**Goal:** Find every keyword competitors rank for that Motor Inn doesn't.

**Steps:**
1. Run competitor gap analysis:
   ```bash
   cd ~/motor-inn-seo
   source .venv/bin/activate
   python tools/seomachine/research_competitor_gaps.py
   ```
2. If the script outputs a report, use it as the primary data source
3. Supplement with GA4 page data to understand what Motor Inn currently ranks for:
   ```bash
   ga4 -p 364125348 -s 30daysAgo -e today -f json pages --limit 100
   ```
4. Use `gstack-browse` to check competitor sites for pages Motor Inn lacks:
   - Service pages (do they have pages for services Motor Inn doesn't cover?)
   - Location pages (do they have city-specific pages Motor Inn lacks?)
   - Model-specific pages (Silverado, Tundra, Equinox, etc.)
   - Comparison pages (vs competitor, vs similar models)

**Filter gap keywords to high-value only:**
- Must contain one of: city name (Carroll, Spirit Lake, Iowa), service type, vehicle make/model, "near me," "best," "dealer"
- Monthly search volume 20+ (local market — lower threshold than national)
- Skip: celebrity, net worth, biography, dating, reddit, lawsuit, complaint (from skip_terms in competitors.json)

**Output table:**

```
| Keyword | Est. Volume | Competitors Ranking | Motor Inn Has Page? | Action |
|---------|-------------|---------------------|---------------------|--------|
| [kw]    | [vol]       | [which ones]        | Yes/No              | Optimize existing / Create new |
```

**Priority scoring:**
- High: Multiple competitors rank + volume 100+ + Motor Inn has no page
- Medium: 1 competitor ranks + volume 50+ OR Motor Inn has weak page
- Low: Volume under 50 or only tangentially relevant

---

### Section 2: Money Page Audit (Prompt 10)

**Goal:** Find pages that are almost ranking and need one push.

**Steps:**
1. Pull GA4 page performance (last 90 days):
   ```bash
   ga4 -p 364125348 -s 90daysAgo -e today -f json pages --limit 200
   ```
2. Pull GA4 traffic sources to understand organic vs paid:
   ```bash
   ga4 -p 364125348 -s 90daysAgo -e today -f json sources
   ```
3. If GSC is connected, pull ranking data:
   ```bash
   # GSC queries — use the seomachine module if available
   python ~/motor-inn-seo/tools/seomachine/data_sources/modules/google_search_console.py
   ```

**Identify four categories:**

| Category | Criteria | Action |
|---|---|---|
| Page 2 goldmine | Ranking positions 11-20, 100+ impressions/month | Title tag + H1 + content optimization |
| High impressions, low clicks | 500+ impressions, CTR under 2% | Meta description + title rewrite |
| Zero-ranking pages | Pages on site with no organic traffic | Evaluate: dead weight or untapped? |
| Cannibalization | Multiple pages ranking for same keyword | Consolidate or differentiate |

**For each page 2 keyword, check the ranking page:**
- Is the keyword in the title tag?
- Is it in the H1?
- Is it in the first 100 words?
- Page word count (thin = under 500 words)
- Internal links pointing to this page

Use `fetch_page.py` + `parse_html.py` to analyze Motor Inn pages:
```bash
python ~/motor-inn-seo/tools/claude-seo/scripts/fetch_page.py [URL] --output /tmp/page.html
python ~/motor-inn-seo/tools/claude-seo/scripts/parse_html.py /tmp/page.html --url [URL] --json
```

**Build 30-day optimization sprint:**
- Week 1: Title tag + H1 fixes for top 10 page 2 keywords — write the exact new title + H1
- Week 2: Content additions for pages under 500 words — list pages + target word count
- Week 3: Internal linking fixes — list exact "from page → to page" link additions
- Week 4: Meta description rewrites for low-CTR pages — write the exact new meta descriptions

---

### Section 3: Service + City Page Gap (Prompt 11)

**Goal:** Identify missing service × city combination pages.

**Build the matrix:**

| Service → | Carroll | Spirit Lake | Okoboji | Denison | Sac City | Lake City |
|---|---|---|---|---|---|---|
| New Chevrolet | ? | ? | ? | ? | ? | ? |
| New Toyota | ? | ? | ? | ? | ? | ? |
| Used Cars | ? | ? | ? | ? | ? | ? |
| Auto Service | ? | ? | ? | ? | ? | ? |
| Oil Change | ? | ? | ? | ? | ? | ? |
| Financing | ? | ? | ? | ? | ? | ? |

**Steps:**
1. Crawl `motorinnautogroup.com` sitemap or use `gstack-browse` to map all existing pages
2. Check each service × city combination — does a dedicated page exist?
3. Mark each cell: Exists / Missing / Partial (mentioned but not dedicated)

**For the top 10 missing combinations by search potential:**
Generate a content brief (NOT full page copy — that's the content-gap-builder skill):
- Suggested URL slug
- SEO title (under 60 chars)
- Meta description (under 155 chars)
- H1
- Key sections to include
- Internal linking targets (3 existing pages to link from)

**Output briefs to:** `~/motor-inn-seo/content/drafts/city-pages/briefs-$(date +%Y-%m).md`

---

### Section 4: GA4 / GSC Deep Analysis (Prompt 12)

**Goal:** Extract the performance signals that inform all other sections.

**GA4 pulls:**
```bash
# Last 90 days vs prior 90 days
ga4 -p 364125348 -s 90daysAgo -e today -f json traffic
ga4 -p 364125348 -s 180daysAgo -e 91daysAgo -f json traffic
ga4 -p 364125348 -s 90daysAgo -e today -f json sources
ga4 -p 364125348 -s 90daysAgo -e today -f json pages --limit 200
ga4 -p 364125348 -s 90daysAgo -e today -f json devices
ga4 -p 364125348 -s 90daysAgo -e today report -d eventName -m eventCount
```

**Key metrics to calculate:**
- Total organic sessions (90d) and trend
- Organic as % of total traffic
- Top organic landing pages (by sessions)
- Key events: used_vdp, new_vdp, form_submit, asc_click_to_call, asc_form_submission
- Mobile vs desktop organic split
- New vs returning organic users

**If GSC is connected, also pull:**
- Top queries by clicks (90d)
- Top queries by impressions (90d)
- Average position for priority keywords
- Pages with position improvements
- Pages with position declines

---

### Section 5: Review Sentiment Analysis (Prompt 13)

**Goal:** Extract the emotional language customers actually use — then feed it into website copy.

**Steps:**
1. Use `gstack-browse` to read last 100 reviews for each competitor GBP
2. Also read last 100 Motor Inn reviews

**Extract from all reviews:**
- Top 20 emotional words used most (e.g., "relieved," "impressed," "finally," "honest")
- Top 10 specific outcomes mentioned (e.g., "fixed in one visit," "no pressure," "best price")
- Top 5 fears/frustrations mentioned before service (e.g., "worried about cost," "bad experience elsewhere")
- Exact phrases used when recommending to others (the money phrases)
- Language patterns in 5-star reviews that don't appear in 3-star reviews

**Compare Motor Inn sentiment vs competitors:**
- What emotional words appear in Motor Inn reviews but not competitors? (differentiators)
- What emotional words appear in competitor reviews but not Motor Inn? (gaps)

**Generate from sentiment data:**
1. Suggested homepage headline + subheadline using customer language
2. 3 social proof statements that mirror how customers actually talk
3. Review request script that naturally prompts customers to use high-value phrases

**Output to:** `~/motor-inn-seo/research/sentiment-analysis-$(date +%Y-%m).md`

---

## Output Format

### Monthly Report: `~/motor-inn-seo/audits/website-seo/website-seo-$(date +%Y-%m).md`

```markdown
# Website SEO Audit — [Month Year]

## Executive Summary
- [3-4 sentences: top finding, biggest opportunity, biggest risk]

## Keyword Gaps
[Top 20 gap table]
### New pages needed: [count]
### Existing pages to optimize: [count]

## Money Pages
### Page 2 Goldmine: [count] keywords within striking distance
### 30-Day Sprint Plan
- Week 1: [title/H1 fixes — list exact changes]
- Week 2: [content additions — list pages]
- Week 3: [internal linking — list connections]
- Week 4: [meta descriptions — list rewrites]

## Service × City Matrix
[Matrix table]
### Missing combinations: [count]
### Top 10 to build: [list with briefs in drafts folder]

## GA4 Performance
[Key metrics table — 90d vs prior 90d]
### Organic trend: [up/down/flat]
### Key events: [conversion metrics]

## Customer Language (Sentiment)
### Money phrases: [top 5]
### Emotional gaps vs competitors: [list]
### Suggested copy updates: [in drafts folder]

## Priority Actions (Next 30 Days)
1. [highest impact — with specific deliverable]
2. [second]
3. [third]
4. [fourth]
5. [fifth]
```

### Symlink latest: Copy to `website-seo-latest.md`

## Delivery

1. Save report to `~/motor-inn-seo/audits/website-seo/`
2. Save briefs to `~/motor-inn-seo/content/drafts/city-pages/`
3. Save sentiment analysis to `~/motor-inn-seo/research/`
4. Git commit: `cd ~/motor-inn-seo && git add -A && git commit -m 'Monthly website SEO audit [month]' && git push`
5. Post executive summary + top 3 actions to Slack #automated-marketing (C0AGB08UY9W)
6. Post any P1 findings to Slack #seo-monitoring (C0AC3BP5XPF)

## Feeds Into

- **content-gap-builder skill:** Takes the city page briefs and keyword gaps to generate full page content
- **seo-monthly-report skill:** Uses GA4/GSC data for the executive rollup
- **Existing programmatic-seo skill:** City page templates built from the matrix

## Escalate When

- GA4 CLI fails or returns no data
- Organic traffic dropped more than 20% month-over-month
- A high-value page disappeared from rankings entirely
- DealerOn site shows indexation issues (redirect chains, canonical problems)
- GSC shows a manual action or security issue

## Rules

- **NEVER mention days on lot** in any generated content
- All content drafts to `~/motor-inn-seo/content/drafts/` — Spencer reviews before anything publishes
- Free tools only — no DataForSEO, no Ahrefs, no SEMrush
- If a seomachine script fails, note the error and continue with other sections — don't block the whole audit
- GA4 property ID is always 364125348 — never guess a different one
