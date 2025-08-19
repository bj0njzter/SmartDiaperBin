# Diaper Tracker – Home Assistant

Track diaper disposals per bag, day, week, and month, undo the last entry, keep a last‑five list, and voice‑query the most recent change. The project ships with blueprints, a Lovelace dashboard, and a shareable package. Optional auto‑calibration increases bag capacity when a finished bag exceeds the current setting.

## Features
- Count on lid **close** (robust to `off/closed` sensors)
- **Per‑bag** counter with `last_period` on reset
- **Per‑day/week/month** via utility meters
- **Time‑of‑day** (Night/Morning/Afternoon/Evening) monthly totals
- **Last 5** timestamps table
- **Undo Last** with safe `utility_meter.calibrate`
- **Auto‑calibrate capacity** (opt‑in) when a bag is emptied
- Voice‑command scripts
- Clean Slate script for quick testing resets
- Ready‑to‑import **Blueprints**
- Lovelace dashboard in English

## Install
1. Copy `packages/diaper/diaper.yaml` into a directory included by `packages:` in your Home Assistant configuration.
2. (Optional) Import the blueprints via HACS (Custom Repository → this repo → Category: Blueprints) or copy YAML files from `blueprints/automation/smartdiaperbin/`.
3. Import `dashboards/diaper_dashboard.yaml` via the Lovelace Raw editor (or copy the relevant cards).
4. Set the correct entity id for your **bin contact sensor** in the package or automation.

## Entities
- `sensor.diaper_bag` – current bag count; `last_period` holds the just‑finished bag size after reset.
- `sensor.diaper_daily/weekly/monthly` – period totals.
- `sensor.diaper_tod_night/morning/afternoon/evening` – time‑of‑day totals (current month).
- `input_datetime.last_diaper_time` – last disposal timestamp.
- `input_text.diaper_recent_times` – last 5 ISO timestamps (newest first).
- `input_number.diaper_bag_capacity` – capacity (auto‑increases on empty if enabled).

## Voice
Expose scripts under `script:` via Nabu Casa → Google Assistant, or call them from Assist/Shortcuts.

## Undo
Use `script.diaper_undo_last` to reverse the most recent registration. It decrements counters only if the original timestamp falls within the current periods and bag.

## Troubleshooting
- If the daily graph is empty, verify the lid close automation fires and `counter.diaper_count` increments. Check the binary sensor id and its states.
- If the Last 5 list is empty, the close automation may not be firing; use the test script to inject a timestamp.
- The 3‑day average updates at 00:05 using completed days (`last_period`).

## License
MIT

