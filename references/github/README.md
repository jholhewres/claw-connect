# GitHub Action Reference

**Provider:** `github`
**Service:** `github`
**App name:** `github`

## Actions

### list_repos

List repositories for the authenticated user.

```bash
curl -s -X POST "https://integraclaw.dev/api/v1/action" \
  -H "Authorization: Bearer $INTEGRACLAW_API_KEY" \
  -H "Content-Type: application/json" \
  -d '{
    "provider": "github",
    "service": "github",
    "action": "list_repos",
    "params": {
      "type": "owner",
      "sort": "updated",
      "per_page": 10
    }
  }'
```

| Parameter | Type | Required | Description |
|-----------|------|----------|-------------|
| `type` | string | no | Type of repos: `all`, `owner`, `public`, `private`, `member` |
| `sort` | string | no | Sort by: `created`, `updated`, `pushed`, `full_name` |
| `per_page` | integer | no | Results per page (max 100) |
| `page` | integer | no | Page number |

### get_repo

Get a repository.

```bash
curl -s -X POST "https://integraclaw.dev/api/v1/action" \
  -H "Authorization: Bearer $INTEGRACLAW_API_KEY" \
  -H "Content-Type: application/json" \
  -d '{
    "provider": "github",
    "service": "github",
    "action": "get_repo",
    "params": {
      "owner": "octocat",
      "repo": "hello-world"
    }
  }'
```

| Parameter | Type | Required | Description |
|-----------|------|----------|-------------|
| `owner` | string | yes | Repository owner |
| `repo` | string | yes | Repository name |

### list_branches

List branches of a repository.

```bash
curl -s -X POST "https://integraclaw.dev/api/v1/action" \
  -H "Authorization: Bearer $INTEGRACLAW_API_KEY" \
  -H "Content-Type: application/json" \
  -d '{
    "provider": "github",
    "service": "github",
    "action": "list_branches",
    "params": {
      "owner": "octocat",
      "repo": "hello-world",
      "per_page": 30
    }
  }'
```

| Parameter | Type | Required | Description |
|-----------|------|----------|-------------|
| `owner` | string | yes | Repository owner |
| `repo` | string | yes | Repository name |
| `per_page` | integer | no | Results per page (max 100) |
| `page` | integer | no | Page number |

### create_repo

Create a repository.

```bash
curl -s -X POST "https://integraclaw.dev/api/v1/action" \
  -H "Authorization: Bearer $INTEGRACLAW_API_KEY" \
  -H "Content-Type: application/json" \
  -d '{
    "provider": "github",
    "service": "github",
    "action": "create_repo",
    "params": {
      "name": "my-new-repo",
      "description": "A new repository",
      "private": true,
      "auto_init": true
    }
  }'
```

| Parameter | Type | Required | Description |
|-----------|------|----------|-------------|
| `name` | string | yes | Repository name |
| `description` | string | no | Repository description |
| `private` | boolean | no | Whether the repo is private (default `false`) |
| `auto_init` | boolean | no | Initialize with a README (default `false`) |

### list_issues

List issues for a repository.

```bash
curl -s -X POST "https://integraclaw.dev/api/v1/action" \
  -H "Authorization: Bearer $INTEGRACLAW_API_KEY" \
  -H "Content-Type: application/json" \
  -d '{
    "provider": "github",
    "service": "github",
    "action": "list_issues",
    "params": {
      "owner": "octocat",
      "repo": "hello-world",
      "state": "open",
      "per_page": 10
    }
  }'
```

| Parameter | Type | Required | Description |
|-----------|------|----------|-------------|
| `owner` | string | yes | Repository owner |
| `repo` | string | yes | Repository name |
| `state` | string | no | State filter: `open`, `closed`, `all` (default `open`) |
| `labels` | string | no | Comma-separated list of label names |
| `sort` | string | no | Sort by: `created`, `updated`, `comments` |
| `per_page` | integer | no | Results per page (max 100) |
| `page` | integer | no | Page number |

### get_issue

Get a specific issue.

```bash
curl -s -X POST "https://integraclaw.dev/api/v1/action" \
  -H "Authorization: Bearer $INTEGRACLAW_API_KEY" \
  -H "Content-Type: application/json" \
  -d '{
    "provider": "github",
    "service": "github",
    "action": "get_issue",
    "params": {
      "owner": "octocat",
      "repo": "hello-world",
      "issue_number": 42
    }
  }'
```

| Parameter | Type | Required | Description |
|-----------|------|----------|-------------|
| `owner` | string | yes | Repository owner |
| `repo` | string | yes | Repository name |
| `issue_number` | integer | yes | Issue number |

### create_issue

Create an issue.

```bash
curl -s -X POST "https://integraclaw.dev/api/v1/action" \
  -H "Authorization: Bearer $INTEGRACLAW_API_KEY" \
  -H "Content-Type: application/json" \
  -d '{
    "provider": "github",
    "service": "github",
    "action": "create_issue",
    "params": {
      "owner": "octocat",
      "repo": "hello-world",
      "title": "Found a bug",
      "body": "Steps to reproduce...",
      "labels": ["bug"],
      "assignees": ["octocat"]
    }
  }'
```

| Parameter | Type | Required | Description |
|-----------|------|----------|-------------|
| `owner` | string | yes | Repository owner |
| `repo` | string | yes | Repository name |
| `title` | string | yes | Issue title |
| `body` | string | no | Issue body |
| `labels` | array | no | Array of label names |
| `assignees` | array | no | Array of usernames to assign |

### update_issue

Update an issue.

```bash
curl -s -X POST "https://integraclaw.dev/api/v1/action" \
  -H "Authorization: Bearer $INTEGRACLAW_API_KEY" \
  -H "Content-Type: application/json" \
  -d '{
    "provider": "github",
    "service": "github",
    "action": "update_issue",
    "params": {
      "owner": "octocat",
      "repo": "hello-world",
      "issue_number": 42,
      "state": "closed",
      "labels": ["bug", "wontfix"]
    }
  }'
```

| Parameter | Type | Required | Description |
|-----------|------|----------|-------------|
| `owner` | string | yes | Repository owner |
| `repo` | string | yes | Repository name |
| `issue_number` | integer | yes | Issue number |
| `title` | string | no | Updated title |
| `body` | string | no | Updated body |
| `state` | string | no | State: `open` or `closed` |
| `labels` | array | no | Array of label names |
| `assignees` | array | no | Array of usernames to assign |

### add_issue_comment

Add a comment to an issue.

```bash
curl -s -X POST "https://integraclaw.dev/api/v1/action" \
  -H "Authorization: Bearer $INTEGRACLAW_API_KEY" \
  -H "Content-Type: application/json" \
  -d '{
    "provider": "github",
    "service": "github",
    "action": "add_issue_comment",
    "params": {
      "owner": "octocat",
      "repo": "hello-world",
      "issue_number": 42,
      "body": "This is a comment."
    }
  }'
```

| Parameter | Type | Required | Description |
|-----------|------|----------|-------------|
| `owner` | string | yes | Repository owner |
| `repo` | string | yes | Repository name |
| `issue_number` | integer | yes | Issue number |
| `body` | string | yes | Comment body |

### list_pull_requests

List pull requests for a repository.

```bash
curl -s -X POST "https://integraclaw.dev/api/v1/action" \
  -H "Authorization: Bearer $INTEGRACLAW_API_KEY" \
  -H "Content-Type: application/json" \
  -d '{
    "provider": "github",
    "service": "github",
    "action": "list_pull_requests",
    "params": {
      "owner": "octocat",
      "repo": "hello-world",
      "state": "open",
      "per_page": 10
    }
  }'
```

| Parameter | Type | Required | Description |
|-----------|------|----------|-------------|
| `owner` | string | yes | Repository owner |
| `repo` | string | yes | Repository name |
| `state` | string | no | State filter: `open`, `closed`, `all` (default `open`) |
| `sort` | string | no | Sort by: `created`, `updated`, `popularity`, `long-running` |
| `per_page` | integer | no | Results per page (max 100) |
| `page` | integer | no | Page number |

### get_pull_request

Get a specific pull request.

```bash
curl -s -X POST "https://integraclaw.dev/api/v1/action" \
  -H "Authorization: Bearer $INTEGRACLAW_API_KEY" \
  -H "Content-Type: application/json" \
  -d '{
    "provider": "github",
    "service": "github",
    "action": "get_pull_request",
    "params": {
      "owner": "octocat",
      "repo": "hello-world",
      "pull_number": 1
    }
  }'
```

| Parameter | Type | Required | Description |
|-----------|------|----------|-------------|
| `owner` | string | yes | Repository owner |
| `repo` | string | yes | Repository name |
| `pull_number` | integer | yes | Pull request number |

### create_pull_request

Create a pull request.

```bash
curl -s -X POST "https://integraclaw.dev/api/v1/action" \
  -H "Authorization: Bearer $INTEGRACLAW_API_KEY" \
  -H "Content-Type: application/json" \
  -d '{
    "provider": "github",
    "service": "github",
    "action": "create_pull_request",
    "params": {
      "owner": "octocat",
      "repo": "hello-world",
      "title": "Add new feature",
      "head": "feature-branch",
      "base": "main",
      "body": "This PR adds a new feature.",
      "draft": false
    }
  }'
```

| Parameter | Type | Required | Description |
|-----------|------|----------|-------------|
| `owner` | string | yes | Repository owner |
| `repo` | string | yes | Repository name |
| `title` | string | yes | PR title |
| `head` | string | yes | Branch containing changes |
| `base` | string | yes | Branch to merge into |
| `body` | string | no | PR description |
| `draft` | boolean | no | Create as draft PR (default `false`) |

### list_pr_comments

List comments on a pull request.

```bash
curl -s -X POST "https://integraclaw.dev/api/v1/action" \
  -H "Authorization: Bearer $INTEGRACLAW_API_KEY" \
  -H "Content-Type: application/json" \
  -d '{
    "provider": "github",
    "service": "github",
    "action": "list_pr_comments",
    "params": {
      "owner": "octocat",
      "repo": "hello-world",
      "pull_number": 1,
      "per_page": 30
    }
  }'
```

| Parameter | Type | Required | Description |
|-----------|------|----------|-------------|
| `owner` | string | yes | Repository owner |
| `repo` | string | yes | Repository name |
| `pull_number` | integer | yes | Pull request number |
| `per_page` | integer | no | Results per page (max 100) |
| `page` | integer | no | Page number |

### get_authenticated_user

Get the currently authenticated user.

```bash
curl -s -X POST "https://integraclaw.dev/api/v1/action" \
  -H "Authorization: Bearer $INTEGRACLAW_API_KEY" \
  -H "Content-Type: application/json" \
  -d '{
    "provider": "github",
    "service": "github",
    "action": "get_authenticated_user",
    "params": {}
  }'
```

No parameters required.

### get_user

Get a user by username.

```bash
curl -s -X POST "https://integraclaw.dev/api/v1/action" \
  -H "Authorization: Bearer $INTEGRACLAW_API_KEY" \
  -H "Content-Type: application/json" \
  -d '{
    "provider": "github",
    "service": "github",
    "action": "get_user",
    "params": {
      "username": "octocat"
    }
  }'
```

| Parameter | Type | Required | Description |
|-----------|------|----------|-------------|
| `username` | string | yes | GitHub username |

### search_repos

Search repositories.

```bash
curl -s -X POST "https://integraclaw.dev/api/v1/action" \
  -H "Authorization: Bearer $INTEGRACLAW_API_KEY" \
  -H "Content-Type: application/json" \
  -d '{
    "provider": "github",
    "service": "github",
    "action": "search_repos",
    "params": {
      "q": "language:python stars:>1000",
      "sort": "stars",
      "order": "desc",
      "per_page": 10
    }
  }'
```

| Parameter | Type | Required | Description |
|-----------|------|----------|-------------|
| `q` | string | yes | Search query (GitHub search syntax) |
| `sort` | string | no | Sort by: `stars`, `forks`, `help-wanted-issues`, `updated` |
| `order` | string | no | Order: `asc` or `desc` |
| `per_page` | integer | no | Results per page (max 100) |
| `page` | integer | no | Page number |

### search_issues

Search issues and pull requests.

```bash
curl -s -X POST "https://integraclaw.dev/api/v1/action" \
  -H "Authorization: Bearer $INTEGRACLAW_API_KEY" \
  -H "Content-Type: application/json" \
  -d '{
    "provider": "github",
    "service": "github",
    "action": "search_issues",
    "params": {
      "q": "repo:octocat/hello-world is:issue is:open",
      "sort": "created",
      "order": "desc",
      "per_page": 10
    }
  }'
```

| Parameter | Type | Required | Description |
|-----------|------|----------|-------------|
| `q` | string | yes | Search query (GitHub search syntax) |
| `sort` | string | no | Sort by: `comments`, `reactions`, `created`, `updated` |
| `order` | string | no | Order: `asc` or `desc` |
| `per_page` | integer | no | Results per page (max 100) |
| `page` | integer | no | Page number |

### create_gist

Create a gist.

```bash
curl -s -X POST "https://integraclaw.dev/api/v1/action" \
  -H "Authorization: Bearer $INTEGRACLAW_API_KEY" \
  -H "Content-Type: application/json" \
  -d '{
    "provider": "github",
    "service": "github",
    "action": "create_gist",
    "params": {
      "description": "Example gist",
      "public": false,
      "files": {
        "hello.py": {"content": "print(\"Hello, world!\")"}
      }
    }
  }'
```

| Parameter | Type | Required | Description |
|-----------|------|----------|-------------|
| `description` | string | no | Gist description |
| `public` | boolean | no | Whether the gist is public (default `false`) |
| `files` | object | yes | Object mapping filenames to `{"content": "..."}` objects |

## Resources

- [GitHub REST API Documentation](https://docs.github.com/en/rest)
- [Search Syntax](https://docs.github.com/en/search-github/searching-on-github)
