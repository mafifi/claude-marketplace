---
name: booking-desk
description: Create provisional operator-assisted consultation checkouts for Dr Souphi using the Souphi MCP booking desk tool. Use when clinic staff are helping a patient book by phone or email but payment still needs to happen through checkout.
mcp_tools:
  - bookings.createProvisionalCheckout
  - bookings.getMonthAvailability
  - bookings.getDayAvailability
---

# Booking Desk

Use the provisional checkout path when staff are creating a booking that still requires payment.

## Default workflow

1. Confirm the tenant first and include `tenantId` in every tool call.
2. Gather the booking request fields needed by `bookings.createProvisionalCheckout`.
3. Confirm the operator reason.
4. Run `bookings.createProvisionalCheckout`.
5. Return the resulting checkout URL and summarize any hold or expiry implications.

## Rules

- this is a provisional checkout flow, not phone card capture
- `tenantId` is a session safety echo, not a selector; if the tenant is wrong, the operator must switch session
- if the requested appointment is too close to start for the operator hold window, report that clearly rather than improvising another path
- keep patient detail handling minimal until the booking is being created

## Relevant tools

- `bookings.createProvisionalCheckout`
- `bookings.getMonthAvailability`
- `bookings.getDayAvailability`
