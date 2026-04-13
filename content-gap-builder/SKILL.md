---
name: content-gap-builder
description: Generates full SEO page content from gaps identified in the monthly website-seo-audit — primarily service+city pages and content gap pages. Produces complete drafts with YAML frontmatter matching the motor-inn-seo pipeline. Use when the cron runs monthly content generation or when the user mentions "build city pages," "content gaps," "generate SEO pages," "service pages," or "location pages."
tags:
  - marketing
  - seo
  - local-seo
  - content-generation
  - programmatic-seo
metadata:
  version: 1.0.0
  cadence: monthly
  covers: prompt 17, prompt 11 generation
---
# Content Gap Builder

Generates full SEO page content for Motor Inn Auto Group based on gaps identified in the monthly website-seo-audit. Produces publish-ready drafts with proper frontmatter for the motor-inn-seo content pipeline.

## Pre-Flight

1. Read `~/.agents/product-marketing-context.md` for business context
2. Read the latest website SEO audit: `~/motor-inn-seo/audits/website-seo/website-seo-latest.md`
3. Read the latest city page briefs: `~/motor-inn-seo/content/drafts/city-pages/briefs-latest.md` (if exists)
4. Read the programmatic-seo skill for template patterns: `~/ClaudeSkills/programmatic-seo/SKILL.md`
5. Read brand voice: `~/clawd/dealership/marketing/brand-voice.json`
6. Read internal links map: `~/motor-inn-seo/tools/seomachine/context/internal-links-map.md` (if exists)
7. Check existing drafts: `ls ~/motor-inn-seo/content/drafts/` — don't duplicate what's already been written

If the website-seo-audit hasn't run this month, **stop and escalate** — building pages without gap data is guessing.

## What to Build

This skill generates two types of content:

### Type 1: Service + City Pages

For each missing service × city combination identified in the website audit matrix.

**Priority order:**
1. Carroll combinations first (primary location)
2. Spirit Lake / Okoboji second (secondary location)
3. Surrounding communities third (only if high search potential)

**Services priority:**
1. New vehicle makes (Chevrolet, Toyota, Buick, GMC)
2. Used cars / pre-owned
3. Auto service / repair
4. Specific services (oil change, tire, brake, etc.)
5. Financing / trade-in

### Type 2: Content Gap Pages

For high-priority keywords from the gap audit where Motor Inn has no ranking page and competitors do.

**Priority:**
1. Stage 4 (ready to hire) keywords — direct revenue pages
2. Stage 3 (solution-aware) keywords — comparison and FAQ pages
3. Stage 2 (problem-aware) keywords — educational content

## Page Template: Service + City

Each page follows this structure. Write complete content, not outlines.

```markdown
---
target_keyword: "[service] [city] Iowa"
secondary_keywords:
  - "[service] near [city]"
  - "best [service] in [city]"
  - "[service] [city] IA"
search_intent: transactional
page_type: location-service
priority: [high/medium/low]
status: draft
created: [YYYY-MM-DD]
author: rory
url_slug: "/[service]-[city]-iowa"
---

# [SEO Title — under 60 chars, includes service + city]

**Meta description:** [Under 155 chars. Service + city + compelling reason to click + CTA.]

## H1: [Service] in [City], Iowa — [Trust Signal]

[Opening paragraph — 100 words. Address the specific pain point of someone in this city needing this service RIGHT NOW. Reference something specific to the area — not generic. If Carroll: mention the community, Le Clark Road location, family-owned since 1934. If Spirit Lake: mention the lake area, seasonal needs, the drive from surrounding towns.]

## Why Choose Motor Inn for [Service] in [City]?

[150 words. Specific to this city. Reference:
- Proximity / drive time from this city
- Local landmarks or neighborhoods if relevant
- Area-specific vehicle needs (farming → trucks, lake → towing, winter → AWD/4WD)
- 90+ years of family ownership
- Specific team or department mention]

## Our [Service] Options

[200 words. What the service includes, what the process looks like, what the customer gets. Be specific to Motor Inn — mention actual makes (Chevrolet, Buick, GMC, Toyota) where relevant. Include pricing indicators if available ("competitive financing" not specific dollar amounts unless Spencer provides them).]

## What [City] Customers Say

[Placeholder section for reviews from this area. Include instruction comment:]
<!-- SPENCER: Add 2-3 actual customer reviews from [city] area here -->

## Frequently Asked Questions

### [Question 1 — specific to this city + service]
[Answer — 2-3 sentences]

### [Question 2 — specific to this city + service]
[Answer — 2-3 sentences]

### [Question 3 — specific to this city + service]
[Answer — 2-3 sentences]

## Ready to [Action]?

Call us today at **(712) 522-2526** or visit us at 1526 Le Clark Road, Carroll, Iowa. [If city is not Carroll: "We're just a [X] minute drive from [city] — worth the trip for [specific benefit]."]

---

**Internal links to add:**
1. [existing page URL] — link from [anchor text]
2. [existing page URL] — link from [anchor text]
3. [existing page URL] — link from [anchor text]
```

## Page Template: Content Gap (Educational / Comparison)

```markdown
---
target_keyword: "[gap keyword]"
secondary_keywords:
  - "[variation 1]"
  - "[variation 2]"
search_intent: [informational/commercial]
page_type: [blog/comparison/faq]
priority: [high/medium/low]
status: draft
created: [YYYY-MM-DD]
author: rory
url_slug: "/blog/[keyword-slug]"
---

# [SEO Title — under 60 chars]

**Meta description:** [Under 155 chars.]

[Full article content — 800-1500 words depending on topic depth. Structure with H2 and H3 headings. Include:
- Answer the search query in the first 100 words
- Provide genuinely useful information (not fluff)
- Reference Motor Inn naturally 2-3 times (not keyword stuffing)
- Include Iowa-specific context where relevant
- End with a soft CTA linking to the relevant service page]

---

**Internal links to add:**
1. [page] — [anchor text]
2. [page] — [anchor text]
```

## Content Rules

- **NEVER mention days on lot** — instant failure (weight 0.98)
- **Sound like Motor Inn, not a marketing agency.** Trust > polish. Neighbors > customers. Practical > flashy.
- **No AI slop.** No "In today's fast-paced world." No "Whether you're looking for X or Y." No "At Motor Inn, we understand that..." Read `~/ClaudeSkills/seo-audit/references/ai-writing-detection.md` if it exists.
- **Iowa voice.** These pages are for rural and small-town Iowans. Don't write like you're selling luxury cars in Manhattan.
- **Every page must provide unique value.** If two city pages differ only by the city name swapped in, the second page is thin content. Each page needs area-specific details.
- **No emoji** unless Spencer explicitly requests it.
- **Limit:** Generate max 5 pages per run. Quality over quantity.

## Output

### Files Created

Each page gets its own file:
- City pages: `~/motor-inn-seo/content/drafts/city-pages/[service]-[city]-iowa.md`
- Gap pages: `~/motor-inn-seo/content/drafts/gap-content/[keyword-slug].md`

### Summary File

`~/motor-inn-seo/content/drafts/build-summary-$(date +%Y-%m).md`:

```markdown
# Content Build Summary — [Month Year]

## Pages Generated: [count]

| Page | Type | Target Keyword | Priority | File |
|------|------|----------------|----------|------|
| [title] | city-page | [kw] | [high/med/low] | [filename] |
```

## Delivery

1. Save all draft files
2. Git commit: `cd ~/motor-inn-seo && git add -A && git commit -m 'Monthly content build: [count] pages [month]' && git push`
3. Post to Slack #automated-marketing (C0AGB08UY9W): "[X] SEO pages drafted: [list titles]. In drafts folder for Spencer's review."

## Escalate When

- Website audit data is missing or stale (dependency)
- All high-priority gaps already have draft content (nothing to build)
- Can't determine area-specific details for a city page (need Spencer's input on local knowledge)
- Generated page count would exceed 5 in a single run (prioritize and defer the rest)

## Feeds Into

- **Existing seo-audit skill:** New pages get audited after publishing
- **Existing programmatic-seo skill:** City page templates can inform future batch generation
- **schema-markup skill:** New pages need LocalBusiness + appropriate schema
