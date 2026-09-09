---
name: Token postmortem
description: Use when a factory task or Linear Zer0 issue becomes Done — retrospective tokenomics before any skill mutation.
---

# Token postmortem

After a successful Done leaf, ask whether the same outcome could have used fewer tokens. Observe first. Propose prune/preserve/repair only. Never auto-apply skill changes.

Stack (when adapters exist):
- Observability: agenttrace (https://github.com/luoyuctl/agenttrace)
- Engine: ClawTrace / CostCraft (https://github.com/epsilla-cloud/clawtrace)
Hold CostCraft runs until Hermes + OMP TraceCard adapters land.

## When to run

Invoke once when either:
1. Linear status becomes Done (Zer0 source of truth), or
2. `tasks.status` in factory.db flips to `done` (scout or ship).

Prefer the Linear issue id as `task_id` when a Linear issue is present. Otherwise reuse the factory.db task id. Do not open a new factory row. Do not open a PR from this skill.

## Inputs

- `task_id`: Linear issue id when present (Zer0 SoT); else factory.db task id
- harness: `hermes` | `omp` | `cursor` | `claude_code` | `openclaw` | `other`
- `session_ref`: path or session_id for the Done run
- optional: factory.db row id if both Linear and factory rows exist (cross-link in the report)

If no transcript is available, write the report with `could-prune: unknown (no session)` and stop.

## Procedure

1. Run agenttrace (or equivalent) on `session_ref`. Capture tokens and $ if present.
2. If CostCraft TraceCard adapters exist for this harness: build TraceCard; list top cost spans and redundancy flags. If not: stop after agenttrace numbers — do not invent TraceCards.
3. Draft prune/preserve/repair candidates with one-line counterfactuals. Mark anything that would mutate skills as needing captain.
4. Write the report file (below). Optionally append a pointer in `tasks.result` if empty, or mention the report path in the scout/ship report / Linear comment. Do not flip status away from done.
5. If proposing a skill patch: create or request `kind=decision` with `gate_kind=captain` (or equivalent captain HITL). Do not call skill_manage. Do not merge. Do not Done Linear HITLs.

## Report path

`/home/box/agent-data/grok-ship/reports/<task_id>-token-postmortem.md`

Create the parent directory if needed. Same path pattern as other factory reports. Sanitize Linear ids for filesystem safety if needed (keep the id recognizable).

## Done checklist (fill in the report)

| Field | Value |
|-------|-------|
| task_id | Linear id preferred; else factory id |
| harness | hermes \| omp \| cursor \| claude_code \| openclaw \| other |
| session_ref | path or session_id |
| tokens | in / out / cache_read / cache_write / total |
| $ | estimated_usd (source: agenttrace \| tracecard \| other) |
| top_cost_spans | 1) … 2) … 3) … |
| could-prune | yes/no + counterfactual one-liner (or none credible) |
| preserve / repair notes | |
| patch link | TraceCard YAML + CostCraft patches path (or n/a) |
| captain_gate | none \| decision:<id> (required if proposing skill mutation) |

## Do not

- Do not replace captain HITL or Linear HITL
- Do not call skill_manage from this skill
- Do not Done Linear HITLs
- Do not open or merge PRs
- Do not invent TraceCards when adapters are missing
- Do not flip task status away from done
- Do not write a second factory.db for postmortems
- Do not run CostCraft on Hermes/OMP until adapters land (clawtrace #2 + adapter issue)
