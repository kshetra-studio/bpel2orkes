# BPEL2Orkes API Reference

Base URL: `https://bpel2orkes.kshetra.studio`

## Authentication

All API and MCP requests require an API key obtained after GitHub login at [bpel2orkes.kshetra.studio](https://bpel2orkes.kshetra.studio).

- **Free tier:** 50 conversions included
- **Additional credits:** Available for purchase via the platform

Pass your API key in the request header:
```
X-Api-Key: your_api_key
```

---

## Endpoints

### POST /api/convert

Convert a BPEL 2.0 XML process definition to Orkes Conductor workflow JSON.

**Request**
```bash
curl -X POST https://bpel2orkes.kshetra.studio/api/convert \
  -H "Content-Type: application/xml" \
  -H "X-Api-Key: your_api_key" \
  --data-binary @your-process.bpel
```

**Response**
```json
{
  "workflow": { ... },
  "assessment": {
    "complexity": "green | amber | red",
    "flags": [ ... ],
    "manual_review_required": false
  },
  "diagram_url": "https://bpel2orkes.kshetra.studio/diagram/...",
  "credits_remaining": 48
}
```

**Response fields**

| Field | Description |
|---|---|
| `workflow` | Deployment-ready Orkes Conductor workflow JSON |
| `assessment.complexity` | Green = auto-deploy ready, Amber = review recommended, Red = manual intervention required |
| `assessment.flags` | List of constructs that need manual review |
| `diagram_url` | Link to visual flow diagram of the converted workflow |
| `credits_remaining` | Remaining conversion credits on your account |

---

### UI Converter

No API key required for the web interface.

→ [bpel2orkes.kshetra.studio](https://bpel2orkes.kshetra.studio)

Upload your `.bpel` file, get instant Orkes Conductor JSON, visual diagram, and migration assessment. Export as PDF. One-click deploy to Orkes Developer Cloud.

---

## MCP Server

Connect BPEL2Orkes directly to Claude, Cursor, Windsurf, or any MCP-compatible AI coding assistant.

See [mcp/README.md](./mcp/README.md) for setup instructions.

---

## Rate Limits

| Tier | Conversions | Rate limit |
|---|---|---|
| Free | 50 total | 10/hour |
| Paid | Per credits purchased | 60/hour |

---

## Support

- Email: hello@kshetra.studio
- Website: [kshetra.studio](https://kshetra.studio)
