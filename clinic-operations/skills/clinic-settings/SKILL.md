---
name: clinic-settings
description: Inspect and update tenant clinic settings, booking readiness, and resource overview through the Souphi MCP clinicSettings tools. Use when clinic staff ask about booking live state, location/default policy settings, or readiness blockers.
mcp_tools:
  - clinicSettings.get
  - clinicSettings.previewUpdate
  - clinicSettings.update
---

# Clinic Settings

Use `clinicSettings.*` for tenant clinic settings and booking readiness. These tools wrap the current tenant clinic settings, readiness, and resource overview surfaces, not the retired global booking configuration object.

## Tenant rule

Every tool call must include `tenantId`. This value is a safety echo for the current MCP session, not a selector. If the operator has not named the tenant, ask which tenant they are working on. To switch from `dr-souphi` to `gbaam` or `aevum`, the operator must switch/start the matching session before tool calls will succeed.

## Default workflow

1. Run `clinicSettings.get` with the named tenant.
2. If a change is needed, run `clinicSettings.previewUpdate` with only the fields the operator asked to change.
3. Show the diff, readiness impact, tenantId, and preview artifact expiry.
4. Execute `clinicSettings.update` only with `operatorConfirmed: true`, an `operatorReason`, and the preview artifact returned by the preview.
5. If execute says the preview is stale, re-run `clinicSettings.previewUpdate` and ask for confirmation again.

## Rules

- use `clinicSettings.get`, `clinicSettings.previewUpdate`, and `clinicSettings.update`; do not use or mention the retired global booking configuration tool names
- never invent a partial update without previewing it first
- do not promise to switch tenants inside a tool call; tenant switching happens at the session level
- live booking enablement depends on readiness checks and may be rejected until blockers are fixed
- keep the operator reason concrete and tied to the requested settings change
