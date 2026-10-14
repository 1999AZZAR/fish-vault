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

  // 2. Generate clean vector SVG with archival plate stamps
  const svgMarkup = window.FishDraw.draw_svg(specimen.polylines, {
    width: 600,
    height: 400,
    bg: '#EFE9DC',
    stroke: '#2A2520',
    strokeWidth: 1.1,
    margin: 12,
    border: true,
    specimen: specimen
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

// Output static SVG with archival accession plate stamps
const svg = FishDraw.draw_svg(specimen.polylines, {
  width: 800,
  height: 500,
  bg: '#EFE9DC',
  stroke: '#2A2520',
  specimen: specimen
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

url = "https://fish-vault.glassgallery.my.id/?narrative=Latimeria%20chalumnae&w=16&h=9&mode=embed"

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

Fish Vault produces clean, non-intersecting polyline strokes ideal for physical CNC pen plotters (AxiDraw, HP-GL, G-Code).

### Standalone Python: Batch Generate Plotter SVGs
```python
# batch_plotter.py
import urllib.request
from js_parity import synthesize_specimen # or via Node CLI

def export_plotter_plate(narrative, filename, ratio=(3, 2)):
    url = f"https://fish-vault.glassgallery.my.id/?narrative={urllib.parse.quote(narrative)}&w={ratio[0]}&h={ratio[1]}&mode=embed"
    # Or generate locally using Node.js SDK
    print(f"Synthesizing plotter plate for: {narrative} -> {filename}")

if __name__ == '__main__':
    export_plotter_plate('Abyssal Trench Fauna', 'plate_01.svg')
```

The exported SVG contains pure `<path>` elements with `fill="none"` and `stroke-width="1.1"`, fully compatible with Inkscape and the AxiDraw Control extension.

---

## 5. Iframe Embed & PostMessage Protocol

Embed Fish Vault into any website and dynamically control it from the parent window.

### HTML
```html
<iframe 
  id="vault-frame"
  src="https://fish-vault.glassgallery.my.id/?narrative=Reflections%20on%20Ichthyology&w=16&h=9&mode=embed" 
  width="100%" 
  height="450" 
  frameborder="0" 
  style="border: 1px solid #DDD5C5; border-radius: 4px;" 
  allowtransparency="true">
</iframe>
```

### JavaScript Control
```javascript
const frame = document.getElementById('vault-frame');

// Method 1: Update narrative injection sequence in real-time
function injectNarrative(narrativeText) {
  frame.contentWindow.postMessage({
    type: 'SET_NARRATIVE',
    data: { narrative: narrativeText }
  }, '*');
}

// Method 2: Recover exact specimen by Accession ID
function loadAccessionId(accessionId) {
  frame.contentWindow.postMessage({
    type: 'SET_ID',
    data: { id: accessionId }
  }, '*');
}

// Method 3: Toggle render mode (static vs live pen plotter animation)
function setRenderMode(mode) {
  // mode: 'static' | 'anim'
  frame.contentWindow.postMessage({
    type: 'SET_MODE',
    data: { mode: mode }
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
    
    $narrative = isset($atts['narrative']) ? $atts['narrative'] : $title;
    $render = isset($atts['render']) ? $atts['render'] : 'static';
    
    $url = sprintf(
        'https://fish-vault.glassgallery.my.id/?narrative=%s&render=%s&w=16&h=9&mode=embed',
        urlencode($narrative),
        urlencode($render)
    );
    
    return sprintf(
        '<div class="fish-vault-embed" style="margin: 2rem 0;">
            <iframe src="%s" width="100%%" height="420" frameborder="0" style="border: 1px solid #DDD5C5; border-radius: 4px;" allowtransparency="true"></iframe>
        </div>',
        esc_url($url)
    );
});
```

Usage in WordPress Block Editor or Markdown:
```text
[fish_specimen_plate]
[fish_specimen_plate narrative="The Future of Distributed Systems" render="anim"]
```
