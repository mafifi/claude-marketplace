---
name: booking-config
description: Explain the current Dr Souphi booking configuration model. Use when clinic staff ask where booking readiness, resource capacity, treatment requirements, or live booking state are managed.
---

# Booking Configuration

The old `bookingConfig.*` MCP tools have been retired. Booking setup is now
split across its natural admin surfaces:

- treatment booking requirements live on treatment edit pages
- practitioners, rooms, and resource availability live under Resources
- clinic live booking state lives under Booking readiness
- MCP booking tools can inspect availability and create operator-assisted
  provisional bookings, but they no longer mutate booking configuration

## Default workflow

1. If staff ask whether public booking is live, direct them to Booking readiness.
2. If staff ask why slots are missing, inspect resource availability and treatment requirements.
3. If staff need a patient booked manually, use the operator booking tools after selecting a returned availability slot.
4. If staff ask to change policy/default location/live-gate settings from MCP, explain that this is not currently an MCP mutation surface.

## Rules

- do not claim `bookingConfig.get` or `bookingConfig.update` exists
- do not invent a booking configuration patch path
- use treatment/resource/readiness language rather than "booking config" as a storage object
