# Google BigQuery Action Reference

**Provider:** `google`
**Service:** `bigquery`
**App name:** `google-bigquery`

## Actions

### list_datasets

List datasets in a BigQuery project.

```bash
curl -s -X POST "$GATORHUB_URL/api/v1/action" \
  -H "Authorization: Bearer $GATORHUB_API_KEY" \
  -H "Content-Type: application/json" \
  -d '{
    "provider": "google",
    "service": "bigquery",
    "action": "list_datasets",
    "params": {"project_id": "my-gcp-project"}
  }'
```

| Parameter | Type | Required | Description |
|-----------|------|----------|-------------|
| `project_id` | string | yes | GCP project ID |

### list_tables

List tables in a BigQuery dataset.

```bash
curl -s -X POST "$GATORHUB_URL/api/v1/action" \
  -H "Authorization: Bearer $GATORHUB_API_KEY" \
  -H "Content-Type: application/json" \
  -d '{
    "provider": "google",
    "service": "bigquery",
    "action": "list_tables",
    "params": {"project_id": "my-gcp-project", "dataset_id": "my_dataset"}
  }'
```

| Parameter | Type | Required | Description |
|-----------|------|----------|-------------|
| `project_id` | string | yes | GCP project ID |
| `dataset_id` | string | yes | Dataset ID |

### run_query

Run a SQL query in BigQuery.

```bash
curl -s -X POST "$GATORHUB_URL/api/v1/action" \
  -H "Authorization: Bearer $GATORHUB_API_KEY" \
  -H "Content-Type: application/json" \
  -d '{
    "provider": "google",
    "service": "bigquery",
    "action": "run_query",
    "params": {
      "project_id": "my-gcp-project",
      "query": "SELECT * FROM `my_dataset.my_table` LIMIT 100",
      "max_results": 100
    }
  }'
```

| Parameter | Type | Required | Description |
|-----------|------|----------|-------------|
| `project_id` | string | yes | GCP project ID |
| `query` | string | yes | SQL query to execute |
| `use_legacy_sql` | boolean | no | Use legacy SQL syntax (default false) |
| `max_results` | integer | no | Maximum rows to return |

### get_query_results

Get results of a completed query job.

```bash
curl -s -X POST "$GATORHUB_URL/api/v1/action" \
  -H "Authorization: Bearer $GATORHUB_API_KEY" \
  -H "Content-Type: application/json" \
  -d '{
    "provider": "google",
    "service": "bigquery",
    "action": "get_query_results",
    "params": {
      "project_id": "my-gcp-project",
      "job_id": "job_abc123xyz"
    }
  }'
```

| Parameter | Type | Required | Description |
|-----------|------|----------|-------------|
| `project_id` | string | yes | GCP project ID |
| `job_id` | string | yes | Query job ID |
| `max_results` | integer | no | Maximum rows to return |
