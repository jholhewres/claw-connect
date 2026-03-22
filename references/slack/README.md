# Slack Action Reference

**Provider:** `slack`
**Service:** `slack`
**App name:** `slack`

## Actions

### send_message

Send a message to a channel.

```bash
curl -s -X POST "https://integraclaw.dev/api/v1/action" \
  -H "Authorization: Bearer $INTEGRACLAW_API_KEY" \
  -H "Content-Type: application/json" \
  -d '{
    "provider": "slack",
    "service": "slack",
    "action": "send_message",
    "params": {
      "channel": "C0123456789",
      "text": "Hello from IntegraClaw!"
    }
  }'
```

| Parameter | Type | Required | Description |
|-----------|------|----------|-------------|
| `channel` | string | yes | Channel ID to post to |
| `text` | string | yes | Message text (supports Slack markdown) |
| `thread_ts` | string | no | Thread timestamp to reply to |
| `unfurl_links` | boolean | no | Enable link unfurling (default `true`) |
| `unfurl_media` | boolean | no | Enable media unfurling (default `true`) |

### list_channels

List channels in the workspace.

```bash
curl -s -X POST "https://integraclaw.dev/api/v1/action" \
  -H "Authorization: Bearer $INTEGRACLAW_API_KEY" \
  -H "Content-Type: application/json" \
  -d '{
    "provider": "slack",
    "service": "slack",
    "action": "list_channels",
    "params": {
      "limit": 20,
      "types": "public_channel,private_channel"
    }
  }'
```

| Parameter | Type | Required | Description |
|-----------|------|----------|-------------|
| `limit` | integer | no | Maximum results (default 100, max 1000) |
| `cursor` | string | no | Pagination cursor |
| `types` | string | no | Channel types: `public_channel`, `private_channel`, `mpim`, `im` (comma-separated) |
| `exclude_archived` | boolean | no | Exclude archived channels (default `false`) |

### channel_history

Fetch message history from a channel.

```bash
curl -s -X POST "https://integraclaw.dev/api/v1/action" \
  -H "Authorization: Bearer $INTEGRACLAW_API_KEY" \
  -H "Content-Type: application/json" \
  -d '{
    "provider": "slack",
    "service": "slack",
    "action": "channel_history",
    "params": {
      "channel": "C0123456789",
      "limit": 50
    }
  }'
```

| Parameter | Type | Required | Description |
|-----------|------|----------|-------------|
| `channel` | string | yes | Channel ID |
| `limit` | integer | no | Number of messages to return (default 100) |
| `cursor` | string | no | Pagination cursor |
| `oldest` | string | no | Start of time range (Unix timestamp) |
| `latest` | string | no | End of time range (Unix timestamp) |
| `inclusive` | boolean | no | Include messages with oldest/latest timestamps |

### lookup_user_by_email

Look up a user by email address.

```bash
curl -s -X POST "https://integraclaw.dev/api/v1/action" \
  -H "Authorization: Bearer $INTEGRACLAW_API_KEY" \
  -H "Content-Type: application/json" \
  -d '{
    "provider": "slack",
    "service": "slack",
    "action": "lookup_user_by_email",
    "params": {
      "email": "jane@example.com"
    }
  }'
```

| Parameter | Type | Required | Description |
|-----------|------|----------|-------------|
| `email` | string | yes | Email address to look up |

### Other Actions

| Action | Description |
|--------|-------------|
| `get_channel` | Get info about a channel |
| `channel_replies` | Get replies in a thread |
| `join_channel` | Join a channel |
| `invite_to_channel` | Invite a user to a channel |
| `update_message` | Update an existing message |
| `delete_message` | Delete a message |
| `list_users` | List users in the workspace |
| `get_user` | Get info about a user |
| `add_reaction` | Add an emoji reaction to a message |
| `remove_reaction` | Remove an emoji reaction from a message |
| `get_reactions` | Get reactions for a message |
| `upload_file` | Upload a file to a channel |
| `list_files` | List files in the workspace |
| `get_team_info` | Get info about the workspace |

## Resources

- [Slack Web API Reference](https://api.slack.com/methods)
- [Message Formatting](https://api.slack.com/reference/surfaces/formatting)
