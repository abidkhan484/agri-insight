---
title: "P8 — Community Farmer Network"
weight: 80
bookFlatSection: true
---

# 🌐 P8 — Community Farmer Network (Tool H)

## Objective

Build a multi-component community platform — Telegram FAQ bot, Leaflet-based farmer map, ZBNF knowledge wiki, and WhatsApp broadcast channel — connecting ZBNF farmers in Bangladesh for knowledge sharing, resource exchange, and collective action.

## Prerequisites

- **P0** completed (Telegram bot infrastructure)
- **P1** completed (bot framework and DB patterns)
- Supabase free account (for farmer map backend)

## Subtasks

### Phase 1: Telegram Community Bot Extensions

- [ ] Add keyword-based FAQ: farmer types "jeevamrutha" → bot replies with recipe
- [ ] Build FAQ dictionary with 50–100 ZBNF keywords in `data/faq.json`
- [ ] Implement pest alert broadcast: farmer reports pest → bot notifies all in same upazila
- [ ] Add desi cow finder: `/registercow` to register as supplier, `/findcow <district>` to search
- [ ] Weekly market price broadcast (manual update via admin command `/setprice`)

### Phase 2: Farmer Map (Leaflet + Supabase)

- [ ] Set up Supabase project (free tier: 50k rows, auth included)
- [ ] Create `farmers_map` table: id, name, lat, lng, crops, methods, district, upazila, created_at
- [ ] Build HTML/JS page with Leaflet.js + OpenStreetMap tiles (no API key needed)
- [ ] Features:
  - [ ] Pin your farm with crops grown and ZBNF methods
  - [ ] Find nearest desi cow dung source (filter cow suppliers)
  - [ ] Regional pest alert pins ("BPH spotted in Mymensingh!")
  - [ ] Filter by crop type, district, experience level
- [ ] Simple registration form (Supabase auth or anonymous with name)
- [ ] Deploy to Netlify/Vercel

### Phase 3: ZBNF Knowledge Wiki

- [ ] Set up Hugo or Docusaurus site with Bangla ZBNF content
- [ ] Translate key sections from this project's `docs/` folder to Bangla
- [ ] Add Algolia DocSearch (free for open-source) or built-in search
- [ ] Deploy to GitHub Pages
- [ ] Mobile-optimized reading experience

### Phase 4: WhatsApp Broadcast Channel

- [ ] Create WhatsApp Channel (no development — content curation)
- [ ] Document content schedule: weekly tips, weather alerts, market prices, success stories
- [ ] Create content templates for consistent formatting

## Acceptance Criteria

- [ ] FAQ bot responds to 50+ ZBNF keywords correctly
- [ ] Pest alert broadcasts reach all farmers in the same upazila
- [ ] Farmer map displays pins with correct data, filterable by crop/district
- [ ] Knowledge wiki is searchable and accessible on mobile
- [ ] WhatsApp channel is created with initial content plan

## Estimated Effort

⏱️ **1–2 days** (bot + WhatsApp) + **3–5 days** (map + wiki) = **4–7 days total**

## Dependencies

| Dependency | Status |
|---|---|
| P0 — Shared Foundation | Must be completed |
| P1 — Farm Scheduler Bot | Must be completed (bot patterns) |
| Supabase account | Required for farmer map |
| Content translation to Bangla | Required for wiki |
