# Fish Vault Quick Start Guide

Get the Fish Vault procedural specimen engine up and running locally or in Docker in minutes.

---

## ⚡ 1-Minute Local Preview (No Docker Required)

Because Fish Vault is built with technical minimalism and zero dependencies, you can run it instantly using any static web server:

```bash
# Using Python 3
python3 -m http.server 8086

# Or using Node.js
npx serve -l 8086
```

Open `http://localhost:8086` in your browser.

---

## 🐳 Docker Deployment

### 1. Using Docker Compose (Recommended)

```bash
docker compose up -d --build
```

Access the UI at `http://localhost:8086`.

### 2. Standalone Docker Build & Run

```bash
# Build the lean Alpine Nginx image (~15MB)
docker build -t fish-vault .

# Run the container
docker run -d -p 8086:80 --name fish-vault fish-vault
```

### 3. Verify Health Check

```bash
curl -sI http://localhost:8086
```

Expected HTTP status:
```text
HTTP/1.1 200 OK
Server: nginx/...
```

---

## 💻 Quick Code Test (Node.js)

Verify the procedural engine determinism and polyline generation directly from the terminal:

```bash
node -e '
const FishDraw = require("./js/fishdraw.js");
const spec = FishDraw.generateSpecimen({ seed: "Salmo salar" });
console.log("Taxon:", spec.metrics.taxon);
console.log("Accession ID:", spec.accessionId);
console.log("Strokes count:", spec.polylines.length);
'
```

---

## 🌐 Public Cloudflare Access

If routed through Cloudflare Tunnel as configured:
- **Production URL:** [https://fish-vault.glassgallery.my.id](https://fish-vault.glassgallery.my.id)
- **Documentation:** [https://fish-vault.glassgallery.my.id/docs.html](https://fish-vault.glassgallery.my.id/docs.html)
