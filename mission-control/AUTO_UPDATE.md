# Mission Control Auto-Update Workflow

This file defines the recurring workflow for refreshing `/workspace/mission-control/index.html` and pushing it to GitHub Pages.

## Goal
Every 6 hours, refresh Mission Control with the latest safe, useful operational data and push the updated dashboard live.

## Required checks before doing anything
1. Verify `/workspace/credentials.md` exists.
2. Verify a `GITHUB_TOKEN=` line exists and is non-empty.
3. Verify `/workspace/mission-control/index.html` exists.
4. Read `/workspace/CONTEXT.md`, `/workspace/MIND.md`, `/workspace/RULES.md`, and recent memory if needed.

## Non-negotiable rules
- Never expose credentials or tokens in output or HTML.
- Never push privacy-unsafe content.
- If a task fails 3 times in one run, stop and alert Josh.
- If API/session costs exceed the rule threshold, stop and alert Josh.
- If a required dependency is unavailable, mark it clearly in the dashboard and in any alert.

## Update targets
Each run should refresh as much of the following as possible:

### 1. Updated timestamp
- Set the header Updated timestamp to the current UTC time.

### 2. Revenue tracker
- Pull any available revenue/product status data from workspace files.
- If no new revenue data exists, keep values unchanged or show $0.00 where appropriate.
- Never invent revenue.

### 3. Project milestones and KPIs
- Review workspace files for changes that affect project status.
- Update project milestones, deadlines, and statuses if the current state has moved.

### 4. Decision queue
- Add new decisions if the current workspace state reveals unresolved strategic or operational choices.
- Remove or mark resolved decisions that are no longer pending.
- Include created dates and waiting time where possible.

### 5. Activity log
- Append meaningful new actions taken since the last dashboard refresh.
- Keep the log reverse chronological.
- Preserve recent important history.

### 6. API cost tracking
- Update token counts and costs using available session/runtime info.
- Codex OAuth should show $0.00 under Josh’s subscription.
- Show MiniMax Worker rates even if usage is zero.
- Only track Bodhi/OpenClaw work, not any other account sharing an upstream key.

### 7. System health
Use available checks and record limitations honestly.

Attempt these checks:
- `openclaw status`
- `df -h / /workspace`
- `docker ps`
- `docker stats --no-stream`
- watchdog/restart log inspection if a known log is available

#### Important environment note
At setup time, Docker is not installed in this environment. If `docker` is unavailable:
- do NOT fake docker health data
- mark container stats as unavailable in the dashboard
- include a warning in System Health
- only alert Josh if this is a regression from prior availability or part of a broader problem

## HTML generation / review policy
Josh requested: use ChatGPT Manager for HTML generation, not Sonnet.

### Environment limitation
If a named delegate like "ChatGPT Manager" is not available in the current runtime:
- do not pretend it was used
- use the closest safe available path
- record the limitation in the activity log only if relevant
- do not block the entire update unless the limitation makes the task unsafe or impossible

## Push workflow
After updating `index.html`:
1. Validate content is privacy-safe.
2. Commit locally if appropriate.
3. Push the updated HTML to the `mission-control` GitHub repo.
4. Confirm GitHub Pages remains live.

## Alerting rules
Alert Josh via the normal Telegram chat if any of the following occur:
- container/service down
- serious health warning
- disk warning / low space
- repeated task failure
- credentials/token problem
- GitHub push failure
- materially stale or inconsistent dashboard state

If there is no problem, a silent successful update is acceptable.

## Default operating style
- Keep updates concise and accurate.
- Prefer honest placeholders over fabricated precision.
- Keep the dashboard public-safe.
- Optimize for continuity and trust over cosmetic perfection.
