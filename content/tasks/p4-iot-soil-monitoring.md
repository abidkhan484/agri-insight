---
title: "P4 — IoT Soil Monitoring"
weight: 40
bookFlatSection: true
---

# 🌡️ P4 — Low-Cost IoT Soil Monitoring (Tool E)

## Objective

Build a hardware + software system using ESP32 + capacitive soil moisture sensor + DHT22 for real-time soil data with MQTT pipeline, Grafana dashboards, and Telegram alerts enforcing ZBNF Whapasa thresholds.

## Prerequisites

- **P0** completed (Telegram bot for alerts)
- **P3** completed (data model reference)
- Hardware: ESP32, capacitive soil moisture sensor, DHT22, jumper wires, breadboard
- Arduino IDE with ESP32 board support

## Subtasks

### Phase 1: ESP32 Firmware

- [ ] Set up Arduino IDE with ESP32 board package
- [ ] Wire capacitive soil moisture sensor (analog) + DHT22 (digital + 10kΩ pull-up)
- [ ] Write firmware (`firmware/soil_monitor.ino`): read sensors, connect WiFi, publish MQTT every 5 min
- [ ] MQTT topic: `farm/{plot_id}/sensors` with JSON payload `{moisture, temp, humidity, ts}`
- [ ] Add deep sleep between readings to conserve battery
- [ ] Calibrate moisture: dry soil = 0%, water = 100%

### Phase 2: MQTT Broker

- [ ] Dev: Use HiveMQ public broker (`broker.hivemq.com:1883`)
- [ ] Prod: Set up Mosquitto on Raspberry Pi with authentication
- [ ] Document topic naming: `farm/{plot_id}/sensors`, `farm/{plot_id}/alerts`, `farm/{plot_id}/config`

### Phase 3: Node-RED Data Processing

- [ ] Install Node-RED, create flow subscribing to `farm/+/sensors`
- [ ] Evaluate Whapasa thresholds: <30% dry, 30-40% getting dry, 40-70% ideal, 70-80% wet, >80% waterlogged
- [ ] Send alerts to Telegram bot, forward data to InfluxDB/Grafana
- [ ] Export flow as `flows/soil-monitoring.json`

### Phase 4: Grafana Dashboard

- [ ] Set up Grafana + InfluxDB data source
- [ ] Panels: real-time moisture gauge, 24h trend line, temp/humidity chart, alert history table
- [ ] Per-plot selector dropdown, export dashboard JSON

### Phase 5: Telegram Alert Integration

- [ ] Format alerts with emoji + plot name + reading + recommendation
- [ ] Alert cooldown: no duplicate alert type per plot within 2 hours
- [ ] Add `/soilstatus` and `/setthreshold` commands to bot

### Phase 6: Field Deployment Docs

- [ ] Document waterproof enclosure build
- [ ] Document solar power setup (5V panel → TP4056 → 18650 → ESP32)
- [ ] Field deployment checklist

## Acceptance Criteria

- [ ] ESP32 publishes sensor data every 5 min via MQTT
- [ ] Node-RED evaluates thresholds and fires Telegram alerts correctly
- [ ] Grafana dashboard shows real-time + historical data
- [ ] Alert cooldown prevents duplicate alerts within 2 hours
- [ ] System runs 24+ hours unattended

## Estimated Effort

⏱️ **3–5 days**

## Dependencies

| Dependency | Status |
|---|---|
| P0 — Shared Foundation | Must be completed |
| P3 — Farm Record Tracker | Recommended |
| Hardware procurement | Required before Phase 1 |
