---
name: project-planner
description: Run structured project planning for new builds. Use when Spencer says "let's build X", "plan out Y", "kick off Z", or asks to plan or scope a new project. Guides through a tight discovery → spec → ship plan, based on how Motor Inn / Jeeves projects actually get built.
tags:
  - planning
  - project-management
  - decomposition
---
# Project Planner

Run this when starting any new project. Goal: get from "let's build X" to a written plan and first cron/code in one session.

## Phase 1 — Discovery (2 minutes max)

Ask these in ONE message (don't spread across messages):

1. **What problem does this solve?** (1 sentence — the business outcome)
2. **Who uses it?** (Spencer only / team / customers / automated)
3. **What does "done" look like?** (specific deliverable: Slack alert, Railway app, cron output, etc.)
4. **Any data sources or integrations needed?** (CallRail, Glo3D, Google Sheets, Mailchimp, etc.)
5. **Is there a deadline or urgency?**

Skip any questions Spencer already answered in his prompt. If he gave enough context, skip Phase 1 entirely and go straight to Phase 2.

## Phase 2 — Spec (write the plan file immediately)

Write `~/clawd/<project-slug>-plan.md` with:

```markdown
# [Project Name] Plan

## Goal
[One sentence: what this does and why it matters]

## Success Criteria
- [ ] [Specific, verifiable outcome]
- [ ] [Specific, verifiable outcome]

## Stack
- Language/framework: [Node.js / Next.js / Python / etc.]
- Hosting: [Railway / local / cron only]
- Data sources: [list APIs, DBs, files]
- Auth/creds needed: [list files in ~/.config/openclaw/credentials/]

## Architecture
[3–5 bullet points: how it works end to end]

## Files / Structure
[Key files and what they do]

## Cron Jobs
[If automated: schedule, payload, channel]

## Rollout
1. Build [X]
2. Test manually — verify [specific output]
3. Deploy to Railway / enable cron
4. Monitor for [Y] days

## Known Risks
- [List any gotchas discovered upfront]
```

## Phase 3 — Delegate or Build

Choose the right tool:

| Task | Tool |
|------|------|
| New greenfield build | Codex: `codex exec --full-auto "Read ~/clawd/<project>-plan.md and build it"` |
| Existing large repo (ScraperLab, VDP, call-tracking) | Cursor in that repo dir |
| <50 lines, single file | Edit directly |
| Strategy / analysis first | sessions_spawn (MiniMax/strategist) |

**Always pass the plan file path to Codex/Cursor.** Never describe the project verbally only.

## Phase 4 — Anchor the Project

After first working version exists:

1. **Update MEMORY.md** — Add to Active Projects table with status, Railway URL, cron IDs
2. **Add cron reference** — If any crons, add IDs to MEMORY.md so future sessions can find them
3. **Verify output** — Don't claim done based on exit 0. Check the actual file/DB row/Slack message
4. **Commit** — `cd ~/clawd && git add -A && git commit -m "feat: [project name] initial build"`

## Phase 5 — Production Checklist

Before calling it live:

- [ ] Trajectory logging added (`~/clawd/tools/trajectory-log/logger.js`)
- [ ] Output verified (not just exit 0)
- [ ] Slack delivery tested (correct channel, correct format)
- [ ] Consecutive failure alerts configured (observability)
- [ ] Cron IDs recorded in MEMORY.md
- [ ] Plan file exists in ~/clawd/
- [ ] Commit pushed to GitHub

## Reference: Lessons From Past Projects

See `references/past-project-patterns.md` for anti-patterns we've hit and what to do instead.

## Improvements Over Old Approach

Old: Spencer describes project → I write code → oops, wrong stack/credentials/structure
New: Spec file first → Codex reads the spec → much less rework

Old: "Done!" based on exit 0
New: Verify step is mandatory before claiming done

Old: Project context lives only in session memory
New: Plan file in ~/clawd/ + MEMORY.md entry = survives session restarts
