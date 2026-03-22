# Document Processing Action Reference

**Provider:** `integraclaw`
**Service:** `processing`
**App name:** `integraclaw-processing`

**No OAuth connection required.** These tools work with just the API key — no need to connect a service through the dashboard.

## Actions

### extract_text

Extract text content from a document file. Supports PDF, DOCX, PPTX, and XLSX formats.

**Two ways to send the file:**

1. **Multipart upload (recommended)** — send the file binary directly:

```bash
curl -s -X POST "https://integraclaw.dev/api/v1/action" \
  -H "Authorization: Bearer $INTEGRACLAW_API_KEY" \
  -F "provider=integraclaw" \
  -F "service=processing" \
  -F "action=extract_text" \
  -F "file=@document.pdf"
```

2. **JSON with file_url** — provide a URL to download from:

```bash
curl -s -X POST "https://integraclaw.dev/api/v1/action" \
  -H "Authorization: Bearer $INTEGRACLAW_API_KEY" \
  -H "Content-Type: application/json" \
  -d '{
    "provider": "integraclaw",
    "service": "processing",
    "action": "extract_text",
    "params": {
      "file_url": "https://example.com/document.pdf"
    }
  }'
```

| Parameter | Type | Required | Description |
|-----------|------|----------|-------------|
| `file` | binary | conditional | The file to extract text from (multipart upload). Either `file` or `file_url` is required |
| `file_url` | string | conditional | URL to the file (public URL, presigned URL, or direct download link). Either `file` or `file_url` is required |
| `format` | string | no | Force format detection: `pdf`, `docx`, `pptx`, `xlsx`. If omitted, auto-detected from filename or file content |
| `pages` | string | no | Page range for PDF extraction (e.g. `"1-5"`, `"1,3,5"`). Only applies to PDF format |

**Multipart with extra params:**

```bash
curl -s -X POST "https://integraclaw.dev/api/v1/action" \
  -H "Authorization: Bearer $INTEGRACLAW_API_KEY" \
  -F "provider=integraclaw" \
  -F "service=processing" \
  -F "action=extract_text" \
  -F 'params={"format":"xlsx"}' \
  -F "file=@spreadsheet.xlsx"
```

**Response:**

```json
{
  "success": true,
  "status": 200,
  "data": {
    "text": "extracted content...",
    "pages": 12,
    "format": "pdf",
    "chars": 45230
  }
}
```

| Field | Type | Description |
|-------|------|-------------|
| `text` | string | The extracted text content |
| `pages` | integer | Number of pages (PDF), slides (PPTX), or sheets (XLSX). 0 for DOCX |
| `format` | string | Detected format: `pdf`, `docx`, `pptx`, `xlsx` |
| `chars` | integer | Character count of the extracted text |
| `truncated` | boolean | Present and `true` if text was truncated (max 10 MB output) |
| `skipped_pages` | integer | Present if any pages could not be read (PDF only) |

**Supported formats:**

| Format | Detection | Notes |
|--------|-----------|-------|
| PDF | `.pdf` extension or `%PDF` magic bytes | Supports page range selection via `pages` param |
| DOCX | `.docx` extension or ZIP with `word/` | Extracts from `document.xml` |
| PPTX | `.pptx` extension or ZIP with `ppt/` | Extracts text from all slides |
| XLSX | `.xlsx` extension or ZIP with `xl/` | Extracts all sheets, tab-separated columns |

**Limits:**

- Maximum file size: 50 MB
- Maximum output text: 10 MB (truncated with `truncated: true` if exceeded)
- Maximum pages/slides/sheets: 10,000
- Only `http` and `https` URLs are allowed (for `file_url`)

**Error responses:**

| Status | Error | Meaning |
|--------|-------|---------|
| 400 | `a file is required` | Neither file upload nor file_url was provided |
| 400 | `could not detect file format` | Format not recognized — provide the `format` parameter |
| 400 | `file is empty` | The file has 0 bytes |
| 413 | `uploaded file exceeds maximum size` | File is larger than 50 MB |
| 422 | `failed to extract text: ...` | File is corrupted or unreadable |
| 502 | `failed to download file: ...` | Could not download from the provided URL |

### Examples

**Upload a PDF and extract specific pages:**

```bash
curl -s -X POST "https://integraclaw.dev/api/v1/action" \
  -H "Authorization: Bearer $INTEGRACLAW_API_KEY" \
  -F "provider=integraclaw" \
  -F "service=processing" \
  -F "action=extract_text" \
  -F 'params={"pages":"1-3"}' \
  -F "file=@report.pdf"
```

**Extract text from an XLSX via URL:**

```bash
curl -s -X POST "https://integraclaw.dev/api/v1/action" \
  -H "Authorization: Bearer $INTEGRACLAW_API_KEY" \
  -H "Content-Type: application/json" \
  -d '{
    "provider": "integraclaw",
    "service": "processing",
    "action": "extract_text",
    "params": {
      "file_url": "https://storage.example.com/data.xlsx"
    }
  }'
```

**Upload with forced format (useful for files without extension):**

```bash
curl -s -X POST "https://integraclaw.dev/api/v1/action" \
  -H "Authorization: Bearer $INTEGRACLAW_API_KEY" \
  -F "provider=integraclaw" \
  -F "service=processing" \
  -F "action=extract_text" \
  -F 'params={"format":"docx"}' \
  -F "file=@downloaded_file"
```

---

### generate_pdf

Generate a PDF document from structured content blocks.

```bash
curl -s -X POST "https://integraclaw.dev/api/v1/action" \
  -H "Authorization: Bearer $INTEGRACLAW_API_KEY" \
  -H "Content-Type: application/json" \
  -d '{
    "provider": "integraclaw",
    "service": "processing",
    "action": "generate_pdf",
    "params": {
      "title": "Monthly Report",
      "content": [
        {"type": "heading", "level": 1, "text": "Summary"},
        {"type": "paragraph", "text": "This report covers Q1 performance metrics."},
        {"type": "table", "headers": ["Metric", "Value"], "rows": [["Revenue", "$1.2M"], ["Growth", "15%"]]},
        {"type": "list", "ordered": true, "items": ["Increased marketing spend", "Launched new product line"]},
        {"type": "separator"},
        {"type": "paragraph", "text": "End of report."}
      ],
      "page_size": "A4",
      "orientation": "portrait",
      "filename": "report.pdf"
    }
  }'
```

| Parameter | Type | Required | Description |
|-----------|------|----------|-------------|
| `title` | string | no | Document title displayed at the top of the first page |
| `content` | array | **yes** | Array of content blocks (see Content Block Types below) |
| `page_size` | string | no | Page size: `A4` (default), `Letter`, `Legal` |
| `orientation` | string | no | Page orientation: `portrait` (default), `landscape` |
| `filename` | string | no | Output filename (default: `document.pdf`) |

**Response:**

```json
{
  "success": true,
  "status": 200,
  "data": {
    "file_base64": "JVBERi0xLjM...",
    "filename": "report.pdf",
    "mime_type": "application/pdf",
    "size": 12345
  }
}
```

| Field | Type | Description |
|-------|------|-------------|
| `file_base64` | string | Base64-encoded PDF file |
| `filename` | string | The output filename |
| `mime_type` | string | Always `application/pdf` |
| `size` | integer | File size in bytes (before base64 encoding) |

**Content Block Types:**

| Type | Fields | Description |
|------|--------|-------------|
| `heading` | `text`, `level` (1-3) | Bold heading — level controls font size (16/14/12pt) |
| `paragraph` | `text` | Normal text paragraph with word-wrap |
| `table` | `headers` (string[]), `rows` (string[][]) | Table with optional grey header row and bordered cells |
| `list` | `items` (string[]), `ordered` (bool) | Bullet list (default) or numbered list |
| `separator` | — | Horizontal line |

**Limits:**

- Maximum content blocks: 500
- Maximum rows per table: 10,000
- Maximum generated file size: 50 MB

**Error responses:**

| Status | Error | Meaning |
|--------|-------|---------|
| 400 | `content is required` | No content blocks provided |
| 400 | `content must be a non-empty array` | Content is not an array or is empty |
| 400 | `content exceeds maximum of 500 blocks` | Too many content blocks |
| 422 | `failed to generate PDF: ...` | PDF generation failed |

---

### generate_excel

Generate an Excel (XLSX) spreadsheet from structured data.

```bash
curl -s -X POST "https://integraclaw.dev/api/v1/action" \
  -H "Authorization: Bearer $INTEGRACLAW_API_KEY" \
  -H "Content-Type: application/json" \
  -d '{
    "provider": "integraclaw",
    "service": "processing",
    "action": "generate_excel",
    "params": {
      "filename": "sales_data.xlsx",
      "sheets": [
        {
          "name": "Q1 Sales",
          "headers": ["Product", "Units", "Revenue"],
          "rows": [
            ["Widget A", 150, 4500.00],
            ["Widget B", 230, 6900.00]
          ]
        },
        {
          "name": "Summary",
          "headers": ["Metric", "Value"],
          "rows": [
            ["Total Units", 380],
            ["Total Revenue", 11400.00]
          ]
        }
      ]
    }
  }'
```

| Parameter | Type | Required | Description |
|-----------|------|----------|-------------|
| `sheets` | array | **yes** | Array of sheet definitions |
| `sheets[].name` | string | no | Sheet name (default: `Sheet1`, `Sheet2`, ...) |
| `sheets[].headers` | string[] | no | Column headers written in bold in row 1 |
| `sheets[].rows` | array[] | no | Data rows. Each row is an array of values (string, number, boolean) |
| `filename` | string | no | Output filename (default: `spreadsheet.xlsx`) |

**Response:**

```json
{
  "success": true,
  "status": 200,
  "data": {
    "file_base64": "UEsDBBQAAAA...",
    "filename": "sales_data.xlsx",
    "mime_type": "application/vnd.openxmlformats-officedocument.spreadsheetml.sheet",
    "size": 8432
  }
}
```

**Limits:**

- Maximum sheets: 100
- Maximum rows per sheet: 100,000
- Maximum columns: 1,000
- Maximum generated file size: 50 MB

**Error responses:**

| Status | Error | Meaning |
|--------|-------|---------|
| 400 | `sheets is required` | No sheets provided |
| 400 | `sheets must be a non-empty array` | Sheets is not an array or is empty |
| 400 | `sheets exceeds maximum of 100` | Too many sheets |
| 422 | `failed to generate Excel: ...` | XLSX generation failed |

---

### generate_docx

Generate a Word (DOCX) document from structured content blocks.

```bash
curl -s -X POST "https://integraclaw.dev/api/v1/action" \
  -H "Authorization: Bearer $INTEGRACLAW_API_KEY" \
  -H "Content-Type: application/json" \
  -d '{
    "provider": "integraclaw",
    "service": "processing",
    "action": "generate_docx",
    "params": {
      "title": "Project Proposal",
      "content": [
        {"type": "heading", "level": 1, "text": "Overview"},
        {"type": "paragraph", "text": "This document outlines the proposed approach."},
        {"type": "heading", "level": 2, "text": "Timeline"},
        {"type": "table", "headers": ["Phase", "Duration", "Status"], "rows": [["Design", "2 weeks", "Complete"], ["Development", "6 weeks", "In Progress"]]},
        {"type": "list", "items": ["Finalize requirements", "Complete testing", "Deploy to production"]}
      ],
      "filename": "proposal.docx"
    }
  }'
```

| Parameter | Type | Required | Description |
|-----------|------|----------|-------------|
| `title` | string | no | Document title displayed as Heading 1 at the top |
| `content` | array | **yes** | Array of content blocks (same format as generate_pdf) |
| `filename` | string | no | Output filename (default: `document.docx`) |

**Response:**

```json
{
  "success": true,
  "status": 200,
  "data": {
    "file_base64": "UEsDBBQAAAA...",
    "filename": "proposal.docx",
    "mime_type": "application/vnd.openxmlformats-officedocument.wordprocessingml.document",
    "size": 5678
  }
}
```

**Content Block Types:** Same as generate_pdf (heading, paragraph, table, list, separator).

**Limits:**

- Maximum content blocks: 500
- Maximum rows per table: 10,000
- Maximum generated file size: 50 MB

**Error responses:**

| Status | Error | Meaning |
|--------|-------|---------|
| 400 | `content is required` | No content blocks provided |
| 400 | `content must be a non-empty array` | Content is not an array or is empty |
| 422 | `failed to generate DOCX: ...` | DOCX generation failed |

---

### extract_data

Extract structured data (typed JSON) from a document file. Unlike `extract_text` which returns plain text, this tool returns structured JSON with typed values.

**Two ways to send the file:**

1. **Multipart upload:**

```bash
curl -s -X POST "https://integraclaw.dev/api/v1/action" \
  -H "Authorization: Bearer $INTEGRACLAW_API_KEY" \
  -F "provider=integraclaw" \
  -F "service=processing" \
  -F "action=extract_data" \
  -F "file=@data.xlsx"
```

2. **JSON with file_url:**

```bash
curl -s -X POST "https://integraclaw.dev/api/v1/action" \
  -H "Authorization: Bearer $INTEGRACLAW_API_KEY" \
  -H "Content-Type: application/json" \
  -d '{
    "provider": "integraclaw",
    "service": "processing",
    "action": "extract_data",
    "params": {
      "file_url": "https://example.com/data.xlsx",
      "sheets": ["Sales", "Summary"]
    }
  }'
```

| Parameter | Type | Required | Description |
|-----------|------|----------|-------------|
| `file` | binary | conditional | The file to extract data from (multipart upload). Either `file` or `file_url` is required |
| `file_url` | string | conditional | URL to the file. Either `file` or `file_url` is required |
| `format` | string | no | Force format detection: `xlsx`, `csv`, `docx`. If omitted, auto-detected |
| `sheets` | string[] | no | Sheet names to extract (XLSX only). If omitted, all sheets are extracted |

**Response (XLSX):**

```json
{
  "success": true,
  "status": 200,
  "data": {
    "format": "xlsx",
    "sheets": [
      {
        "name": "Sales",
        "headers": ["Product", "Units", "Revenue"],
        "rows": [
          ["Widget A", 150, 4500.0],
          ["Widget B", 230, 6900.0]
        ],
        "row_count": 2
      }
    ]
  }
}
```

**Response (CSV):**

```json
{
  "success": true,
  "status": 200,
  "data": {
    "format": "csv",
    "headers": ["Name", "Email", "Active"],
    "rows": [
      ["Alice", "alice@example.com", true],
      ["Bob", "bob@example.com", false]
    ],
    "row_count": 2
  }
}
```

**Response (DOCX):**

```json
{
  "success": true,
  "status": 200,
  "data": {
    "format": "docx",
    "sections": [
      {"type": "heading", "level": 1, "text": "Introduction"},
      {"type": "paragraph", "text": "This document describes..."},
      {"type": "table", "headers": ["Column A", "Column B"], "rows": [["value1", "value2"]]}
    ]
  }
}
```

**Supported formats:**

| Format | Detection | Output |
|--------|-----------|--------|
| XLSX | `.xlsx` extension or ZIP with `xl/` | Sheets with headers and typed rows (numbers, booleans, strings) |
| CSV | `.csv` extension | Headers and typed rows with auto-detected delimiter (comma or tab) |
| DOCX | `.docx` extension or ZIP with `word/` | Sections: headings (with level), paragraphs, tables |

**Value typing (XLSX/CSV):** Cell values are automatically converted: numbers become `float64`, `"true"`/`"false"` become `bool`, everything else stays `string`. The first row is used as headers if all non-empty cells look like strings.

**Limits:**

- Maximum file size: 50 MB
- Maximum rows per sheet: 100,000
- Maximum sheets: 100
- Only `http` and `https` URLs are allowed (for `file_url`)

**Error responses:**

| Status | Error | Meaning |
|--------|-------|---------|
| 400 | `a file is required` | Neither file upload nor file_url was provided |
| 400 | `could not detect file format` | Format not recognized — provide the `format` parameter |
| 400 | `unsupported format for data extraction` | Format is not xlsx, csv, or docx |
| 422 | `failed to extract data: ...` | File is corrupted or unreadable |
| 502 | `failed to download file: ...` | Could not download from the provided URL |
