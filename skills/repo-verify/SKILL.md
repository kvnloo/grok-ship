---
name: Repo verify
description: Use to verify a candidate change against a specific upstream repo's contribution rules. Parameterized by repo - do not create a new bot per repository.
---

# Repo verify

One skill, many repos. Repo-specific rules live in data, not in agent clones.

## Inputs

- repo: `OWNER/NAME` (origin)
- baseline: sha or ref to prove fail or absent
- candidate: sha or ref to prove pass
- Optional policies path: `/home/box/agent-data/grok-ship/pack/repo_policies.json`

## Steps

1. Load `repo_policies.json`. If the repo is missing, read live CONTRIBUTING.md and the PR template; then propose a policy stub to Firstmate (do not CreateAgent).
2. If `automated_code` is false, stop implementation and publish paths and report that automated code is forbidden.
3. Run the policy's verify commands, or derive checks and run fail-then-pass on baseline then candidate.
4. For PR-bound work, ensure every `pr_required_headings` entry is present in the draft body.
5. Apply `docs_rule` and `notes` when relevant.

## Output

Use the fail-then-pass RESULT block, plus:

```text
REPO: OWNER/NAME
POLICY: pinned | live-fallback
AUTOMATED_CODE: true | false
TEMPLATE: ok | missing:<headings>
```

## Do not

- Stand up one bot per repo that only wraps this skill
- Ignore pinned `automated_code: false`
- Open an origin PR with a wiped or generic template
