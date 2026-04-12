---
name: booking-desk
description: Create provisional operator-assisted consultation checkouts for Dr Souphi using the Souphi MCP booking desk tool. Use when clinic staff are helping a patient book by phone or email but payment still needs to happen through checkout.
---

# Booking Desk

Use the provisional checkout path when staff are creating a booking that still requires payment.

## Default workflow

1. Gather the booking request fields needed by `bookings.createProvisionalCheckout`.
2. Confirm the operator reason.
3. Run `bookings.createProvisionalCheckout`.
4. Return the resulting checkout URL and summarize any hold or expiry implications.

## Rules

- this is a provisional checkout flow, not phone card capture
- if the requested appointment is too close to start for the operator hold window, report that clearly rather than improvising another path
- keep patient detail handling minimal until the booking is being created

## Relevant tools

- `bookings.createProvisionalCheckout`
- `bookings.getMonthAvailability`
- `bookings.getDayAvailability`
