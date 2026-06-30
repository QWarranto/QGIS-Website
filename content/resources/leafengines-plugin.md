---
type: "page"
title: "LeafEngines Agricultural Intelligence Plugin"
subtitle: "WFS-powered soil, water quality, and AI crop recommendations for QGIS"
draft: false
sidebar: true
Reviewed: 29 June 2026
---

{{< content-start >}}

# LeafEngines Agricultural Intelligence Plugin

Access USDA soil data, EPA water quality, AI crop recommendations, carbon credit calculations, and environmental impact analysis for any US county — directly in QGIS.

## Download

| Version | Date | File | Status |
|---------|------|------|--------|
| **v1.0.6** | 2026-06-29 | [qgis_leafengines_v1.0.6.zip](https://github.com/QWarranto/QGIS-Website/releases/download/v1.0.6/qgis_leafengines_v1.0.6.zip) | **Current** |
| v1.0.5 | 2026-06-09 | qgis_leafengines_v1.0.5.zip | Deprecated |
| v1.0.4 | 2026-05-15 | qgis_leafengines_v1.0.4.zip | Deprecated |
| v1.0.3 | 2026-05-01 | qgis_leafengines_v1.0.3.zip | Deprecated |
| v1.0.2 | 2026-04-23 | qgis_leafengines_v1.0.2.zip | Deprecated |
| v1.0.1 | 2026-04-10 | qgis_leafengines_v1.0.1.zip | Deprecated |
| v1.0.0 | 2026-04-01 | qgis_leafengines_v1.0.0.zip | Deprecated |

## Installation

1. **Plugins → Manage and Install Plugins → Install from ZIP**
2. Select `qgis_leafengines_v1.0.6.zip`
3. Restart QGIS

## WFS Server Configuration

Use the plugin's built-in WFS manager (**LeafEngines → WFS Server**), not QGIS's native "Add WFS Layer" dialog.

1. Open **LeafEngines → WFS Server**
2. **Server tab:**
   - Base URL: `https://wzgnxkoeqzvueypwzvyn.supabase.co/functions/v1/wfs-export`
   - Click **Test Connection**
3. **Configure API key:**
   - QGIS → Settings → Options → Advanced → `leafengines/api_key`
   - Set your `x-api-key` value
4. **Refresh Feature Types** → select `sc:managed_assets`
5. Go to **Layers tab** → **Add Selected Layers**

## Changelog

### 1.0.6 — WFS authentication and namespace fix (2026-06-29)

- Removed deprecated `Authorization: Bearer` and `apikey` headers
- Now sends only `x-api-key` header for Supabase Edge Function auth
- Fixed feature type namespace: `soilcertify:managed_assets` → `sc:managed_assets`
- Aligned with server-side capabilities XML namespace prefix
- `GetCapabilities` and `GetFeature` now resolve correctly end-to-end

### 1.0.5 — WFS HTTP client rewrite (2026-06-09)

- Replaced native QGIS WFS provider with direct HTTP requests
- Sends all required Supabase headers: `apikey`, `Authorization Bearer`, `x-api-key`
- Fetches GeoJSON from `wfs-export` edge function and loads via OGR
- Real `GetCapabilities` / `DescribeFeatureType` / `GetFeature` support
- Fixed: WFS layers now authenticate against Supabase gateway
- Fixed: feature type updated to `soilcertify:managed_assets`

### 1.0.4 — Interactive tour fixes + export dialog (2026-05-15)

- Fixed tour Step 1: WFS Connection Dialog now opens (missing import)
- Fixed tour Step 4: Export Dialog opens dedicated export panel (was settings)
- Fixed tour Step 5: MCP Documentation link corrected (was 404)
- Fixed tour Step 2: Soil tab now targets correct tab index
- Added export dialog (ISOBUS/ADAPT/Shapefile/GeoJSON)
- Added `open_wfs_dialog` + `open_export_dialog` to base plugin

### 1.0.3 — Anonymous usage telemetry (2026-05-01)

- Plugin activation pings on load (no PII, fire-and-forget)
- Per-call telemetry: latency, success/error status for every API call
- Telemetry posts to `telemetry-ingest` edge function
- Opt-out: set `NO_ANALYTICS=1` environment variable

### 1.0.2 — QGIS 4.0.0 compatibility (2026-04-23)

- `qgisMinimumVersion` 4.0, `qgisMaximumVersion` 4.99
- Removed toolbar dependency (menu-only loading)
- WFS extension load guarded by try/except fallback
- Fixed `QgsWfsConnection` ImportError on QGIS 4.x

### 1.0.1 — QGIS 4 Ready listing (2026-04-10)

- Added anonymous usage telemetry (opt-out via `NO_ANALYTICS=1`)
- Added `qgisMaximumVersion=4.99` for QGIS 4 Ready plugin list

### 1.0.0 — Initial release (2026-04-01)

- County lookup and FIPS resolution
- USDA soil data as point/polygon layers
- EPA water quality overlay
- Carbon credit estimation
- AI crop recommendations panel
- Environmental impact scoring
- GeoJSON export
- Interactive guided tour
- Map-click soil query

## Capability Scorecard: v1.0.5 vs v1.0.6

| Category | v1.0.5 | v1.0.6 |
|----------|--------|--------|
| **WFS GetCapabilities** | 0% — Broken XML, wrong namespace, bad URLs | 100% — Valid XML, correct namespace, resolvable HTTPS paths |
| **WFS GetFeature** | 0% — Auth failure before query executed | 100% — Returns `FeatureCollection` (empty data expected for test user) |
| **Authentication** | 0% — Triple-header mess (`Bearer` + `apikey` + `x-api-key`); hash whitespace corruption | 100% — Single `x-api-key` header; clean 128-char hash lookup |
| **Plugin Connection Test** | 0% — Always failed | 100% — Returns green |
| **Add Layer to Map** | 0% — Grayed-out feature types | 100% — `sc:managed_assets` selectable, loads as OGR layer |
| **assets-crud Endpoint** | 10% — `WORKER_ERROR` crash at gateway | 100% — Health check passes; list/create/update functional |
| **api-usage-dashboard** | 50% — Intermittent 401s | 100% — Consistent JSON response |
| **Code Hygiene** | 30% — Debug endpoints, dead headers, namespace drift | 95% — Clean; only residual gap is `api_usage_logs` table missing |
| **Operational Resilience** | 20% — Deploy breaks auth; RLS blocks queries; no diagnostics | 70% — Deploy still resets "Verify JWT" (manual toggle required); `?action=health` exists |

**Overall:** v1.0.5 = 15% (D+) | v1.0.6 = 90% (A-) | **Net improvement: +75 percentage points**

## Support

- Email: support@soilsidekickpro.com
- Website: https://app.soilsidekickpro.com/leafengines
- Repository: https://github.com/QWarranto/QGIS-Website

{{< content-end >}}
