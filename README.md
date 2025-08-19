# Diaper Tracker – Home Assistant

Track diaper disposals per bag/day/week/month, get per–time-of-day stats, undo the last event, and voice-query the last change. Includes blueprints, a Lovelace dashboard, and a shareable package. Optional auto-calibration increases capacity when a finished bag exceeds the current capacity.

## Features
- Count on lid **close** (robust to `off/closed` sensors)
- **Per-bag** counter with `last_period` on reset
- **Per-day/week/month** via utility_meters
- **Time-of-day** (Night/Morning/Afternoon/Evening) monthly totals
- **Last 5** timestamps table
- **Undo Last** with safe `utility_meter.calibrate`
- **Auto-calibrate capacity** (opt-in) on bag empty
- Voice-command scripts (Google/Nabu Casa friendly)
- Clean Slate script for quick reset during testing
- Ready-to-import **Blueprints**
- Lovelace Dashboard in English

## Install
1. Copy `packages/diaper/diaper.yaml` into a directory included by `packages:` in your HA configuration.
2. (Optional) Import blueprints via HACS (Custom Repository → this repo → Category: Blueprints), or copy YAMLs under `blueprints/automation/...`.
3. Import `dashboards/diaper_dashboard.yaml` via Lovelace Raw editor (or copy relevant cards).
4. Set the right entity id for your **bin contact sensor** (see comments in the YAML).

## Entities you’ll use
- `sensor.diaper_bag` — current bag count; `last_period` holds the just-finished bag size after reset.
- `sensor.diaper_daily/weekly/monthly` — period totals.
- `sensor.diaper_tod_night/morning/afternoon/evening` — time-of-day totals (current month).
- `input_datetime.last_diaper_time` — last disposal timestamp.
- `input_text.diaper_recent_times` — last 5 ISO timestamps (newest first).
- `input_number.diaper_bag_capacity` — capacity (auto-increases on empty if enabled).

## Voice
Expose scripts under `script:` via Nabu Casa → Google Assistant, and create routines mapping your phrases to **activate** those scripts, or call them from Assist/Shortcuts.

## Undo
Use `script.diaper_undo_last` to reverse the most recent registration. It decrements the lifetime counter, the bag meter, and calibrates daily/weekly/monthly/time-of-day meters **only if** the undone timestamp falls within those periods.

## Troubleshooting
- If daily graph is empty: verify the lid close automation fires and `counter.diaper_count` increments. Check the binary sensor id and its states.
- If Last 5 is empty: same root cause; use the test script to inject a timestamp.
- 3‑day average updates at 00:05 using completed days (via `last_period`).

## License
MIT
