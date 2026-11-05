# SPACE EXPO — Developer & Local Deployment Guide

This guide provides technical specifications, local execution instructions, web server configuration, and deployment procedures for the **SPACE EXPO** simulation engine.

---

## 📋 Table of Contents
1. [Prerequisites](#prerequisites)
2. [Local Development Setup](#local-development-setup)
3. [Cloudflare Pages & Production Deployment](#cloudflare-pages--production-deployment)
4. [Web Server MIME Types](#web-server-mime-types)
5. [Browser Audio Autoplay Compliance](#browser-audio-autoplay-compliance)
6. [Architecture & Engine Subsystems](#architecture--engine-subsystems)
7. [Simulation Keyboard & Flight Controls](#simulation-keyboard--flight-controls)

---

## 1. Prerequisites

Because SPACE EXPO leverages modern WebGL shaders, Three.js 3D rendering pipelines, and the Web Audio API, modern web browsers enforce the **Same-Origin Policy** (CORS). Opening the HTML files directly via `file:///` in your browser will block binary textures, audio files, and scripts. 

To run the simulator locally, you must serve the directory through any standard local HTTP server.

---

## 2. Local Development Setup

Choose any of the following methods to start a local development server:

### Option A: Python 3 (Recommended)
Python is pre-installed on most developer environments. Run from the project root:
```bash
python -m http.server 8080
```
Open your browser and navigate to:
```
http://localhost:8080/
```

### Option B: Node.js (`npx serve` or `http-server`)
If you have Node.js installed:
```bash
# Using serve
npx serve . -p 8080

# Or using http-server
npx http-server -p 8080 -c-1
```
Open your browser at `http://localhost:8080/`.

### Option C: VS Code Live Server
1. Install the **Live Server** extension (by Ritwick Dey) in Visual Studio Code.
2. Right-click `index.html` or `Space Expo Enhanced Upgrade.html` in the file explorer.
3. Select **"Open with Live Server"**.

---

## 3. Cloudflare Pages & Production Deployment

SPACE EXPO is deployed continuously to **Cloudflare Pages**:
- **Live URL**: [https://spacexpo.pages.dev](https://spacexpo.pages.dev)
- **Deployment Platform**: Cloudflare Pages
- **Repository**: Connected to `https://github.com/bhawsarhardik195-blip/SPACE-EXPO.git` (`main` branch)

### Cloudflare Routing Architecture:
- `_redirects`: Configures standard Cloudflare Pages SPA rewrite and redirect rules:
  ```text
  /    /Space%20Expo%20Enhanced%20Upgrade.html    302
  ```
- `index.html`: Contains a lightweight client-side fallback redirect (`<meta http-equiv="refresh">` and `window.location.replace`) to `/Space%20Expo%20Enhanced%20Upgrade.html` to guarantee universal compatibility across edge CDNs, mobile devices, and direct links.

---

## 4. Web Server MIME Types

Ensure your host or reverse proxy serves binary assets with proper `Content-Type` headers:

| File Extension | Content-Type | Usage |
| :--- | :--- | :--- |
| `.mpeg` / `.mp3` | `audio/mpeg` | Spatial audio soundscapes, cockpit hum, engine thrusters |
| `.mp4` | `video/mp4` | Background video overlays |
| `.jpeg` / `.jpg` | `image/jpeg` | High-resolution planetary surface textures |
| `.png` | `image/png` | Screenshots, UI icons, normal maps |
| `.html` | `text/html; charset=utf-8` | Core simulation engine |

### Nginx Example:
```nginx
server {
    listen 80;
    server_name spaceexpo.example.com;
    root /var/www/spaceexpo;
    index index.html;

    location / {
        try_files $uri $uri/ /Space%20Expo%20Enhanced%20Upgrade.html;
    }

    location /audio/ {
        add_header Cache-Control "public, max-age=604800, immutable";
    }
}
```

---

## 5. Browser Audio Autoplay Compliance

Modern browsers (Chrome, Firefox, Safari, Edge) block audio context initialization before the user has performed an explicit gesture (click or tap) on the document:
1. The Web Audio graph (`AudioContext`) is lazily instantiated or resumed upon the first click on the document or upon clicking **"ENTER SIMULATOR"**.
2. If audio does not start immediately, click anywhere on the viewport or toggle the **Audio (🔊)** button in the top HUD.
3. Individual volume sliders are available in the in-game settings modal for Master, Intro Music, Cosmic Ambience, and Engine SFX.

---

## 6. Architecture & Engine Subsystems

```
┌──────────────────────────────────────────────────────────────┐
│                    SPACE EXPO Engine Core                    │
├──────────────────────────────┬───────────────────────────────┤
│    Astrodynamics Engine      │       Rendering Engine        │
│  • Keplerian Orbit Solver    │  • Three.js WebGL Renderer    │
│  • Lambert Solver (RK4)      │  • Atmosphere Shader & Clouds │
│  • NASA JPL Ephemeris        │  • Atlantis 1 Earth Transition│
├──────────────────────────────┼───────────────────────────────┤
│       Avionics & Flight      │         Audio Manager         │
│  • 360° Cockpit Camera       │  • Web Audio Bus Routing      │
│  • Launch Ascent Timeline    │  • Dynamic Ducking / Gain     │
│  • ESC Skip / L Abort Keys   │  • Low-Pass Cabin Isolation   │
└──────────────────────────────┴───────────────────────────────┘
```

---

## 7. Simulation Keyboard & Flight Controls

| Key / Input | Action | Subsystem |
| :--- | :--- | :--- |
| `ESC` | **Skip Initial Launch** (Immediate transition to Earth orbital scene) | Launch Sequencer |
| `L` | **Abort Mission** (Clean instantaneous mission reset) | Mission Control |
| `C` | **Toggle Cockpit View** (Switch between external orbiter and internal flight deck) | Camera Avionics |
| `Left Mouse Drag` | Rotate camera / Orbit celestial body | Camera Controller |
| `Right Mouse Drag` | Pan camera across viewport | Camera Controller |
| `Scroll Wheel` | Zoom in / Zoom out | Camera Controller |
| `Time Scale Buttons` | Adjust simulation speed (`0.5x`, `1x`, `5x`, `10x`, `50x`, `100x`) | Orbital Clock |
