---
name: admin-booking-changes
description: Handle admin cancellation and reschedule for Dr Souphi consultations using the Souphi MCP preview and execute tools. Use when clinic staff need to override self-service constraints while keeping workflow invariants intact.
mcp_tools:
  - bookings.get
  - bookings.getMonthAvailability
  - bookings.getDayAvailability
  - bookings.previewAdminReschedule
  - bookings.adminReschedule
  - bookings.previewAdminCancel
  - bookings.adminCancel
---

# Admin Booking Changes

Admin cancellation and reschedule must stay preview-first.

## Reschedule workflow

1. Confirm the tenant and include `tenantId` in every tool call.
2. Load `bookings.get`.
3. If needed, use `bookings.getMonthAvailability` or `bookings.getDayAvailability` to inspect candidate slots.
4. Run `bookings.previewAdminReschedule`.
5. If the preview is acceptable, run `bookings.adminReschedule` with an explicit operator reason and the preview artifact.

## Cancellation workflow

1. Confirm the tenant and include `tenantId` in every tool call.
2. Load `bookings.get`.
3. Run `bookings.previewAdminCancel`.
4. If the preview is acceptable, run `bookings.adminCancel` with an explicit operator reason and the preview artifact.

## Mutation rules

- always inspect the preview before executing
- `tenantId` is a session safety echo, not a selector; switching tenant means switching session
- execute calls must include the preview artifact returned by the matching preview
- call out refund implications on cancellation
- call out slot availability and booking authority on reschedule
- preserve the operator reason in the final mutation call
- do not weaken or reinterpret the Souphi workflow invariants

## Relevant tools

- `bookings.get`
- `bookings.getMonthAvailability`
- `bookings.getDayAvailability`
- `bookings.previewAdminReschedule`
- `bookings.adminReschedule`
- `bookings.previewAdminCancel`
- `bookings.adminCancel`
