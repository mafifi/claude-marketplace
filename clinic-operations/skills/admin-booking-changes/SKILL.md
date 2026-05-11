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

1. Load `bookings.get`.
2. If needed, use `bookings.getMonthAvailability` or `bookings.getDayAvailability` to inspect candidate slots.
3. Run `bookings.previewAdminReschedule`.
4. If the preview is acceptable, run `bookings.adminReschedule` with an explicit operator reason.

## Cancellation workflow

1. Load `bookings.get`.
2. Run `bookings.previewAdminCancel`.
3. If the preview is acceptable, run `bookings.adminCancel` with an explicit operator reason.

## Mutation rules

- always inspect the preview before executing
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
