---
name: booking-config
description: Inspect and update Dr Souphi booking configuration through the Souphi MCP config tools. Use when clinic staff need to adjust availability, locations, or booking policy settings.
mcp_tools:
  - bookingConfig.get
  - bookingConfig.update
---

# Booking Configuration

Treat booking configuration as a whole-object read-before-write surface.

## Default workflow

1. Load the current configuration with `bookingConfig.get`.
2. Work from the full returned object.
3. Prepare the intended full-object update.
4. Run `bookingConfig.update` with `previousConfiguration`, the complete updated `configuration`, and an explicit operator reason.

## Rules

- do not send partial patch semantics unless the tool contract changes
- read the current object first
- preserve fields that are not intentionally changing
- include the required round-trip fields in the submitted configuration
- summarize the effective config change before executing the update

## Relevant tools

- `bookingConfig.get`
- `bookingConfig.update`
