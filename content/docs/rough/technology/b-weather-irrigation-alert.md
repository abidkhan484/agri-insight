# 🌦️ B. Weather-Based Smart Irrigation Alert

## Overview

A lightweight service that checks the weather forecast daily and alerts the farmer whether to irrigate or skip — respecting the ZBNF **Whapasa** principle (soil needs both air and water, never waterlogging).

## Problem It Solves

Farmers often irrigate on a fixed schedule regardless of incoming rain. This wastes water, damages soil aeration, and kills beneficial microbes. In ZBNF, Whapasa (moisture + air balance) is a core pillar.

## Core Features

| Feature | Description |
|---|---|
| Daily forecast check | Fetches 48-hour rainfall prediction for the farmer's GPS coordinates |
| Irrigation decision | If rain > 5mm expected in next 48 hrs → "Skip irrigation" alert |
| Spray advisory | If rain expected within 6 hrs → "Don't spray Neemastra/Agniastra today — rain will wash it off" |
| Temperature alert | If temp > 38°C → "Apply extra mulch, water early morning only" |
| Monsoon onset | Notify when monsoon is approaching based on multi-day rain patterns |

## Tech Stack

| Component | Technology | Cost |
|---|---|---|
| Weather API | [Open-Meteo](https://open-meteo.com) — **no API key, no registration, no rate limits** | Free |
| Scheduler | GitHub Actions cron (runs daily at 6 AM BDT) or `node-cron` on free host | Free |
| Notification | Telegram Bot (reuse bot from Tool A) | Free |
| Logic | Simple Node.js or Python script (< 100 lines) | Free |
| Hosting | GitHub Actions (2000 free minutes/month) — no server needed | Free |

## How It Works

```
[Daily 6 AM cron] →
  Fetch: https://api.open-meteo.com/v1/forecast
    ?latitude=23.8&longitude=90.4
    &daily=precipitation_sum,temperature_2m_max
    &timezone=Asia/Dhaka
  →
  If precipitation_sum[today or tomorrow] > 5mm:
    Send: "🌧️ Skip irrigation — 12mm rain expected tomorrow."
  Else:
    Send: "☀️ No rain expected. Irrigate if soil feels dry 2 inches below surface."
  →
  If temperature_2m_max > 38:
    Send: "🔥 Extreme heat — add extra mulch layer, water before 7 AM only."
```

## Key Design Decisions

- **Open-Meteo over OpenWeather**: Open-Meteo requires zero registration, zero API key, and has no rate limits for personal use. OpenWeather's free tier works too (1000 calls/day) but needs a key.
- **GitHub Actions as cron**: No server to maintain. A single YAML file runs the script daily for free. Falls well within the 2000 minutes/month limit.
- **GPS per plot**: Each farmer's plots can have different coordinates. Store them in the SQLite DB from Tool A.

## Open-Meteo API Example

```
GET https://api.open-meteo.com/v1/forecast
  ?latitude=23.81
  &longitude=90.41
  &daily=precipitation_sum,temperature_2m_max,temperature_2m_min
  &timezone=Asia%2FDhaka
  &forecast_days=3
```

No key. No signup. Just fetch.

## Complexity

🟢 **Beginner** — 1 day to build.

## References

- [Open-Meteo API docs](https://open-meteo.com/en/docs)
- [GitHub Actions cron syntax](https://docs.github.com/en/actions/using-workflows/events-that-trigger-workflows#schedule)
