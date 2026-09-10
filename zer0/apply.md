# Apply pack → live agents (Phase 2+)

**Phase 1 status:** export only. Live agents remain source of truth until Kevin authorizes apply.

## Rules
- No secrets in pack or chat
- Do **not** run upstream `GROK_SHIP.md` installer against the live fleet
- Do **not** replace Linear with sqlite
- Apply one role at a time via `UpdateAgent` description from the matching `zer0/GROK_BOT_*.md`
- Pilot order: Linear maintainer → frontier → factory → CM → watchdog → CoS last
- Rollback = re-apply previous charter from git tag

## How
1. Read `zer0/roster.yaml`
2. For the target agent id, open its `charter` markdown
3. Copy the **Charter (live description)** section into `UpdateAgent` description
4. Confirm in chat; no bulk apply
