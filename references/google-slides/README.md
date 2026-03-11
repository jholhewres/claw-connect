# Google Slides Action Reference

**Provider:** `google`
**Service:** `slides`
**App name:** `google-slides`

## Important: How text insertion works

In Google Slides, a **slide** is a container — you CANNOT insert text directly into a slide objectId. Text must be inserted into **placeholder elements** or **shapes** on the slide.

When you create a slide with a layout (e.g. `TITLE_AND_BODY`), placeholder elements (title, body) are created with their own objectIds. To insert text you must target those element IDs, not the slide ID.

**Use `placeholderIdMappings`** in `createSlide` to assign known IDs to placeholders, then use those IDs with `insertText` in the same batch request.

## Actions

### get_presentation

Retrieve metadata and structure of a Google Slides presentation.

```bash
curl -s -X POST "https://integraclaw.dev/api/v1/action" \
  -H "Authorization: Bearer $INTEGRACLAW_API_KEY" \
  -H "Content-Type: application/json" \
  -d '{
    "provider": "google",
    "service": "slides",
    "action": "get_presentation",
    "params": {"presentation_id": "1a2b3c4d5e6f7g8h9i0j"}
  }'
```

| Parameter | Type | Required | Description |
|-----------|------|----------|-------------|
| `presentation_id` | string | yes | The ID of the presentation (from the URL) |

### create_presentation

Create a new blank Google Slides presentation.

```bash
curl -s -X POST "https://integraclaw.dev/api/v1/action" \
  -H "Authorization: Bearer $INTEGRACLAW_API_KEY" \
  -H "Content-Type: application/json" \
  -d '{
    "provider": "google",
    "service": "slides",
    "action": "create_presentation",
    "params": {"title": "Q4 Product Launch Deck"}
  }'
```

| Parameter | Type | Required | Description |
|-----------|------|----------|-------------|
| `title` | string | yes | Title for the new presentation |

### get_page

Get a specific slide/page from a presentation by its object ID. Useful to discover element/placeholder objectIds on existing slides.

```bash
curl -s -X POST "https://integraclaw.dev/api/v1/action" \
  -H "Authorization: Bearer $INTEGRACLAW_API_KEY" \
  -H "Content-Type: application/json" \
  -d '{
    "provider": "google",
    "service": "slides",
    "action": "get_page",
    "params": {
      "presentation_id": "1a2b3c4d5e6f7g8h9i0j",
      "page_id": "p"
    }
  }'
```

| Parameter | Type | Required | Description |
|-----------|------|----------|-------------|
| `presentation_id` | string | yes | The ID of the presentation |
| `page_id` | string | yes | The object ID of the slide (e.g. `p` for first slide) |

### batch_update

Apply multiple updates to a presentation in a single request (add slides, insert text, create shapes, update styles, etc.).

| Parameter | Type | Required | Description |
|-----------|------|----------|-------------|
| `presentation_id` | string | yes | The ID of the presentation |
| `requests` | array | yes | Array of update request objects |

#### Workflow: Create slides with text content

To create slides AND insert text, use `placeholderIdMappings` to assign custom IDs to placeholder elements, then reference those IDs in `insertText`. This is the recommended approach — everything in a single batch request.

Available placeholder types: `TITLE`, `SUBTITLE`, `BODY`, `CENTERED_TITLE`.
Available predefined layouts: `BLANK`, `TITLE`, `TITLE_AND_BODY`, `TITLE_AND_TWO_COLUMNS`, `TITLE_ONLY`, `SECTION_HEADER`, `ONE_COLUMN_TEXT`, `MAIN_POINT`, `BIG_NUMBER`, `CAPTION_ONLY`.

```bash
curl -s -X POST "https://integraclaw.dev/api/v1/action" \
  -H "Authorization: Bearer $INTEGRACLAW_API_KEY" \
  -H "Content-Type: application/json" \
  -d '{
    "provider": "google",
    "service": "slides",
    "action": "batch_update",
    "params": {
      "presentation_id": "PRESENTATION_ID",
      "requests": [
        {
          "createSlide": {
            "objectId": "slide_1",
            "slideLayoutReference": {"predefinedLayout": "TITLE"},
            "placeholderIdMappings": [
              {"layoutPlaceholder": {"type": "CENTERED_TITLE", "index": 0}, "objectId": "slide_1_title"},
              {"layoutPlaceholder": {"type": "SUBTITLE", "index": 0}, "objectId": "slide_1_subtitle"}
            ]
          }
        },
        {"insertText": {"objectId": "slide_1_title", "text": "My Presentation Title", "insertionIndex": 0}},
        {"insertText": {"objectId": "slide_1_subtitle", "text": "A subtitle goes here", "insertionIndex": 0}},
        {
          "createSlide": {
            "objectId": "slide_2",
            "slideLayoutReference": {"predefinedLayout": "TITLE_AND_BODY"},
            "placeholderIdMappings": [
              {"layoutPlaceholder": {"type": "TITLE", "index": 0}, "objectId": "slide_2_title"},
              {"layoutPlaceholder": {"type": "BODY", "index": 0}, "objectId": "slide_2_body"}
            ]
          }
        },
        {"insertText": {"objectId": "slide_2_title", "text": "Agenda", "insertionIndex": 0}},
        {"insertText": {"objectId": "slide_2_body", "text": "First topic\nSecond topic\nThird topic", "insertionIndex": 0}}
      ]
    }
  }'
```

#### Workflow: Add text to existing slides

For existing slides where you don't know the element IDs, first use `get_page` to discover placeholder objectIds, then use `batch_update` with `insertText` targeting those IDs.

#### Workflow: Create a text box on a blank slide

Use `createShape` to add a text box with a custom objectId, then `insertText` into it.

```bash
curl -s -X POST "https://integraclaw.dev/api/v1/action" \
  -H "Authorization: Bearer $INTEGRACLAW_API_KEY" \
  -H "Content-Type: application/json" \
  -d '{
    "provider": "google",
    "service": "slides",
    "action": "batch_update",
    "params": {
      "presentation_id": "PRESENTATION_ID",
      "requests": [
        {
          "createSlide": {
            "objectId": "slide_blank",
            "slideLayoutReference": {"predefinedLayout": "BLANK"}
          }
        },
        {
          "createShape": {
            "objectId": "textbox_1",
            "shapeType": "TEXT_BOX",
            "elementProperties": {
              "pageObjectId": "slide_blank",
              "size": {
                "width": {"magnitude": 400, "unit": "PT"},
                "height": {"magnitude": 50, "unit": "PT"}
              },
              "transform": {
                "scaleX": 1, "scaleY": 1,
                "translateX": 50, "translateY": 100,
                "unit": "PT"
              }
            }
          }
        },
        {"insertText": {"objectId": "textbox_1", "text": "Custom positioned text", "insertionIndex": 0}}
      ]
    }
  }'
```

#### Common mistakes

- **DO NOT** use a slide objectId (e.g. `slide_2`) with `insertText` — slides don't accept text directly.
- **DO NOT** fabricate element IDs — always use `placeholderIdMappings` or `createShape` to assign IDs you control.
- Match the placeholder type to the layout: `TITLE` layout uses `CENTERED_TITLE` + `SUBTITLE`; `TITLE_AND_BODY` uses `TITLE` + `BODY`.
