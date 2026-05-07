---
title: "P0 — Shared Foundation"
weight: 1
bookFlatSection: true
---

# 🏗️ P0 — Shared Foundation Setup

## Objective

Set up the shared infrastructure that multiple advanced-technology tools depend on — Telegram bot skeleton, SQLite database, cron scheduler, and free-tier hosting. This avoids duplicated effort and ensures all tools share a single bot identity and database.

## Prerequisites

- Node.js ≥ 18 or Python ≥ 3.10 installed locally
- A Telegram account to create a bot via [@BotFather](https://t.me/BotFather)
- A GitHub account (for Actions cron + Pages hosting)
- A free-tier hosting account: Railway.app or Render.com

## Subtasks

### Phase 1: Telegram Bot Skeleton

- [ ] Create a new Telegram bot via BotFather, store the token securely in `.env`
- [ ] Initialize a Node.js project (`npm init`) with `telegraf` as the bot framework
- [ ] Implement a basic `/start` command that replies with a welcome message
- [ ] Implement a `/help` command listing all available commands (placeholder list)
- [ ] Add error handling middleware (catch + log all unhandled errors)
- [ ] Verify the bot responds to `/start` and `/help` locally

### Phase 2: SQLite Database Setup

- [ ] Install `better-sqlite3` (Node.js) — chosen over `sqlite3` for synchronous, simpler API
- [ ] Create a `db/schema.sql` file with the initial schema:
  ```sql
  CREATE TABLE IF NOT EXISTS farmers (
    id INTEGER PRIMARY KEY AUTOINCREMENT,
    telegram_id TEXT UNIQUE NOT NULL,
    name TEXT,
    district TEXT,
    upazila TEXT,
    created_at DATETIME DEFAULT CURRENT_TIMESTAMP
  );

  CREATE TABLE IF NOT EXISTS plots (
    id INTEGER PRIMARY KEY AUTOINCREMENT,
    farmer_id INTEGER NOT NULL,
    name TEXT NOT NULL,
    area_decimal REAL NOT NULL,
    soil_type TEXT,
    latitude REAL,
    longitude REAL,
    crop TEXT,
    start_date DATE,
    created_at DATETIME DEFAULT CURRENT_TIMESTAMP,
    FOREIGN KEY (farmer_id) REFERENCES farmers(id)
  );
  ```
- [ ] Write a `db/init.js` script that creates tables from `schema.sql` on first run
- [ ] Add a database connection utility (`db/connection.js`) with proper close-on-exit handling
- [ ] Seed a test farmer and plot for development

### Phase 3: Cron / Scheduling Infrastructure

- [ ] Install `node-cron` for in-process scheduling
- [ ] Create a `scheduler/index.js` module that:
  - Loads all registered cron jobs from a job registry
  - Logs each job's next execution time on startup
  - Provides `registerJob(name, cronExpr, handler)` API
- [ ] Create a GitHub Actions workflow file (`.github/workflows/daily-cron.yml`) for daily 6 AM BDT execution as a backup/alternative scheduler
- [ ] Verify cron job triggers correctly in local testing

### Phase 4: Project Structure & Configuration

- [ ] Establish the following project structure:
  ```
  agri-bot/
  ├── bot/
  │   ├── commands/        # One file per command (/start, /register, etc.)
  │   ├── middleware/       # Error handler, logger, auth check
  │   └── index.js         # Bot initialization and launch
  ├── db/
  │   ├── schema.sql
  │   ├── connection.js
  │   └── init.js
  ├── scheduler/
  │   └── index.js
  ├── services/            # Business logic (weather, reminders, etc.)
  ├── config/
  │   └── index.js         # Load .env, export config object
  ├── .env.example
  ├── .gitignore
  ├── package.json
  └── README.md
  ```
- [ ] Configure `.env.example` with required variables: `BOT_TOKEN`, `DB_PATH`, `TIMEZONE`
- [ ] Add `.gitignore` for `node_modules/`, `.env`, `*.sqlite`
- [ ] Write a `README.md` with setup instructions

### Phase 5: Deployment

- [ ] Create a `Procfile` or `railway.json` for Railway.app deployment
- [ ] Test deployment on Railway.app free tier (or Render.com)
- [ ] Verify bot responds from the deployed instance
- [ ] Set up environment variables on the hosting platform

## Acceptance Criteria

- [ ] Bot responds to `/start` and `/help` on Telegram (both locally and deployed)
- [ ] SQLite database file is created on first run with the correct schema
- [ ] At least one cron job runs on schedule and logs output
- [ ] Project structure matches the defined layout
- [ ] `.env.example` documents all required environment variables
- [ ] GitHub Actions workflow file is valid YAML

## Estimated Effort

⏱️ **1 day** (4–6 hours focused work)

## Dependencies

None — this is the foundational task.

## Tools That Depend on This

- **P1** Farm Scheduler Bot (A) — uses bot + DB + cron
- **P2** Weather Irrigation Alert (B) — uses bot + cron
- **P4** IoT Soil Monitoring (E) — uses bot for alerts
- **P8** Community Farmer Network (H) — extends the bot
