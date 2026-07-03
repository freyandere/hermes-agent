# Plugins Sorting & Filtering — Fix Report

## Issue
Filtering by source="user" produced duplicate plugin entries (fal, xai-bundled).

## Root Cause
`_discover_all_plugins()` scans both bundled and user directories. Same plugin with different `key` values in each directory creates separate entries in the API response — visible as duplicates.

## Fix Applied
Added deduplication by name in `_merged_plugins_hub()` (web_server.py line ~13697):
- `rows_by_name` dict keeps last entry per plugin name
- User plugins override bundled ones (last-write-wins)

## Files
- `web/src/pages/PluginsPage.tsx` — sorting + filtering
- `web/src/plugins/slots.ts` — slot system
- `web/src/lib/api.ts:2284-2298` — `HubAgentPluginRow` interface
- `hermes_cli/web_server.py` — `_merged_plugins_hub` dedup logic
- `hermes_cli/plugins_cmd.py` — `_discover_all_plugins`, `_scan_level`

## Key Findings
- PluginSlot `name="plugins:bottom"` was removed (duplicate source)
- Filter controls OUTSIDE `<Card>` — in the `<div>` directly
- i18n missing — strings hardcoded (`source`, `status`, `name`)
- Build completes in ~370ms (481 modules)
- Dashboard on port 9119

## Sorting Structure
Sorts via `localeCompare()` on string representation of the field.

## Filter Pipeline
1. Sort → 2. Filter (source + status) — pipeline order matters for performance.

## Backup
Path: `C:\Users\Admin\AppData\Local\hermes\hermes-agent`
