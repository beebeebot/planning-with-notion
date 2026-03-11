---
name: planning-with-notion
description: Notion-based task planning for OpenClaw droids. Use for any task estimated at ~10+ minutes. Checks the shared "Droid Wrangler" board on heartbeat, creates tasks before starting work, and tracks progress via Notion comments.
---

# Planning With Notion

A shared Notion board ("Droid Wrangler") is the single source of truth for tasks across all droids. If a task takes ~10+ minutes, it goes on the board. No exceptions.

## Setup

1. Clone this repo and symlink into your workspace skills:

```bash
cd ~/Developer
git clone https://github.com/beebeebot/planning-with-notion.git
ln -s ~/Developer/planning-with-notion ~/.openclaw/workspace/skills/planning-with-notion
```

2. Ensure Notion API key exists at `~/.config/notion/api_key`
3. The database and data source IDs are in `config.json` (same directory as this file)

To update the skill: `cd ~/Developer/planning-with-notion && git pull`

## Database Schema

| Property | Type | Notes |
|---|---|---|
| Name | Title | Task name |
| Status | Select | Backlog, To Do, Ready, In Progress, QA, Completed |
| Priority | Select | Low 🟢, Medium 🟠, High 🔴, Critical 🚨 |
| Assignee | Select | Mitch, Beebee, Artoo, Kaytoo, Threepio |
| Created time | Auto | Set by Notion |

Task details (goal, plan, acceptance criteria) go in the page body as markdown.
Progress updates go in **comments** (not page body edits).

## Statuses

| Status | Meaning | Action |
|---|---|---|
| Backlog | Mitch's working area | Ignore |
| To Do | Awaiting approval | Don't start until moved to In Progress or Ready |
| Ready | Approved — free to start | Pick up, move to In Progress, and begin work |
| In Progress | Active work | Pick up and progress |
| QA | Droid thinks it's done | Reassign to Mitch, alert him |
| Completed | Mitch accepted | Nothing to do |

## Rules

### On Heartbeat
1. Query the board for tasks assigned to you that are `In Progress`, `Ready`, or `To Do`
2. `In Progress` → continue working on it
3. `Ready` → pick it up, move to `In Progress`, and start work
4. `To Do` → note it exists, but don't start (wait for Mitch to move to Ready or In Progress)
5. If Mitch moved a task to `Ready` or `In Progress` since last check → pick it up

### Creating Tasks
- **You spot something that needs doing** → create as `To Do`, assign to yourself, wait for approval
- **Mitch verbally asks you to do something** → create as `To Do`, confirm the plan with Mitch, then move to `In Progress` once he approves (unless he says to skip confirmation and just go)
- Always write a goal, plan, and acceptance criteria in the page body before starting

### Working a Task
1. Move status to `In Progress`
2. Add progress updates as **comments** (not page edits)
3. Before major decisions, re-read the task page (goal + plan + acceptance criteria)
4. When done, verify against acceptance criteria
5. Move to `QA`, reassign to Mitch, and alert him

### Task Page Template

The page body should contain:

```markdown
# Task Goal
[One sentence: what does "done" look like?]

# Plan
[Numbered steps or bullet points — how you'll get there]

# Acceptance Criteria
[Specific, testable conditions that prove it works]
```

### Error Handling
- If blocked, add a comment explaining what's blocking you and move on to other tasks
- After 3 failed attempts at the same approach, stop, comment what you tried, and ask Mitch
- Never repeat the same failing action — mutate the approach

### Comments Format
Keep comments brief and useful:
```
✅ Step 2 complete — API endpoint working, tested with sample data
🚧 Blocked on Domain API approval — can't test listings endpoint yet
❌ ffmpeg remux failed (exit code 1) — trying different codec flags
```

## API Reference

See `references/notion-api.md` for curl examples for querying tasks, creating pages, and adding comments.
