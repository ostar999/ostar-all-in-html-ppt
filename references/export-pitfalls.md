# Export Pitfalls — Lessons Learned

This document catalogs real-world export bugs discovered across multiple decks,
their root causes, and the verified fixes. **Read this before debugging any
P-key export issue.**

---

## Pitfall 1: Print CSS destroys grid layouts

**Symptom:** Sidebar slides appear stacked vertically in PDF (SVG), sidebar
border-right vanishes, content overflows.

**Root cause:** `exportToPDFSVG()` injects `@media print` CSS that forces
`display:flex!important;flex-direction:column!important` on ALL `.slide`
elements. When slides use `display:grid;grid-template-columns:260px 1fr`
(sidebar+main layout), the grid is destroyed and children stack vertically.

**Fix in `runtime.js` `exportToPDFSVG()`:**
```javascript
// ❌ Wrong — destroys grid layout
'.slide{display:flex!important;...flex-direction:column!important;justify-content:center!important}'+

// ✅ Correct — preserves grid for sidebar slides, flex only for full slides
'.slide{display:grid!important;grid-template-columns:260px 1fr!important;gap:56px!important;align-content:start!important;...}'+
'.slide.full{display:flex!important;flex-direction:column!important;justify-content:center!important}'+
```

**Why it matters:** Any template that uses CSS Grid on `.slide` (course-module, etc.)
will be silently broken in PDF export unless print CSS respects the layout.

---

## Pitfall 2: `::before` on grid container creates unwanted grid item

**Symptom:** After fixing Pitfall 1, sidebar still broken — content shifted,
empty first column visible.

**Root cause:** The print CSS had `.slide::before{content:""!important}`.
In CSS Grid, `::before` pseudo-elements on the grid container become REAL grid
items. With `grid-template-columns:260px 1fr`, the empty `::before` occupies
column 1 (260px), `.sidebar` goes to column 2, and `.main` wraps to an implicit
row 2. Layout is completely broken.

**Fix:** Remove `.slide::before{content:""!important}` from print CSS entirely.
It was a vestigial workaround for flex layouts that doesn't apply to grid.

**Key insight:** `::before`/`::after` on a grid container are grid items.
Never add them to `.slide` in print CSS unless you explicitly account for them
in `grid-template-*`.

---

## Pitfall 3: `backdrop-filter` in SVG foreignObject

**Symptom:** Exported PNG/SVG images have blocky, blurry artifacts around card
edges and shadows. Browser rendering is fine.

**Root cause:** `slideToSVG()` and `slideToPNGBlob()` clone slide elements and
embed them in SVG `<foreignObject>`. The `.card` class (and `.overview`,
`.export-overlay`) use `backdrop-filter:blur(24px)` for glassmorphism effects.
SVG foreignObject does NOT support `backdrop-filter` — it renders as blocky
noise or is silently ignored with visual corruption.

**Fix in `runtime.js` `slideToSVG()` and `slideToPNGBlob()`:**
```javascript
var css=_getAllCSS_sync();
var rootBlock=_buildRootBlock();
// Strip backdrop-filter from CSS before SVG embedding
css=css.replace(/backdrop-filter:[^;]+;/gi,'').replace(/-webkit-backdrop-filter:[^;]+;/gi,'');
var fullCSS=_escapeXML(rootBlock+'\n'+css);
```

**Affected themes:** Any theme that uses `backdrop-filter` on cards or surfaces:
aurora, glassmorphism, cyberpunk-neon, vaporwave, tokyo-night (if customized), etc.

---

## Pitfall 4: Large box-shadow blur in SVG

**Symptom:** Exported images have blocky/pixelated shadows that look different
from the smooth browser rendering.

**Root cause:** Themes like `aurora` define `--shadow:0 20px 60px rgba(0,0,0,.4)`.
The 60px blur radius is too large for SVG renderers to handle smoothly, causing
visible banding and blocky artifacts.

**Fix in `runtime.js` `slideToSVG()` and `slideToPNGBlob()`:**
```javascript
// Reduce --shadow blur from 60px to 16px in SVG export CSS
rootBlock=rootBlock.replace(/(--shadow:)[^;]+;/,'$1 0 8px 16px rgba(0,0,0,.4),inset 0 1px 0 rgba(255,255,255,.08);');
```

**Note:** This only affects the CSS copy embedded in SVG exports. The browser
display CSS is unchanged. The regex targets `--shadow:` followed by any chars
until `;`, and replaces the value with a reduced-blur version.

---

## Pitfall 5: `gradient-text` creates visible rectangles in print

**Symptom:** PDF export shows vertical bars `|` at the boundary between normal
text and `<span class="gradient-text">` text. Example: "先了解你能|看到什么".

**Root cause:** `.gradient-text` uses:
```css
background:var(--grad);
-webkit-background-clip:text;
background-clip:text;
-webkit-text-fill-color:transparent;
color:transparent;
```
In print/PDF rendering, `background-clip:text` is NOT supported. The gradient
background renders as a visible rectangle behind the span, and its left edge
appears as a vertical bar at the text boundary.

**Fix in `base.css` `@media print`:**
```css
@media print{
  .gradient-text{
    background:none!important;
    -webkit-background-clip:initial!important;
    background-clip:initial!important;
    -webkit-text-fill-color:var(--accent)!important;
    color:var(--accent)!important;
  }
}
```

**Effect:** Gradient text degrades gracefully to solid accent-colored text in
print, preserving readability and emphasis without visual artifacts.

---

## Pitfall 6: `._pdf_show_` overrides grid/flex display

**Symptom:** After fixing print CSS grid rules, selected slides still show
broken layout.

**Root cause:** `._pdf_show_` class had `display:flex!important`. Since it's
applied directly to `.slide` elements and comes after `@media print` rules
in the stylesheet, it overrides the grid layout. With `!important` on both
sides, the later rule wins.

**Fix:** Remove `display:flex!important` from `._pdf_show_`. This class only
needs to control visibility/opacity/position — the display type should come
from the `@media print` `.slide`/`.slide.full` rules.
```javascript
// ❌ Wrong
'._pdf_show_{display:flex!important;visibility:visible!important;...}'
// ✅ Correct
'._pdf_show_{visibility:visible!important;opacity:1!important;position:relative!important;...}'
```

---

## Pitfall 7: SVG PDF export missing `_svg` filename suffix

**Symptom:** PDF (SVG) exports as "title.pdf" while PDF (PNG) exports as "title_png.pdf".

**Root cause:** `exportToPDFSVG()` calls `window.print()` without setting a custom
filename. The browser uses `<title>` as-is. `exportToPDFPNG()` creates a new
window with a title that includes `_png`.

**Fix in `runtime.js` `exportToPDFSVG()`:**
```javascript
var _origTitle=document.title;
document.title=_getExportFilename('pdf','_svg').replace('.pdf','');
setTimeout(function(){
  window.print();
  document.title=_origTitle;
  // ... cleanup
},350);
```

---

## Pitfall 8: Sidebar border too faint for export

**Symptom:** Sidebar vertical divider barely visible in exported images/PDF.

**Root cause:** Using `var(--border)` which has very low opacity (14% in aurora
theme: `rgba(180,220,255,.14)`). In browser with the aurora radial-gradient body
background, the border is visible due to the backdrop. In flat-background exports,
it's nearly invisible.

**Fix:** Use `var(--border-strong)` for sidebar borders. This doubles the opacity
(28% in aurora: `rgba(180,220,255,.28)`), making the divider visible in all
contexts.
```css
/* ❌ Too faint */
.sidebar{border-right:1px solid var(--border);...}
/* ✅ Visible */
.sidebar{border-right:1px solid var(--border-strong);...}
```

---

## Summary: When authoring a deck, always check

| # | Check | Where |
|---|-------|-------|
| 1 | Print CSS preserves grid layout | `exportToPDFSVG()` in runtime.js |
| 2 | No `::before` on grid `.slide` in print | `exportToPDFSVG()` in runtime.js |
| 3 | `backdrop-filter` stripped for SVG | `slideToSVG()` + `slideToPNGBlob()` in runtime.js |
| 4 | `--shadow` blur reduced for SVG | `slideToSVG()` + `slideToPNGBlob()` in runtime.js |
| 5 | `gradient-text` has print fallback | `@media print` in base.css |
| 6 | `._pdf_show_` doesn't set `display` | `exportToPDFSVG()` in runtime.js |
| 7 | SVG PDF has `_svg` suffix | `exportToPDFSVG()` in runtime.js |
| 8 | Sidebar uses `--border-strong` | Template/theme CSS |
