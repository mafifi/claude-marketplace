---
name: workflow-recovery
description: Recover stuck Dr Souphi booking workflows through the Souphi MCP recovery tools. Use when a booking appears operationally stuck, waiting on an external event, or has failed after partial progress.
mcp_tools:
  - bookings.get
  - bookings.getWorkflowState
  - bookings.recoverConfirmation
  - bookings.recoverModification
  - bookings.recoverCancellation
---

# Workflow Recovery

Treat recovery as a workflow operation, not a manual state repair.

## Default workflow

1. Load `bookings.get`.
2. Load `bookings.getWorkflowState`.
3. Identify which workflow is stuck:
   - confirmation
   - modification
   - cancellation
4. Use the matching recovery tool with an explicit operator reason:
   - `bookings.recoverConfirmation`
   - `bookings.recoverModification`
   - `bookings.recoverCancellation`

## Recovery rules

- do not invent manual database edits
- do not treat email retry as the first move when the workflow state itself is unclear
- keep the operator reason concrete and tied to the observed issue
- explain whether the workflow is failed versus awaiting an external event before recovering it

## Relevant tools

- `bookings.get`
- `bookings.getWorkflowState`
- `bookings.recoverConfirmation`
- `bookings.recoverModification`
- `bookings.recoverCancellation`
