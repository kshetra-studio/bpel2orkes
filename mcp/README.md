# BPEL2Orkes MCP Server

Connect BPEL2Orkes to Claude, Cursor, Windsurf, or any MCP-compatible AI coding assistant.

## What this enables

Once connected, you can ask your AI assistant directly:
- *"Convert this BPEL file to Orkes Conductor"*
- *"Assess the migration complexity of my BPEL process"*
- *"Generate an Orkes workflow JSON from this IBM BPM export"*

The assistant calls BPEL2Orkes automatically and returns the converted workflow, visual diagram, and migration assessment inline in your conversation.

---

## Setup

### Prerequisites
- GitHub account (for API key)
- Sign in at [bpel2orkes.kshetra.studio](https://bpel2orkes.kshetra.studio) to get your API key
- 50 free conversions included

---

### Claude (Claude Code / claude.ai)

```bash
claude mcp add --transport http bpel2orkes https://bpel2orkes.kshetra.studio/mcp/ --header "X-Api-Key: your_api_key"
```

Replace `your_api_key` with your key from the platform.

---

### Cursor

Add to your `~/.cursor/mcp.json`:

```json
{
  "mcpServers": {
    "bpel2orkes": {
      "url": "https://bpel2orkes.kshetra.studio/mcp/",
      "headers": {
        "X-Api-Key": "your_api_key"
      }
    }
  }
}
```

---

### Windsurf

Add to your Windsurf MCP config:

```json
{
  "mcpServers": {
    "bpel2orkes": {
      "url": "https://bpel2orkes.kshetra.studio/mcp/",
      "headers": {
        "X-Api-Key": "your_api_key"
      }
    }
  }
}
```

---

## Example Usage

Once connected, in any MCP-compatible assistant:

```
Convert this BPEL process to Orkes Conductor:

<process name="LoanApproval" ...>
  ...
</process>
```

The assistant will:
1. Send your BPEL to the conversion engine
2. Return deployment-ready Orkes Conductor workflow JSON
3. Provide a migration complexity assessment (green/amber/red)
4. Link to a visual flow diagram

---

## Credits

Each conversion uses one credit. Free tier includes 50 conversions.
Purchase additional credits at [bpel2orkes.kshetra.studio](https://bpel2orkes.kshetra.studio).

---

## Support

- Email: hello@kshetra.studio
- Website: [kshetra.studio](https://kshetra.studio)
