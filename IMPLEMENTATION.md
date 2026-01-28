# Ray Agent Skills Implementation Plan

## Overview

Create agent skills that allow Claude Code to send payloads to Ray's local HTTP server. Starting with 4 basic payload types: Log, Text, Table, and JSON.

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

```json
{
  "type": "custom",
  "content": {
    "content": "HTML-escaped text (spaces→&nbsp; newlines→<br>)",
    "label": "HTML"
  },
  "origin": {...}
}
```

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

```json
{
  "type": "json_string",
  "content": {
    "value": "{\"serialized\": \"json\"}"
  },
  "origin": {...}
}
```

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

### 5. Create `skills/ray/rules/json.md`

Document JsonStringPayload:

- When to use (structured JSON display)
- Payload structure with examples
- Bash/curl example

### 6. Update `skills/ray/SKILL.md`

Add links to new rule files.

### 7. Remove `skills/ray/rules/application-log.md`

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

## Verification

1. Start Ray desktop app
2. Run one of the curl examples above
3. Verify the payload appears in Ray UI
