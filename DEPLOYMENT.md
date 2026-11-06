# VORQASTRA — Deployment & Systems Documentation

This document covers production and staging deployment procedures for **VORQASTRA: Advanced Interactive Solar System and Spaceflight Simulator**, along with audio pipeline specifications and complete asset credits.

---

## 1. Production Web Deployment

### Web Server Requirements
- **Protocol**: HTTPS recommended (HTTP acceptable for local development).
- **MIME Types**: Ensure your web server serves `.mpeg`, `.mp3`, `.ogg`, `.wasm`, `.glb`, and `.json` with proper Content-Type headers:
  - `.mpeg` / `.mp3` ➔ `audio/mpeg`
  - `.wasm` ➔ `application/wasm`
  - `.glb` ➔ `model/gltf-binary`
  - `.hdr` ➔ `application/octet-stream`

### Quick Deployment Options

#### Option A: Static Web Hosting (Vercel, Netlify, GitHub Pages, Cloudflare Pages)
1. Push repository or upload folder containing `index.html`, `audio/`, `textures/`, `models/`, and supporting libraries.
2. Set build command to empty (static files).
3. Set publish directory to `./` (root).
4. Direct entry point: `index.html`.

#### Option B: Nginx Configuration
```nginx
server {
    listen 80;
    server_name vorqastra.example.com;
    root /var/www/vorqastra;
    index VORQASTRA.html index.html;

    location / {
        try_files $uri $uri/ /VORQASTRA.html;
    }

    location /audio/ {
        add_header Cache-Control "public, max-age=604800, immutable";
    }

    location /textures/ {
        add_header Cache-Control "public, max-age=604800, immutable";
    }
}
```

#### Option C: Docker Container
```dockerfile
FROM nginx:alpine
COPY . /usr/share/nginx/html
EXPOSE 80
CMD ["nginx", "-g", "daemon off;"]
```

---

## 2. Audio Pipeline Architecture & Credits

The simulator uses a custom Web Audio API node graph (`AudioManager`):
- **Master Gain Node** ➔ Connected to destination with dynamic compressor to eliminate audio clipping.
- **BGM Bus (Music)** ➔ Dedicated channel for intro and cinematic music.
- **Cosmic Bus (Ambience)** ➔ Dedicated channel for celestial space travel.
- **SFX Bus (Engines)** ➔ Spatial engine acceleration, staging separation, and atmospheric rumble.
- **Cockpit Bus (Avionics)** ➔ Filtered cabin acoustic layer for internal 360° cockpit view.

### Audio Credits & Attributions

All audio assets utilized in the production deployment are cataloged below:

| Track Role | Project Asset Path | Composition / Track Title | Creator / Channel | Source URL | Verification Status |
| :--- | :--- | :--- | :--- | :--- | :--- |
| **🌌 Intro Sound** | `audio/intro.mpeg` | *Interstellar Shores \| Dream Space Ambient Music Journey Through Cosmic Landscapes \| 4K* | **VIATMOS** | [Watch on YouTube](https://youtu.be/bbTgNS7_BVc) | **Verified** |
| **🔭 Explorer Sound** | `audio/scale tour.mpeg` | *Not verified* | *Not verified* | [Watch on YouTube Shorts](https://youtube.com/shorts/o4_gUguEhkk) | **Source URL Verified**<br>*(Creator & Title Not verified)* |
| **🎛️ Cockpit Sound** | `audio/cockpit 1.mpeg`<br>`audio/cockpit 2.mpeg` | *Cockpit Flight Deck Ambience (Layered A & B)* | *Not verified (Project Asset)* | Local Project Asset | **Project Local Asset**<br>*(Creator Not verified)* |
| **🚀 Rocket Thrust SFX** | `audio/thrust.mpeg` | *Spacecraft Main Engine Ascent & Burn SFX* | *Not verified (Project Asset)* | Local Project Asset | **Project Local Asset**<br>*(Creator Not verified)* |
| **🎙️ Voice Narration** | `audio/naration sample.mpeg` | *Mission Voice Briefing Sample* | *Not verified (Project Asset)* | Local Project Asset | **Project Local Asset**<br>*(Creator Not verified)* |

> Note: Per project policy, all sources are verified against author channels where available. Entries without verified public credentials are intentionally marked **"Not verified"** rather than estimated.

---

## 3. Simulator Entry Points & Files

- `VORQASTRA.html`: Complete high-precision production simulator build containing photorealistic launchpad environment, volumetric atmospheric ascent, 360° glass cockpit, and NASA-validated astrodynamics trajectories.
- `_redirects`: Cloudflare Pages edge routing and URL rewrite rules directing `/` directly to `/VORQASTRA`.
- `audio/`: Local directory housing all compressed MPEG audio tracks.
- `eath view/`: High-resolution orbital and surface textures for Earth and launch facilities.
- `screenshots/`: Reference visual captures from live simulation sessions.
