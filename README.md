# DWR-On-Call
Alert system for on-call engineer and resource for basin conditions and forecasting.



* Build a web-first Python app (runs locally or in the cloud) with:

* FastAPI (data API + alert engine + background jobs)

* A lightweight UI (either Streamlit for fastest MVP or a small React/Vue front-end) that works on desktop and phone

* Scheduler (APScheduler) to poll your source links (USGS/NOAA/RFC/SNOTEL/etc.)

* Rules engine for alerts (thresholds, rate-of-change, ensemble exceedance)

* Notifications via SMS/Push (Twilio SMS, Pushover/Telegram, or web push)

* SQLite (MVP) → Postgres (later) to store pulls and alert state

* Packaging: run in Docker or as a local app; mobile use via PWA (add to home screen) so you get a phone-friendly “app” without separate native builds

*** This gives you one place on your computer (or a VPS) to see all basin conditions at once and get phone alerts.
.
.
.
.
.
2) Step-by-step (how to reach it)
A. Define the MVP

Inputs: your specific URLs (JSON, CSV, XML, HTML pages—whatever the agencies publish)

Core metrics: e.g., flow (cfs), stage (ft), SWE, precip, reservoir storage, forecast traces

Basin config: YAML/JSON listing stations, parameters, thresholds, units, tz

Alerts: simple thresholds first (e.g., stage >= 12.0 ft), then derivatives (rate of rise), then forecast exceedance (prob. > 30% to exceed X)

B. Architecture

FastAPI service with routes:

GET /dashboard (Streamlit or a small front-end)

GET /api/latest?basin=…

GET /api/alerts (current & recent)

POST /api/subscribe (add phone/push endpoint)

Ingestion jobs (APScheduler):

Every 5–15 min, pull the configured links, normalize units/timezones, cache to DB

Rules engine:

Evaluate new data against per-basin thresholds

Debounce & rate-limit alerts; store sent-alert hashes to avoid spam

Notifications:

SMS (Twilio) or push (Pushover/Telegram/Web Push)

Storage:

SQLite for MVP (sqlmodel/SQLAlchemy) → easy to deploy anywhere

C. UI (two good paths)

Fastest: Streamlit app embedded or launched alongside FastAPI

Pros: hours to first dashboard; built-in charts; great for ops

Cons: less control, but fine for a regulator’s cockpit

Polished: small React UI (Vite) + Recharts/Plotly + responsive layout

Pros: best mobile UX + PWA installable; Cons: a bit more setup

D. Packaging/Deploy

Local: uv/pipx + .env → run on laptop (great for on-call)

Server: Docker Compose on a VPS (fly.io, Render, Lightsail) to keep polling 24/7

PWA: add manifest + service worker for “install to phone” feel

E. Ops concerns

Timezones: use zoneinfo("America/Boise")

Retries/circuit-breakers for flaky upstreams

Logging & audit trail for decisions (helpful if you explain release choices)

Health checks: /healthz route; alert if data goes stale

3) Alternatives & trade-offs

Kivy/BeeWare (pure Python desktop/mobile): possible, but mobile packaging and push alerts are harder. Web-first + PWA is faster to operate.

No scheduler (cron only): simpler but you lose in-process state and debounce; APScheduler keeps it all in one place.

Email vs SMS vs Push: SMS is universal (Twilio cost). Push (Pushover/Telegram) is cheap/fast. Web Push needs HTTPS + browser permissions.

4) Practical summary / action plan
Milestone 1 — “Hello Basin” MVP (1–2 sessions)

Create repo; add FastAPI, APScheduler, pandas, pydantic, sqlmodel, httpx.

Add basins.yaml with stations/params/thresholds.

Write one ingestor for a JSON endpoint (swap in your first link).

Store latest observations in SQLite.

Evaluate a simple threshold; send a test SMS via Twilio (or Pushover).

Build a Streamlit view to see current values + sparkline.

Milestone 2 — Forecasts & rules

Add a forecast ingestor (RFC/NBM/etc. from your link).

Add exceedance rules (e.g., any ensemble member above flood stage).

Debounce & shift-based alerts (only alert on state change).

Milestone 3 — PWA polish / deploy

Replace Streamlit (optional) with a small React UI and PWA manifest.

Dockerize; deploy to a small VPS for 24/7 polling.

Add /admin page to edit thresholds, contacts, quiet hours.
