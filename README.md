# LightsOut

## 🏁 Formula 1 · MotoGP · IndyCar · DTM · GT World Challenge Europe · Isle of Man TT · WRC

**Motorsports schedules and push notifications to your device, in one place.**

LightsOut is a Progressive Web App (PWA) for following motorsports events across multiple racing series. It combines a React frontend with a Node.js/Express API that serves event calendars and schedules, stores user preferences, and supports browser push notifications.

**Live app:** https://lightsout-notify.vercel.app

## Install LightsOut as a PWA

LightsOut can be installed from a supported browser and opened from your Home Screen or apps list, like an app.

- **iPhone or iPad:** Open the live app in **Safari**, tap **Share**, choose **Add to Home Screen**, then tap **Add**. If the **Open as Web App** option is shown, enable it.
- **Android:** Open the live app in **Chrome**, tap the three-dot menu, choose **Install app** or **Add to Home screen**, and follow the prompts.
- **Desktop (Chrome or Edge):** Open the live app and select the install icon in the address bar if it appears. Otherwise, open the browser menu and choose **Install LightsOut** or **Install this site as an app**.

Install options vary by browser and device. For the best results, open the link directly in your browser rather than inside another app's built-in browser.

## Features

- **Upcoming events:** Browse upcoming motorsports events and explore events by series and year.
- **Event schedules:** View event/session information and schedule views for upcoming, live, today, and this week.
- **Multi-series coverage:** Event ingestion is organized around series including Formula 1, MotoGP, IndyCar, DTM, GTWC Europe, the Isle of Man TT, and WRC. Availability depends on the data ingested for each series.
- **Calendar:** Explore motorsports events in a calendar-oriented view.
- **Notifications:** View notifications, unread counts, and paginated notification history.
- **Browser push notifications:** Supports push subscriptions and server-side notification processing.
- **User preferences:** Stores preferences used by the PWA.
- **Installable PWA:** Includes a service worker and web app manifest for supported browsers.

## Architecture

| Area | Technology | Responsibility |
| --- | --- | --- |
| Frontend | React, Vite, JavaScript | PWA interface, event browsing, calendar, notifications, and user preferences |
| Backend | Node.js, Express | HTTP API and application routes |
| Database | PostgreSQL (`pg`) | Application data, including events and notifications |
| Data ingestion | Node.js scrapers and series-specific providers | Collecting and updating motorsports event and schedule data |
| Push notifications | `web-push`, service worker | Browser push subscription and delivery support |
| Frontend hosting | Vercel | Hosts the live PWA |

## Repository layout

```text
.
├── db/                 # PostgreSQL connection pool
├── routes/             # Express API routes
├── services/           # Application and schedule services
├── scrapers/           # Scraping helpers
├── scripts/            # Ingestion and maintenance scripts
├── src/
│   ├── config/         # Environment and web-push configuration
│   ├── middleware/     # Express middleware
│   ├── providers/      # Series-specific data providers and jobs
│   └── services/       # Shared ingestion and provider services
└── pwa-react/          # React + Vite Progressive Web App
```

## Getting started

### Prerequisites

- Node.js and npm
- A PostgreSQL database
- Environment variables required by the backend and frontend

### Backend API

From the repository root, install dependencies:

```bash
npm install
```

Create a local environment file from the example:

```bash
cp .env.example .env
```

Set the required values in `.env`, including `DATABASE_URL` and `PORT`. Some ingestion providers and push-notification features may require additional environment variables; configure those according to the relevant code and deployment settings. Never commit real credentials.

Start the API:

```bash
npm start
```

The API defaults to port `3000`. The `/` endpoint returns a basic status message, while `/health` returns a JSON health response. For development, `npm run dev` is available and expects a `.env.local` file.

### React PWA

From the repository root:

```bash
cd pwa-react
npm install
```

Create a frontend `.env` file and configure the variables used by the app:

- `VITE_API_BASE_URL` — base URL of the LightsOut API.
- `VITE_VAPID_PUBLIC_KEY` — public VAPID key used for browser push subscriptions.

Start the Vite development server:

```bash
npm run dev
```

Create a production build:

```bash
npm run build
```

See `pwa-react/package.json` for the frontend scripts.

## API overview

The Express server mounts route groups for these resources:

| Route | Purpose |
| --- | --- |
| `/events` | Events by year, upcoming events, events by series, event details, and event schedules |
| `/series` | Racing-series information |
| `/calendar` | Calendar data |
| `/units` | Event/session schedule data |
| `/schedule` | Next, live, today, and week schedule views |
| `/notifications` | Notifications and read-state updates |
| `/users` | User-related endpoints |
| `/user-preferences` | User preference endpoints |
| `/push` | Push subscription endpoints |
| `/api/cron`, `/push-cron` | Scheduled jobs and notification processing |
| `/health` | Health check |

The API also includes internal operational routes. Check the route files for exact parameters and request/response formats.

## Configuration and security

- Keep database URLs, VAPID private keys, and other credentials out of source control.
- Use `.env.example` as a starting point for local backend configuration.
- Configure `VITE_API_BASE_URL` for the environment where the frontend is running.
- Browser push notifications require HTTPS in production and compatible browser support.
- Restrict internal and scheduled-job endpoints appropriately in the deployment environment.

## Project goal

LightsOut helps motorsports fans keep track of upcoming events and schedules across racing series through a mobile-friendly interface and timely notifications.

---

*LightsOut is an independent project and is not affiliated with or endorsed by any racing series or governing body. Series and event coverage depends on available data sources.*
