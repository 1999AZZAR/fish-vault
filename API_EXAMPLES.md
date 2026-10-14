# Fish Vault Integration & Code Examples

Complete implementation examples for integrating Fish Vault into web apps, backend pipelines, CMS platforms, and physical pen plotters.

---

## 1. Browser JavaScript SDK

Fish Vault includes a zero-dependency client library in `js/fishdraw.js` that attaches to `window.FishDraw`.

### Basic Specimen Synthesis & Vector Rendering
```html
<script src="https://fish-vault.glassgallery.my.id/js/fishdraw.js"></script>
<script>
  // 1. Synthesize specimen deterministically from a species name or seed string
  const specimen = window.FishDraw.generateSpecimen({
    seed: 'Salmo salar',
    width: 600,
    height: 400,
    pad: 30,
    includeLabel: true
  });

  console.log('Accession Number:', specimen.accessionId);
  console.log('Standard Length:', specimen.metrics.standardLength);
  console.log('Caudal Tail Type:', specimen.metrics.tailMorphology);

  // 2. Generate clean vector SVG
  const svgMarkup = window.FishDraw.draw_svg(specimen.polylines, {
    width: 600,
    height: 400,
    bg: '#EFE9DC',
    stroke: '#2A2520',
    strokeWidth: 1.1,
    margin: 12,
    border: true
  });

  document.getElementById('plate-container').innerHTML = svgMarkup;
</script>
```

### Live Pen Plotter Simulation (SMIL Animated SVG)
```javascript
// Render stroke-by-stroke drawing animation
const animatedSvg = window.FishDraw.draw_svg_anim(specimen.polylines, 0.005, {
  width: 600,
  height: 400,
  bg: '#EFE9DC',
  stroke: '#2A2520',
  strokeWidth: 1.1
});

document.getElementById('plotter-view').innerHTML = animatedSvg;
```

---

## 2. Node.js (Server-Side / CLI)

`fishdraw.js` natively supports CommonJS `require()`:

```javascript
// generate_plate.js
const fs = require('fs');
const FishDraw = require('./js/fishdraw.js');

// Generate specimen
const specimen = FishDraw.generateSpecimen({
  seed: 'Chrosomus nebulosus',
  width: 800,
  height: 500,
  pad: 40
});

// Output static SVG
const svg = FishDraw.draw_svg(specimen.polylines, {
  width: 800,
  height: 500,
  bg: '#EFE9DC',
  stroke: '#2A2520'
});

fs.writeFileSync('specimen_plate.svg', svg);
console.log(`Saved specimen ${specimen.accessionId} (${specimen.metrics.strokeCount} strokes)`);
```

Run directly:
```bash
node generate_plate.js
```

---

## 3. Python Integration

Fetch or parse Fish Vault specimens programmatically:

### Fetching via HTTP & Parsing Telemetry
```python
import urllib.request
import json
import re

url = "https://fish-vault.glassgallery.my.id/?seed=Latimeria%20chalumnae&theme=cyanotype&w=16&h=9&mode=embed"

req = urllib.request.Request(
    url, 
    headers={'User-Agent': 'FishVault-Client/1.0'}
)

with urllib.request.urlopen(req) as response:
    html = response.read().decode('utf-8')
    print("Fetched embed plate. Length:", len(html))
```

---

## 4. Pen Plotter & AxiDraw Workflow

Fish Vault produces clean, non-intersecting polyline strokes ideal for physical plotters:

### Python: Convert Exported CSV to Inkscape / AxiDraw SVG
```python
# csv_to_plotter.py
import sys

def csv_to_svg(csv_path, output_svg_path, width=600, height=400):
    paths = []
    with open(csv_path, 'r') as f:
        for line in f:
            line = line.strip()
            if not line or line.startswith('#'):
                continue
            coords = line.split(';')
            d_pts = []
            for i, coord in enumerate(coords):
                x, y = coord.split(',')
                cmd = 'M' if i == 0 else 'L'
                d_pts.append(f"{cmd} {x} {y}")
            paths.append(f'<path d="{" ".join(d_pts)}" fill="none" stroke="black" stroke-width="0.8" />')

    svg_content = f"""<svg xmlns="http://www.w3.org/2000/svg" viewBox="0 0 {width} {height}" width="{width}mm" height="{height}mm">
    <g id="plotter_layer">
        {"\n        ".join(paths)}
    </g>
</svg>"""

    with open(output_svg_path, 'w') as f:
        f.write(svg_content)
    print(f"Exported {len(paths)} plotter paths to {output_svg_path}")

if __name__ == '__main__':
    csv_to_svg('plotter_FV-25-9AC9.csv', 'axidraw_ready.svg')
```

---

## 5. Iframe Embed & PostMessage Protocol

Embed Fish Vault into any website and dynamically control it from the parent window.

### HTML
```html
<iframe 
  id="vault-frame"
  src="https://fish-vault.glassgallery.my.id/?seed=Salmo%20salar&theme=linnaean&w=16&h=9&mode=embed" 
  width="100%" 
  height="450" 
  frameborder="0" 
  style="border: 1px solid #DDD5C5; background: #EFE9DC;" 
  allowtransparency="true">
</iframe>
```

### JavaScript Control
```javascript
const frame = document.getElementById('vault-frame');

// Update species seed in real-time
function updateFishSpecies(newSpecies) {
  frame.contentWindow.postMessage({
    type: 'SET_SEED',
    data: { seed: newSpecies }
  }, '*');
}

// Change color theme
function setArchivalTheme(themeName) {
  // themeName: 'linnaean' | 'cyanotype' | 'challenger' | 'vellum'
  frame.contentWindow.postMessage({
    type: 'SET_THEME',
    data: { theme: themeName }
  }, '*');
}
```

---

## 6. WordPress & CMS Integration

Add dynamic, deterministic biological specimen plates to WordPress articles based on the post title:

```php
// In functions.php
add_shortcode('fish_specimen_plate', function($atts) {
    global $post;
    $title = $post ? get_the_title($post->ID) : 'Archival Specimen';
    
    $seed = isset($atts['seed']) ? $atts['seed'] : $title;
    $theme = isset($atts['theme']) ? $atts['theme'] : 'linnaean';
    
    $url = sprintf(
        'https://fish-vault.glassgallery.my.id/?seed=%s&theme=%s&w=16&h=9&mode=embed',
        urlencode($seed),
        urlencode($theme)
    );
    
    return sprintf(
        '<div class="fish-vault-embed" style="margin: 2rem 0;">
            <iframe src="%s" width="100%%" height="420" frameborder="0" style="border: 1px solid #DDD5C5; border-radius: 2px;" allowtransparency="true"></iframe>
        </div>',
        esc_url($url)
    );
});
```

Usage in WordPress Block Editor or Markdown:
```text
[fish_specimen_plate theme="cyanotype"]
```
