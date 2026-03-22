# Trello Action Reference

**Provider:** `trello`
**Service:** `trello`
**App name:** `trello`

## Actions

### list_boards

List boards for the authenticated user.

```bash
curl -s -X POST "https://integraclaw.dev/api/v1/action" \
  -H "Authorization: Bearer $INTEGRACLAW_API_KEY" \
  -H "Content-Type: application/json" \
  -d '{
    "provider": "trello",
    "service": "trello",
    "action": "list_boards",
    "params": {
      "filter": "open",
      "fields": "name,url,dateLastActivity"
    }
  }'
```

| Parameter | Type | Required | Description |
|-----------|------|----------|-------------|
| `filter` | string | no | Filter: `all`, `closed`, `members`, `open`, `organization`, `public`, `starred` |
| `fields` | string | no | Comma-separated list of board fields to return |

### create_card

Create a card on a list.

```bash
curl -s -X POST "https://integraclaw.dev/api/v1/action" \
  -H "Authorization: Bearer $INTEGRACLAW_API_KEY" \
  -H "Content-Type: application/json" \
  -d '{
    "provider": "trello",
    "service": "trello",
    "action": "create_card",
    "params": {
      "idList": "60d5f1a2b3c4d5e6f7890123",
      "name": "New task",
      "desc": "Task description",
      "pos": "top",
      "idLabels": ["60d5f1a2b3c4d5e6f7890124"]
    }
  }'
```

| Parameter | Type | Required | Description |
|-----------|------|----------|-------------|
| `idList` | string | yes | ID of the list to add the card to |
| `name` | string | yes | Card name |
| `desc` | string | no | Card description |
| `pos` | string | no | Position: `top`, `bottom`, or a positive float |
| `due` | string | no | Due date (ISO 8601 format) |
| `idLabels` | array | no | Array of label IDs |
| `idMembers` | array | no | Array of member IDs |

### get_cards

Get cards on a board or list.

```bash
curl -s -X POST "https://integraclaw.dev/api/v1/action" \
  -H "Authorization: Bearer $INTEGRACLAW_API_KEY" \
  -H "Content-Type: application/json" \
  -d '{
    "provider": "trello",
    "service": "trello",
    "action": "get_cards",
    "params": {
      "board_id": "60d5f1a2b3c4d5e6f7890123",
      "filter": "open",
      "fields": "name,idList,labels,due"
    }
  }'
```

| Parameter | Type | Required | Description |
|-----------|------|----------|-------------|
| `board_id` | string | no | Board ID (provide either `board_id` or `list_id`) |
| `list_id` | string | no | List ID (provide either `board_id` or `list_id`) |
| `filter` | string | no | Filter: `all`, `closed`, `none`, `open`, `visible` |
| `fields` | string | no | Comma-separated list of card fields to return |

### search

Search Trello for boards, cards, and more.

```bash
curl -s -X POST "https://integraclaw.dev/api/v1/action" \
  -H "Authorization: Bearer $INTEGRACLAW_API_KEY" \
  -H "Content-Type: application/json" \
  -d '{
    "provider": "trello",
    "service": "trello",
    "action": "search",
    "params": {
      "query": "bug fix",
      "modelTypes": "cards",
      "cards_limit": 10
    }
  }'
```

| Parameter | Type | Required | Description |
|-----------|------|----------|-------------|
| `query` | string | yes | Search query |
| `modelTypes` | string | no | Types to search: `actions`, `boards`, `cards`, `members`, `organizations` (comma-separated) |
| `board_fields` | string | no | Board fields to return |
| `cards_limit` | integer | no | Max number of cards to return |
| `idBoards` | string | no | Board ID to limit search to |

### Other Actions

| Action | Description |
|--------|-------------|
| `get_board` | Get a board by ID |
| `create_board` | Create a new board |
| `get_lists` | Get lists on a board |
| `create_list` | Create a list on a board |
| `archive_list` | Archive a list |
| `get_card` | Get a card by ID |
| `update_card` | Update a card by ID |
| `delete_card` | Delete a card by ID |
| `add_comment` | Add a comment to a card |
| `get_labels` | Get labels on a board |
| `create_label` | Create a label on a board |
| `get_board_members` | Get members of a board |
| `create_checklist` | Create a checklist on a card |
| `add_checklist_item` | Add an item to a checklist |

## Resources

- [Trello REST API Documentation](https://developer.atlassian.com/cloud/trello/rest/)
- [Search Query Syntax](https://developer.atlassian.com/cloud/trello/rest/api-group-search/)
