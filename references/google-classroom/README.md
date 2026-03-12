# Google Classroom Action Reference

**Provider:** `google`
**Service:** `classroom`
**App name:** `google-classroom`

## Actions

### list_courses

List all Google Classroom courses. Use page_size to control results (default 20) and page_token for pagination.

```bash
curl -s -X POST "$GATORHUB_URL/api/v1/action" \
  -H "Authorization: Bearer $GATORHUB_API_KEY" \
  -H "Content-Type: application/json" \
  -d '{
    "provider": "google",
    "service": "classroom",
    "action": "list_courses",
    "params": {"page_size": 20}
  }'
```

| Parameter | Type | Required | Description |
|-----------|------|----------|-------------|
| `page_size` | integer | no | Max courses to return (default 20) |
| `page_token` | string | no | Pagination token |

### get_course

Get details of a specific Google Classroom course. Returns course name, section, enrollment code, and state.

```bash
curl -s -X POST "$GATORHUB_URL/api/v1/action" \
  -H "Authorization: Bearer $GATORHUB_API_KEY" \
  -H "Content-Type: application/json" \
  -d '{
    "provider": "google",
    "service": "classroom",
    "action": "get_course",
    "params": {"course_id": "123456789"}
  }'
```

| Parameter | Type | Required | Description |
|-----------|------|----------|-------------|
| `course_id` | string | yes | Course ID |

### list_students

List students enrolled in a Google Classroom course.

```bash
curl -s -X POST "$GATORHUB_URL/api/v1/action" \
  -H "Authorization: Bearer $GATORHUB_API_KEY" \
  -H "Content-Type: application/json" \
  -d '{
    "provider": "google",
    "service": "classroom",
    "action": "list_students",
    "params": {"course_id": "123456789", "page_size": 30}
  }'
```

| Parameter | Type | Required | Description |
|-----------|------|----------|-------------|
| `course_id` | string | yes | Course ID |
| `page_size` | integer | no | Max students to return (default 30) |

### create_assignment

Create a coursework assignment in a Google Classroom course. The assignment is published immediately.

```bash
curl -s -X POST "$GATORHUB_URL/api/v1/action" \
  -H "Authorization: Bearer $GATORHUB_API_KEY" \
  -H "Content-Type: application/json" \
  -d '{
    "provider": "google",
    "service": "classroom",
    "action": "create_assignment",
    "params": {
      "course_id": "123456789",
      "title": "Week 5 Homework",
      "description": "Complete exercises 1-10",
      "max_points": 100,
      "due_date": "2026-03-20"
    }
  }'
```

| Parameter | Type | Required | Description |
|-----------|------|----------|-------------|
| `course_id` | string | yes | Course ID |
| `title` | string | yes | Assignment title |
| `description` | string | no | Assignment description |
| `max_points` | integer | no | Maximum points |
| `due_date` | string | no | Due date (YYYY-MM-DD) |

### list_submissions

List student submissions for a coursework item. Returns submission states and grades.

```bash
curl -s -X POST "$GATORHUB_URL/api/v1/action" \
  -H "Authorization: Bearer $GATORHUB_API_KEY" \
  -H "Content-Type: application/json" \
  -d '{
    "provider": "google",
    "service": "classroom",
    "action": "list_submissions",
    "params": {
      "course_id": "123456789",
      "coursework_id": "987654321"
    }
  }'
```

| Parameter | Type | Required | Description |
|-----------|------|----------|-------------|
| `course_id` | string | yes | Course ID |
| `coursework_id` | string | yes | Coursework item ID |

### create_announcement

Create an announcement in a Google Classroom course. The announcement is published immediately.

```bash
curl -s -X POST "$GATORHUB_URL/api/v1/action" \
  -H "Authorization: Bearer $GATORHUB_API_KEY" \
  -H "Content-Type: application/json" \
  -d '{
    "provider": "google",
    "service": "classroom",
    "action": "create_announcement",
    "params": {
      "course_id": "123456789",
      "text": "Class cancelled tomorrow due to holiday. See you next week!"
    }
  }'
```

| Parameter | Type | Required | Description |
|-----------|------|----------|-------------|
| `course_id` | string | yes | Course ID |
| `text` | string | yes | Announcement text |
