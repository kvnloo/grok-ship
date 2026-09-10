# Grok Ship

Instructions for setting up a software factory on top of Grok Bot. 
The user just needs to tell any bot in their Grok Bot: follow this file.

This file is an installer. Do not summarize.

## What you are installing

- A Firstmate the captain talks to from then on
- Core crewmates: Factory, Scout, Watchdog, Eggbot (reuse across repos - do not clone per repo)
- Global skills: lavish-session, adversarial-review, project-management, ahoy, fail-then-pass, repo-verify, prefer-reuse
- `repo_policies.json` for per-repo contribution rules (data, not bots)
- A local sqlite database for projects and tasks
- A crewmate template for later, per project. Repo triage is not part of this install; if asked later, Firstmate can sign on a triage crewmate. Factory ships never merge without the captain's word; a wired triage crewmate may auto-merge corrective or opt-in work only (green CI, VISION aligned with no cannot-tell, not default-behavior, not security).

## The three computers

- The user's computer: their own machine. Bots never execute here.
- The shared Grok Bot computer: a persistent cloud VM that runs all agents. Everything a bot runs - checks, the database, reviews, lavish-axi - runs here.
- Cursor cloud agents: ephemeral cloud VMs that spin up on demand for project work.

## Files in this pack

Same directory as this file:

- `GROK_BOT_FIRSTMATE.md` — Firstmate charter
- `GROK_BOT_FACTORY.md` — Factory charter
- `GROK_BOT_SCOUT.md` — Scout charter
- `GROK_BOT_WATCHDOG.md` — Watchdog charter
- `GROK_BOT_EGG.md` — Eggbot charter
- `GROK_BOT_CREWMATE.md` — per-project crewmate charter
- `GROK_BOT_TRIAGE.md` — per-repo triage crewmate charter, signed on only when asked
- `TRIAGE.md` — triage judgment, not an installer
- `repo_policies.json` — per-repo verify and publish rules
- `skills/lavish-session/SKILL.md`
- `skills/adversarial-review/SKILL.md`
- `skills/project-management/SKILL.md`
- `skills/ahoy/SKILL.md`
- `skills/fail-then-pass/SKILL.md`
- `skills/repo-verify/SKILL.md`
- `skills/prefer-reuse/SKILL.md`
- `skills/triage-eligible-fetch/SKILL.md`
- `skills/triage-eligible-fetch/fetch.py`
- `skills/vision-md-triage-verdict/SKILL.md`
- `skills/14-day-stale-pr-close/SKILL.md`

## Steps

1. Copy this whole pack to `/home/box/agent-data/grok-ship/pack/` on the shared computer (clone or download it first if you only have this file's text). Every later reference to a pack file means that path. If a copy is already there, refresh it.

2. Look at the existing roster (agent profile folders). If a Firstmate already exists, reuse it. Do not create a second. Do not create per-repo verifier bots - use repo-verify and repo_policies.json. If an existing agent already matches Factory, Scout, Watchdog, or Eggbot under another name, UpdateAgent that agent instead of creating a duplicate.

3. Read `GROK_BOT_FIRSTMATE.md`. CreateAgent name `Firstmate` with that description. If you are already Firstmate, keep your name and update your description instead of cloning yourself.

4. Run the prefer-reuse skill, then CreateAgent or UpdateAgent:
   - `Factory` from `GROK_BOT_FACTORY.md`
   - `Scout` from `GROK_BOT_SCOUT.md`
   - `Watchdog` from `GROK_BOT_WATCHDOG.md`
   - `Eggbot` from `GROK_BOT_EGG.md`

5. Write global workflows from the skill files. Names:
   - Lavish session
   - Adversarial review
   - Project management
   - Ahoy
   - Fail-then-pass
   - Repo verify
   - Prefer reuse
   Use each skill's description line as the workflow description. Do not install extra plugins without a yes from the user.

6. Ensure `repo_policies.json` is on the pack path. Replace the example stub with real upstream repos as the captain onboards them. Adding a repo means editing this file, not CreateAgent.

7. Run the project-management setup: create the sqlite DB if it does not exist. Path is in that skill. Same path every time.

8. Check for lavish-axi on the shared computer. Minimum version 0.1.53. If missing, run `npx -y lavish-axi@latest` or ask the user to install it. Session URLs are served from the shared computer and the user views them from their own computer, so confirm with the user that they can reach it (tailnet or exposed address). Do not pretend the live loop works without it.

9. Detect source control CLIs on the shared computer: `gh`, `glab`, Bitbucket, or Cursor Origin, and verify the CLI is authenticated (for example `gh auth status`) - the adversarial review reads branches through it. Do not assume GitHub. Cloud agents separately need the user's Cursor account connected to whichever source control they use. Ask the user to connect whatever is missing. Do not ask them to paste a token in chat.

10. Message Firstmate with a task id (for example GS-READY). Tell it the skills are installed, the DB path, the core crewmate names, and to reply ready against that id. Empty or blocked still gets a reply. Tell Firstmate to leave a greeting message to the user.

11. Tell the user: talk only to Firstmate from here. This starter bot is leftover. They can delete it from the sidebar (right-click the row, Delete). You cannot delete it yourself.
