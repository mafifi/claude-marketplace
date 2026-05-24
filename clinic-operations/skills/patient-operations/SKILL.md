---
name: patient-operations
description: Create, find, and follow up with Dr Souphi patients through the Souphi MCP patient tools. Use when clinic staff ask Claude to add a patient, inspect missing patient items, or send patient-facing access links or invoices.
mcp_tools:
  - patients.search
  - patients.get
  - patients.create
  - patients.getOutstandingItems
  - patients.previewSendAccessRequest
  - patients.sendAccessRequest
  - patients.previewSendInvoice
  - patients.sendInvoice
---

# Patient Operations

Use the patient tools when the operator needs to find a patient, create a patient, inspect missing items, or send patient-facing requests.

## Search and create

1. Confirm the tenant and include `tenantId` in every tool call.
2. Start with `patients.search` using explicit user input or a prior tool result.
3. Never call `patients.search` without `patientId`, `email`, or a name of at least 2 characters.
4. Before `patients.create`, search by exact email when available.
5. If `patients.create` reports an exact email duplicate, stop and use the returned patient.
6. If deterministic name candidates are returned, review them and retry with `confirmedNoMatch: true` only after the operator confirms none match.

## Outstanding items

Use `patients.getOutstandingItems` before sending requests. Treat items as complete only under these rules:

- details are complete when submitted or verified for review
- medical history is complete when completed or signed and reusable
- consent is complete when signed
- invoice is complete when no current outstanding invoice remains

## Patient-facing sends

Patient-facing sends are preview-first.

1. Run the matching preview tool.
2. Show the patient name, masked patient email, tenantId, and item identity.
3. Ask for explicit confirmation before sending.
4. Execute only with `operatorConfirmed: true`, patientId, item identity, an `operatorReason`, and the preview artifact.
5. If execute reports drift, preview again before attempting another send.
