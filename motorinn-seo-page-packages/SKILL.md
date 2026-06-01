---
name: motorinn-seo-page-packages
description: Build Motor Inn SEO/GEO page recommendations as ready-to-copy DealerOn/CMS HTML page packages. Use when monthly SEO reports, content gap reports, GEO audits, or local SEO workflows identify pages that should be created for Motor Inn Auto Group.
---

# Motor Inn SEO Page Packages

Use this when SEO/GEO analysis produces pages that need to become publishable website content, not just markdown recommendations.

## Ownership

- Rory owns SEO/GEO strategy, keyword selection, page facts, draft copy, CMS-ready HTML packages, Slack/email delivery, and the recurring job.
- Emily owns visual design only when a page needs a custom layout, visual module, Figma design, or page-builder guidance beyond normal CMS copy/paste content.
- Archie may break Emily design work into implementation tasks, but Rory remains accountable for the monthly SEO/GEO output and final delivery to Spencer.

## Required Output

For every recommended page that should be built, produce both:

1. A markdown draft under `~/motor-inn-seo/content/drafts/`.
2. A ready-to-copy HTML package under `~/motor-inn-seo/content/page-packages/YYYY-MM/`.

Each HTML package must include:

- Suggested URL slug and canonical URL.
- SEO title and meta description.
- Body HTML with one visible H1, clear H2 sections, local internal links, and CTA links.
- FAQ section and FAQPage JSON-LD.
- GEO answer summary metadata and WebPage + FAQPage JSON-LD.
- Publishing notes for the SEO company.
- Clear review notes for any assumptions or data that needs human confirmation.

Do not publish to DealerOn automatically.

## DealerOn Compatibility Gate

Treat generated HTML as a CMS copy/paste package unless a DealerOn preview has been tested.

Before claiming a page "matches DealerOn visually," verify one of these:

- It was previewed inside DealerOn/page-builder or an equivalent staging page.
- It uses an approved DealerOn module/template from the current site.
- Emily delivered a Figma/page-builder design package and Rory converted it into CMS instructions.

If none of those happened, say: "CMS-ready content package; visual match not verified."

## Emily/Figma Gate

Route work to Emily when the recommendation needs:

- A new page layout or landing page design.
- Custom imagery, comparison tables, hero sections, page-builder modules, or visual hierarchy decisions.
- A Figma artifact for the SEO company or implementation team.

Create the design request under the Linear project `SEO/GEO Page Design` when Linear is being used. The request should include target keyword, search intent, page purpose, required sections, CTA goals, internal links, and the HTML package path. Emily should return a design package to Rory; Rory then delivers the final SEO/CMS package.

Do not make Emily a clone of Archie. Emily's scope is design, visual QA, and Figma/page presentation.

## Content Rules

- Keep claims local, useful, and reviewable.
- Do not mention days on lot.
- Do not invent pricing, incentives, inventory counts, reviews, or customer quotes.
- Prefer answer-first copy for GEO visibility.
- Include FAQ answers that can be cited by AI search engines.
- Use structured data only when the visible page content supports it.

## GEO Answer Pages

When the monthly SEO process identifies AI-search opportunities, create specific GEO answer pages in addition to normal city/service pages. These pages target conversational questions that AI tools are likely to answer directly, such as:

- `best place to buy a used car in Carroll Iowa`
- `should I buy new or used car Carroll Iowa`
- `where to service Toyota Carroll Iowa`

GEO answer pages must include:

- H1 phrased as the exact buyer/service question.
- A short direct answer in the first visible section.
- Reviewable proof points using only supported Motor Inn facts.
- Visible source notes/internal links that an SEO company can keep on-page.
- FAQ content with FAQPage schema.
- WebPage schema joined with FAQPage schema via `@graph`.
- A publishing note that the package is CMS-ready and visual match is not verified unless Emily or DealerOn preview verified it.

## Validation

Run these checks after workflow changes:

```bash
node --check ~/clawd/scripts/no-model/monthly-seo-content-build.js
bash -n ~/clawd/scripts/no-model/monthly-seo-content-pipeline.sh
bash -n ~/motorinn-dispatch/scripts/no-model/monthly-seo-content-pipeline.sh
```

For each monthly run, confirm:

- HTML package count is at least the markdown draft count.
- GEO answer HTML package count is at least `SEO_CONTENT_MIN_GEO_PACKAGES` (default 2).
- The delivery summary lists the HTML package paths.
- Email/Slack delivery attaches or links the HTML packages.
