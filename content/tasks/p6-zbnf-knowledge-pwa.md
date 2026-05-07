---
title: "P6 — ZBNF Knowledge PWA"
weight: 60
bookFlatSection: true
---

# 📱 P6 — ZBNF Knowledge Base PWA (Tool F)

## Objective

Build an offline-first PWA that gives farmers instant access to ZBNF formulation recipes, batch calculators, pest photo gallery, crop calendar, and troubleshooting guides — all cached on first load, works without internet.

## Prerequisites

- Node.js ≥ 18
- Content: ZBNF recipes, pest photos, crop calendar data prepared

## Subtasks

### Phase 1: Project Setup

- [ ] Initialize React + Vite project with `vite-plugin-pwa`
- [ ] Configure manifest: name "ZBNF জ্ঞানভাণ্ডার", icons, `display: standalone`
- [ ] Set up Bangla-first UI with Inter/Noto Sans Bengali fonts
- [ ] Create content data structure:
  ```
  /data/recipes.json, pests.json, crops.json, troubleshooting.json, soil-checklist.json
  /images/pests/, /images/formulations/, /images/crops/
  ```

### Phase 2: Recipe Calculator

- [ ] Implement Jeevamrutha batch calculator: input area (decimal) → output quantities
- [ ] Implement Beejamrutha, Neemastra, Agniastra, Brahmastra calculators
- [ ] Countdown timer: days until next application per plot (uses IndexedDB for plot data)
- [ ] Visual step-by-step preparation guide with photos

### Phase 3: Pest Photo Gallery

- [ ] Create `pests.json` with 30+ common BD pests: name (Bangla/English), photo, symptoms, ZBNF treatment
- [ ] Gallery UI with search/filter by crop type
- [ ] Tap for detail view with full treatment protocol
- [ ] Compress all images to WebP, target <50KB each, total gallery <5MB

### Phase 4: Crop Calendar

- [ ] Create `crops.json` with planting windows per BD division (8 divisions)
- [ ] Three seasons: Kharif (monsoon), Rabi (winter), Zaid (summer)
- [ ] Interactive calendar view: select division → see recommended crops per month
- [ ] Highlight current month and upcoming planting windows

### Phase 5: Troubleshooting Guide

- [ ] Create `troubleshooting.json`: symptom → possible causes → ZBNF solutions
- [ ] Examples: "Yellow leaves?", "Wilting?", "White powder on leaves?", "Stunted growth?"
- [ ] Decision tree UI: farmer selects symptoms → narrows down diagnosis
- [ ] Link to relevant pest gallery entries and formulation recipes

### Phase 6: Offline Caching & PWA

- [ ] Configure Workbox to precache all HTML/CSS/JS, JSON data, and images
- [ ] Total cache target: 20–30 MB (including compressed pest photos and tutorial videos)
- [ ] Implement cache-first strategy with background update check
- [ ] Add install prompt banner
- [ ] Test: full functionality in airplane mode after first visit

## Acceptance Criteria

- [ ] All recipe calculators return correct quantities for any input area
- [ ] Pest gallery loads with photos and searchable by crop
- [ ] Crop calendar displays correct planting windows per division
- [ ] Troubleshooting guide navigates symptom → cause → solution
- [ ] PWA installs and works 100% offline
- [ ] All text in Bangla, total app size < 30 MB

## Estimated Effort

⏱️ **5–7 days** (1 week)

## Dependencies

| Dependency | Status |
|---|---|
| Content preparation (recipes, pest photos, crop data) | Required |
| No code dependencies on other tasks | — |
