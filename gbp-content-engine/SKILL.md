---
name: gbp-content-engine
description: Generates GBP post copy and review response drafts based on the velocity audit output. Produces an 8-week posting calendar with full copy, image suggestions, and CTAs. Use when the cron runs the Thursday content generation step or when the user mentions "GBP posts," "write GBP content," "posting calendar," or "review response drafts."
tags:
  - marketing
  - seo
  - local-seo
  - gbp
  - content-generation
metadata:
  version: 1.0.0
  cadence: weekly
  day: thursday
  covers: prompt 5 output, prompt 4 output
---
# GBP Content Engine

You generate the actual content that goes on Motor Inn's Google Business Profile — post copy and review response drafts. This runs after the gbp-velocity-audit skill produces its analysis.

## Pre-Flight

1. Read `~/.agents/product-marketing-context.md` for business context
2. Read the latest velocity audit: `~/motor-inn-seo/audits/gbp-velocity/gbp-velocity-latest.md`
3. Read the latest posting strategy: `~/motor-inn-seo/content/drafts/gbp-posts/strategy-latest.md` (if exists)
4. Read the latest review templates: `~/motor-inn-seo/content/drafts/review-templates/` (if exists)
5. Check what posts have already been written: `~/motor-inn-seo/content/drafts/gbp-posts/`

If the velocity audit hasn't run this week, **stop and escalate** — content without data is guessing.

## GBP Post Calendar

### Structure

Generate posts for the next 2 weeks (rolling). Each week gets 2-3 posts.

### Post Types (rotate weekly)

| Week Pattern | Post 1 | Post 2 | Post 3 (optional) |
|---|---|---|---|
| A | Seasonal service promo | Neighborhood spotlight | Vehicle showcase |
| B | Before/after or completed work | Team spotlight | Educational/FAQ |

Alternate A/B weeks. Check what was posted last to maintain rotation.

### Post Requirements

Every post must include:
- **Word count:** 100-150 words
- **Target keyword:** At least one from the priority keywords list in product-marketing-context.md
- **Location mention:** At least one of: Carroll, Spirit Lake, Okoboji, or a surrounding community
- **CTA:** Clear call to action with phone number (712) 522-2526 or "visit us at motorinnautogroup.com"
- **Image suggestion:** Specific description of what photo to use (not "a photo of a car" — say "photo of the service bay with a technician working on a Silverado, shot from the customer waiting area angle")

### Post Copy Format

For each post, output:

```markdown
### Post [N]: [Title]
**Type:** [offer/update/event]
**Publish date:** [YYYY-MM-DD]
**Target keyword:** [keyword]
**Location:** [area mentioned]

**Copy:**
[Full post text, 100-150 words]

**CTA button:** [Book Now / Call Now / Learn More]
**CTA link:** [URL]

**Image:** [Specific photo description and where to get it — Glo3D inventory photo, staff photo, service bay shot, etc.]
```

### Neighborhood Rotation

Cycle through these areas across posts — never repeat the same area two posts in a row:
- Carroll (primary — 40% of mentions)
- Spirit Lake / Okoboji (secondary — 30%)
- Surrounding: Denison, Sac City, Lake City, Storm Lake, Spencer, Estherville (30%)

### Seasonal Content Calendar

| Month | Themes |
|---|---|
| Jan-Feb | Winter service (battery, tires, heat), tax refund vehicle deals |
| Mar-Apr | Spring maintenance, new model arrivals, trade-in season |
| May-Jun | Road trip prep, summer service, graduation gifts |
| Jul-Aug | Back-to-school vehicles, summer clearance |
| Sep-Oct | Fall maintenance, harvest season trucks, new model year |
| Nov-Dec | Winter prep, holiday deals, year-end clearance |

Use current month's themes. Reference Iowa-specific context (harvest, lake season, winter driving conditions).

### Content Rules

- **NEVER mention days on lot** — instant failure (weight 0.98)
- **NEVER use stock photo language** — every image suggestion must be something Motor Inn can actually photograph
- **Sound local.** "Your neighbors at Motor Inn" not "Our professional team of experts"
- **Brand voice:** Trust > flash. 90 years of being neighbors. Practical, not polished.
- **No emoji** unless Spencer explicitly requests it
- Match the brand voice in `~/clawd/dealership/marketing/brand-voice.json`

## Review Response Drafts

### When to Generate

Only generate new review response drafts if:
- The velocity audit flagged response rate below 80%
- There are unresponded reviews visible on the GBP listing
- Spencer requests updated templates

### Response Format

Output to `~/motor-inn-seo/content/drafts/review-templates/responses-$(date +%Y-%m-%d).md`:

```markdown
# Review Response Drafts — [date]

## 5-Star Responses (3 variations)

### Variation A
[40-80 words. Mentions service keyword + location.]

### Variation B
[40-80 words. Different opening, different service mention.]

### Variation C
[40-80 words. Different angle — references community/family/trust.]

## 4-Star Responses (3 variations)
[Same pattern — acknowledge the gap, invite return]

## 3-Star Responses (3 variations)
[Take accountability, offer resolution, provide contact]

## 1-2 Star Responses (3 variations)
[Empathetic, professional, offline resolution: "call us at (712) 522-2526"]
```

### Response Rules

- No two variations start with the same word
- Never use: "We appreciate your feedback," "Thank you for your kind words," "We're sorry to hear"
- 5-star responses must include one of: vehicle make, service type, or area name
- All negative responses include the phone number for offline resolution
- Sound like the owner wrote it at the end of a long day — human, not corporate

## Output

### Files Created

1. `~/motor-inn-seo/content/drafts/gbp-posts/posts-week-of-[date].md` — next 2 weeks of posts
2. `~/motor-inn-seo/content/drafts/review-templates/responses-[date].md` — if new templates needed

### Delivery

1. Save all files
2. Git commit: `cd ~/motor-inn-seo && git add -A && git commit -m 'GBP content: posts + responses [date]' && git push`
3. Post to Slack #automated-marketing (C0AGB08UY9W): "[X] GBP posts drafted for next 2 weeks + [Y] review templates. In drafts folder for Spencer's review."

## Escalate When

- Velocity audit not available (dependency missing)
- Brand voice file missing or contradicts these instructions
- All post slots for next 2 weeks already have content (nothing to generate)
