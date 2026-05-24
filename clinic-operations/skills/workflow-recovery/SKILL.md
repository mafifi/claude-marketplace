---
name: workflow-recovery
description: Recover stuck Dr Souphi booking workflows through the Souphi MCP recovery tools. Use when a booking appears operationally stuck, waiting on an external event, or has failed after partial progress.
mcp_tools:
  - bookings.get
  - bookings.getWorkflowState
  - bookings.previewWorkflowRecovery
  - bookings.recoverConfirmation
  - bookings.recoverModification
  - bookings.recoverCancellation
---

# Workflow Recovery

Treat recovery as a workflow operation, not a manual state repair.

## Default workflow

1. Confirm the tenant and include `tenantId` in every tool call.
2. Load `bookings.get`.
3. Load `bookings.getWorkflowState`.
4. Identify which workflow is stuck:
   - confirmation
   - modification
   - cancellation
5. Run `bookings.previewWorkflowRecovery` for that workflow type.
6. Use the matching recovery tool with an explicit operator reason and the preview artifact:
   - `bookings.recoverConfirmation`
   - `bookings.recoverModification`
   - `bookings.recoverCancellation`

## Recovery rules

- do not invent manual database edits
- `tenantId` is a session safety echo, not a selector; switching tenant means switching session
- execute recovery only with the preview artifact from `bookings.previewWorkflowRecovery`
- do not treat email retry as the first move when the workflow state itself is unclear
- keep the operator reason concrete and tied to the observed issue
- explain whether the workflow is failed versus awaiting an external event before recovering it

## Relevant tools

- `bookings.get`
- `bookings.getWorkflowState`
- `bookings.previewWorkflowRecovery`
- `bookings.recoverConfirmation`
- `bookings.recoverModification`
- `bookings.recoverCancellation`
