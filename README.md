# Fish Vault: Archival Ichthyology & Specimen Vault

> A digital synthesis engine and taxonomic archival repository designed for technical minimalism, structural honesty, and procedural biology.

[![Live Site](https://img.shields.io/badge/Live-fish--vault.glassgallery.my.id-8B1A1A?style=flat-square)](https://fish-vault.glassgallery.my.id)
[![Architecture](https://img.shields.io/badge/Design_System-Swiss--Archival-2A2520?style=flat-square)](#architectural-principles)
[![Engine](https://img.shields.io/badge/Engine-Lingdong_fishdraw-7A7068?style=flat-square)](https://github.com/LingDong-/fishdraw)
[![License](https://img.shields.io/badge/License-MIT-blue?style=flat-square)](LICENSE)

---

## 🏛️ Lineage & Design Heritage

**Fish Vault** is built directly upon the intersection of two open-source creative engineering projects:

| Project | Author & Source | Contributions to Fish Vault |
|---|---|---|
| **[Geometry Vault](https://geometry-vault.glassgallery.my.id/)** | **Azzar Budiyanto**<br>([Portfolio](https://glassgallery.my.id/porto/)) | • **Swiss-Archival Design System**: Tactile 300gsm rag paper aesthetic, coordinate ticks, and accession numbering.<br>• **Interface Architecture**: Multi-ratio format matrix, hardware-accelerated cursor, and telemetry side-panel.<br>• **Embed Protocol**: Iframe embed mode (`?mode=embed`), URL state synchronization, and PostMessage API. |
| **[fishdraw](https://github.com/LingDong-/fishdraw)** | **Lingdong Huang**<br>([Demo](https://fishdraw.glitch.me/)) | • **Procedural Anatomical Synthesis**: Algorithmic generation of body curvature, dorsal fins, caudal tails, spines, and meristic scales.<br>• **Taxonomic Generator**: Built-in pseudo-Latin binomial nomenclature generator (`binomen()`).<br>• **Plotter Mechanics**: Pure vector polyline coordinate synthesis and Hershey vector typography engraving. |

---

## 📐 Architectural Principles

1. **Taxonomic Naturalism & Structural Honesty:** Visible field grids, technical metadata, and Linnaean binomial nomenclature as primary aesthetic and functional elements.
2. **Technical Minimalism:** Zero-dependency vanilla engine (~75KB). Runs 100% client-side with zero build steps or heavyweight dependencies.
3. **Physicality & Archival Media:** Simulates 300gsm matte rag paper, tactile noise textures, and antique field expedition plates (*HMS Challenger*, Linnaean Society monographs, and Victorian biological taxonomy plates).
4. **Plotter-Centric Precision:** Synthesizes pure vector polylines rather than raster approximations, ensuring infinite fidelity, SMIL live stroke animations, and direct compatibility with CNC pen plotters (AxiDraw).

---

## ✨ Features

### 1. Deterministic Biological Synthesis
Transform narrative sequences, phrases, or names into reproducible morphological parameters. The engine calculates body curvature, spine density, scale morphology, dentition, and pseudo-Latin binomens (*e.g. Salmo salar*, *Chrosomus macrocephalus*).
> **Determinism Guarantee:** Features an engine patch ensuring the internal PRNG noise table resets cleanly between runs, guaranteeing that identical seeds yield identical specimens across prolonged web sessions.

### 2. Dual Render Modes
- **Static Vector Plate:** Instantaneous rendering of clean SVG vector strokes.
- **Plotter Simulation:** Live animated SMIL pen plotter simulation illustrating each stroke being drawn across the plate in real-time, accompanied by a `[ RE-PLOT ]` button.

### 3. Morphometric Telemetry Dashboard
Real-time extraction and inspection of procedural biological measurements:
- **Standard Length (SL) & Body Depth (BD):** Morphological proportions and fin aspect ratios.
- **Curvature Index:** Measured percentage and curve profile (Sinusoidal vs. Arcuate).
- **Meristic Scale Classification:** Cycloid, Ctenoid, Ganoid, or Placoid dermal structures.
- **Dermal Patterns:** Uniform, Transverse Fasciation, Maculated/Mottled, Striae, or Punctate.
- **Dentition & Barbels:** Spines and sensory appendages detection.
- **Strokes & Points:** Exact vector metrics count.

### 4. Archival Themes
- **Linnaean Sepia (Default):** Traditional bone black ink on 300gsm aged rag paper (`#E8E0D0`).
- **Prussian Cyanotype:** Deep oceanic indigo and cyanotype blue on sun-bleached vellum.
- **Challenger Deep:** Abyssal deep navy with bioluminescent amber markings.
- **Drafting Vellum:** Technical architectural vellum with cobalt drafting ink.

### 5. Multi-Ratio Format Matrix
Instantly adapt specimens across 9 standard publishing aspect ratios:
`9:16`, `2:3`, `3:4`, `4:5`, `1:1`, `5:4`, `4:3`, `3:2`, `16:9`.

### 6. Archival Export Protocol
- **SVG:** Standalone clean vector linework framed in archival border.
- **ANIM SVG:** Vector SVG featuring built-in SMIL animations for stroke-by-stroke playback in web browsers.
- **PNG 300DPI:** High-resolution matte canvas render with paper grain noise.
- **PLOTTER CSV:** Polyline coordinates ready for AxiDraw, G-code generators, and CNC machines.
- **EMBED Protocol:** Iframe snippet generator for seamless embedding into documentation, blogs, or portfolios.

### 7. Progressive Web App (PWA) & Offline Capability
- Service Worker caching for core assets.
- Web App Manifest for mobile and desktop home-screen installation.
- Responsive, hardware-accelerated cursor and touch navigation.

---

## 🛠️ Tech Stack

- **Frontend:** HTML5, Vanilla JavaScript (ES6+), Tailwind CSS (Utility Layer).
- **Core Engine:** Procedural polyline generation algorithm adapted from [fishdraw](https://github.com/LingDong-/fishdraw).
- **Typography:** JetBrains Mono (Technical telemetry) & Plus Jakarta Sans (Editorial).
- **Deployment & Server:** Alpine Nginx, Docker, Cloudflare Tunnel (`cloudflared`).

---

## 🚀 Deployment & Local Execution

### Local Preview (No Docker required)
Because the core engine is pure vanilla JavaScript, you can run it with any static file server:

```bash
# Python 3
python3 -m http.server 8086

# Node.js
npx serve -l 8086
```

Navigate to `http://localhost:8086`.

### Docker Execution

```bash
# Build & run with Docker Compose
docker compose up -d --build
```

The service will be accessible locally at `http://localhost:8086`.

---

## 🌐 Production URL & Integration

**Live Production Domain:** [https://fish-vault.glassgallery.my.id](https://fish-vault.glassgallery.my.id)

### URL Query Parameters

| Parameter | Type | Description | Example |
|---|---|---|---|
| `seed` | String | Narrative sequence or species name | `?seed=Salmo%20salar` |
| `theme` | String | Palette theme (`linnaean`, `cyanotype`, `challenger`, `vellum`) | `?theme=cyanotype` |
| `w` & `h` | Number | Aspect ratio width and height | `?w=16&h=9` |
| `mode` | String | Set to `embed` for iframe portfolio embedding | `?mode=embed` |
| `mode_render` | String | Set to `anim` to default to live plotter simulation | `?mode_render=anim` |

### Iframe Embed Example

```html
<iframe 
  src="https://fish-vault.glassgallery.my.id/?mode=embed&seed=Salmo%20salar&theme=linnaean&w=3&h=2" 
  width="500" 
  height="500" 
  frameborder="0" 
  style="border:1px solid #DDD5C5; border-radius:4px;">
</iframe>
```

### PostMessage Protocol (Geometry Vault Parity)

Parent windows can interact with embedded Fish Vault iframes programmatically without reloading:

```javascript
const iframe = document.querySelector('iframe');

// Method 1: Full Specimen Update
iframe.contentWindow.postMessage({
  type: 'UPDATE_SPECIMEN', // or 'SET_SPECIMEN'
  data: {
    seed: 'Thunnus alalunga',
    theme: 'challenger',
    w: 1,
    h: 1,
    mode_render: 'static' // or 'anim'
  }
}, '*');

// Method 2: Granular controls
iframe.contentWindow.postMessage({
  type: 'SET_SEED',
  data: { seed: 'Thunnus alalunga' }
}, '*');

iframe.contentWindow.postMessage({
  type: 'SET_THEME',
  data: { theme: 'challenger' }
}, '*');
```

---

## 📜 Credits & Citations

1. **[Geometry Vault](https://geometry-vault.glassgallery.my.id/)** by **[Azzar Budiyanto](https://glassgallery.my.id/porto/)**
   - For the Swiss-Archival UI design system, telemetry dashboard, embed architecture, and tactile paper styling.
2. **[fishdraw](https://github.com/LingDong-/fishdraw)** by **[Lingdong Huang](https://lingdong.works/)**
   - For the procedural fish drawing mathematics, Bézier anatomical modeling, and pseudo-Latin binomen nomenclature generator.

---

## 📄 License

This project is open-source under the [MIT License](LICENSE).
