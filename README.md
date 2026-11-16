# AI initiatives map

A single web page that shows EIF's AI initiatives, who they involve and where they overlap. It has four views: **Map**, **Overlaps**, **All initiatives** and **Missing info**. Click any initiative to see its details or edit it.

It is plain HTML. There is no build step, no server, no database and nothing to install.

| File | What it is |
| --- | --- |
| `index.html` | The app. |
| `data.js` | The initiatives. This is the only file you change to update the map. |
| `README.md` | This guide. |

Only `index.html` and `data.js` are needed for the site. The `reference/` and `seed/` folders were inputs for building it. The page doesn't use `seed/initiatives.json`; it reads `data.js`.

## Try it on your computer

Keep `index.html` and `data.js` in the same folder and double-click `index.html`. It opens in your browser and works without a server.

The map library and fonts load from the internet. Without a connection, the Overlaps, All initiatives and Missing info views still work, and the map shows a short message instead.

## Publish with GitHub Pages

1. **Create a repository** on GitHub, for example `ai-initiatives-map`.
   On a free account the repository must be public. In almost every setup, the published page is public too, so anyone with the link can open it. Check that the data is fine to share.
2. **Upload the files.** In the repository, choose **Add file → Upload files**, drag in `index.html`, `data.js` and `README.md`, then click **Commit changes**.
3. **Turn on Pages.** Go to **Settings → Pages**. Under **Build and deployment**, set **Source** to **Deploy from a branch**, choose the branch **main** and the folder **/ (root)**, then click **Save**.
4. **Open the site.** After a minute or two, the address appears at the top of the Pages settings:
   `https://<your-user-or-organization>.github.io/<repository-name>/`

The page uses only relative paths, so it works at that `/repository-name/` address and when opened from your computer.

## Update the data

### Option A: edit in the app (recommended)

1. Open the published site and reload it, so you start from the latest data.
2. Edit, add or delete initiatives, or click **Mark as checked today**.
   Changes stay only in that browser tab. A banner appears: *"You have unsaved changes. Download the updated data file and upload it to GitHub to publish them."* If you close or reload the tab before downloading, the changes are lost. The browser warns you first.
3. In the banner, click **Download updated data.js**.
4. In the GitHub repository, choose **Add file → Upload files**, drag in the downloaded `data.js`, and click **Commit changes**. A file with the same name replaces the old one.
5. After about a minute, reload the site. If you still see the old data, press **Ctrl+F5**. GitHub Pages can cache files for up to 10 minutes.

Good to know:

- If your browser saved the file as `data (1).js`, rename it to `data.js` before uploading. Otherwise GitHub adds a second file and the site doesn't change.
- Chrome and Edge sometimes warn that a downloaded `.js` file could be harmful. This file only contains your data, so choose **Keep**.
- The downloaded file is formatted one value per line. On GitHub, open `data.js` and click **History** to see what changed in each upload. You can also restore an earlier version there.
- If two people edit at the same time, the last upload replaces the earlier one. Agree who updates the file, or reload the site just before you start editing.

### Option B: edit `data.js` directly on GitHub

Open `data.js` in the repository, click the pencil icon, make your change and click **Commit changes**.

Everything between `window.INITIATIVES =` and the final `;` is JSON, so:

- use double quotes `"` around every name and text value
- put a comma between items, but not after the last item in a list
- keep the first line `window.INITIATIVES = [` and the last line `];`

If the file gets broken, the site shows *"The initiatives couldn't be loaded"* instead of the map. Fix the typo, or restore the previous version from the file's **History**.

#### Fields

| Field | What to put in it |
| --- | --- |
| `id` | Unique ID, using lowercase letters, digits and hyphens, such as `master-ai`. Don't change it once published. |
| `name` | Full name. **Required.** |
| `shortName` | Short name shown on the map and in table headings. Must be different from other initiatives. |
| `kind` | `Funded project`, `Research grant`, `Education program`, `Community` or `Initiative` |
| `stage` | `Active`, `Planned`, `Completed` or `To confirm` |
| `eifTeam`, `owner` | EIF team, and the person who keeps the entry up to date |
| `period`, `funding` | Free text, such as `2026–2028` |
| `summary`, `eifRole`, `notes`, `source` | Free text |
| `lastVerified` | Date the entry was last checked, as `YYYY-MM-DD` |
| `order` | Position in lists (1, 2, 3 …) |
| `themes` | List of themes, such as `["AI agents", "Industry linkage"]` |
| `partners` | List of `{ "country": "…", "name": "…", "role": "…" }`. Roles: `Coordinator`, `Partner`, `Co-implementer`, `Funder`, `Sponsor`, `Industry partner`, `Academic partner`, `Commissioning body`. `Funder` and `Sponsor` are drawn as funders on the map. |
| `outputs`, `audience`, `tools`, `locations` | Lists of text, such as `["Gyumri", "Vanadzor"]` |

Copy an existing entry as a template when adding a new one by hand.

### Change the allowed themes, stages, types or roles

The edit form only accepts values from fixed lists. They are near the top of the script in `index.html`, in the section marked **Allowed values**: `THEMES`, `STAGES`, `KINDS`, `PARTNER_ROLES` and `FUNDER_ROLES`. Edit `index.html` on GitHub to change them.

Renaming a value doesn't update the entries in `data.js` that use the old name, so update those too.

## Technical notes

- The data loads with a `<script src="data.js">` tag, not `fetch()`, so the page works when opened straight from the file system (`file://`).
- Cytoscape.js 3.26.0 comes from cdnjs with a pinned version and an integrity hash. Fonts come from Google Fonts.
- Files are UTF-8, so Armenian text renders and is saved correctly.
- **Download updated data.js** writes the same format as the original file: same header, 2-space indentation and the same field order. A GitHub diff then shows only the lines that changed.
- The color theme (Auto, Light or Dark, top right) is remembered in your browser. Nothing else is saved in the browser.
