# Raspdarts

A browser extension for managing your Raspberry Pi (running [Autodarts](https://autodarts.io)) directly from [play.autodarts.io](https://play.autodarts.io) — no SSH required.

![Chrome](https://img.shields.io/badge/Chrome-supported-brightgreen) ![Firefox](https://img.shields.io/badge/Firefox-supported-brightgreen)

## What it does

Raspdarts adds a button to the Autodarts navigation bar. Click it to open a panel showing live stats and controls for your Pi:

- **CPU, RAM, Temperature, Uptime** — live stats, updated every 10 seconds
- **Autodarts** — install, update, or uninstall Autodarts; open the Monitor UI
- **Raspberry Pi** — update or uninstall the Raspdarts backend; restart or shut down the Pi

## Requirements

- A Raspberry Pi running the [Raspdarts backend](https://github.com/HerobrickHD/Raspdarts-backend)
- The Pi must be reachable at `raspdarts.local` on your local network (set via hostname)
- Chrome or Firefox on the same network as the Pi

## Installation

### 1. Set up the backend on your Raspberry Pi

Follow the instructions in the [Raspdarts backend repository](https://github.com/HerobrickHD/Raspdarts-backend).

### 2. Install the browser extension

#### Chrome (unpacked)

1. Download or clone this repository
2. Run `build.sh` to generate the `dist/` folder:
   ```bash
   bash build.sh
   ```
3. Open Chrome and go to `chrome://extensions`
4. Enable **Developer mode** (top right)
5. Click **Load unpacked** and select the `dist/chrome/` folder

#### Firefox (temporary)

1. Open Firefox and go to `about:debugging#/runtime/this-firefox`
2. Click **Load Temporary Add-on**
3. Select `dist/firefox/manifest.json`

## How it works

```
play.autodarts.io
    └─ content.js         (injects the UI into the page)
          │
          ▼
       background.js      (proxies requests — bypasses CORS)
          │
          ▼
    raspdarts.local:8743  (Node.js backend on your Pi)
```

The extension communicates with the backend via:
- **Regular fetch** for quick actions (status, reboot, shutdown)
- **Server-Sent Events (SSE)** for long-running operations (installs, updates) — output streams line by line into the log window

## Repository structure

```
├── background.js          # Background service worker (request proxy + SSE stream handler)
├── content.js             # Content script (UI logic, modal, status polling)
├── modal.html             # Modal markup
├── modal.css              # Modal styles
├── manifest.chrome.json   # Chrome Manifest V3
├── manifest.firefox.json  # Firefox Manifest V2
├── build.sh               # Builds dist/chrome/ and dist/firefox/
└── icons/                 # Extension icons
```

## Backend

The backend (Node.js/Express) runs on the Raspberry Pi as a systemd service on port 8743.  
Source and installation instructions: [github.com/HerobrickHD/Raspdarts-backend](https://github.com/HerobrickHD/Raspdarts-backend)
"# Raspdarts" 
"# Raspdarts" 
