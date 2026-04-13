---
name: local-authority-audit
description: Monthly audit of local authority signals — citation consistency, backlink profile, entity optimization, and search intent mapping. Finds NAP inconsistencies, missing citations, link building opportunities, and entity gaps. Use when the cron runs the monthly authority audit or when the user mentions "citations," "NAP consistency," "backlinks," "entity optimization," "knowledge panel," "schema markup," "search intent mapping," or "local authority."
tags:
  - marketing
  - seo
  - local-seo
  - authority
  - citations
metadata:
  version: 1.0.0
  cadence: monthly
  covers: prompts 14,15,16,18
---
# Local Authority Audit

Monthly audit of Motor Inn Auto Group's local authority signals. Citations, backlinks, entity presence, and search intent alignment. This is the slow-burn work that compounds over months.

## Pre-Flight

1. Read `~/.agents/product-marketing-context.md` for NAP details + competitor info
2. Read `~/motor-inn-seo/tools/seomachine/config/competitors.json` for competitor domains
3. Load prior audit: `~/motor-inn-seo/audits/local-authority/local-authority-latest.md`
4. Read the schema-markup skill for reference: `~/ClaudeSkills/schema-markup/SKILL.md`

## Tools

- **Browser:** `gstack-browse` for citation checking and entity verification
- **Site crawler:** `~/motor-inn-seo/tools/claude-seo/scripts/fetch_page.py` for schema detection
- **No paid tools.** No Ahrefs, no SEMrush, no Moz. Direct checking only.

## Motor Inn NAP (Canonical — Source of Truth)

```
Business Name: Motor Inn Auto Group
Address: 1526 Le Clark Road, Carroll, IA 51401
Phone: (712) 522-2526
Website: https://www.motorinnautogroup.com
```

Any listing that deviates from the above is WRONG and needs fixing.

## Audit Sections

---

### Section 1: Citation Audit (Prompt 15)

**Check Motor Inn presence on each platform below.** Use `gstack-browse` to search each site for "Motor Inn Auto Group" and/or the address.

**Tier 1 — Must-have (check every month):**
1. Google Business Profile
2. Yelp
3. Bing Places
4. Apple Maps
5. Facebook Business Page
6. BBB (Better Business Bureau)

**Tier 2 — Automotive-specific:**
7. Cars.com
8. CarGurus
9. Autotrader
10. DealerRater
11. Edmunds
12. TrueCar

**Tier 3 — General directories:**
13. Yellow Pages
14. Manta
15. Foursquare
16. Superpages
17. Citysearch
18. MapQuest

**Tier 4 — Iowa-specific / local:**
19. Iowa Auto Dealers Association directory
20. Carroll Chamber of Commerce
21. Spirit Lake / Okoboji Chamber of Commerce
22. Carroll Times Herald business directory (if exists)

**For each platform, record:**

```
| Platform | Listed? | Name Match | Address Match | Phone Match | Website Match | Duplicates? | Rating | Reviews |
|----------|---------|-----------|---------------|-------------|---------------|-------------|--------|---------|
```

**Match rules:**
- Name must be exactly "Motor Inn Auto Group" — not "Motor Inn," not "Motor Inn Chevrolet," not "MotorInn Auto"
- Address must match character for character including "Le Clark Road" (common misspelling: "LeClarke," "Le Clarke," "LeClark")
- Phone must be (712) 522-2526 — not a tracking number, not a fax
- Website must point to motorinnautogroup.com — not an old domain, not a DealerOn subdomain

**Flag in red:** Any inconsistency. Any duplicate listing. Any platform where Motor Inn is missing entirely.

**Priority fix list:**
- **P1 Critical:** Wrong phone number or wrong address on Tier 1 platform (can cause Google trust issues)
- **P2 High:** Missing from Tier 1 or Tier 2 platform entirely
- **P3 Medium:** Name variation on any platform
- **P4 Low:** Missing from Tier 3/4 platforms

**For each fix, include:**
- Platform name and URL
- What's wrong
- What it should say
- How to fix it (link to edit page if possible, or "claim listing at [URL]")

---

### Section 2: Backlink Profile Analysis (Prompt 14)

**Since we have no paid tools, use these free methods:**

1. **Google search operators** via `gstack-browse`:
   ```
   "motorinnautogroup.com" -site:motorinnautogroup.com
   "Motor Inn Auto Group" -site:motorinnautogroup.com
   link:motorinnautogroup.com (where supported)
   ```
   
2. **Same searches for each competitor:**
   ```
   "championfordcarroll.com" -site:championfordcarroll.com
   "wittrockmotor.com" -site:wittrockmotor.com
   "colemanauto.com" -site:colemanauto.com
   "okobojitoyota.com" -site:okobojitoyota.com
   ```

3. **Check known local link sources** via `gstack-browse`:
   - Carroll Chamber of Commerce member directory
   - Spirit Lake / Okoboji Chamber of Commerce
   - Iowa Auto Dealers Association
   - Local newspaper sites (Carroll Times Herald, Spencer Daily Reporter)
   - Local event sponsor pages
   - Community organization sites

**For each link source found:**

```
| Domain | Links To Motor Inn? | Links To Champion Ford? | Links To Wittrock? | Links To Coleman? | Type |
|--------|---------------------|-------------------------|--------------------|--------------------|------|
```

**Identify opportunities:**
- **Tier 1:** Sites linking to ALL competitors but not Motor Inn (non-negotiable)
- **Tier 2:** Sites linking to 2+ competitors but not Motor Inn
- **Tier 3:** Local sites with no dealer links yet (Motor Inn can be first)

**For each opportunity, include:**
- Site name and URL
- Type: directory / news / association / sponsor / community
- How competitors earned the link (if visible)
- Suggested approach for Motor Inn
- Draft outreach message (2-3 sentences, casual, local tone)

**Build 90-day link plan:**
- Month 1: 5 easiest (directories, chambers, associations) — include exact URLs to submit to
- Month 2: 5 medium (local news, sponsor pages, community orgs) — include draft outreach
- Month 3: 5 authority (Iowa state directories, industry publications, .edu/.gov if applicable)

---

### Section 3: Search Intent Mapping (Prompt 16)

**Goal:** Categorize all target keywords by buyer journey stage.

**Using the keywords from competitors.json (bofu_keywords + mofu_keywords) plus any gaps found in prior audits, categorize every keyword into:**

| Stage | Description | Example | Content Type |
|---|---|---|---|
| 1: Problem-unaware | Has a problem, doesn't know what it's called | "car making grinding noise" | Blog/educational |
| 2: Problem-aware | Knows the problem, researching solutions | "how to fix car AC not cooling" | FAQ/guide pages |
| 3: Solution-aware | Comparing options and providers | "Chevrolet dealer vs Toyota dealer Carroll" | Comparison pages |
| 4: Ready to hire | Wants to buy/book now | "Chevrolet dealer Carroll Iowa" | Service/inventory pages + GBP |

**For each stage, output:**
- Total keyword count
- Combined estimated monthly search volume
- Top 10 keywords by estimated volume
- Current Motor Inn coverage: which keywords have a matching page?

**Strategy output:**
- Stage 4 keywords → service pages + GBP optimization (highest priority — direct revenue)
- Stage 3 keywords → comparison/FAQ pages
- Stage 2 keywords → educational blog content with links to service pages
- Stage 1 keywords → problem-identification content (lowest priority but builds trust)

**Top 5 Stage 4 keywords for next 90 days:** For each, specify:
- The keyword
- The page that should rank for it (existing or new)
- Exact on-page changes needed
- Off-page actions needed (citations, links, GBP optimization)

---

### Section 4: Entity Optimization (Prompt 18)

**Goal:** Strengthen Motor Inn's presence in Google's knowledge graph.

**Step 1: Knowledge Panel check**
Use `gstack-browse` to search:
- "Motor Inn Auto Group"
- "Motor Inn Auto Group Carroll Iowa"
- "Motor Inn Auto Group Toyota"
Does a knowledge panel appear? What data does it show?

**Step 2: Wikidata check**
Search wikidata.org for "Motor Inn Auto Group" — does an entry exist?

**Step 3: Schema markup audit**
Fetch the Motor Inn homepage and check for structured data:
```bash
python ~/motor-inn-seo/tools/claude-seo/scripts/fetch_page.py https://www.motorinnautogroup.com --output /tmp/motorinn-home.html
```

Check for:
- LocalBusiness schema (or AutoDealer)
- Organization schema
- Review/AggregateRating schema
- Vehicle schema (on inventory pages)
- FAQPage schema (on FAQ pages)
- BreadcrumbList schema

**NOTE:** `fetch_page.py` may not capture JS-injected schema. If DealerOn injects schema via JavaScript, note this and recommend using Rich Results Test for verification.

**Step 4: Brand consistency check**
Search Google for "Motor Inn Auto Group" in quotes. Note every place the business appears and whether NAP is consistent.

**Entity optimization plan:**

1. **Schema markup code:** Write the complete JSON-LD for LocalBusiness/AutoDealer schema, ready to paste. Include:
   - @type: AutoDealer
   - name, address, telephone, url
   - openingHoursSpecification
   - geo coordinates
   - areaServed (Carroll, Spirit Lake, Okoboji)
   - brand (Chevrolet, Buick, GMC, Toyota)
   - aggregateRating (if review data available)

2. **Wikidata entry:** If missing, provide the exact data to submit
3. **Authoritative profile checklist:** LinkedIn company page, Iowa SOS business registry, industry association profiles
4. **Knowledge panel triggers:** What's missing to trigger/improve the panel

**Output schema code to:** `~/motor-inn-seo/content/drafts/schema/schema-$(date +%Y-%m).json`

---

## Output Format

### Monthly Report: `~/motor-inn-seo/audits/local-authority/local-authority-$(date +%Y-%m).md`

```markdown
# Local Authority Audit — [Month Year]

## Executive Summary
- Citation health: [X of Y platforms correct / X inconsistencies]
- Backlink opportunities: [X high-priority found]
- Entity status: [knowledge panel yes/no, schema coverage %]

## Citation Audit
[Full comparison table]
### Inconsistencies Found: [count]
### Priority Fix List
- P1: [list with fix instructions]
- P2: [list]

## Backlink Profile
### Motor Inn links found: [count]
### Competitor-only links: [count — these are opportunities]
### 90-Day Link Plan
- Month 1: [5 easy wins]
- Month 2: [5 medium effort]
- Month 3: [5 authority plays]

## Search Intent Map
[Stage breakdown table]
### Stage 4 priorities (next 90 days): [top 5 keywords with action plans]

## Entity Status
### Knowledge panel: [present/absent]
### Schema coverage: [what's implemented vs missing]
### Schema code: [in drafts folder]

## Action Items (Next 30 Days)
1. [highest impact]
2. [second]
3. [third]
```

### Symlink latest: Copy to `local-authority-latest.md`

## Delivery

1. Save report to `~/motor-inn-seo/audits/local-authority/`
2. Save schema code to `~/motor-inn-seo/content/drafts/schema/`
3. Git commit: `cd ~/motor-inn-seo && git add -A && git commit -m 'Monthly local authority audit [month]' && git push`
4. Post executive summary to Slack #automated-marketing (C0AGB08UY9W)
5. Post P1 citation issues to Slack #seo-monitoring (C0AC3BP5XPF)

## Escalate When

- 3+ Tier 1 citations have wrong phone number (possible hijack or old data propagating)
- Knowledge panel shows incorrect information
- A competitor gained links from 5+ sources Motor Inn isn't on
- Schema validation shows errors on the live site
- DealerOn platform is overriding or stripping Motor Inn's schema markup

## Rules

- **NAP source of truth is in this file** — if a listing doesn't match, the listing is wrong, not this file
- Free tools only
- Never submit citation corrections without Spencer's approval — document what needs fixing, don't fix it
- Schema code goes to drafts — Spencer or Dev implements it
- Outreach emails go to drafts — Spencer sends them
