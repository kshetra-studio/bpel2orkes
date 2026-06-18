# Examples

Ready-to-use BPEL sample files are built into the live tool — no download needed.

## Try them now

→ [bpel2orkes.kshetra.studio](https://bpel2orkes.kshetra.studio)

Select any sample directly in the UI and convert instantly. No signup required for evaluation.

## Available samples

| Sample | Use case | BPEL constructs | Complexity |
|---|---|---|---|
| `loan_approval` | Retail lending origination | sequence, invoke, if/elseif/else, receive, reply | 🟢 Green |
| `credit_card_provisioning` | Card issuance + KYC gate | sequence, flow (parallel), if/else, invoke | 🟢 Green |
| `income_verification` | Mortgage serviceability | sequence, flow, scope, compensationHandler | 🟡 Amber |
| `communication` | Multi-channel customer notification | sequence, invoke, if/else, while | 🟢 Green |

## Use via API

```bash
curl -X POST https://bpel2orkes.kshetra.studio/api/convert \
  -H "Content-Type: application/xml" \
  -H "X-Api-Key: your_api_key" \
  --data-binary @your-process.bpel
```

## Use via MCP in Claude

```
Convert this BPEL process to Orkes Conductor: [paste your BPEL XML]
```

## Bring your own

Your real BPEL files work directly — upload in the UI or via API. For complex patterns (BPEL4People, WS-HumanTask, proprietary IBM extensions) contact [hello@kshetra.studio](mailto:hello@kshetra.studio).
