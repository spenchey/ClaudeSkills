# Past Project Patterns — Anti-Patterns & Fixes

Based on projects actually built: Mission Control, Call Coaching, VDP Analytics, Messenger Bot, Google Review System, Vehicle Intelligence, Social Media Automation, Jeeves Voice Relay.

---

## Anti-Patterns We've Hit

### 1. No Plan File → Lost Context on Restart
**Problem:** Project described verbally in session → session ends → next session has no context → repeats work or goes in wrong direction.
**Fix:** Write `~/clawd/<project>-plan.md` before touching code. Reference it from all cron instructions.

### 2. Wrong Credentials / Missing Auth
**Problem:** Codex builds the whole thing then it doesn't work because a cred isn't where it expects.
**Fix:** List all required creds in the plan file. Check `~/.config/openclaw/credentials/` FIRST. Known creds:
- Google (work): `jeeves-motorinn` client → `--account spencer.heywood@motorinnmail.com --client jeeves-motorinn`
- Glo3D photos: `~/.config/openclaw/credentials/glo3d.json`
- CARFAX: `~/.config/openclaw/credentials/carfax.json`
- Facebook/Meta: `~/.config/openclaw/credentials/facebook-ads.json`
- Twilio: `~/.config/openclaw/credentials/twilio.json`
- X/Twitter: `~/.config/openclaw/credentials/x-predictwhales.json`

### 3. Email Sent as Raw HTML
**Problem:** `gog gmail send --body` sends Content-Type: text/plain — HTML renders as code.
**Fix:** ALWAYS use `--body-html` for HTML emails. No exceptions.

### 4. Monday Cron Cluster Overload
**Problem:** 5+ crons triggering simultaneously on Mondays → Anthropic 529 overload errors.
**Fix:** Stagger Monday crons by 5+ minutes each.

### 5. Carbase CDN Photos (Unreliable)
**Problem:** Carbase CDN URLs 404 randomly — photos break in posts and emails.
**Fix:** ALWAYS use Glo3D API for photos. Never Carbase CDN.

### 6. SQLite Direct Queries → Schema Drift
**Problem:** Scripts query vdp_analytics.db directly → schema changes break everything.
**Fix:** Use `~/clawd/tools/knowledge-store/api.js` for all inventory/VDP data.

### 7. Claimed Done Without Verifying
**Problem:** Script exits 0 but data didn't land. Post didn't go out. DB row wasn't written.
**Fix:** Verify section in every plan. Check the actual output, not just the exit code.

### 8. DST Hardcoding
**Problem:** Hardcoded DST cutoff dates break twice a year.
**Fix:** Use `Intl.DateTimeFormat` shortOffset on target date. Never hardcode DST.

### 9. Caption Mentions Days on Lot
**Problem:** "Been here 21 days" signals desperation to customers.
**Fix:** Never mention days on lot, age, or urgency in customer-facing content. Sell features.

### 10. Railway Env Missing
**Problem:** App works locally, breaks on Railway because env vars not set.
**Fix:** List all env vars in plan file. Set them in Railway before first deploy.

---

## Patterns That Work

### Railway Deployments
- Always: connect GitHub repo → Railway auto-deploys on push
- Add Railway project ID to MEMORY.md immediately
- Custom domain: CNAME → Railway `*.up.railway.app` subdomain (DNS takes ~24h)

### Cron Setup
- After building: run once manually before scheduling
- Record cron ID in MEMORY.md right when created
- Add trajectory logging before going to production

### Codex Delegation
- Pass `--full-auto` + plan file path
- Use `pty:true` always
- Run in project workdir, not ~/clawd/
- Background with tmux for anything >5 min

### Data Sources Available
| Source | How to Access | Notes |
|--------|--------------|-------|
| Inventory/VDP | `~/clawd/tools/knowledge-store/api.js` | Canonical |
| Vehicle photos | Glo3D API (creds above) | Use `?stockNumber=` or `?vin=` |
| Window stickers | `~/clawd/dealership/vehicle-intelligence/lookup.js` | Toyota + GM/Chevy |
| Call stats | `~/clawd/tools/knowledge-store/api.js` → `getCallStats()` | |
| Google Sheets (Fixed Ops, Sales) | `gog sheets` CLI | See MEMORY.md for sheet IDs |
| Mailchimp | `~/clawd/dealership/email-marketing/` | 7k subs, audience IDs in MEMORY.md |
| Facebook/Meta | Graph API via `~/.config/openclaw/credentials/facebook-ads.json` | |
