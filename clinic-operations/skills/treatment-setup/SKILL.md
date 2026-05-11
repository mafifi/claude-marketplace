---
name: treatment-setup
description: Set up internal Dr Souphi treatments through the Souphi MCP treatment tools. Use when clinic staff need a non-website treatment, Stripe state sync, consent linkage, or aftercare linkage.
mcp_tools:
  - treatments.search
  - treatments.get
  - treatments.previewInternalSetup
  - treatments.createInternal
  - treatments.syncStripeState
  - treatments.linkConsentTemplate
  - treatments.linkAftercareTemplate
  - treatments.createConsentTemplateDraft
  - treatments.createAftercareTemplateDraft
  - treatments.getSetupStatus
---

# Treatment Setup

Use these tools for internal-only treatment setup. Public website publishing, clinical notes, treatment plans, photos, and documents are outside this skill.

## Default workflow

1. Search with `treatments.search` using explicit `treatmentId`, `slug`, or a name of at least 2 characters.
2. Run `treatments.previewInternalSetup` for the proposed slug and known template links.
3. If there is no slug collision, run `treatments.createInternal`.
4. Use the Stripe connector to create or update flat-fee prices.
5. Run `treatments.syncStripeState`.
6. Link or create treatment-specific consent and aftercare templates.
7. Finish with `treatments.getSetupStatus`.

## Rules

- never call `treatments.search` without an explicit identifier or search term
- do not create a treatment when a slug collision is reported
- internal treatments use `status: "published"` with `isVisible: false`; public treatment routes and sitemap generation gate on `isVisible`
- template link tools must preserve full template content and mutate only `treatmentIds`
- global consent templates cannot be linked directly to a treatment; create a treatment-specific draft instead
- a draft template is not complete until content is authored and published
- Stripe product and price work remains actionable through the Stripe connector; the Souphi MCP reports and syncs state
