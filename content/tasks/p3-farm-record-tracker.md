---
title: "P3 — Farm Record Tracker"
weight: 30
bookFlatSection: true
---

# 📊 P3 — Farm Record Keeping & Yield Tracker (Tool D)

## Objective

Build a React PWA with fully offline IndexedDB storage that lets farmers log ZBNF inputs, costs, crop observations, and harvest yields — with auto-generated seasonal comparison reports proving ZBNF savings.

## Prerequisites

- **P0 — Shared Foundation** completed (reuses plot data model concepts)
- Node.js ≥ 18, npm/pnpm installed

## Subtasks

### Phase 1: Project Setup (React + Vite PWA)

- [ ] Initialize project: `npx -y create-vite@latest ./ -- --template react`
- [ ] Install dependencies:
  ```bash
  npm install dexie dexie-react-hooks chart.js react-chartjs-2 jspdf papaparse
  npm install -D vite-plugin-pwa workbox-precaching
  ```
- [ ] Configure `vite-plugin-pwa` in `vite.config.js` with:
  - `registerType: 'autoUpdate'`
  - `manifest` with app name "কৃষি রেকর্ড" (Krishi Record), icons, theme color
  - `workbox.runtimeCaching` for all static assets
- [ ] Add Bangla + English language support structure (`i18n/bn.json`, `i18n/en.json`)

### Phase 2: IndexedDB Data Model (Dexie.js)

- [ ] Create `db/index.js` with Dexie database definition:
  ```javascript
  import Dexie from 'dexie';

  const db = new Dexie('agri-record');
  db.version(1).stores({
    plots: '++id, name, area_decimal, soil_type',
    inputLogs: '++id, plot_id, date, type, quantity, cost_bdt',
    observations: '++id, plot_id, date, earthworm_count, pest_sighting',
    harvests: '++id, plot_id, date, crop, quantity_kg, revenue_bdt, season'
  });
  ```
- [ ] Create CRUD helper functions for each table
- [ ] Add data validation before writes (area > 0, cost ≥ 0, etc.)

### Phase 3: Core UI — Plot Management

- [ ] Plot registration form: name, area (decimal + bigha converter), soil type
- [ ] Plot list view with edit/delete
- [ ] Plot detail view showing all logs, observations, and harvests for that plot
- [ ] Bangla labels on all form fields with icon hints

### Phase 4: Input Logging

- [ ] Input log form with fields: date, type (dropdown: Jeevamrutha/Beejamrutha/Neemastra/Agniastra/Mulch/Other), quantity, cost (৳)
- [ ] Auto-calculate Jeevamrutha batch quantities from plot area (reuse calculator from Tool F spec)
- [ ] Input log list with date filtering and type grouping
- [ ] Quick-add buttons for common inputs (one-tap "Applied Jeevamrutha today")

### Phase 5: Observations & Harvest Recording

- [ ] Observation form: date, earthworm count, pest sighting (text + optional photo via camera API)
- [ ] Harvest form: date, crop, quantity (kg), revenue (৳), season tag (Kharif/Rabi/Zaid)
- [ ] Timeline view showing all activities (inputs + observations + harvests) chronologically

### Phase 6: Reports & Charts

- [ ] **Cost comparison chart** (Chart.js bar chart): ZBNF season total cost vs previous chemical season (manual input for chemical baseline)
- [ ] **Yield trend chart** (line chart): kg per decimal over seasons
- [ ] **Input frequency chart** (stacked bar): which formulations applied how often
- [ ] **Soil health index** (line chart): earthworm count trend + moisture observations
- [ ] **ROI calculator**: Revenue - Input Cost = Net Profit per bigha, displayed as a card

### Phase 7: Export

- [ ] CSV export of all data (Papa Parse) — one CSV per table
- [ ] PDF seasonal report (jsPDF):
  - Summary card: total cost, total revenue, net profit
  - Charts embedded as PNG (canvas → image)
  - Printable A4 layout with Bangla headers
- [ ] Share via native Web Share API (if available) or download

### Phase 8: Offline & PWA

- [ ] Verify full offline functionality after first load
- [ ] Add install prompt banner ("Add to Home Screen")
- [ ] Test on Android Chrome with airplane mode
- [ ] Confirm app icon appears on home screen correctly

## Acceptance Criteria

- [ ] App installs as PWA and works 100% offline
- [ ] All CRUD operations persist in IndexedDB across app restarts
- [ ] Cost comparison and yield trend charts render correctly
- [ ] CSV and PDF exports contain accurate data
- [ ] All UI labels available in Bangla
- [ ] App size < 2 MB (excluding user data)

## Estimated Effort

⏱️ **5–7 days** (1 week)

## Dependencies

| Dependency | Status |
|---|---|
| P0 — Shared Foundation | Completed (data model reference) |
