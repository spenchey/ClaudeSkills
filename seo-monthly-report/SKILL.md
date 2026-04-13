---
name: seo-monthly-report
description: Monthly SEO performance report rolling up GA4 traffic, GBP insights, GSC rankings, and audit findings into one executive summary. Tracks what moved, what didn't, and the single most important action for next month. Use when the cron runs the monthly report or when the user mentions "monthly SEO report," "SEO performance," "monthly rollup," or "SEO dashboard."
tags:
  - marketing
  - seo
  - local-seo
  - reporting
  - analytics
metadata:
  version: 1.0.0
  cadence: monthly
  covers: prompt 20
---
# SEO Monthly Report

Monthly rollup of all SEO performance data for Motor Inn Auto Group. This is the read-in-5-minutes executive report that answers: is SEO making us money, and what do we do next?

## Pre-Flight

1. Read `~/.agents/product-marketing-context.md` for business context
2. Load prior month's report: `~/motor-inn-seo/audits/monthly-report/monthly-report-latest.md`
3. Load latest GBP structural audit: `~/motor-inn-seo/audits/gbp-structural/gbp-structural-latest.md`
4. Load latest GBP velocity audit: `~/motor-inn-seo/audits/gbp-velocity/gbp-velocity-latest.md`
5. Load latest website SEO audit: `~/motor-inn-seo/audits/website-seo/website-seo-latest.md`
6. Load latest local authority audit: `~/motor-inn-seo/audits/local-authority/local-authority-latest.md`

This report aggregates — it does NOT re-run audits. If an audit is stale (over 45 days old), note it in the report.

## Data Pulls

### GA4 (Required)

```bash
# This month vs last month
ga4 -p 364125348 -s <first-of-last-month> -e <last-of-last-month> -f json traffic
ga4 -p 364125348 -s <first-of-two-months-ago> -e <last-of-two-months-ago> -f json traffic

# Organic sources
ga4 -p 364125348 -s <first-of-last-month> -e <last-of-last-month> -f json sources

# Top pages
ga4 -p 364125348 -s <first-of-last-month> -e <last-of-last-month> -f json pages --limit 50

# Devices
ga4 -p 364125348 -s <first-of-last-month> -e <last-of-last-month> -f json devices

# Key events
ga4 -p 364125348 -s <first-of-last-month> -e <last-of-last-month> report -d eventName -m eventCount
ga4 -p 364125348 -s <first-of-two-months-ago> -e <last-of-two-months-ago> report -d eventName -m eventCount
```

**Date calculation:** Use `date` commands to compute first/last of last month dynamically. Example:
```bash
LAST_MONTH_START=$(date -v-1m -v1d +%Y-%m-%d)
LAST_MONTH_END=$(date -v1d -v-1d +%Y-%m-%d)
PRIOR_MONTH_START=$(date -v-2m -v1d +%Y-%m-%d)
PRIOR_MONTH_END=$(date -v-1m -v1d -v-1d +%Y-%m-%d)
```

### GSC (If Connected)

If Google Search Console is connected, also pull:
- Top queries by clicks
- Top queries by impressions
- Average position for priority keywords
- Queries that improved vs declined

If GSC is NOT connected, note "GSC: Not yet connected" and skip.

### GBP Insights (From Audit Data)

Pull from the latest GBP audits, not from GBP directly:
- Review count and velocity (from gbp-velocity audit)
- Posting frequency (from gbp-velocity audit)
- Category/attribute status (from gbp-structural audit)

## Report Structure

### The Report

```markdown
# Motor Inn Auto Group — Monthly SEO Report
## [Month Year]

---

### Read This First (30-Second Summary)

**Calls from organic:** [X] ([+/-X% vs last month])
**Organic traffic:** [X sessions] ([+/-X% vs last month])
**Review velocity:** [X reviews/month] ([up/down/flat])
**Bottom line:** [One sentence — are we winning or losing and why]

---

### 3 Wins This Month

1. **[Win]** — [What happened, why it matters, what metric moved]
2. **[Win]** — [What happened, why it matters, what metric moved]
3. **[Win]** — [What happened, why it matters, what metric moved]

### 3 Problems That Need Fixing

1. **[Problem]** — [What happened, impact, suggested fix]
2. **[Problem]** — [What happened, impact, suggested fix]
3. **[Problem]** — [What happened, impact, suggested fix]

### The One Thing for Next Month

**[Single most important action]** — [Why this, what it will impact, who does it]

---

### Traffic Scorecard

| Metric | [Month] | Prior Month | MoM Change |
|--------|---------|-------------|------------|
| Total sessions | X | X | +/-X% |
| Organic sessions | X | X | +/-X% |
| Organic % of total | X% | X% | +/-X pp |
| New users (organic) | X | X | +/-X% |

### Key Events (Conversions)

| Event | [Month] | Prior Month | MoM Change |
|-------|---------|-------------|------------|
| used_vdp | X | X | +/-X% |
| new_vdp | X | X | +/-X% |
| form_submit | X | X | +/-X% |
| asc_click_to_call | X | X | +/-X% |
| asc_form_submission | X | X | +/-X% |

### Traffic Sources

| Source | Sessions | % of Total | MoM Change |
|--------|----------|-----------|------------|
| Organic | X | X% | +/-X% |
| Direct | X | X% | +/-X% |
| Facebook CPC | X | X% | +/-X% |
| Google CPC | X | X% | +/-X% |
| Referral | X | X% | +/-X% |

### Top Organic Pages

| Page | Sessions | MoM Change |
|------|----------|------------|
| [url] | X | +/-X% |
| ... | ... | ... |

### GBP Status

| Metric | Current | Last Month | Trend |
|--------|---------|------------|-------|
| Total reviews | X | X | +X |
| Review velocity/mo | X | X | +/-X |
| GBP posts this month | X | X | +/-X |
| Categories optimized | X/Y | X/Y | — |
| Attributes optimized | X/Y | X/Y | — |

### GSC Rankings (if available)

| Keyword | Current Position | Last Month | Change |
|---------|-----------------|------------|--------|
| [priority kw] | X | X | +/-X |
| ... | ... | ... | ... |

### Citation Health

| Status | Count |
|--------|-------|
| Correct | X |
| Inconsistent | X |
| Missing | X |

### Content Pipeline

| Status | Pages |
|--------|-------|
| Published this month | X |
| In review | X |
| Drafted | X |
| Planned | X |

---

### Data Notes
- GA4 property: 364125348
- GSC: [connected/not connected]
- Audit freshness: structural [date], velocity [date], website [date], authority [date]
- [Any data quality issues or missing sources]
```

## Metric Priorities

**These are the only metrics that actually matter.** Everything else is context.

1. **Calls from organic** (asc_click_to_call from organic sources) — money
2. **Form submissions from organic** (asc_form_submission from organic) — money
3. **VDP views from organic** (used_vdp + new_vdp from organic) — buying intent
4. **Organic sessions** — volume
5. **Review velocity** — trust signal
6. **Keyword positions** — leading indicator

If organic calls went up, SEO is working. If they went down, everything else is noise until we figure out why.

## Output

### Report File

`~/motor-inn-seo/audits/monthly-report/monthly-report-$(date +%Y-%m).md`

Symlink to `monthly-report-latest.md`.

### Email Report

Format the report as HTML using the email template pattern:
```bash
node ~/clawd/dealership/marketing/ga4-email-template.js
```

Send via:
```bash
gog gmail send --to tony.franklin@example.com,david.bremser@example.com --bcc spencer.heywood@motorinnmail.com --subject "Motor Inn SEO Report — [Month Year]" --body-html "<html>...</html>"
```

**IMPORTANT:** Use `--body-html` flag, NOT `--body`. Use BCC for Spencer. Confirm recipient addresses from the existing Monthly GA4 Deep Dive cron job.

## Delivery

1. Save report to `~/motor-inn-seo/audits/monthly-report/`
2. Send HTML email to stakeholders
3. Git commit: `cd ~/motor-inn-seo && git add -A && git commit -m 'Monthly SEO report [month]' && git push`
4. Post the "Read This First" summary to Slack #automated-marketing (C0AGB08UY9W)

## Escalate When

- GA4 returns no data or errors
- Organic traffic dropped more than 25% MoM (major issue)
- Key events (calls, forms) dropped more than 30% MoM
- All audit data is stale (nothing updated in 45+ days)
- Email send fails

## Rules

- This report must be readable in 5 minutes by a non-technical person
- No jargon without explanation (don't say "CTR" — say "click rate")
- No vanity metrics (don't celebrate "impressions up 40%" if clicks didn't move)
- Always BCC spencer.heywood@motorinnmail.com on all emails
- If a metric is unavailable, show "—" not a guess
- Round percentages to whole numbers (37%, not 37.2%)
