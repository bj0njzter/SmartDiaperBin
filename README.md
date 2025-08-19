# Diaper Tracker – Home Assistant

Track diaper disposals per bag/day/week/month, get per–time-of-day stats, and voice-query last change.
Includes blueprints, a Lovelace dashboard, and a shareable package.

## Install
1. Copy `packages/diaper/diaper.yaml` into your HA `packages` include.
2. (Optional) Import blueprints via HACS (Custom Repository → this repo → Category: Blueprints),
   or manually drop `blueprints/automation/...` into your config.
3. Add `dashboards/diaper_dashboard.yaml` via the UI (Raw config editor).

## Entities you’ll use
- **sensor.diaper_bag** – current bag count (resets on empty). `last_period` = previous bag count.
- **sensor.diaper_daily/weekly/monthly** – period totals.
- **sensor.diaper_tod_night/morning/afternoon/evening** – monthly time-of-day totals.
- **input_datetime.last_diaper_time** – last event timestamp.
- **input_text.diaper_recent_times** – last 5 ISO timestamps (newest first).

## Buttons
- `input_button.diaper_bag_taken_out` → empties bag (resets `sensor.diaper_bag`) and starts a new bag.
- Script `script.diaper_undo_last` → reverses the last registration.

## Troubleshooting
- If the “per day” graph is empty, ensure `counter.diaper_count` increments on lid close (check your binary sensor id).
- “Last 5” list is empty → same root cause: the close automation must fire.
