# 🌐 H. Community Farmer Network (Free Infrastructure)

## Overview

A platform connecting ZBNF farmers in Bangladesh for knowledge sharing, input exchange (e.g., sharing desi cow dung sources), group buying of sensors, and collective market access.

## Problem It Solves

ZBNF farmers in Bangladesh are isolated. They don't know who else is practicing ZBNF nearby, can't find desi cow dung sources, and have no community to troubleshoot problems with. A connected network accelerates adoption and reduces individual risk.

## Components

### 1. Telegram Community Bot

| Feature | How |
|---|---|
| FAQ answers | Keyword matching (no AI needed) — farmer types "jeevamrutha" → bot replies with recipe |
| Pest alerts | Farmer reports pest sighting → bot broadcasts to all members in same upazila |
| Market prices | Weekly manual update of local vegetable prices shared via broadcast |
| Desi cow finder | Farmers register as cow dung suppliers → others search by location |

**Tech:** `telegraf` (Node.js) or `python-telegram-bot` + SQLite. Free hosting.

### 2. ZBNF Knowledge Wiki

| Component | Technology | Cost |
|---|---|---|
| Static site generator | Docusaurus or Hugo | Free |
| Content | Bangla ZBNF guides (translated from this docs/ folder) | Free |
| Hosting | GitHub Pages | Free |
| Search | Algolia DocSearch (free for open-source) or built-in | Free |

A searchable, Bangla-language ZBNF reference website that any farmer can access from their phone browser.

### 3. Farmer Map (Who's practicing ZBNF near me?)

| Component | Technology | Cost |
|---|---|---|
| Map library | Leaflet.js + OpenStreetMap tiles | Free (no API key) |
| Backend | Supabase free tier (50k rows, auth included) or static JSON on GitHub | Free |
| Frontend | Simple HTML/JS page | Free |
| Hosting | Netlify / Vercel | Free |

**Features:**
- Pin your farm on the map with crops grown and ZBNF methods used
- Find nearest desi cow dung/urine source
- See regional pest alerts ("BPH spotted in Mymensingh!")
- Filter by crop type, district, or experience level
- Simple comment thread per pin

### 4. WhatsApp Channel (Broadcast)

| Feature | Details |
|---|---|
| Platform | WhatsApp Channel (free, one-way broadcast) |
| Content | Weekly tips, weather alerts, market prices, success stories |
| Audience | Farmers who prefer WhatsApp over Telegram |
| Cost | Free |

No development needed — just create and curate content.

## Key Design Decisions

- **Telegram + WhatsApp dual channel**: Some farmers use Telegram, others WhatsApp. Cover both.
- **Leaflet + OpenStreetMap over Google Maps**: Completely free. No billing, no API key, no usage limits.
- **Supabase free tier**: 50,000 rows and built-in auth is more than enough for a community of hundreds of farmers.
- **Keyword FAQ over AI chatbot**: Simpler, faster, no API cost. A dictionary of 50-100 ZBNF keywords covers 90% of questions.

## Community Growth Strategy

1. **Start**: You + 5 nearby farmers as a Telegram group
2. **Month 1**: Add the FAQ bot. Share the knowledge wiki link.
3. **Month 3**: Launch the farmer map. Encourage members to pin their farms.
4. **Month 6**: 50+ farmers. Start group buying (seeds, sensors, tools).
5. **Year 1**: Become the regional ZBNF hub. Connect with DAE and NGOs.

## Complexity

🟢 **Beginner** (Telegram bot + WhatsApp channel) — 1–2 days
🟡 **Intermediate** (Farmer map + knowledge wiki) — 1 week

## References

- [Leaflet.js](https://leafletjs.com/)
- [OpenStreetMap](https://www.openstreetmap.org/)
- [Supabase](https://supabase.com/)
- [Docusaurus](https://docusaurus.io/)
