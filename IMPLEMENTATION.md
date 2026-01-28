# Ray Agent Skills Implementation Plan

## Overview

Create agent skills that allow Claude Code to send payloads to Ray's local HTTP server. Supporting 11 payload types: Log, Text, Table, JsonString, Html, Xml, Carbon, Custom, DecodedJson, and FileContents.

## HTTP API Reference

**Documentation**: https://myray.app/docs/developing-ray-libraries/payload

**Endpoint**: `POST http://localhost:23517/`

**Headers**:

- `Content-Type: application/json`
- `Accept: application/json`

**Request body**:

```json
{
  "uuid": "<unique-id>",
  "payloads": [
    {
      "type": "<payload-type>",
      "content": { /* payload-specific */ },
      "origin": {
        "function_name": "<function-name>",
        "file": "<file-path>",
        "line_number": <number>,
        "hostname": "<hostname>"
      }
    }
  ],
  "meta": {}
}
```

### Request Structure Details

All sections are **mandatory** when creating payloads.

#### uuid

A valid UUIDv4 value. This identifier is important for future modifications to the payload, such as changing its color after transmission. Use `$(uuidgen)` in bash to generate one.

#### payloads

An array containing the actual data being sent. Each element includes:

- `type`: Specifies the kind of payload (e.g., `log`, `custom`, `table`, `json_string`)
- `content`: Holds payload-specific data for display in Ray, varying by type
- `origin`: Provides contextual information about the call's source

#### origin

Captures where the ray() call originated. Ray uses this information to enable file linking functionality within the desktop app.

- `function_name`: The calling function (use `"code-agent"` or similar identifier)
- `file`: The file path where the call occurred
- `line_number`: The specific line number
- `hostname`: The machine's hostname

#### meta

Metadata about the integration library. Can contain language version and Ray package version details. For Claude agent use, can be empty `{}`.

## Payload Structures

### 1. LogPayload (type: `log`)

```json
{
  "type": "log",
  "content": {
    "values": ["value1", "value2"],
    "meta": [{"clipboard_data": "plain text for clipboard"}]
  },
  "origin": {...}
}
```

### 2. TextPayload (type: `custom`)

For plain text with preserved whitespace formatting.

```json
{
  "type": "custom",
  "content": {
    "content": "HTML-escaped text (spaces→&nbsp; newlines→<br>)",
    "label": "Text"
  },
  "origin": {...}
}
```

**Formatting rules:**

- HTML-escape the text using `htmlspecialchars()` equivalent
- Replace spaces with `&nbsp;`
- Replace newlines with `<br>`

### 3. TablePayload (type: `table`)

```json
{
  "type": "table",
  "content": {
    "values": {"key1": "value1", "key2": "value2"},
    "label": "Table"
  },
  "origin": {...}
}
```

### 4. JsonStringPayload (type: `json_string`)

For displaying a serialized JSON string with syntax highlighting.

```json
{
  "type": "json_string",
  "content": {
    "value": "{\"serialized\": \"json\"}"
  },
  "origin": {...}
}
```

### 5. HtmlPayload (type: `custom`)

For displaying raw HTML content. No escaping is applied.

```json
{
  "type": "custom",
  "content": {
    "content": "<h1>Hello World</h1><p>This is <strong>HTML</strong></p>",
    "label": "HTML"
  },
  "origin": {...}
}
```

### 6. XmlPayload (type: `custom`)

For displaying formatted XML with syntax highlighting.

```json
{
  "type": "custom",
  "content": {
    "content": "&lt;one&gt;<br>&nbsp;&nbsp;&lt;two&gt;<br>&nbsp;&nbsp;&nbsp;&nbsp;&lt;three&gt;3&lt;/three&gt;<br>&nbsp;&nbsp;&lt;/two&gt;<br>&lt;/one&gt;",
    "label": "XML"
  },
  "origin": {...}
}
```

**Formatting rules:**

- Pretty-print/indent the XML (if possible)
- HTML-encode all entities using `htmlentities()` equivalent
- Replace newlines with `<br>`
- Replace spaces with `&nbsp;`

### 7. CarbonPayload (type: `carbon`)

For displaying date/time information. Uses a dedicated `carbon` type.

```json
{
  "type": "carbon",
  "content": {
    "formatted": "2024-01-15 14:30:00",
    "timestamp": 1705329000,
    "timezone": "UTC"
  },
  "origin": {...}
}
```

**Fields:**

- `formatted`: Date string in specified format (default: `Y-m-d H:i:s`)
- `timestamp`: Unix timestamp (integer)
- `timezone`: Timezone name string

### 8. CustomPayload (type: `custom`)

For displaying arbitrary content with a custom label. No formatting applied.

```json
{
  "type": "custom",
  "content": {
    "content": "my custom content",
    "label": "Custom Label"
  },
  "origin": {...}
}
```

### 9. DecodedJsonPayload (type: `custom`)

For displaying decoded/parsed JSON as a structured view. The JSON is decoded and converted to primitive types.

```json
{
  "type": "custom",
  "content": {
    "content": {"key": "value", "nested": {"a": 1}},
    "label": ""
  },
  "origin": {...}
}
```

**Note:** The `content` field contains the decoded JSON value (object/array), not a string. Label is always empty.

### 10. FileContentsPayload (type: `custom`)

For displaying the contents of a file. The label shows the filename.

```json
{
  "type": "custom",
  "content": {
    "content": "file contents here (HTML-encoded, newlines→&lt;br /&gt;)",
    "label": "filename.txt"
  },
  "origin": {...}
}
```

**Formatting rules:**

- HTML-encode the content using `htmlentities()` equivalent
- Replace newlines with `<br />`
- Label is the basename of the file (e.g., `config.json` for `/path/to/config.json`)
- If file not found, content is `"File not found: '/path/to/file'"` with label `"File"`

## Files to Create/Update

### 0. Create `IMPLEMENTATION.md` in ray-skills root

Copy this implementation plan to the repository for reference.

### 1. Update `skills/ray/rules/ray-local-http.md`

Document the HTTP API details:

- Endpoint, headers, request structure
- UUID generation (any unique string)
- Origin object requirements

### 2. Update `skills/ray/rules/log.md`

Document LogPayload:

- When to use (general value logging)
- Payload structure with examples
- Bash/curl example for sending

### 3. Create `skills/ray/rules/text.md`

Document TextPayload:

- When to use (plain text with formatting preserved)
- HTML escaping requirements
- Bash/curl example

### 4. Create `skills/ray/rules/table.md`

Document TablePayload:

- When to use (key-value data display)
- Payload structure with examples
- Bash/curl example

### 5. Create `skills/ray/rules/json-string.md`

Document JsonStringPayload:

- When to use (serialized JSON display with syntax highlighting)
- Payload structure with examples
- Bash/curl example

### 6. Create `skills/ray/rules/html.md`

Document HtmlPayload:

- When to use (raw HTML display)
- Payload structure with examples
- Bash/curl example

### 7. Create `skills/ray/rules/xml.md`

Document XmlPayload:

- When to use (formatted XML display)
- Formatting rules (pretty-print, HTML-encode, newlines→`<br>`, spaces→`&nbsp;`)
- Bash/curl example

### 8. Create `skills/ray/rules/carbon.md`

Document CarbonPayload:

- When to use (date/time display)
- Content fields: `formatted`, `timestamp`, `timezone`
- Bash/curl example

### 9. Create `skills/ray/rules/custom.md`

Document CustomPayload:

- When to use (arbitrary content with custom label)
- Payload structure with examples
- Bash/curl example

### 10. Create `skills/ray/rules/decoded-json.md`

Document DecodedJsonPayload:

- When to use (structured JSON view)
- Note about decoded content (not string)
- Bash/curl example

### 11. Create `skills/ray/rules/file-contents.md`

Document FileContentsPayload:

- When to use (displaying file contents)
- Formatting rules (HTML-encode, newlines→`<br />`)
- Label shows filename (basename)
- Bash/curl example

### 12. Update `skills/ray/SKILL.md`

Add links to all rule files.

### 13. Remove `skills/ray/rules/application-log.md`

Not needed for basic implementation (can add later).

## Example curl Commands

Each rule file will include a ready-to-use curl example. The agent will use Bash with curl to send payloads.

**Log example**:

```bash
curl -X POST http://localhost:23517/ \
  -H "Content-Type: application/json" \
  -d '{"uuid":"'$(uuidgen)'","payloads":[{"type":"log","content":{"values":["Hello from Claude"],"meta":[]},"origin":{"function_name":"code-agent","file":"code-agent","line_number":1,"hostname":"my-computer"}}],"meta":{}}'
```

**Table example**:

```bash
curl -X POST http://localhost:23517/ \
  -H "Content-Type: application/json" \
  -d '{"uuid":"'$(uuidgen)'","payloads":[{"type":"table","content":{"values":{"status":"success","items":3},"label":"Results"},"origin":{"function_name":"code-agent","file":"code-agent","line_number":1,"hostname":"my-computer"}}],"meta":{}}'
```

**Html example**:

```bash
curl -X POST http://localhost:23517/ \
  -H "Content-Type: application/json" \
  -d '{"uuid":"'$(uuidgen)'","payloads":[{"type":"custom","content":{"content":"<h1>Hello</h1><p>This is <strong>HTML</strong></p>","label":"HTML"},"origin":{"function_name":"code-agent","file":"code-agent","line_number":1,"hostname":"my-computer"}}],"meta":{}}'
```

**Text example**:

```bash
curl -X POST http://localhost:23517/ \
  -H "Content-Type: application/json" \
  -d '{"uuid":"'$(uuidgen)'","payloads":[{"type":"custom","content":{"content":"Hello&nbsp;World<br>Line&nbsp;two","label":"Text"},"origin":{"function_name":"code-agent","file":"code-agent","line_number":1,"hostname":"my-computer"}}],"meta":{}}'
```

**Xml example**:

```bash
curl -X POST http://localhost:23517/ \
  -H "Content-Type: application/json" \
  -d '{"uuid":"'$(uuidgen)'","payloads":[{"type":"custom","content":{"content":"&lt;root&gt;<br>&nbsp;&nbsp;&lt;item&gt;value&lt;/item&gt;<br>&lt;/root&gt;","label":"XML"},"origin":{"function_name":"code-agent","file":"code-agent","line_number":1,"hostname":"my-computer"}}],"meta":{}}'
```

**Carbon example**:

```bash
curl -X POST http://localhost:23517/ \
  -H "Content-Type: application/json" \
  -d '{"uuid":"'$(uuidgen)'","payloads":[{"type":"carbon","content":{"formatted":"2024-01-15 14:30:00","timestamp":1705329000,"timezone":"UTC"},"origin":{"function_name":"code-agent","file":"code-agent","line_number":1,"hostname":"my-computer"}}],"meta":{}}'
```

**Custom example**:

```bash
curl -X POST http://localhost:23517/ \
  -H "Content-Type: application/json" \
  -d '{"uuid":"'$(uuidgen)'","payloads":[{"type":"custom","content":{"content":"my custom content","label":"My Label"},"origin":{"function_name":"code-agent","file":"code-agent","line_number":1,"hostname":"my-computer"}}],"meta":{}}'
```

**JsonString example**:

```bash
curl -X POST http://localhost:23517/ \
  -H "Content-Type: application/json" \
  -d '{"uuid":"'$(uuidgen)'","payloads":[{"type":"json_string","content":{"value":"{\"name\":\"Claude\",\"type\":\"agent\"}"},"origin":{"function_name":"code-agent","file":"code-agent","line_number":1,"hostname":"my-computer"}}],"meta":{}}'
```

**DecodedJson example**:

```bash
curl -X POST http://localhost:23517/ \
  -H "Content-Type: application/json" \
  -d '{"uuid":"'$(uuidgen)'","payloads":[{"type":"custom","content":{"content":{"name":"Claude","nested":{"a":1,"b":2}},"label":""},"origin":{"function_name":"code-agent","file":"code-agent","line_number":1,"hostname":"my-computer"}}],"meta":{}}'
```

**FileContents example**:

```bash
curl -X POST http://localhost:23517/ \
  -H "Content-Type: application/json" \
  -d '{"uuid":"'$(uuidgen)'","payloads":[{"type":"custom","content":{"content":"line 1<br />line 2<br />line 3","label":"example.txt"},"origin":{"function_name":"code-agent","file":"code-agent","line_number":1,"hostname":"my-computer"}}],"meta":{}}'
```

## Verification

1. Start Ray desktop app
2. Run one of the curl examples above
3. Verify the payload appears in Ray UI
