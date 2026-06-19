# BPEL2Orkes

<!-- mcp-name: io.github.kshetra-studio/bpel2orkes -->

**Migrate IBM WS-BPEL 2.0 workflows to Orkes Conductor — automatically.**

`bpel2orkes` is a migration accelerator that converts IBM WS-BPEL 2.0 process definitions into Orkes Conductor workflow JSON. What takes weeks manually takes minutes.

Live tool → [bpel2orkes.kshetra.studio](https://bpel2orkes.kshetra.studio)

**MCP Registry:** [registry.modelcontextprotocol.io/servers/io.github.kshetra-studio/bpel2orkes](https://registry.modelcontextprotocol.io/servers/io.github.kshetra-studio/bpel2orkes)

---

## The Problem

Organisations running IBM BPM, TIBCO, or legacy BPEL-based integration platforms face a common challenge: migrating to modern workflow orchestration is expensive, risky, and slow. The primary reason is manual — engineers read BPEL XML and hand-translate it into the target platform's constructs, one workflow at a time.

At scale (hundreds of BPEL processes, common in banks and insurers), this is a multi-year programme with significant incumbent vendor lock-in.

---

## What BPEL2Orkes Does

- Parses IBM WS-BPEL 2.0 XML process definitions
- Maps BPEL constructs to Orkes Conductor workflow primitives
- Generates deployment-ready Orkes Conductor JSON
- Produces a visual flow diagram of the migrated workflow
- Provides a migration assessment (complexity score, manual review flags)
- Supports direct deploy to Orkes Cloud

### Supported BPEL Constructs

| BPEL Construct | Orkes Conductor Equivalent |
|---|---|
| `<sequence>` | Sequential task chain |
| `<flow>` | Fork/Join parallel tasks |
| `<switch>` / `<if>` | Switch task |
| `<while>` / `<repeatUntil>` | Do-While task |
| `<invoke>` | HTTP / gRPC worker task |
| `<receive>` / `<reply>` | Wait / Event listener task |
| `<assign>` | Set Variable task |
| `<throw>` / `<catch>` | Terminate + error handling |
| `<compensate>` | Compensation workflow |
| `<scope>` | Sub-workflow |

---

## Who Is This For

**Engineers** decommissioning IBM BPM, WebSphere Process Server, or TIBCO who need to migrate BPEL process definitions to Orkes Conductor.

**Architects** evaluating Orkes Conductor as the target platform for workflow modernisation — use this to generate a migration complexity assessment across your BPEL inventory.

**Programme Managers** running UNITE-style legacy decommission programmes who need to de-risk the workflow migration workstream and demonstrate accelerated delivery.

---

## Quick Start

### Use the Live Tool
Upload your `.bpel` file at [bpel2orkes.kshetra.studio](https://bpel2orkes.kshetra.studio) — no signup required for evaluation.

### Use the API
See [api.md](./api.md) for the full API reference with Swagger UI at [bpel2orkes.kshetra.studio/api/docs](https://bpel2orkes.kshetra.studio/api/docs).

```bash
curl -X POST https://bpel2orkes.kshetra.studio/api/v1/convert \
  -H "Content-Type: application/xml" \
  -H "X-Api-Key: your-api-key" \
  --data-binary @your-process.bpel
```

### Use the MCP Server
Connect BPEL2Orkes to Claude, Cursor, Windsurf, or any MCP-compatible AI coding assistant.

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

---

## Examples

See the [examples/](./examples/) folder for:
- Sample BPEL input files (loan approval, payment processing, document archival)
- Corresponding Orkes Conductor JSON output
- Migration assessment reports

---

## Migration Patterns

### IBM BPM → Orkes
IBM BPM processes typically export as BPEL. BPEL2Orkes handles the conversion; Orkes workers replace IBM BPM human tasks and service integrations.

### TIBCO → Orkes
TIBCO BusinessWorks processes can be exported to BPEL format for conversion. Contact us for the TIBCO export guide.

### WebSphere Process Server → Orkes
WPS BPEL processes are fully supported. Compensation and long-running transaction patterns are handled.

---

## Commercial Use

BPEL2Orkes is free for evaluation and non-commercial use.

For commercial engagements — including enterprise migration programmes, SOW-based delivery, and managed migration services — contact [Kshetra](https://kshetra.studio) or email [hello@kshetra.studio](mailto:hello@kshetra.studio).

Kshetra partners with DXC Technology for enterprise delivery across ANZ financial services.

---

## About

Built by [Kshetra](https://kshetra.studio) — an AI and workflow engineering practice specialising in legacy modernisation for regulated industries.

- Website: [kshetra.studio](https://kshetra.studio)
- Tool: [bpel2orkes.kshetra.studio](https://bpel2orkes.kshetra.studio)
- LinkedIn: [Dinesh Singh Panwar](https://www.linkedin.com/in/dinesh-panwar-2734471a/)

---

## Related

- [Orkes Conductor Documentation](https://orkes.io/content)
- [IBM WS-BPEL 2.0 Specification](https://docs.oasis-open.org/wsbpel/2.0/OS/wsbpel-v2.0-OS.html)
- [askmybank.ai](https://askmybank.ai) — AI chat over archived banking documents, built on ClickHouse Cloud
