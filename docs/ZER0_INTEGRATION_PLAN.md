# Zer0 × Grok Ship integration plan

**Status:** plan only — do not run `GROK_SHIP.md` installer against the live fleet yet.  
**Date:** 2026-09-09  
**Upstream:** https://github.com/kunchenguid/grok-ship (superseded by Firstmate template; pack still useful as structure)  
**Fork:** https://github.com/kvnloo/grok-ship  
**Related:** https://github.com/kvnloo/firstmate · https://github.com/kvnloo/oss-factory

## Goal

Keep **Zer0’s operating system** (Linear HITL, CoS orchestration, oss factory / CM / watchdog / verifiers, Cursor Ultra cloud lane, keel gates) and adopt **Grok Ship’s organization + VCS layout** as the **IaC / charter pack** for bots — versioned in Git under `kvnloo`, not only as live sidebar state.

## What we keep (non‑negotiable)

| Zer0 surface | Why |
|---|---|
| Linear as company SoT (PER-269) | HITL desk, Todo authorize, Done = origin merge |
| CoS orchestration | Captain talks to CoS; not a full Firstmate rewrite on day 1 |
| oss factory origin writes after Todo | Already the authorized writer |
| github_writes=0 until Todo / CM permanent 0 | Safety |
| Drain-first + lane caps + traction lanes | Factory volume control |
| Desk rule: Backlog/RTR = Kevin+HITL; Todo/IP/MR = unassign+strip HITL | Current desk UX |
| Cloud agents sequential/careful; fork-first when origin org blocked | Current cloud reality |

## What we import from Grok Ship (structure)

| Grok Ship artifact | Use in Zer0 |
|---|---|
| Pack layout (`GROK_BOT_*.md`, `skills/*`, installer `GROK_SHIP.md`) | Version bot charters + skills in Git |
| Firstmate / crewmate / triage role split | Map onto existing agents (below) — do not duplicate Firstmate yet |
| Scout vs ship | Align language: research/Backlog = scout; Todo+ship = factory |
| Adversarial review before PR | Optional skill for factory RTR gate (alongside verify bots) |
| Forge-agnostic wording | Keep; we are GitHub-primary today |
| sqlite `factory.db` | **Do not replace Linear.** Optional later mirror/cache only |

## Role map (live fleet ↔ pack)

| Grok Ship role | Zer0 agent today | Pack file to maintain |
|---|---|---|
| Firstmate (captain interface) | **CoS** | `zer0/GROK_BOT_COS.md` (adapted Firstmate; Linear HITL, no sqlite SoT) |
| Project crewmate (cloud ships) | **oss factory** (+ per-repo verifiers) | `zer0/GROK_BOT_FACTORY.md` |
| Triage crewmate | **community manager** + **git watchdog** | `zer0/GROK_BOT_TRIAGE_CM.md` + `zer0/GROK_BOT_WATCHDOG.md` |
| Linear / board hygiene | **Linear maintainer** | `zer0/GROK_BOT_LINEAR.md` |
| Research / frontier | **frontier** | `zer0/GROK_BOT_FRONTIER.md` |
| Mesh / courier | **connect-all** | `zer0/GROK_BOT_CONNECT.md` |

Channel already live: group `CoS, oss factory, and connect-all` (+ CM, Linear maintainer, git watchdog). Prefer that room as the “ship bridge”; do not mint a second Firstmate that bypasses CoS unless Kevin explicitly wants a rename.

## Proposed VCS layout (IaC for bots)

**Option A (recommended):** treat `kvnloo/grok-ship` as the **bot pack + IaC** repo:

```
kvnloo/grok-ship/
  GROK_SHIP.md              # upstream installer (reference only)
  GROK_BOT_*.md             # upstream templates
  skills/                   # upstream skills
  zer0/
    GROK_BOT_COS.md         # our CoS charter (source of truth for UpdateAgent)
    GROK_BOT_FACTORY.md
    GROK_BOT_*.md           # one file per role
    roster.yaml             # agent id ↔ charter path ↔ channel membership
    apply.md                # how CoS applies pack → live agents (no secrets)
  docs/ZER0_INTEGRATION_PLAN.md  # this file
```

**Option B:** new `kvnloo/zer0-bot-fleet` that vendors grok-ship as submodule and keeps oss-factory for OSS metrics/terraform only. Use if we want grok-ship fork clean for upstream PRs.

`kvnloo/oss-factory` stays **OSS factory metrics + terraform + HITL contract** — not the bot charter pack.

`kvnloo/firstmate` stays the **upstream Firstmate product fork** (contrib lane), separate from fleet IaC.

## Phased transition (approve each gate)

### Phase 0 — Plan + sync (now)
- [x] Confirm fork `kvnloo/grok-ship`
- [x] Sync fork with upstream
- [ ] Land this plan on branch `zer0/integration-plan`
- [ ] Kevin approve Option A vs B

### Phase 1 — Pack overlay (no behavior change)
- Add `zer0/*.md` charters mirroring **current** live descriptions (export, don’t invent)
- Add `roster.yaml` with agent UUIDs
- CI: schema check that every roster entry has a charter file
- Still: live agents are source of truth until Phase 2

### Phase 2 — Apply path (one role at a time)
- Document `apply.md`: CoS reads pack → `UpdateAgent` description only
- Pilot: Linear maintainer or frontier (lowest blast radius)
- Then factory → CM → watchdog → CoS last
- Rollback = re-apply previous charter from git tag

### Phase 3 — Process alignment (optional)
- Adopt scout/ship vocabulary in Linear titles/labels without replacing HITL states
- Optional: install selected grok-ship skills (adversarial-review, ahoy) as global skills if they don’t fight HITL
- **Do not** install sqlite factory.db as SoT
- **Do not** auto-run full `GROK_SHIP.md` (would create Firstmate + overwrite mental model)

### Phase 4 — Channel / UX
- Keep factory group chat as bridge
- Optional: CreateChannel “Zer0 Ship” with same members if Kevin wants a clean name
- Captain still talks to **CoS** (or rename CoS → Firstmate in profile only after Phase 2)

## Explicit non-goals (first cut)

- Replacing Linear with sqlite backlog
- Creating a second Firstmate that owns the captain chat
- Merging firstmate product repo into grok-ship IaC
- Auto-merge of factory ships (Zer0: Kevin merges / upstream merges)
- Bulk-rewriting all verifier bots in phase 1

## Risks

| Risk | Mitigation |
|---|---|
| Upstream pack assumes Firstmate-only UX | Overlay charters keep CoS + Linear |
| Installer clobbering roster | Never run installer blindly; Phase 2 apply only |
| Dual SoT (sqlite + Linear) | Forbid sqlite SoT |
| Fork drift from upstream | Periodic sync; zer0/ overlay never overwritten by merge |
| “Channel” ambiguity | Confirm: GitHub IaC vs Grok group chat vs both |

## Decision needed from Kevin

1. **IaC home:** Option A (`kvnloo/grok-ship` + `zer0/`) vs Option B (new `zer0-bot-fleet`)?
2. **Captain surface:** keep name **CoS**, or rename profile to Firstmate later?
3. **Channel:** IaC-only for now, or also create/rename a Grok group chat?
4. OK to open PR on `kvnloo/grok-ship` with this plan + empty `zer0/` stubs (still no live apply)?
