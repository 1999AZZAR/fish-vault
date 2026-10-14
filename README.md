# Fish Vault: Archival Ichthyology & Specimen Vault

> A digital synthesis engine and taxonomic archival repository designed for technical minimalism, structural honesty, and procedural biology. Built on the Swiss-Archival design system and powered by Lingdong Huang's procedural `fishdraw` algorithm.

---

## Architectural Principles

1. **Taxonomic Naturalism & Structural Honesty:** Visible field grids, technical metadata, and Linnaean binomial nomenclature as primary aesthetic and functional elements.
2. **Technical Minimalism:** Zero-dependency vanilla engine (~75KB). Runs 100% client-side with zero build steps or heavyweight frameworks.
3. **Physicality & Archival Media:** Simulates 300gsm matte rag paper, tactile noise textures, and antique field expedition plates (*HMS Challenger*, Linnaean Society monographs, and Victorian natural history plates).
4. **Plotter-Centric Precision:** Synthesizes pure vector polylines rather than raster approximations, ensuring infinite fidelity, SMIL live stroke animations, and direct compatibility with CNC pen plotters (AxiDraw).

---

## Features

### 1. Deterministic Biological Synthesis
Transform narrative sequences, phrases, or names into reproducible morphological parameters. The engine calculates body curvature, spine density, scale morphology, dentition, and pseudo-Latin binomens (*e.g. Salmo salar*, *Chrosomus macrocephalus*). 

### 2. Live Pen Plotter Simulation
Toggle between two rendering engines:
- **Static Vector Plate:** Instantaneous rendering of clean SVG paths.
- **Plotter Simulation:** Live animated SMIL pen plotter simulation illustrating each stroke being drawn across the plate in real-time.

### 3. Morphometric Telemetry Dashboard
Real-time extraction and inspection of procedural biological measurements:
- **Standard Length (SL) & Body Depth (BD):** Morphological proportions and fin aspect ratios.
- **Curvature Index:** Measured percentage and curve profile (Sinusoidal vs. Arcuate).
- **Meristic Scale Classification:** Cycloid, Ctenoid, Ganoid, or Placoid dermal structures.
- **Dermal Patterns:** Uniform, Transverse Fasciation, Maculated/Mottled, Striae, or Punctate.
- **Dentition & Barbels:** Spines and sensory appendages detection.
- **Strokes & Points:** Exact vector metrics count.

### 4. Archival Themes
- **Linnaean Sepia:** Traditional bone black ink on 300gsm aged rag paper (`#E8E0D0`).
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

## Tech Stack

- **Frontend:** HTML5, Vanilla JavaScript (ES6+), Tailwind CSS (Utility Layer).
- **Core Engine:** Procedural polyline generation algorithm adapted from [fishdraw](https://github.com/LingDong-/fishdraw).
- **Typography:** JetBrains Mono (Technical telemetry) & Plus Jakarta Sans (Editorial).
- **Server / Deployment:** Alpine Nginx, Docker, Docker Compose.

---

## Deployment & Local Execution

### Local Preview (No Docker required)
Because the core engine is pure vanilla JavaScript, you can run it with any static file server:

```bash
# Python 3
python3 -m http.server 8085

# Node.js
npx serve -l 8085
```

Navigate to `http://localhost:8085`.

### Docker Execution

```bash
# Build & run with Docker
docker build -t fish-vault .
docker run -d -p 8085:80 --name fish-vault fish-vault
```

Or using Docker Compose:

```bash
docker compose up -d
```

The service will be accessible at `http://localhost:8086`.

---

## URL Parameters & External Integration

Fish Vault supports deep-linking and state synchronization:

| Parameter | Type | Description |
|---|---|---|
| `seed` | String | Narrative sequence or species name (e.g. `?seed=Salmo%20salar`) |
| `theme` | String | Palette theme: `linnaean`, `cyanotype`, `challenger`, `vellum` |
| `w` & `h` | Number | Aspect ratio width and height (e.g. `?w=16&h=9`) |
| `mode` | String | Set to `embed` for iframe portfolio embedding |
| `mode_render` | String | Set to `anim` to default to live plotter simulation |

### Iframe Embed Example

```html
<iframe 
  src="https://fish-vault.glassgallery.my.id/?seed=Chrosomus%20nebulosus&theme=cyanotype&w=16&h=9&mode=embed" 
  width="100%" 
  height="450" 
  frameborder="0" 
  style="border:1px solid #DDD5C5; background:#223244;" 
  allowtransparency="true">
</iframe>
```

---

## PostMessage Interface

Parent applications can control embedded Fish Vault iframes programmatically:

```javascript
const iframe = document.querySelector('iframe');

// Set a new species seed
iframe.contentWindow.postMessage({
  type: 'SET_SEED',
  data: { seed: 'Thunnus alalunga' }
}, '*');

// Change archival palette theme
iframe.contentWindow.postMessage({
  type: 'SET_THEME',
  data: { theme: 'challenger' }
}, '*');
```

---

## Credits & Citations

- **Lead Architect & Concept:** [Azzar Budiyanto](https://glassgallery.my.id/porto/)
- **Original Procedural Algorithm:** [Lingdong Huang (fishdraw)](https://github.com/LingDong-/fishdraw)
- **License:** MIT License
