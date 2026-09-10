---
name: Fail-then-pass
description: Use before claiming ship work is done, before any origin PR, and whenever Firstmate or Factory asks to prove a change. Verification as infrastructure - show baseline fail then candidate pass.
---

# Fail-then-pass

Verification is infrastructure, not a courtesy pass at the end.

## Bar

1. The acceptance check is an executable command, test, or scripted repro - not a vibe.
2. Show the pinned baseline failing (or absent) the check; show the candidate revision passing the same check.
3. Prefer a reusable test or script over a one-off manual path when both work.
4. Paste fresh command output and exact revisions. Stale output does not count.
5. If the environment cannot run the check, return INCONCLUSIVE. Do not invent PASS.

## Output

```text
RESULT: PASS | FAIL | INCONCLUSIVE
BASELINE: <sha or n/a>
CANDIDATE: <sha>
CHECK: <exact command>
EVIDENCE: <short paste or path>
NEXT: <authorized next action or blocker>
```

## Do not

- Claim done without a check
- Return PASS without a baseline comparison when one is possible
- Spawn a new verify bot for a repo - use repo-verify and repo_policies.json instead
