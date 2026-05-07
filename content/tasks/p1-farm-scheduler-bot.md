---
title: "P1 — Farm Scheduler Bot"
weight: 10
bookFlatSection: true
---

# 🔔 P1 — Farm Scheduler Bot (Tool A)

## Objective

Build the Telegram-based farm scheduler and reminder system that sends automated, recurring ZBNF task reminders — Jeevamrutha application, mulch checks, Neemastra spray, and irrigation advisories — calculated per-plot based on area and planting date.

## Prerequisites

- **P0 — Shared Foundation** completed (bot skeleton, SQLite, cron running)
- At least one test farmer + plot registered in the database

## Subtasks

### Phase 1: Plot Registration Command

- [ ] Implement `/register` command that collects:
  - Plot name
  - Area in decimals (with bigha-to-decimal conversion: 1 bigha = 33 decimals)
  - Primary crop
  - Planting date (DD-MM-YYYY format)
- [ ] Support conversational flow (multi-step input with `telegraf` scenes/wizards)
- [ ] Store plot data in the `plots` table
- [ ] Implement `/myplots` command to list all registered plots for a farmer
- [ ] Implement `/deleteplot <name>` to remove a plot
- [ ] Validate inputs (area > 0, valid date, etc.)

### Phase 2: Jeevamrutha Reminder Engine

- [ ] Create a `services/jeevamrutha.js` module with:
  - Batch quantity calculator based on plot area:
    ```javascript
    // Standard: 200L for 33 decimals (1 bigha)
    function calculateBatch(areaDecimal) {
      const ratio = areaDecimal / 33;
      return {
        water_liters: Math.round(200 * ratio),
        cow_dung_kg: +(10 * ratio).toFixed(1),
        cow_urine_liters: +(7.5 * ratio).toFixed(1),
        jaggery_kg: +(2 * ratio).toFixed(1),
        pulse_flour_kg: +(2 * ratio).toFixed(1),
        soil_handful: Math.max(1, Math.round(ratio)),
      };
    }
    ```
  - Message formatter that generates the reminder text in Bangla + English
- [ ] Register a cron job that runs every 15 days from each plot's start date
- [ ] Calculate per-plot batch quantities in the reminder message
- [ ] Send reminder via Telegram to the farmer's chat ID
- [ ] Log each sent reminder in a `reminder_logs` table

### Phase 3: Additional Reminder Types

- [ ] **Mulch check** — Weekly reminder: "Check mulch layer on [plot]. Replenish if below 4 inches."
- [ ] **Neemastra spray** — Every 14 days: "Apply preventive Neemastra spray on [plot]."
- [ ] **Irrigation advisory** — Seasonal cadence: "Check soil moisture before watering [plot] today."
- [ ] Each reminder type stored as a `reminder_type` in the database
- [ ] Create a `reminders` table:
  ```sql
  CREATE TABLE reminders (
    id INTEGER PRIMARY KEY AUTOINCREMENT,
    plot_id INTEGER NOT NULL,
    type TEXT NOT NULL,          -- 'jeevamrutha' | 'mulch' | 'neemastra' | 'irrigation'
    interval_days INTEGER NOT NULL,
    next_due DATE NOT NULL,
    active BOOLEAN DEFAULT 1,
    FOREIGN KEY (plot_id) REFERENCES plots(id)
  );
  ```
- [ ] Auto-create all default reminders when a new plot is registered

### Phase 4: Custom Reminders

- [ ] Implement `/remind` command for farmer-defined reminders
  - One-time: `/remind once 2025-02-01 "Buy seeds from Karim bhai"`
  - Recurring: `/remind every 7 "Inspect compost heap"`
- [ ] Store custom reminders in the same `reminders` table with `type = 'custom'`
- [ ] Support `/myreminders` to list all active reminders
- [ ] Support `/cancelreminder <id>` to deactivate a reminder

### Phase 5: SMS Fallback (Optional)

- [ ] Research BD telco SMS APIs (Banglalink, Grameenphone developer portals)
- [ ] Abstract the notification layer:
  ```
  NotificationService.send(farmerId, message)
    → if farmer has Telegram → send via bot
    → else if farmer has phone number → send via SMS API
  ```
- [ ] Document SMS API costs and rate limits

## Acceptance Criteria

- [ ] `/register` successfully stores a plot with all required fields
- [ ] Jeevamrutha reminder fires every 15 days with correct batch quantities for the plot area
- [ ] All 4 default reminder types auto-activate on plot registration
- [ ] Custom one-time and recurring reminders work correctly
- [ ] `/myplots` and `/myreminders` display correct data
- [ ] Reminder messages include both Bangla and English text
- [ ] `reminder_logs` table records every sent reminder

## Estimated Effort

⏱️ **1–2 days** (8–12 hours)

## Dependencies

| Dependency | Status |
|---|---|
| P0 — Shared Foundation | Must be completed |
