---
name: marketing-review
description: Review, approve, or reject Dr Souphi marketing review artifacts through Souphi MCP marketing tools. Use when clinic staff ask about pending marketing reviews, approval, rejection, or review decisions.
mcp_tools:
  - marketing.listPendingReviews
  - marketing.approveArtifact
  - marketing.rejectArtifact
---

# Marketing Review

Use review tools for governed review of generated marketing outputs.

**Review model (as of the review-model-collapse ADR, ADR-0087 in the repo):**
review = deliverables whose latest generation run completed and have no
recorded approval yet. Approving a deliverable **writes an immutable ledger
row** (the `marketingApprovals` ledger) — it is the permanent compliance
record, pinning exactly which artifact content was approved, by whom, and
when. Rejecting a
deliverable resolves the family's revision point and reruns generation from
there; it does not write a ledger row. There is no separate "readiness" or
"review preparation" step before an item is approvable — a completed,
unapproved run is immediately reviewable.

## Default workflow

1. Confirm the tenant and include `tenantId` in every tool call.
2. Run `marketing.listPendingReviews` before any decision — this lists
   deliverables currently in the derived review stage (completed run, no
   approval on record).
3. Use exact `deliverableId` values returned by the pending review list.
4. Approve only with the required approver name and a concrete
   operator reason — approving records a permanent, append-only ledger
   entry.
5. Reject only with a concrete reason that can be recorded in the decision
   history; rejection reruns generation from the appropriate point, it does
   not publish or approve anything.

## Rules

- never approve without first inspecting the pending artifact
- preserve the config-derived required approver name
- do not fabricate generated output, review state, or approval
- use rejection when the artifact needs changes; do not publish rejected outputs
- treat approval as permanent and append-only: re-approving after a reject
  and regeneration records a new decision, it never edits a prior one

## Relevant tools

- `marketing.listPendingReviews`
- `marketing.approveArtifact`
- `marketing.rejectArtifact`
