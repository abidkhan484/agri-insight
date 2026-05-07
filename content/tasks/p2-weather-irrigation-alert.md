---
title: "P2 — Weather Irrigation Alert"
weight: 20
bookFlatSection: true
---

# 🌦️ P2 — Weather-Based Smart Irrigation Alert (Tool B)

## Objective

Build a lightweight weather service that checks daily forecasts via Open-Meteo API and sends irrigation/spray/heat advisories to farmers via the existing Telegram bot — enforcing the ZBNF Whapasa principle with data, not guesswork.

## Prerequisites

- **P0 — Shared Foundation** completed (bot + cron)
- **P1 — Farm Scheduler Bot** completed (plots with GPS coordinates stored in DB)

## Subtasks

### Phase 1: Open-Meteo API Integration

- [ ] Create a `services/weather.js` module
- [ ] Implement `fetchForecast(latitude, longitude)` function:
  ```
  GET https://api.open-meteo.com/v1/forecast
    ?latitude={lat}&longitude={lon}
    &daily=precipitation_sum,temperature_2m_max,temperature_2m_min
    &hourly=precipitation
    &timezone=Asia/Dhaka
    &forecast_days=3
  ```
- [ ] Parse the API response into a structured object:
  ```javascript
  {
    today: { precip_mm, temp_max, temp_min },
    tomorrow: { precip_mm, temp_max, temp_min },
    next48h_precip_total: Number,
    rain_in_next_6h: Boolean
  }
  ```
- [ ] Add error handling for API failures (retry with exponential backoff, max 3 attempts)
- [ ] Add unit tests for response parsing

### Phase 2: Decision Logic

- [ ] Implement `services/irrigation-advisor.js` with the following rules:

  | Condition | Alert |
  |---|---|
  | `next48h_precip_total > 5mm` | 🌧️ "Skip irrigation on [plot] — rain expected" |
  | `next48h_precip_total <= 5mm` and soil is dry | ☀️ "No rain forecast. Irrigate [plot] if soil feels dry." |
  | `rain_in_next_6h === true` | 🚫 "Don't spray Neemastra/Agniastra today — rain will wash it off" |
  | `temp_max > 38°C` | 🔥 "Extreme heat alert: Apply extra mulch, water early morning only" |
  | Multi-day rain pattern (3+ days) | 🌊 "Monsoon approaching — prepare drainage channels" |

- [ ] Each rule returns a message + severity level (info / warning / critical)
- [ ] Support per-plot decisions (different GPS → different weather)

### Phase 3: Daily Cron Job

- [ ] Register a daily cron job at **6:00 AM BDT** (00:00 UTC)
- [ ] Job workflow:
  1. Fetch all active plots from DB
  2. Group plots by GPS coordinates (avoid duplicate API calls for same location)
  3. Fetch weather for each unique coordinate
  4. Run decision logic per plot
  5. Send relevant alerts to each farmer via Telegram
- [ ] Log all alerts in a `weather_alerts` table:
  ```sql
  CREATE TABLE weather_alerts (
    id INTEGER PRIMARY KEY AUTOINCREMENT,
    plot_id INTEGER NOT NULL,
    alert_type TEXT NOT NULL,
    message TEXT NOT NULL,
    forecast_data TEXT,        -- JSON blob of the raw forecast
    sent_at DATETIME DEFAULT CURRENT_TIMESTAMP,
    FOREIGN KEY (plot_id) REFERENCES plots(id)
  );
  ```

### Phase 4: GitHub Actions Cron (Backup)

- [ ] Create `.github/workflows/weather-alert.yml`:
  ```yaml
  name: Daily Weather Alert
  on:
    schedule:
      - cron: '0 0 * * *'   # 6 AM BDT = 00:00 UTC
  jobs:
    alert:
      runs-on: ubuntu-latest
      steps:
        - uses: actions/checkout@v4
        - uses: actions/setup-node@v4
          with:
            node-version: 20
        - run: npm ci
        - run: node scripts/run-weather-check.js
          env:
            BOT_TOKEN: ${{ secrets.BOT_TOKEN }}
            DB_PATH: ./data/agri.sqlite
  ```
- [ ] Create `scripts/run-weather-check.js` as a standalone entry point
- [ ] Test the workflow with `workflow_dispatch` trigger

## Acceptance Criteria

- [ ] Weather data fetches correctly for any BD GPS coordinate
- [ ] Irrigation skip alert fires when precipitation > 5mm in next 48h
- [ ] Spray warning fires when rain expected within 6h
- [ ] Heat alert fires when max temp > 38°C
- [ ] Alerts are sent to the correct farmer via Telegram
- [ ] `weather_alerts` table logs every sent alert
- [ ] GitHub Actions workflow runs without error

## Estimated Effort

⏱️ **1 day** (4–6 hours)

## Dependencies

| Dependency | Status |
|---|---|
| P0 — Shared Foundation | Must be completed |
| P1 — Farm Scheduler Bot | Must be completed (for plots with GPS data) |
