---
name: consultation-triage
description: Search and triage Dr Souphi consultations using the Souphi MCP booking tools. Use when an operator needs to find a booking, inspect current state, or narrow operational status before taking action.
mcp_tools:
  - bookings.search
  - bookings.get
  - bookings.getWorkflowState
---

# Consultation Triage

Use the Souphi MCP tools as the source of truth for consultation lookup and triage.

## Default workflow

1. Start with `bookings.search`.
2. If you have a strong identifier, include it:
   - `bookingId` only if the user explicitly gave it or a prior tool returned it
   - exact `patientEmail` (or plain `email`, which is accepted as an alias) only if the user explicitly gave the patient email
   - otherwise supported status/date filters
3. If you do not yet have a strong identifier, call `bookings.search` with no arguments first to load the recent operator inbox, then refine from there.
4. Never infer patient identifiers from the authenticated operator identity.
5. If a single booking is identified, load `bookings.get`.
6. If the problem sounds operational, also load `bookings.getWorkflowState`.

## Triage rules

- keep list results PII-minimal unless the operator explicitly needs detail
- use `bookings.get` before quoting full patient email or phone information
- distinguish customer-facing booking status from workflow state
- if the issue is cancellation, reschedule, refund, or a stuck flow, transition to the relevant Souphi MCP action rather than speculating

## Relevant tools

- `bookings.search`
- `bookings.get`
- `bookings.getWorkflowState`
