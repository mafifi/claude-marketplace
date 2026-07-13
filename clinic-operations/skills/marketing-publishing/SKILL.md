---
name: marketing-publishing
description: Inspect approval-owned Dr Souphi marketing publication and explain when review is still required. Use when clinic staff ask to publish or check an approved marketing output.
mcp_tools:
  - marketing.getOverview
---

# Marketing Publishing

Approval is the publication decision. There is no separate manual publish tool.

## Default workflow

1. Confirm the tenant and include `tenantId` in every tool call.
2. Run `marketing.getOverview` to inspect the current derived lifecycle.
3. If the item is still in review, route the operator to the
   `marketing-review` workflow rather than claiming it can publish.
4. If it is approved, explain that publication is already scheduled and that
   provider attention is handled in the Channels operator surface.

## Rules

- do not offer or invent a second Publish or Retry action after approval
- do not claim provider success from approval alone
- do not invent schedule-set artifact ids or provider evidence
- treat provider failures as technical attention, not a new approval state

## Relevant tools

- `marketing.getOverview`
