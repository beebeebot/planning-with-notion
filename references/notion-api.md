# Notion API Reference

All commands assume:
```bash
NOTION_KEY=$(cat ~/.config/notion/api_key)
DB_ID="31f3d7d1-79ad-80a3-80df-d2d194ae711c"
```

## Query Tasks Assigned to Me

```bash
curl -s -X POST "https://api.notion.com/v1/databases/$DB_ID/query" \
  -H "Authorization: Bearer $NOTION_KEY" \
  -H "Notion-Version: 2025-09-03" \
  -H "Content-Type: application/json" \
  -d '{
    "filter": {
      "and": [
        {"property": "Assignee", "select": {"equals": "Beebee"}},
        {"property": "Status", "select": {"does_not_equal": "Completed"}},
        {"property": "Status", "select": {"does_not_equal": "Backlog"}}
      ]
    }
  }'
```

## Create a Task

```bash
curl -s -X POST "https://api.notion.com/v1/pages" \
  -H "Authorization: Bearer $NOTION_KEY" \
  -H "Notion-Version: 2025-09-03" \
  -H "Content-Type: application/json" \
  -d '{
    "parent": {"database_id": "'$DB_ID'"},
    "properties": {
      "Name": {"title": [{"text": {"content": "Task name here"}}]},
      "Status": {"select": {"name": "To Do"}},
      "Priority": {"select": {"name": "Medium 🟠"}},
      "Assignee": {"select": {"name": "Beebee"}}
    },
    "children": [
      {"heading_1": {"rich_text": [{"text": {"content": "Task Goal"}}]}},
      {"paragraph": {"rich_text": [{"text": {"content": "What does done look like?"}}]}},
      {"heading_1": {"rich_text": [{"text": {"content": "Plan"}}]}},
      {"paragraph": {"rich_text": [{"text": {"content": "Steps to get there"}}]}},
      {"heading_1": {"rich_text": [{"text": {"content": "Acceptance Criteria"}}]}},
      {"paragraph": {"rich_text": [{"text": {"content": "How to prove it works"}}]}}
    ]
  }'
```

## Update Task Status

```bash
PAGE_ID="your-page-id"
curl -s -X PATCH "https://api.notion.com/v1/pages/$PAGE_ID" \
  -H "Authorization: Bearer $NOTION_KEY" \
  -H "Notion-Version: 2025-09-03" \
  -H "Content-Type: application/json" \
  -d '{"properties": {"Status": {"select": {"name": "In Progress"}}}}'
```

## Add a Comment

```bash
PAGE_ID="your-page-id"
curl -s -X POST "https://api.notion.com/v1/comments" \
  -H "Authorization: Bearer $NOTION_KEY" \
  -H "Notion-Version: 2025-09-03" \
  -H "Content-Type: application/json" \
  -d '{
    "parent": {"page_id": "'$PAGE_ID'"},
    "rich_text": [{"text": {"content": "✅ Step 1 complete — pipeline config updated"}}]
  }'
```

## Read Page Content (Goal/Plan/Criteria)

```bash
PAGE_ID="your-page-id"
curl -s "https://api.notion.com/v1/blocks/$PAGE_ID/children" \
  -H "Authorization: Bearer $NOTION_KEY" \
  -H "Notion-Version: 2025-09-03"
```

## Reassign to Mitch (for QA)

```bash
PAGE_ID="your-page-id"
curl -s -X PATCH "https://api.notion.com/v1/pages/$PAGE_ID" \
  -H "Authorization: Bearer $NOTION_KEY" \
  -H "Notion-Version: 2025-09-03" \
  -H "Content-Type: application/json" \
  -d '{
    "properties": {
      "Status": {"select": {"name": "QA"}},
      "Assignee": {"select": {"name": "Mitch"}}
    }
  }'
```
