# EVE Combat Site Scanner

A simple static web app for EVE Online probe-scan paste parsing with DED rating and escalation lookup.

## What it does

Paste your probe scanner results directly into the page. The tool:

1. Filters the pasted rows to **Combat Sites only**.
2. Looks each site up in `sites.json` and shows:
   - **ID** – signature ID (e.g. `ABC-123`)
   - **Scan Type** – Cosmic Signature or Cosmic Anomaly
   - **Site Name**
   - **DED** – rating badge (e.g. `4/10`) if it is a DED complex
   - **Not DED** – `Yes` for unrated combat sites; blank for DED entries
   - **Can Escal** – escalation target tier if known (e.g. `→ 2/10`)
   - **Note** – extra information from the database
3. Lists any **unknown site names** (not in `sites.json`) with a JSON template you can copy to add them.

## How to use

1. Open EVE Online and run your probes.
2. In the Probe Scanner window, press **Ctrl+A** then **Ctrl+C** to copy all results.
3. Navigate to the GitHub Pages URL for this repo.
4. Press **Ctrl+V** inside the text box – the results parse automatically.
   Alternatively, paste manually and click **Parse**.

## How to update `sites.json`

`sites.json` contains the site database. Each entry follows this schema:

```json
{
  "name": "Blood Hideaway",
  "isDed": false,
  "ded": null,
  "canEscalateToDed": "1/10",
  "note": "Blood Raiders unrated complex; can escalate to DED 1/10"
}
```

| Field              | Type            | Description |
|--------------------|-----------------|-------------|
| `name`             | string          | Exact in-game site name (case-insensitive match) |
| `isDed`            | boolean         | `true` if the site is a DED-rated complex |
| `ded`              | string \| null  | DED tier, e.g. `"4/10"` – required when `isDed` is `true` |
| `canEscalateToDed` | string \| null  | Escalation tier if known, e.g. `"2/10"`; `null` if unknown or N/A |
| `note`             | string          | Free-text notes |

To add an unknown site:

1. Paste your scan into the tool.
2. Scroll to the **Unknown Site Names** section and click **Copy JSON template**.
3. Open `sites.json`, find the `"sites"` array, and paste the template at the end (inside the `]`).
4. Fill in the correct values (`isDed`, `ded`, `canEscalateToDed`, `note`).
5. Commit and push – the page will automatically use the updated data.

## Enabling GitHub Pages

1. Go to **Settings → Pages** in this repository.
2. Under *Source*, select **Deploy from a branch**.
3. Choose **`main`** branch and **`/ (root)`** directory.
4. Click **Save**.

The app will be live at `https://WiganSucks.github.io/eve-cbsites/` within a minute.

## Data source

Site data is sourced from [EVE University – Combat Sites](https://wiki.eveuniversity.org/Combat_site).
