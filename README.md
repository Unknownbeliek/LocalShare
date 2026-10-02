# 🚀 LocalFlux Media — Local Network Media Streaming & Download System

**LocalFlux Media** is an open-source, lightweight, self-hosted web application that transforms any local folder on your host PC (e.g., `D:\moviespc` or `./moviespc`) into a high-speed media streaming and direct download portal accessible across your local Wi-Fi network.

Designed with an **aggressively minimal Glassmorphism UI**, mobile-first touch ergonomics, and **mDNS zero-config network discovery** (`localflux.local`), it allows family members and mobile devices to stream or pull high-definition movies directly without cloud bandwidth, external apps, cables, or complex media servers.

---

## 🌟 Key Features

* **Strict Glassmorphism UI:** Frosted glass panels (`backdrop-filter: blur(18px)`), subtle neon indigo/cyan glow accents, and dark-mode backdrop (`#07090e`).
* **Smart File Filtering:** Backend engine automatically scans target folder and explicitly ignores incomplete download files (`.crdownload`, `.part`, `.tmp`, `.!qb`, `.aria2`), system metadata (`.DS_Store`, `Thumbs.db`), and write-locked files.
* **HTTP 206 Partial Content Video Streaming Engine:** Range-request video server supporting instant seeking, smooth scrubbing, and low memory usage. Handles client disconnects (`req.on('close')`) without memory leaks or broken pipe crashes.
* **One-Tap Direct Downloads:** Dedicated attachment download endpoints (`Content-Disposition: attachment`) for instant high-speed file transfers directly to mobile phone internal storage.
* **Zero-Config mDNS Discovery (`localflux.local`):** Powered by `bonjour-service`, allowing mobile devices to type `http://localflux.local:5000` or scan a terminal QR code without memorizing host IPv4 addresses.
* **Mobile-First Touch Web Player:** Custom HTML5 video player overlay featuring double-tap 10s skip gestures, volume/brightness drag handles, glowing scrub bar, picture-in-picture, and full-screen triggers.

---

## 📂 Project Structure

```text
LocalFlux-Media/
├── README.md                 # Documentation & Architecture Overview
├── PRD.md                    # Detailed Product Requirements Document
├── package.json              # Root script runner & concurrent manager
├── moviespc/                 # Default media folder for video storage (.mp4, .mkv, .webm)
│
├── server/                   # Express.js Backend Service
│   ├── package.json          # Express, chokidar, bonjour-service, qrcode-terminal
│   ├── config.js             # Environment & file extension filter rules
│   ├── index.js              # Server entry point & mDNS advertisement
│   ├── services/
│   │   ├── smartFilter.js    # Smart filter logic (.crdownload exclusion & lock check)
│   │   ├── mediaScanner.js   # Directory scanner & real-time chokidar watcher
│   │   └── mdns.js           # Bonjour mDNS service publisher (localflux.local)
│   ├── routes/
│   │   └── mediaRoutes.js    # API endpoints (/health, /media, /stream, /download, /events)
│   └── scripts/
│       └── createSampleMedia.js # Script to generate test video files & verify filtering
│
└── client/                   # React.js + Vite Mobile-First Frontend
    ├── package.json          # React, Vite, Lucide-React icons
    ├── index.html            # HTML entry point with mobile viewport settings
    ├── vite.config.js        # Vite dev server configuration & API proxy
    └── src/
        ├── main.jsx          # React DOM root mounting
        ├── index.css         # Glassmorphism design tokens & utility classes
        ├── App.jsx           # Main SPA layout & state orchestrator
        └── components/
            ├── Navbar.jsx            # Floating glass header with LAN IP & search
            ├── MediaCard.jsx         # Card displaying movie details & download action
            ├── CustomWebPlayer.jsx   # Fullscreen glass touch player with 10s skip
            ├── SmartFilterBar.jsx    # Category chips & active filter indicator
            └── ToastNotification.jsx # Glass notification toast for downloads
```

---

## 🛠️ Quick Start & Installation Commands

### 1. Prerequisites
* Node.js v18.0.0 or higher
* npm v9.0.0 or higher

### 2. Clone Repository & Install Dependencies

Run the following commands in your terminal:

```bash
# Clone the repository
git clone https://github.com/LocalFlux/localflux-media.git
cd localflux-media

# Install root, server, and client dependencies
npm install
cd server && npm install
cd ../client && npm install
cd ..
```

### 3. Generate Sample Media Files (Optional)

Generate sample test videos (and test `.crdownload` files to confirm smart filtering):

```bash
node server/scripts/createSampleMedia.js
```

### 4. Build Client & Launch Server

```bash
# Build the production React frontend bundle
npm run build

# Start the LocalFlux Express Media Server
npm start
```

Or for concurrent development:

```bash
npm run dev
```

---

## 📱 Connecting Mobile Devices

When the server starts, it automatically binds to `0.0.0.0:5000` and displays:
1. **Terminal QR Code:** Scan directly with your mobile camera.
2. **mDNS Hostname:** Navigate to `http://localflux.local:5000` on any device connected to the home Wi-Fi.
3. **Local Wi-Fi IP:** Navigate to `http://<YOUR_LOCAL_IP>:5000`.

---

## 🔌 API Endpoints Summary

| Endpoint | Method | Description | Header / Query Params | Status Response |
| :--- | :--- | :--- | :--- | :--- |
| `/api/v1/health` | `GET` | System health & local LAN IP | None | `200 OK` |
| `/api/v1/media` | `GET` | List clean, playable media files | `?sort=date\|size\|name` | `200 OK` |
| `/api/v1/media/:id/stream` | `GET` | Stream video with Range support | `Range: bytes=0-` | `206 Partial Content` |
| `/api/stream/:filename` | `GET` | Direct filename stream | `Range: bytes=0-` | `206 Partial Content` |
| `/api/v1/media/:id/download` | `GET` | Direct attachment file download | None | `200 OK` (Attachment) |
| `/api/download/:filename` | `GET` | Direct filename attachment download | None | `200 OK` (Attachment) |
| `/api/v1/events` | `GET` | Real-time Server-Sent Events (SSE) | None | `200 OK` (Event Stream) |

---

## 🎨 Glassmorphism Design System

LocalFlux Media adheres strictly to a minimalist Glassmorphic UI specification:
* **Background:** Deep space dark backdrop (`#07090e`).
* **Glass Surfaces:** Semi-transparent panels (`rgba(255, 255, 255, 0.05)`).
* **Backdrop Filters:** `backdrop-filter: blur(18px) saturate(180%)`.
* **Glow Accents:** Indigo (`#6366f1`) & Cyan (`#06b6d4`).
* **Ergonomics:** Large minimum 48px touch targets for effortless one-handed mobile operation.

---

## 📜 License
Released under the [MIT License](LICENSE).
