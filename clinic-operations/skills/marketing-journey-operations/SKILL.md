---
name: marketing-journey-operations
description: Inspect and pause or activate Dr Souphi marketing journeys through Souphi MCP marketing tools. Use when clinic staff ask about journey status, campaign automation, pausing a journey, or reactivating a journey.
mcp_tools:
  - marketing.listJourneys
  - marketing.setJourneyStatus
---

# Marketing Journey Operations

Use journey tools for live journey inspection and active/paused transitions.

## Default workflow

1. Confirm the tenant and include `tenantId` in every tool call.
2. Run `marketing.listJourneys` before any journey mutation.
3. Use the exact `journeyId` returned by `marketing.listJourneys`.
4. Use `marketing.setJourneyStatus` only for `active` or `paused`.

## Rules

- list before mutation
- only `active` and `paused` transitions are supported
- do not invent draft, archived, or completed transitions
- do not silently pause event-triggered patient journeys unless the operator explicitly asks for that journey

## Relevant tools

- `marketing.listJourneys`
- `marketing.setJourneyStatus`
