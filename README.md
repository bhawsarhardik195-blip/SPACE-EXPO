# SPACE EXPO — Advanced Interactive Solar System & Spaceflight Simulator

![Space Expo Simulator](https://img.shields.io/badge/SPACE%20EXPO-v2.5%20Enhanced-00e5ff?style=for-the-badge)
![WebGL 3D](https://img.shields.io/badge/WebGL-Three.js-orange?style=for-the-badge)
![Physics](https://img.shields.io/badge/Orbital%20Mechanics-NASA%20JPL%20Validated-4fd1ff?style=for-the-badge)
![License](https://img.shields.io/badge/License-CC%20BY%204.0%20%2F%20Public%20Domain-brightgreen?style=for-the-badge)

**SPACE EXPO** is an interactive, browser-based solar system and spaceflight simulation engine. It pairs physically grounded orbital astrodynamics (validated against NASA JPL Horizons and NASA GMAT) with realistic 3D spacecraft piloting, interactive 360° cockpit avionics, realistic atmospheric ascent, and high-fidelity spatial audio.

---

## 🎵 Audio Credits & Attributions

SPACE EXPO features an adaptive multi-channel spatial audio architecture built on the Web Audio API with dynamic ducking, cross-fading, and cabin isolation filters. All audio tracks, soundscapes, and acoustic assets used within the simulator are credited below with their verified creator and source attributions.

| Audio Role / Simulator Usage | Project File Path | Audio Track Title | Creator / Channel | Source Link | Verification Status |
| :--- | :--- | :--- | :--- | :--- | :--- |
| **🌌 Intro Sound**<br>Homepage ambient backdrop, cinematic intro sequence, and launch countdown audio | [`audio/intro.mpeg`](audio/intro.mpeg) | *Interstellar Shores \| Dream Space Ambient Music Journey Through Cosmic Landscapes \| 4K* | **VIATMOS** | [Watch on YouTube](https://youtu.be/bbTgNS7_BVc) | **Verified** |
| **🔭 Explorer / Tour Sound**<br>Solar System scale tour, cosmic navigation, and deep-space celestial exploration | [`audio/scale tour.mpeg`](audio/scale%20tour.mpeg) | *Not verified* | *Not verified* | [Watch on YouTube Shorts](https://youtube.com/shorts/o4_gUguEhkk) | **Source URL Verified**<br>*(Creator & Title Not verified)* |
| **🎛️ Cockpit Sound**<br>Interactive 360° flight deck ambience, dual-channel avionics hum & cabin ventilation | [`audio/cockpit 1.mpeg`](audio/cockpit%201.mpeg)<br>[`audio/cockpit 2.mpeg`](audio/cockpit%202.mpeg) | *Cockpit Flight Deck Ambience (Layered A & B)* | *Not verified (Project Asset)* | Local Project Asset | **Project Local Asset**<br>*(Creator Not verified)* |
| **🚀 Rocket Thrust SFX**<br>Main engine atmospheric boost phase and interplanetary orbital transfer burn | [`audio/thrust.mpeg`](audio/thrust.mpeg) | *Spacecraft Main Engine Ascent & Burn SFX* | *Not verified (Project Asset)* | Local Project Asset | **Project Local Asset**<br>*(Creator Not verified)* |
| **🎙️ Celestial Voice Narration**<br>Automated celestial body guide and mission briefing voiceover sample | [`audio/naration sample.mpeg`](audio/naration%20sample.mpeg) | *Mission Voice Briefing Sample* | *Not verified (Project Asset)* | Local Project Asset | **Project Local Asset**<br>*(Creator Not verified)* |

> [!NOTE]
> Per attribution and verification guidelines:
> - If an audio asset's creator name or specific composition title could not be confirmed independently from author records, it is explicitly cataloged as **Not verified** rather than assuming or inventing placeholder data.
> - Source links point to the verified original upload locations on YouTube.

---

## 🚀 Key Simulator Features

1. **Keplerian Solar System & N-Body Precision**:
   - 28+ planets, moons, dwarf planets, and asteroids modeled with real NASA ephemeris parameters (eccentricity, semi-major axis, orbital inclination, longitude of ascending node, argument of periapsis, and axial tilt).
2. **Scientifically Grounded Interplanetary Trajectory Engine**:
   - 3D Lambert solver, patched-conic Hohmann transfers, and numerical Runge-Kutta 4th-order (RK4) integration calibrated with NASA JPL Horizons state vectors and NASA GMAT mission reference runs.
3. **Space Shuttle Orbiter & Atmospheric Ascent**:
   - Exterior spacecraft mesh with thermal protection tiles, realistic RCS/OMS thrusters, launchpad gantries, smoke/condensation effects, volumetric clouds, and smooth ground-to-orbit transitions.
4. **Interactive 360° Glass Cockpit**:
   - Flight instruments, primary flight display (PFD), orbital velocity readouts, navigational heading indicators, and dual-layer acoustic cabin atmosphere.
5. **Universal Mission Control & Abort System**:
   - Real-time mission planner, target selection across all planets (including Pluto), countdown sequence, burn telemetry, and clean instant mission abort reset.
6. **Integrated Audio Suite**:
   - Master volume slider, individual channel mixing (Cinematic Music, Cosmic Ambience, Engines SFX, Voice Narration), mute toggle, and in-simulator Audio Credits access.

---

## 🛠️ Local Deployment Guide

Because the simulator loads 3D textures, models, and audio files through standard WebGL and Web Audio APIs, modern web browsers require files to be served over an HTTP/HTTPS origin (CORS restrictions prevent loading binary audio/textures via direct `file://` URIs).

### Method 1: Python Built-In Server (Recommended)
Open a terminal in the project directory:
```bash
# Python 3
python -m http.server 8080
```
Then navigate to:
```
http://localhost:8080/
```
*(or `http://localhost:8080/Space%20Expo%20Enhanced%20Upgrade.html`)*

### Method 2: Node.js `npx serve` or `http-server`
```bash
npx serve .
# or
npx http-server -p 8080 -c-1
```

### Method 3: VS Code / IDE Live Server
Right-click `index.html` or `Space Expo Enhanced Upgrade.html` and click **"Open with Live Server"**.

---

## 🔊 Browser Audio Autoplay Compliance

Modern browsers (Chrome, Edge, Firefox, Safari) enforce strict Autoplay Policies that prevent background audio from playing prior to explicit user interaction with the document:
- Audio will initialize and play as soon as the user clicks anywhere on the page, clicks **"ENTER SIMULATOR"**, or toggles the audio button in the top HUD.
- Full volume and balance controls are accessible anytime via the **Audio (🔊)** button in the top bar or via the **ABOUT & CREDITS** modal dialog.

---

## 📄 License & Asset Attributions

- **Orbital Mechanics & Planetary Ephemeris**: Public domain data courtesy of NASA Jet Propulsion Laboratory (JPL) and NASA Goddard Space Flight Center.
- **Planetary Textures**: NASA, ESA, USGS, and Solar System Scope (Creative Commons Attribution 4.0 International).
- **Audio Assets**: Individual rights remain with their respective creators as detailed in the [Audio Credits](#-audio-credits--attributions) section above.
