---
name: Prefer reuse
description: Use before CreateAgent or when the captain asks for a bot for a new repo. Prefer extending an existing crewmate over spawning a duplicate job.
---

# Prefer reuse

New repository is not a new bot. A new job might be.

## When to run

- Before Eggbot or Firstmate calls CreateAgent
- When someone asks for a bot "for" a specific repository

## Steps

1. List live agents with name and one-line job.
2. Classify the request:
   - same job, new repo - reuse (Factory, Scout, Watchdog, or a skill)
   - same job, tighter voice - UpdateAgent learning notes, do not clone
   - genuinely new job - CreateAgent
3. For verifiers: always reuse repo-verify and add or adjust repo_policies.json. Never CreateAgent per-repo verifier siblings.
4. For project software work: reuse Factory or the existing projects-table crewmate; do not overwrite crewmate_id.

## Decision output

```text
DECISION: REUSE | EXTEND_POLICY | CREATE
MATCH: <agent name or n/a>
JOB: <one line>
REPO_SCOPE: <OWNER/NAME or n/a>
ACTION: <UpdateAgent note | policy stub | CreateAgent name>
WHY: <one line>
```

## Default bias

REUSE, then EXTEND_POLICY, then CREATE.
