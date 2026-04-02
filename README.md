# IntuneSettingsCatalogData

Nightly fetch of the full Microsoft Intune Settings Catalog from Microsoft Graph (beta API). Data is published as GitHub Release assets for consumption by downstream tools.

## Release Assets

| File | Size | Description |
|------|------|-------------|
| `settings.json` | ~65 MB | All setting definitions (polymorphic — choice, simple, group, etc.) |
| `categories.json` | ~561 KB | Category hierarchy with parent/child relationships |
| `last-updated.json` | ~60 B | ISO timestamp + schema version |

**Download (no auth required):**

```
https://github.com/royklo/IntuneSettingsCatalogData/releases/latest/download/settings.json
https://github.com/royklo/IntuneSettingsCatalogData/releases/latest/download/categories.json
https://github.com/royklo/IntuneSettingsCatalogData/releases/latest/download/last-updated.json
```

## How It Works

A GitHub Actions workflow runs daily at 06:00 UTC:
1. Authenticates with Microsoft Graph via Azure AD service principal
2. Fetches all configuration settings and categories (including orphan categories)
3. Compares with previous data — skips release if unchanged
4. Publishes assets to a rolling `latest` release + a dated tag (`vYYYY-MM-DD`)

## Secrets Required

| Secret | Purpose |
|--------|---------|
| `AZURE_TENANT_ID` | Azure AD tenant ID |
| `AZURE_CLIENT_ID` | Service principal app ID |
| `AZURE_CLIENT_SECRET` | Service principal secret |

The app registration needs `DeviceManagementConfiguration.Read.All` (Application permission).

## Local Development

```bash
npm install
AZURE_TENANT_ID=xxx AZURE_CLIENT_ID=xxx AZURE_CLIENT_SECRET=xxx npm run fetch-settings
```
