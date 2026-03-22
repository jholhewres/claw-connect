# HubSpot Action Reference

**Provider:** `hubspot`
**Service:** `hubspot`
**App name:** `hubspot`

## Actions

### list_contacts

List contacts.

```bash
curl -s -X POST "https://integraclaw.dev/api/v1/action" \
  -H "Authorization: Bearer $INTEGRACLAW_API_KEY" \
  -H "Content-Type: application/json" \
  -d '{
    "provider": "hubspot",
    "service": "hubspot",
    "action": "list_contacts",
    "params": {
      "limit": 10,
      "properties": ["email", "firstname", "lastname"]
    }
  }'
```

| Parameter | Type | Required | Description |
|-----------|------|----------|-------------|
| `limit` | integer | no | Maximum number of results (default 10) |
| `after` | string | no | Cursor for pagination |
| `properties` | array | no | List of property names to include |

### create_contact

Create a contact.

```bash
curl -s -X POST "https://integraclaw.dev/api/v1/action" \
  -H "Authorization: Bearer $INTEGRACLAW_API_KEY" \
  -H "Content-Type: application/json" \
  -d '{
    "provider": "hubspot",
    "service": "hubspot",
    "action": "create_contact",
    "params": {
      "properties": {
        "email": "jane@example.com",
        "firstname": "Jane",
        "lastname": "Doe",
        "phone": "+1234567890"
      }
    }
  }'
```

| Parameter | Type | Required | Description |
|-----------|------|----------|-------------|
| `properties` | object | yes | Contact properties (e.g. `email`, `firstname`, `lastname`, `phone`) |

### search_contacts

Search contacts using filters.

```bash
curl -s -X POST "https://integraclaw.dev/api/v1/action" \
  -H "Authorization: Bearer $INTEGRACLAW_API_KEY" \
  -H "Content-Type: application/json" \
  -d '{
    "provider": "hubspot",
    "service": "hubspot",
    "action": "search_contacts",
    "params": {
      "query": "jane@example.com",
      "limit": 10,
      "properties": ["email", "firstname", "lastname"]
    }
  }'
```

| Parameter | Type | Required | Description |
|-----------|------|----------|-------------|
| `query` | string | no | Search query string |
| `filter_groups` | array | no | Array of filter group objects for advanced filtering |
| `sorts` | array | no | Array of sort objects |
| `limit` | integer | no | Maximum number of results |
| `after` | string | no | Cursor for pagination |
| `properties` | array | no | List of property names to include |

### create_deal

Create a deal.

```bash
curl -s -X POST "https://integraclaw.dev/api/v1/action" \
  -H "Authorization: Bearer $INTEGRACLAW_API_KEY" \
  -H "Content-Type: application/json" \
  -d '{
    "provider": "hubspot",
    "service": "hubspot",
    "action": "create_deal",
    "params": {
      "properties": {
        "dealname": "New Business Deal",
        "dealstage": "appointmentscheduled",
        "amount": "50000",
        "pipeline": "default"
      }
    }
  }'
```

| Parameter | Type | Required | Description |
|-----------|------|----------|-------------|
| `properties` | object | yes | Deal properties (e.g. `dealname`, `dealstage`, `amount`, `pipeline`) |

### Other Actions

| Action | Description |
|--------|-------------|
| `get_contact` | Get a contact by ID |
| `update_contact` | Update a contact by ID |
| `delete_contact` | Delete a contact by ID |
| `list_companies` | List companies |
| `get_company` | Get a company by ID |
| `create_company` | Create a company |
| `update_company` | Update a company by ID |
| `delete_company` | Delete a company by ID |
| `search_companies` | Search companies using filters |
| `list_deals` | List deals |
| `get_deal` | Get a deal by ID |
| `update_deal` | Update a deal by ID |
| `delete_deal` | Delete a deal by ID |
| `search_deals` | Search deals using filters |
| `list_owners` | List owners in the account |
| `get_owner` | Get an owner by ID |
| `get_properties` | Get properties for an object type |

## Resources

- [HubSpot API Documentation](https://developers.hubspot.com/docs/api/overview)
- [CRM Properties](https://developers.hubspot.com/docs/api/crm/properties)
