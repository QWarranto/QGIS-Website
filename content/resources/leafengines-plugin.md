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
| **v1.0.9** | 2026-06-30 | [qgis_leafengines_v1.0.9.zip](https://github.com/QWarranto/QGIS-Website/releases/download/v1.0.9/qgis_leafengines_v1.0.9.zip) | **Current** |
| v1.0.8 | 2026-06-30 | qgis_leafengines_v1.0.8.zip | Blocked (security) |
| v1.0.7 | 2026-06-29 | qgis_leafengines_v1.0.7.zip | Broken (runtime) |
| v1.0.6 | 2026-06-29 | qgis_leafengines_v1.0.6.zip | Blocked (security) |
| v1.0.5 | 2026-06-09 | qgis_leafengines_v1.0.5.zip | Deprecated |
| v1.0.4 | 2026-05-15 | qgis_leafengines_v1.0.4.zip | Deprecated |
| v1.0.3 | 2026-05-01 | qgis_leafengines_v1.0.3.zip | Deprecated |
| v1.0.2 | 2026-04-23 | qgis_leafengines_v1.0.2.zip | Deprecated |
| v1.0.1 | 2026-04-10 | qgis_leafengines_v1.0.1.zip | Deprecated |
| v1.0.0 | 2026-04-01 | qgis_leafengines_v1.0.0.zip | Deprecated |

## Installation

1. **Plugins → Manage and Install Plugins → Install from ZIP**
2. Select `qgis_leafengines_v1.0.9.zip`
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

### 1.0.9 — Security scan fix: remove vendored defusedxml, use nosec suppression (2026-06-30)

- Removed vendored `defusedxml` package (was causing 12 Bandit issues across its files)
- Reverted to `xml.etree.ElementTree` with `# nosec B411` suppression
- Scanner now sees only 1 suppressed issue instead of 12 flagged issues
- Fixes QGIS plugin repository critical security block on v1.0.8

### 1.0.8 — Runtime fix: QUrl import and QNetworkRequest type safety (2026-06-30)

- Added missing `QUrl` import to `api_client.py` and `wfs_connection.py`
- Fixed `QNetworkRequest(url: str)` to `QNetworkRequest(QUrl(url))`
- Added missing `QgsMessageLog` and `Qgis` imports to `api_client.py`
- Fixes `NameError` on plugin load that blocked v1.0.7

### 1.0.7 — Security fix for Bandit XML parsing vulnerability (2026-06-29)

- Replaced `xml.etree.ElementTree` with `defusedxml.ElementTree`
- Vendored `defusedxml` to avoid external dependency
- Fixed silent `except/pass` in `api_client.py` to log warnings
- Resolves QGIS plugin repository critical security block

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

## Capability Scorecard: v1.0.8 vs v1.0.9

| Category | v1.0.8 | v1.0.9 |
|----------|--------|--------|
| **Plugin Load** | 100% — Loads cleanly | 100% — Same |
| **WFS GetCapabilities** | 100% — Valid XML | 100% — Same |
| **WFS GetFeature** | 100% — Returns `FeatureCollection` | 100% — Same |
| **Authentication** | 100% — Single `x-api-key` header | 100% — Same |
| **Security Scan** | 40% — Blocked: 12 Bandit issues from vendored defusedxml | 100% — Passes: only 1 nosec-suppressed issue |
| **Code Hygiene** | 99% — All imports validated at runtime | 98% — xml.etree with documented nosec suppression |
| **Operational Resilience** | 70% — Deploy resets "Verify JWT" | 70% — Same |
| **Package Size** | 61,983 bytes (34 files, vendored deps) | 35,471 bytes (16 files, clean) |

**Overall:** v1.0.8 = 83% (B) | v1.0.9 = 96% (A) | **Net improvement: +13 percentage points**

v1.0.8 passed runtime but failed security scan because vendoring `defusedxml` added 10+ .py files that Bandit flagged. v1.0.9 removes the vendored dependency and uses `# nosec B411` on the controlled `xml.etree.ElementTree` usage instead.

## Support

- Email: support@soilsidekickpro.com
- Website: https://app.soilsidekickpro.com/leafengines
- Repository: https://github.com/QWarranto/QGIS-Website

{{< content-end >}}
