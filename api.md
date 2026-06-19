# BPEL2Orkes API Reference

Base URL: `https://bpel2orkes.kshetra.studio`

## Interactive API Explorer

- **Swagger UI:** [bpel2orkes.kshetra.studio/api/docs](https://bpel2orkes.kshetra.studio/api/docs) — try every endpoint directly in the browser
- **OpenAPI 3.0 spec:** [bpel2orkes.kshetra.studio/openapi.json](https://bpel2orkes.kshetra.studio/openapi.json) — machine-readable spec for code generation

---

## Authentication

All conversion and deployment endpoints require an API key. Get one by signing in with GitHub at [bpel2orkes.kshetra.studio](https://bpel2orkes.kshetra.studio) — 50 free conversions included, no credit card required.

Pass your key in the request header:

```
X-Api-Key: your_api_key_here
```

Diagnostic endpoints (`/api/v1/parse`, `/api/v1/health`, `/api/v1/version`) are public and require no key.

---

## Endpoints

### POST /api/v1/convert _(deducts 1 credit)_

Convert a WS-BPEL 2.0 XML process to a complete Orkes Conductor workflow bundle. Returns the main workflow, sub-workflows, compensation flows, and fault handler flows.

```bash
curl -X POST https://bpel2orkes.kshetra.studio/api/v1/convert \
  -H "Content-Type: application/xml" \
  -H "X-Api-Key: your_api_key" \
  --data-binary @your-process.bpel
```

**Response**
```json
{
  "durationMs": 14.2,
  "warningCount": 2,
  "workflowCount": 3,
  "bundle": {
    "mainWorkflow": { "name": "LoanApprovalProcess", "version": 1, "tasks": [ ... ] },
    "subWorkflows": [ ... ],
    "compensationFlows": [ ... ],
    "faultHandlerFlows": [ ... ],
    "warnings": [ "SIMPLE task 'manualReview' requires a worker implementation", ... ]
  }
}
```

---

### POST /api/v1/convert/file _(deducts 1 credit)_

Same as `/api/v1/convert` but accepts a multipart file upload. Useful in CI pipelines.

```bash
curl -X POST https://bpel2orkes.kshetra.studio/api/v1/convert/file \
  -H "X-Api-Key: your_api_key" \
  -F "file=@your-process.bpel"
```

---

### POST /api/v1/convert/clean _(no credit cost)_

Returns a flattened response with workflows at the top level. Authentication optional.

```bash
curl -X POST https://bpel2orkes.kshetra.studio/api/v1/convert/clean \
  -H "Content-Type: application/xml" \
  -H "X-Api-Key: your_api_key" \
  --data-binary @your-process.bpel
```

---

### POST /api/v1/convert/diagram _(no credit cost)_

Returns a [Mermaid.js](https://mermaid.js.org) flowchart and a migration complexity summary (auto-converted vs. needs-work counts). Authentication optional. Rate limit: 30 req/min per IP for unauthenticated callers.

```bash
curl -X POST https://bpel2orkes.kshetra.studio/api/v1/convert/diagram \
  -H "Content-Type: application/xml" \
  --data-binary @your-process.bpel
```

**Response**
```json
{
  "mermaid": "flowchart TD
  start([Start]) --> task_1[HTTP: getCustomer]
 ...",
  "summary": {
    "total": 8,
    "autoConverted": 6,
    "needsWork": 2,
    "subWorkflows": 1,
    "warningCount": 2
  },
  "warnings": [ ... ]
}
```

---

### POST /api/v1/validate _(deducts 1 credit)_

Converts the BPEL and registers the main workflow on your Orkes Conductor instance via `PUT /api/metadata/workflow`.

```bash
curl -X POST https://bpel2orkes.kshetra.studio/api/v1/validate \
  -H "Content-Type: application/xml" \
  -H "X-Api-Key: your_api_key" \
  -H "X-Orkes-Key-Id: your_orkes_key_id" \
  -H "X-Orkes-Key-Secret: your_orkes_key_secret" \
  --data-binary @your-process.bpel
```

Optionally override the Orkes cluster URL:

```
-H "X-Orkes-Base-Url: https://your-cluster.orkescloud.com"
```

**Response**
```json
{
  "durationMs": 340.1,
  "orkesOk": true,
  "orkesStatus": 200,
  "workflowName": "LoanApprovalProcess",
  "bundle": { ... }
}
```

---

### POST /api/v1/parse _(public, no credit cost)_

Runs the BPEL parser and returns the internal AST. Useful for debugging parse failures. Rate limit: 20 req/min per IP.

```bash
curl -X POST https://bpel2orkes.kshetra.studio/api/v1/parse \
  -H "Content-Type: application/xml" \
  --data-binary @your-process.bpel
```

---

### GET /api/v1/health

Liveness check. Returns `{ "status": "ok", "env": "production", "version": "0.2.0" }`.

### GET /api/v1/version

Returns supported BPEL versions and IBM extensions.

---

## Credit System

| Event | Credits |
|---|---|
| Account creation (GitHub sign-in) | +50 free |
| `/api/v1/convert` | −1 |
| `/api/v1/convert/file` | −1 |
| `/api/v1/validate` | −1 |
| `/api/v1/convert/clean` | 0 |
| `/api/v1/convert/diagram` | 0 |

Credits never expire. Top up from your [dashboard](https://bpel2orkes.kshetra.studio/dashboard).

On credit exhaustion, conversion endpoints return HTTP 429:

```json
{
  "detail": {
    "error": "quota_exceeded",
    "creditBalanceCents": 0,
    "conversionsRemaining": 0,
    "topUpUrl": "https://bpel2orkes.kshetra.studio/dashboard"
  }
}
```

---

## MCP Integration

BPEL2Orkes is available as a Model Context Protocol server, letting AI assistants like Claude drive your BPEL migrations.

**Claude Code CLI:**
```bash
claude mcp add --transport http bpel2orkes \
  https://bpel2orkes.kshetra.studio/mcp/ \
  --header "X-Api-Key: your_api_key"
```

**Claude Desktop (`claude_desktop_config.json`):**
```json
{
  "mcpServers": {
    "bpel2orkes": {
      "command": "npx",
      "args": ["mcp-remote", "https://bpel2orkes.kshetra.studio/mcp/",
               "--header", "X-Api-Key:your_api_key"]
    }
  }
}
```

MCP tools: `convert_bpel`, `get_migration_tips`, `validate_on_orkes`

---

## llms.txt

Machine-readable description for AI assistants: [bpel2orkes.kshetra.studio/llms.txt](https://bpel2orkes.kshetra.studio/llms.txt)
