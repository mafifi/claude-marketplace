---
name: marketing-campaign-operations
description: Inspect Dr Souphi marketing campaign state, pause or resume a campaign run, and manage deliverable-scoped generation-mode overrides through the Souphi MCP marketing tools. Use when clinic staff ask about campaign overview, pacing, generation mode, or campaign operating status.
mcp_tools:
  - marketing.getOverview
  - marketing.setGenerationMode
  - marketing.setCampaignPhase
---

# Marketing Campaign Operations

Use these tools for campaign overview, campaign pause/resume, and deliverable generation-mode operations.

## Default workflow

1. Confirm the tenant and include `tenantId` in every tool call.
2. Start with `marketing.getOverview` to load the current campaign read models.
3. For generation-mode changes, use the exact `deliverableId` returned by an earlier marketing read.
4. Use `marketing.setGenerationMode` only for a deliverable-scoped override.
5. Use `marketing.setCampaignPhase` only to set campaign operating `status` to `active` or `paused`; campaign phase itself remains computed from the seeded timeline.

## Rules

- generation-mode override is deliverable-scoped; it does not change the brand default
- clear an override with `generationModeOverride: null`
- do not invent deliverable ids, campaign run slugs, or campaign phase state
- `marketing.setCampaignPhase` is a compatibility tool name for campaign pause/resume
- campaign phase is computed from the seeded timeline
- campaign pause/resume is exposed on MCP as `status: "active" | "paused"`

## Relevant tools

- `marketing.getOverview`
- `marketing.setGenerationMode`
- `marketing.setCampaignPhase`
