# Clinic Operations Plugin

`clinic-operations` is the Claude Code plugin for clinic operations. Phase 1 is Souphi-only and packages the deployed `dr-souphi-mcp` server behind a single operator-facing plugin for Dr Souphi.

## Phase 1 scope

- consultation search and detail
- workflow state and recovery
- admin cancellation and reschedule
- provisional booking checkout creation
- booking configuration read and whole-object update

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

Use `/mcp` inside Claude Code to inspect the server and complete any authentication flow required by the remote MCP service.

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
- `bookings.recoverConfirmation`
- `bookings.recoverModification`
- `bookings.recoverCancellation`
- `bookings.createProvisionalCheckout`
- `bookingConfig.get`
- `bookingConfig.update`

The bundled skills and agent are written to prefer these tools directly rather than inventing aliases or wrapper semantics.

Important usage expectations:

- `bookings.search` with no arguments returns a recent operator inbox
- `bookings.search` becomes more precise with `bookingId`, exact `patientEmail`, or explicit operational filters
- admin cancel and reschedule stay preview-first
- `bookingConfig.update` is whole-object read-before-write

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
