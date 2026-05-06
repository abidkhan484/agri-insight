# 📱 F. ZBNF Knowledge Base PWA (Offline-First)

## Overview

A mobile web app that works **fully offline** — farmers open it on any Android phone and get the complete ZBNF guide, recipe calculators, pest identification gallery, and seasonal crop calendar. No internet needed in the field.

## Problem It Solves

Farmers learn ZBNF techniques at workshops but forget exact recipes and ratios when they're actually in the field. Printed materials get damaged. Internet isn't available in rural areas. An offline-first PWA solves all three problems.

## Core Features

| Feature | Description |
|---|---|
| **Recipe calculator** | Input: acreage → Output: exact Jeevamrutha/Beejamrutha quantities |
| **Countdown timer** | Days until next Jeevamrutha application (per plot) |
| **Pest photo gallery** | Static images of common BD pests with ZBNF treatment for each |
| **Formulation step-by-step** | Jeevamrutha, Beejamrutha, Neemastra, Agniastra, Brahmastra — with photos |
| **Crop calendar** | Region-specific planting calendar (Kharif / Rabi / Zaid for each BD division) |
| **Troubleshooting guide** | "Yellow leaves?" → Check list of possible causes and ZBNF solutions |
| **Offline video cache** | Short tutorial videos cached on first load |
| **Soil health checklist** | Weekly/monthly soil observation guide with scoring |

## Tech Stack

| Component | Technology | Cost |
|---|---|---|
| Framework | React + Vite or plain HTML/CSS/JS | Free |
| Offline engine | Service Worker + Workbox (Google's SW library) | Free |
| Local storage | IndexedDB via Dexie.js (for user data like plot info) | Free |
| Static content | Hardcoded JSON files for recipes, crop calendar, pest gallery | Free |
| Hosting | GitHub Pages / Netlify / Cloudflare Pages | Free |
| Build tool | Vite | Free |

## Content Structure

```
/data
  ├── recipes.json         # All ZBNF formulation recipes with quantities
  ├── pests.json            # Pest name, photo filename, symptoms, ZBNF treatment
  ├── crops.json            # Crop calendar per BD division
  ├── troubleshooting.json  # Symptom → cause → ZBNF solution mapping
  └── soil-checklist.json   # Weekly/monthly observation items
/images
  ├── pests/               # Static pest photos (compressed WebP)
  ├── formulations/        # Step-by-step photos of recipe preparation
  └── crops/               # Crop reference images
/videos
  └── tutorials/           # Short (<2 min) tutorial videos (compressed)
```

## Recipe Calculator Logic

```javascript
function calculateJeevamrutha(areaDecimal) {
  // Standard ratio: 200L water for 33 decimals (1 bigha)
  const ratio = areaDecimal / 33;
  return {
    water_liters: Math.round(200 * ratio),
    cow_dung_kg: Math.round(10 * ratio * 10) / 10,
    cow_urine_liters: Math.round(7.5 * ratio * 10) / 10,
    jaggery_kg: Math.round(2 * ratio * 10) / 10,
    pulse_flour_kg: Math.round(2 * ratio * 10) / 10,
    soil_handful: Math.max(1, Math.round(ratio)),
  };
}
```

## Key Design Decisions

- **Content-first, not feature-first**: The most important thing is that formulation recipes are instantly accessible offline. Everything else is secondary.
- **Static JSON over database**: Recipes and pest data rarely change. Hardcoded JSON is faster, simpler, and works perfectly offline.
- **WebP images**: Compressed to minimize storage. A full pest gallery (50 images) should be < 5 MB total.
- **Bangla-first UI**: All text in Bangla. Icons and visuals heavily used — many farmers read slowly.
- **Installable PWA**: Appears on home screen like a native app. No Play Store needed.

## Offline Strategy

1. On first visit (needs internet), Service Worker caches:
   - All HTML/CSS/JS
   - All JSON data files
   - All images and videos
2. After initial cache (~20-30 MB), app works **100% offline forever**.
3. When internet is available, SW checks for content updates and refreshes cache silently.

## Complexity

🟡 **Intermediate** — 1 week for a solid v1.

## References

- [Workbox (Google's Service Worker library)](https://developer.chrome.com/docs/workbox/)
- [Dexie.js](https://dexie.org/)
- [Vite PWA plugin](https://vite-pwa-org.netlify.app/)
