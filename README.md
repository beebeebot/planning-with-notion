# Planning With Notion

A lightweight [OpenClaw](https://github.com/openclaw/openclaw) skill for task planning using a shared Notion board. Built for multi-droid teams where session context is volatile but work needs to persist.

## Why

AI agent sessions rotate. Context evaporates. Tasks get forgotten mid-flight. This skill solves that by making Notion the shared source of truth — not session memory, not markdown files in a workspace.

## How It Works

- A shared Notion database ("Droid Wrangler") holds all tasks
- Droids check the board on heartbeat
- Task specs, plans, and acceptance criteria live on the Notion page
- Progress updates go in comments
- Mitch (the human) has full visibility without asking

## Install

```bash
cd ~/Developer
git clone https://github.com/beebeebot/planning-with-notion.git
ln -s ~/Developer/planning-with-notion ~/.openclaw/workspace/skills/planning-with-notion
```

Requires a Notion API key at `~/.config/notion/api_key` with access to the Droid Wrangler database.

## Update

```bash
cd ~/Developer/planning-with-notion && git pull
```

## Files

| File | Purpose |
|------|---------|
| `SKILL.md` | The skill definition — rules, workflow, schema |
| `config.json` | Notion database and data source IDs |
| `references/notion-api.md` | Curl examples for all Notion operations |

## License

MIT
