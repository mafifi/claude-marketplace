# Clinic Operations Plugin

`clinic-operations` is the Claude Code plugin for Souphi operator work. Phase 1 is Souphi-only and packages the deployed `dr-souphi-mcp` server behind a single operator-facing plugin for Dr Souphi.

## Phase 1 scope

- patient search, creation, outstanding-item review, access requests, and invoice sends
- internal treatment setup, Stripe state sync, consent linkage, and aftercare linkage
- consultation search and detail
- workflow state and recovery
- admin cancellation and reschedule
- provisional booking checkout creation
- clinic settings read, preview, and update
- marketing campaign operations, review, journeys, demand, and dry-run publishing

Current runtime boundary:

- plugin packaging: `clinic-operations`
- runtime MCP server: external deployed `dr-souphi-mcp`

The plugin does not add a second business layer. It reuses the existing Souphi MCP tools directly.

## Install and validate

Validate the plugin structure:

```bash
claude plugin validate ./clinic-operations
```

Load the plugin locally for development:

```bash
claude --plugin-dir ./clinic-operations
```

Then run:

```text
/reload-plugins
```

The plugin ships with a static remote MCP registration in `.mcp.json`:

- `https://mcp.drsouphi.com/mcp`
- OAuth uses the static public client id `claude-code`; no local callback port is pinned.

Use `/mcp` inside Claude Code to inspect the server and complete any authentication flow required by the remote MCP service.

## Version 0.3.1 operations taxonomy

Version `0.3.1` adds first-class marketing operations skills and renames the
shipped MCP server key from `dr-souphi-bookings` to `dr-souphi-operations`.
Operators may need to refresh the plugin and reconnect the MCP server entry.

## Version 0.3.0 auth migration

The MCP runtime now uses Convex-backed opaque OAuth tokens instead of the
previous dynamic-client JWT flow. After updating to `0.3.0`, operators need to
reauthenticate once through Claude Code's MCP authentication flow. This rotates
credentials only; no clinic data is lost.

## Updating the installed plugin

When the shipped plugin changes, increment the version in:

- `clinic-operations/.claude-plugin/plugin.json`

Treat version bumps as required for:

- skill changes
- agent changes
- `.mcp.json` changes
- `settings.json` changes
- shipped README/instruction changes that alter operator behavior

After pushing an updated plugin, refresh it in Claude Code with:

```text
/plugin marketplace update projects
/reload-plugins
```

## Tool surface

The plugin expects the Souphi MCP runtime to expose these tools:

- `bookings.search`
- `bookings.get`
- `bookings.getWorkflowState`
- `bookings.getMonthAvailability`
- `bookings.getDayAvailability`
- `bookings.previewAdminReschedule`
- `bookings.adminReschedule`
- `bookings.previewAdminCancel`
- `bookings.adminCancel`
- `bookings.previewWorkflowRecovery`
- `bookings.recoverConfirmation`
- `bookings.recoverModification`
- `bookings.recoverCancellation`
- `bookings.createProvisionalCheckout`
- `patients.search`
- `patients.get`
- `patients.create`
- `patients.getOutstandingItems`
- `patients.previewSendAccessRequest`
- `patients.sendAccessRequest`
- `patients.previewSendInvoice`
- `patients.sendInvoice`
- `treatments.search`
- `treatments.get`
- `treatments.previewInternalSetup`
- `treatments.createInternal`
- `treatments.previewStripeSync`
- `treatments.syncStripeState`
- `treatments.linkConsentTemplate`
- `treatments.linkAftercareTemplate`
- `treatments.createConsentTemplateDraft`
- `treatments.createAftercareTemplateDraft`
- `treatments.getSetupStatus`
- `clinicSettings.get`
- `clinicSettings.previewUpdate`
- `clinicSettings.update`
- `marketing.getOverview`
- `marketing.listPendingReviews`
- `marketing.approveArtifact`
- `marketing.rejectArtifact`
- `marketing.setGenerationMode`
- `marketing.setCampaignPhase`
- `marketing.listJourneys`
- `marketing.setJourneyStatus`
- `marketing.getAttributionSummary`
- `marketing.listGiftVouchers`
- `marketing.listEnquiries`
- `marketing.setEnquiryStage`
- `marketing.addEnquiryLog`

The bundled skills and agent are written to prefer these tools directly rather than inventing aliases or wrapper semantics.

Important usage expectations:

- every tool call includes `tenantId`; this is a session safety echo, not a selector
- `bookings.search` with only `tenantId` returns a recent operator inbox
- `bookings.search` should be called with only `tenantId` unless the user explicitly provided a filter or a prior tool returned one
- `bookings.search` becomes more precise with `bookingId`, exact `patientEmail` (plain `email` is also accepted), or explicit operational filters
- `patientEmail`, `email`, and `bookingId` must not be inferred from the authenticated operator identity
- admin cancel and reschedule stay preview-first
- patient-facing access and invoice sends stay preview-first and require patientId, preview artifact, and `operatorReason`
- `patients.search` and `treatments.search` do not allow no-argument browsing
- treatment slugs are checked before create, and template link updates preserve full rich-text content
- `clinicSettings.update` requires `clinicSettings.previewUpdate`, explicit confirmation, `operatorReason`, and the preview artifact
- marketing tools follow read/list-before-mutate discipline
- `marketing.setGenerationMode` is deliverable-scoped and does not change brand defaults
- `marketing.setCampaignPhase` is a compatibility tool name for campaign pause/resume; campaign phase is computed from the seeded timeline
- approval is the publication decision; MCP does not expose a separate Publish
  or Retry command after approval

## Staff install path

The shipped plugin is static and remote-first:

- no environment variable setup
- no local MCP runtime required
- one plugin directory / one validation command / one load command

This is the artifact intended for Dr Souphi and clinic staff.

## Local development override

Developer local testing is separate from the shipped staff plugin.

Use the example override file:

- `clinic-operations/.mcp.local.example.json`

Recommended local flow:

1. Start the local Souphi MCP runtime.
2. Copy `.mcp.local.example.json` to a local-only variant such as `.mcp.local.json`.
3. Swap the plugin’s `.mcp.json` to the local copy only in your dev workspace, or point Claude Code at the local server through your own project/user MCP config.
4. Do not commit the local override as the shared plugin artifact.

This keeps the clinic-facing install path static while preserving a straightforward local developer workflow.

## Future expansion

`Clinic Operations` is intentionally broader than Souphi Phase 1. Future domains such as GBAAM can be added later by:

- registering additional MCP servers in `.mcp.json`
- adding domain-specific skills and agents
- keeping runtime services separate where the domain boundary differs

Do not merge Souphi and GBAAM into one runtime app just to share a plugin package.
