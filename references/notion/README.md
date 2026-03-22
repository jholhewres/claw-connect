# Notion Action Reference

**Provider:** `notion`
**Service:** `notion`
**App name:** `notion`

## Actions

### search

Search pages and databases.

```bash
curl -s -X POST "https://integraclaw.dev/api/v1/action" \
  -H "Authorization: Bearer $INTEGRACLAW_API_KEY" \
  -H "Content-Type: application/json" \
  -d '{
    "provider": "notion",
    "service": "notion",
    "action": "search",
    "params": {
      "query": "Project Plan",
      "filter": {"property": "object", "value": "page"},
      "page_size": 10
    }
  }'
```

| Parameter | Type | Required | Description |
|-----------|------|----------|-------------|
| `query` | string | no | Search query text |
| `filter` | object | no | Filter by object type (e.g. `{"property": "object", "value": "page"}` or `"database"`) |
| `sort` | object | no | Sort results (e.g. `{"direction": "descending", "timestamp": "last_edited_time"}`) |
| `start_cursor` | string | no | Pagination cursor |
| `page_size` | integer | no | Number of results (max 100) |

### query_data_source

Query a database.

```bash
curl -s -X POST "https://integraclaw.dev/api/v1/action" \
  -H "Authorization: Bearer $INTEGRACLAW_API_KEY" \
  -H "Content-Type: application/json" \
  -d '{
    "provider": "notion",
    "service": "notion",
    "action": "query_data_source",
    "params": {
      "database_id": "abc123def456",
      "filter": {
        "property": "Status",
        "select": {"equals": "In Progress"}
      },
      "page_size": 20
    }
  }'
```

| Parameter | Type | Required | Description |
|-----------|------|----------|-------------|
| `database_id` | string | yes | Database ID to query |
| `filter` | object | no | Filter object (Notion filter syntax) |
| `sorts` | array | no | Array of sort objects |
| `start_cursor` | string | no | Pagination cursor |
| `page_size` | integer | no | Number of results (max 100) |

### create_page

Create a page.

```bash
curl -s -X POST "https://integraclaw.dev/api/v1/action" \
  -H "Authorization: Bearer $INTEGRACLAW_API_KEY" \
  -H "Content-Type: application/json" \
  -d '{
    "provider": "notion",
    "service": "notion",
    "action": "create_page",
    "params": {
      "parent": {"database_id": "abc123def456"},
      "properties": {
        "Name": {"title": [{"text": {"content": "New Task"}}]},
        "Status": {"select": {"name": "To Do"}}
      }
    }
  }'
```

| Parameter | Type | Required | Description |
|-----------|------|----------|-------------|
| `parent` | object | yes | Parent object (`{"database_id": "..."}` or `{"page_id": "..."}`) |
| `properties` | object | yes | Page properties (Notion property format) |
| `children` | array | no | Array of block objects for page content |
| `icon` | object | no | Page icon (emoji or external URL) |
| `cover` | object | no | Page cover image |

### append_block_children

Append content blocks to a page or block.

```bash
curl -s -X POST "https://integraclaw.dev/api/v1/action" \
  -H "Authorization: Bearer $INTEGRACLAW_API_KEY" \
  -H "Content-Type: application/json" \
  -d '{
    "provider": "notion",
    "service": "notion",
    "action": "append_block_children",
    "params": {
      "block_id": "abc123def456",
      "children": [
        {
          "object": "block",
          "type": "paragraph",
          "paragraph": {
            "rich_text": [{"type": "text", "text": {"content": "Hello from IntegraClaw!"}}]
          }
        }
      ]
    }
  }'
```

| Parameter | Type | Required | Description |
|-----------|------|----------|-------------|
| `block_id` | string | yes | Parent block or page ID |
| `children` | array | yes | Array of block objects to append |
| `after` | string | no | Block ID to append after |

### Other Actions

| Action | Description |
|--------|-------------|
| `get_database` | Get a database by ID |
| `create_database` | Create a database |
| `get_data_source` | Get a data source by ID |
| `update_data_source` | Update a data source by ID |
| `get_page` | Get a page by ID |
| `update_page` | Update page properties |
| `archive_page` | Archive (delete) a page |
| `get_block` | Get a block by ID |
| `get_block_children` | Get child blocks of a block |
| `update_block` | Update a block |
| `delete_block` | Delete a block |
| `list_users` | List users in the workspace |
| `get_user` | Get a user by ID |
| `get_bot_user` | Get the bot user |

## Resources

- [Notion API Reference](https://developers.notion.com/reference)
- [Property Values](https://developers.notion.com/reference/property-value-object)
- [Block Types](https://developers.notion.com/reference/block)
