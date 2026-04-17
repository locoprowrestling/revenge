Mac Admin Team ID Lookup – Status & Ideas
========================================

Project Snapshot
----------------
- Single-page HTML app (`index.html`) loads app metadata from `team_app_mac_team_ids.js`.
- Dataset now stores objects with `teamId`, `domain`, and `codeRequirement`.
- UI shows selected app details and offers a helper form to draft JSON snippets for updates.

Recent Changes
--------------
- Normalized `team_app_mac_team_ids.json` to the new object schema and regenerated the JS bundle.
- Enhanced the dropdown results panel to display domain and code requirement in addition to the Team ID.
- Added an inline form and JSON output textarea so updates can be composed from the browser.

Maintaining The Data Files
--------------------------
- Primary source of truth is `team_app_mac_team_ids.json`; keep fields populated with strings (empty when unknown).
- Regenerate the browser bundle after edits so the UI stays in sync:
  ```
  python3 - <<'PY'
  import json
  from pathlib import Path
  data = json.loads(Path('team_app_mac_team_ids.json').read_text())
  Path('team_app_mac_team_ids.js').write_text(
      "window.teamAppMacTeamIdsData = " + json.dumps(data, indent=2) + ";\n"
  )
  PY
  ```
- Placeholder entry `\"${name} ${nameVersion}\"` has been removed; ensure future CSV/automation steps don't reintroduce it.
- Domain values remain blank for most entries; installing or sourcing bundle identifiers (e.g., via vendor manifests, Installomator metadata, or CFBundleIdentifier exports) is required before bulk population.

Next Ideas
----------
- Persist edits made via the form (e.g., download updated JSON or POST to a backend endpoint).
- Add search/filter to the dropdown for faster navigation through large datasets.
- Provide validation hints for domains and code requirement syntax to avoid malformed entries.
- Consider modularizing the data generation script (Node/CLI) and automate via npm/yarn task.
- Expand styling/accessibility (labels, focus states, responsive layout) for better usability.

Verification Checklist
----------------------
- After data changes, reload the page and confirm the dropdown populates and details render.
- Ensure generated JSON snippets are valid and merge cleanly into `team_app_mac_team_ids.json`.
- Spot-check multiple apps to confirm legacy entries still display sane defaults when optional fields are missing.
