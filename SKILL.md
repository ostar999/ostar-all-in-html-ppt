---
name: ostar-all-in-html-ppt
description: ostar-all-in-html-ppt — All-in-one HTML PPT Studio with built-in PDF/SVG export. Author professional static HTML presentations with P-key export to PDF (browser print) and SVG (per-slide ZIP). Use when the user asks for a presentation, PPT, slides, keynote, deck, slideshow, "幻灯片", "演讲稿", "做一份 PPT", "做一份 slides", a reveal-style HTML deck, a 小红书 图文, or any kind of multi-slide pitch/report/sharing document that should look tasteful and be usable with keyboard navigation. Triggers include keywords like "presentation", "ppt", "slides", "deck", "keynote", "reveal", "slideshow", "幻灯片", "演讲稿", "分享稿", "小红书图文", "talk slides", "pitch deck", "tech sharing", "technical presentation".
---

# ostar-all-in-html-ppt — All-in-One HTML PPT Studio

Author professional HTML presentations as static files. One theme file = one
look. One layout file = one page type. One animation class = one entry effect.
All pages share a token-based design system in `assets/base.css`.

## Install

```bash
npx skills add https://github.com/ostar999/ostar-all-in-html-ppt.git
```

One command, no build. Pure static HTML/CSS/JS with only CDN webfonts.

## What the skill gives you

- **36 themes** (`assets/themes/*.css`) — minimal-white, editorial-serif, soft-pastel, sharp-mono, arctic-cool, sunset-warm, catppuccin-latte/mocha, dracula, tokyo-night, nord, solarized-light, gruvbox-dark, rose-pine, neo-brutalism, glassmorphism, bauhaus, swiss-grid, terminal-green, xiaohongshu-white, rainbow-gradient, aurora, blueprint, memphis-pop, cyberpunk-neon, y2k-chrome, retro-tv, japanese-minimal, vaporwave, midcentury, corporate-clean, academic-paper, news-broadcast, pitch-deck-vc, magazine-bold, engineering-whiteprint
- **15 full-deck templates** (`templates/full-decks/<name>/`) — complete multi-slide decks with scoped `.tpl-<name>` CSS. 8 extracted from real-world decks (xhs-white-editorial, graphify-dark-graph, knowledge-arch-blueprint, hermes-cyber-terminal, obsidian-claude-gradient, testing-safety-alert, xhs-pastel-card, dir-key-nav-minimal), 7 scenario scaffolds (pitch-deck, product-launch, tech-sharing, weekly-report, xhs-post 3:4, course-module, **presenter-mode-reveal** — 演讲者模式专用)
- **31 layouts** (`templates/single-page/*.html`) with realistic demo data
- **27 CSS animations** (`assets/animations/animations.css`) via `data-anim`
- **20 canvas FX animations** (`assets/animations/fx/*.js`) via `data-fx` — particle-burst, confetti-cannon, firework, starfield, matrix-rain, knowledge-graph (force-directed), neural-net (pulses), constellation, orbit-ring, galaxy-swirl, word-cascade, letter-explode, chain-react, magnetic-field, data-stream, gradient-blob, sparkle-trail, shockwave, typewriter-multi, counter-explosion
- **Keyboard runtime** (`assets/runtime.js`) — arrows, T (theme), A (anim), F/O, **S (presenter mode: magnetic-card popup with CURRENT / NEXT / SCRIPT / TIMER cards)**, **P (export: thumbnail picker → PDF via browser print (SVG) / PDF via PNG merge / SVG ZIP / PNG ZIP download)**, N (notes drawer), R (reset timer in presenter)
- **FX runtime** (`assets/animations/fx-runtime.js`) — auto-inits `[data-fx]` on slide enter, cleans up on leave
- **Showcase decks** for themes / layouts / animations / full-decks gallery
- **Headless Chrome render script** for PNG export

## When to use

Use when the user asks for any kind of slide-based output or wants to turn
text/notes into a presentable deck. Prefer this over building from scratch.

### 🎤 Presenter Mode (演讲者模式 + 逐字稿)

If the user mentions any of: **演讲 / 分享 / 讲稿 / 逐字稿 / speaker notes / presenter view / 演讲者视图 / 提词器**, or says things like "我要去给团队讲 xxx", "要做一场技术分享", "怕讲不流畅", "想要一份带逐字稿的 PPT" — **use the `presenter-mode-reveal` full-deck template** and write 150–300 words of 逐字稿 in each slide's `<aside class="notes">`.

See [references/presenter-mode.md](references/presenter-mode.md) for the full authoring guide including the 3 rules of speaker script writing:
1. **不是讲稿，是提示信号** — 加粗核心词 + 过渡句独立成段
2. **每页 150–300 字** — 2–3 分钟/页的节奏
3. **用口语，不用书面语** — "因此"→"所以"，"该方案"→"这个方案"

All full-deck templates support the S key presenter mode (it's built into `runtime.js`). **S opens a new popup window with 4 magnetic cards**:
- 🔵 **CURRENT** — pixel-perfect iframe preview of the current slide
- 🟣 **NEXT** — pixel-perfect iframe preview of the next slide
- 🟠 **SPEAKER SCRIPT** — large-font 逐字稿 (scrollable)
- 🟢 **TIMER** — elapsed time + slide counter + prev/next/reset buttons

Each card is **draggable by its header** and **resizable by the bottom-right corner handle**. Card positions/sizes persist to `localStorage` per deck. A "Reset layout" button restores the default arrangement.

**Why the previews are pixel-perfect**: each preview is an `<iframe>` that loads the actual deck HTML with a `?preview=N` query param; `runtime.js` detects this and renders only slide N with no chrome. So the preview uses the **same CSS, theme, fonts, and viewport as the audience view** — colors and layout are guaranteed identical.

**Smooth navigation**: on slide change, the presenter window sends `postMessage({type:'preview-goto', idx:N})` to each iframe. The iframe just toggles `.is-active` between slides — **no reload, no flicker**. The two windows also stay in sync via `BroadcastChannel`.

Only `presenter-mode-reveal` is designed from the ground up around the feature with proper example 逐字稿 on every slide.

Keyboard in presenter window: `← →` navigate (syncs audience) · `R` reset timer · `Esc` close popup.
Keyboard in audience window: `S` open presenter · `P` export PDF(SVG)/PDF(PNG)/SVG/PNG · `T` cycle theme · `← →` navigate (syncs presenter) · `F` fullscreen · `O` overview.

## Before you author anything — ALWAYS ask or recommend

**Do not start writing slides until you understand three things.** Either ask
the user directly, or — if they already handed you rich content — propose a
tasteful default and confirm.

1. **Content & audience.** What's the deck about, how many slides, who's
   watching (engineers / execs / 小红书读者 / 学生 / VC)?
2. **Style / theme.** Which of the 36 themes fits? If unsure, recommend 2-3
   candidates based on tone:
   - Business / investor pitch → `pitch-deck-vc`, `corporate-clean`, `swiss-grid`
   - Tech sharing / engineering → `tokyo-night`, `dracula`, `catppuccin-mocha`,
     `terminal-green`, `blueprint`
   - 小红书图文 → `xiaohongshu-white`, `soft-pastel`, `rainbow-gradient`,
     `magazine-bold`
   - Academic / report → `academic-paper`, `editorial-serif`, `minimal-white`
   - Edgy / cyber / launch → `cyberpunk-neon`, `vaporwave`, `y2k-chrome`,
     `neo-brutalism`
3. **Starting point.** One of the 15 full-deck templates, or scratch? Point
   to the closest `templates/full-decks/<name>/` and ask if it fits. If the
   user's content suggests something obvious (e.g. "我要做产品发布会" →
   `product-launch`), propose it confidently instead of asking blindly.

A good opening message looks like:

> 我可以给你做这份 PPT！先确认三件事：
> 1. 大致内容 / 页数 / 观众是谁？
> 2. 风格偏好？我建议从这 3 个主题里选一个：`tokyo-night`（技术分享默认好看）、`xiaohongshu-white`（小红书风）、`corporate-clean`（正式汇报）。
> 3. 要不要用我现成的 `tech-sharing` 全 deck 模板打底？

Only after those are clear, scaffold the deck and start writing.

## Quick start

1. **Scaffold a new deck.** From the repo root:
   ```bash
   ./scripts/new-deck.sh my-talk
   open examples/my-talk/index.html
   ```
2. **Pick a theme.** Open the deck and press `T` to cycle. Or hard-code it:
   ```html
   <link rel="stylesheet" id="theme-link" href="../assets/themes/aurora.css">
   ```
   Catalog in [references/themes.md](references/themes.md).
3. **Pick layouts.** Copy `<section class="slide">...</section>` blocks out of
   files in `templates/single-page/` into your deck. Replace the demo data.
   Catalog in [references/layouts.md](references/layouts.md).
4. **Add animations.** Put `data-anim="fade-up"` (or `class="anim-fade-up"`) on
   any element. On `<ul>`/grids, use `anim-stagger-list` for sequenced reveals.
   For canvas FX, use `<div data-fx="knowledge-graph">...</div>` and include
   `<script src="../assets/animations/fx-runtime.js"></script>`.
   Catalog in [references/animations.md](references/animations.md).
5. **Use a full-deck template.** Copy `templates/full-decks/<name>/` into
   `examples/my-talk/` as a starting point. Each folder is self-contained with
   scoped CSS. Catalog in [references/full-decks.md](references/full-decks.md)
   and gallery at `templates/full-decks-index.html`.
6. **Render to PNG.**
   ```bash
   ./scripts/render.sh templates/theme-showcase.html       # one shot
   ./scripts/render.sh examples/my-talk/index.html 12      # 12 slides
   ```

## Authoring rules (important)

- **🛑 DEFAULT: Single self-contained HTML file.** Every generated deck MUST be a
  single `.html` file containing ALL CSS and JS inline — no external `<link>`
  stylesheets, no `<script src="...">` references to `assets/`. The only exception
  is Google Fonts CDN `@import` (fonts are binary, cannot be inlined). This means:
  - Read and inline these source files into the HTML (paths relative to skill root):
    - `assets/base.css` → full content into `<style>` (layout primitives, chrome, overview, notes, export dialog, print)
    - `assets/animations/animations.css` → all `@keyframes` + animation classes into same `<style>`
    - `assets/themes/<name>.css` → `:root` token values into same `<style>` (or copy tokens from template's scoped style)
    - `templates/full-decks/<name>/style.css` → custom component styles into same `<style>` (strip `.tpl-xxx` scoping)
    - `assets/runtime.js` → full content into `<script>` at end of `<body>`
  - **Do NOT** create `style.css`, `theme.css`, or any external asset files alongside the HTML
  - **Do NOT** create symlinks to `assets/`
  - Only use external `<link>` / `<script src>` if the user explicitly says "用外联相对路径" or similar
  - When user switches themes, templates, or adds animations, re-read the source files and update
    the inline `<style>` and `<script>` contents — never mix inline and external

- **🛑 Split generation into multiple steps — NEVER write the full HTML in one shot.**
  A complete deck with inlined CSS + JS is large. Writing it in a single API call
  causes hangs and truncated output. Always use this 3-step workflow:
  **Step 1 — Write skeleton.** Create the HTML file with `<style>` (all CSS inlined from
  source files) + `<script>` (runtime.js inlined) + `<div class="deck"></div>` — but NO slides.
  **Step 2 — Insert slides.** Use Edit to insert `<section class="slide">...</section>`
  blocks between `<div class="deck">` and `</div>`. If the deck has many slides,
  insert them in batches rather than all at once.
  **Step 3 — Verify.** Read back the file, check slide count matches expected, test P-key export.
- **Always start from a template.** Don't author slides from scratch — copy the
  closest layout from `templates/single-page/` first, then replace content.
- **Use tokens, not literal colors.** Every color, radius, shadow should come
  from CSS variables defined in `assets/base.css` and overridden by a theme.
  Good: `color: var(--text-1)`. Bad: `color: #111`.
- **Don't invent new layout files.** Prefer composing existing ones. Only add
  a new `templates/single-page/*.html` if none of the 31 fit.
- **Respect chrome slots.** `.deck-header`, `.slide-number` and the progress
  bar are provided by `assets/base.css` + `runtime.js`.
- **Do NOT add `.deck-footer` by default.** `.deck-footer` uses `position:absolute`
  and can overlap with slide content (especially centered layouts like Cover).
  Only add it if the user explicitly asks for tags or page numbers in the footer.
  If added, you MUST visually verify that it does not overlap with any slide
  content on every page — test at different viewport sizes.
- **Keyboard-first.** Always inline `assets/runtime.js` content in a `<script>` tag
  at the end of `<body>` so the deck supports ← → / T / A / F / P / S / O / hash deep-links.
  Do NOT use `<script src="...">` — copy the runtime.js source into the HTML file.
- **One `.slide` per logical page.** `runtime.js` makes `.slide.is-active`
  visible; all others are hidden.
- **Supply notes.** Wrap speaker notes in `<div class="notes">…</div>` inside
  each slide. Press S to open the overlay.
- **NEVER put presenter-only text on the slide itself.** Descriptive text like
  "这一页展示了……" or "Speaker: 这里可以补充……" or small explanatory captions
  aimed at the presenter MUST go inside `<div class="notes">`, NOT as visible
  `<p>` / `<span>` elements on the slide. The `.notes` class is `display:none`
  by default — it only appears in the S overlay. Slides should contain ONLY
  audience-facing content (titles, bullet points, data, charts, images).

### SVG/PNG export rules (CRITICAL — read before authoring custom styles)

**🛑 MANDATORY: After generating ANY deck, you MUST verify export works.** Open
the deck in a browser, press `P`, and confirm:
1. The export dialog opens with all slide thumbnails visible and correctly styled.
2. Click "导出 PDF (SVG)" or "导出 SVG (.zip)" — the download starts.
3. Open the downloaded file — layout, colors, fonts, and terminal blocks match the browser rendering.

If ANY of the above fails, run the checklist below BEFORE telling the user the deck is done.
The same checklist applies when the user switches themes, templates, or adds animations —
the inline `<style>` must stay in sync with every change.

**Export repair checklist (run in order until fixed):**

1. ☐ Are all critical CSS rules in an inline `<style>` tag (not external `<link>`)?
2. ☐ Does `.slide` have explicit `background:var(--bg);color:var(--text-1)`?
3. ☐ Are scoping classes (`.tpl-xxx`) removed or also added to each `<section class="slide">`?
4. ☐ Are `.overview` / `.notes-overlay` / `.export-overlay` CSS blocks present?
5. ☐ Does `.export-bar::after` contain the patience hint?
6. ☐ Does `.export-thumb.selected` use `border-color: var(--accent-2)` (NOT `--accent`)?
7. ☐ If user switched themes: are `:root` token values updated to the new theme?
8. ☐ If user added animations: are the `@keyframes` definitions present in inline `<style>`?

---

- **All critical CSS MUST be inline in a `<style>` tag.** Do NOT put theme
  variables or base layout styles in external `<link>` stylesheets and rely on
  runtime's `fetch()` to load them at export time. `fetch()` fails silently on
  `file://` (browser CORS policy), and SVGs rendered without those styles will
  be broken. See `examples/export-reference/index.html` for the canonical
  pattern: one big `<style>` block containing `:root` theme tokens + all
  layout primitives + custom component styles + export dialog CSS.

- **Scoping classes (`.tpl-<name>`) MUST be present in the SVG DOM.** The
  `slideToSVG()` function clones only the `<section class="slide">` element —
  it does NOT include the outer `<body>` or `<div class="deck">`. If you use
  a scoping class like `.tpl-suno-tutorial` on `<body>` and write selectors
  like `.tpl-suno-tutorial .code-block`, those selectors will FAIL in the SVG
  because the `.tpl-suno-tutorial` ancestor is missing. **Fix**: add the scoping
  class to each `<section class="slide tpl-suno-tutorial">` as well, and update
  selectors that target `.slide` as a descendant (e.g., `.tpl-suno-tutorial .slide::before`
  must also become `.slide.tpl-suno-tutorial::before`).

- **Export thumb selected border MUST use `var(--accent-2)`, NOT `var(--accent)`.**
  Many themes set `--accent` to white (`#fff`), which is the same color used
  for the checkbox background (`.export-thumb.selected .export-check`). When
  both border and checkbox are white, users can't tell which thumbnails are
  selected. Use `var(--accent-2)` for `.export-thumb.selected { border-color }`
  — it's always visibly distinct from the white checkbox.
  ```css
  /* ✅ Correct */
  .export-thumb.selected { border-color: var(--accent-2); }
  /* ❌ Wrong — invisible on white-accent themes like blueprint */
  .export-thumb.selected { border-color: var(--accent); }
  ```

- **The `.slide` class must set `background` and `color` explicitly.** In a
  browser, these are inherited from `<html>`/`<body>` via `base.css`. In an SVG
  `foreignObject`, there is no `<html>` or `<body>`, so the slide falls back to
  transparent/black. Always add `background:var(--bg);color:var(--text-1)` to
  `.slide` in your inline CSS.

- **SVG `foreignObject` is browser-only.** Non-browser tools (ImageMagick,
  Mac Preview, sharp, etc.) do not render `foreignObject` content — they
  produce blank images. For PNG output, use the built-in P-key PNG export
  (SVG → data URL → canvas → PNG) or `scripts/render.sh` (headless Chrome).
  The built-in PNG export uses `data:` URLs (NOT `blob:` URLs) to avoid
  canvas tainting from cross-origin font references.

- **`replace_all` is fragile with varying HTML attributes.** When doing
  batch replacements like adding a class to all slides, elements with extra
  attributes (e.g., `<section class="slide center">`) won't match
  `<section class="slide"`. Always grep for leftovers after `replace_all`.

- **Export `updateCount()` MUST use a cached slide total, NEVER `document.querySelectorAll('.slide').length`.** The export dialog's `renderThumbs()` clones each slide into thumbnails — every clone keeps the `.slide` class. After cloning, `querySelectorAll('.slide')` returns double (or more) the real count. Cache the real slide count in a module-level var (`_slideTotal`) inside `renderThumbs()` BEFORE the cloning loop, and use that cached value in `updateCount()`.
  ```javascript
  // ✅ Correct
  var _slideTotal = 0;
  function renderThumbs(){
    grid.innerHTML = '';
    var slides = document.querySelectorAll('.deck .slide');
    _slideTotal = slides.length; // cache before cloning
    slides.forEach(function(s,i){ /* clone .slide */ });
    updateCount();
  }
  function updateCount(){
    var n = dialog.querySelectorAll('.export-thumb.selected').length;
    dialog.querySelector('#exp-count').textContent = '已选 '+n+'/'+_slideTotal;
  }
  // ❌ Wrong — counts cloned thumb .slide elements too
  function updateCount(){
    var total = document.querySelectorAll('.slide').length; // e.g. 28 instead of 14
  }
  ```

- **Every deck MUST include an export wait hint.** The P-key export dialog must
  show a patience message below the action buttons, because large decks take
  noticeable time to render. Add this CSS rule to every deck's inline `<style>`:
  ```css
  .export-bar::after {
    content: "由于信息较多，导出文件所花时间可能比较长，请耐心等待。";
    flex: 0 0 100%;
    text-align: center;
    padding-top: 10px;
    font-size: 12px;
    color: var(--text-3);
    opacity: .8;
  }
  ```
  The `::after` pseudo-element on `.export-bar` (which is `display:flex; flex-wrap:wrap`)
  takes a full row below the buttons. No JavaScript needed — pure CSS.

### Runtime chrome CSS (CRITICAL — when NOT loading `base.css`)

If you choose to put all CSS inline in a `<style>` tag (instead of linking
`assets/base.css`), you MUST also include CSS for the **runtime-created chrome
elements**. `runtime.js` creates these DOM elements, but they have no built-in
styles — they rely entirely on `base.css`:

- **`.overview` / `.overview.open` / `.overview .thumb`** — O 键幻灯片总览网格。
  缺失 = 按 O 键后只看到透明遮罩，看不到任何缩略图。
- **`.notes-overlay` / `.notes-overlay.open`** — N 键演讲备注底部抽屉。
  缺失 = 按 N 键后抽屉不显示。

**Why this happens:** `runtime.js` dynamically creates `<div class="overview">`
and appends it to `<body>`, then toggles `.open` to show it. If no CSS rule
targets `.overview.open`, the element stays `display:none` (or invisible with
no layout). The user presses O and nothing happens — but the DOM is correct.

**How to fix:** Copy the `.overview` and `.notes-overlay` CSS blocks from
`assets/base.css` into your inline `<style>`. The minimal set:

```css
.overview{position:fixed;inset:0;background:rgba(10,12,18,.92);backdrop-filter:blur(12px);
  z-index:50;display:none;overflow-y:auto;padding:24px}
.overview.open{display:grid;grid-template-columns:repeat(4,1fr);gap:16px;align-content:start}
.overview .thumb{position:relative;background:var(--surface);border:1px solid var(--border);
  border-radius:12px;overflow:hidden;cursor:pointer;transition:transform .2s;aspect-ratio:16/9}
.overview .thumb:hover{transform:scale(1.04)}
.notes-overlay{position:fixed;bottom:0;left:0;right:0;max-height:40vh;
  background:rgba(10,12,18,.96);border-top:1px solid var(--border);z-index:50;
  overflow-y:auto;padding:24px 32px;font-size:16px;line-height:1.8;color:var(--text-1);display:none}
.notes-overlay.open{display:block}
```

**Checklist before shipping an inline-CSS deck:**
- [ ] O 键 → 总览网格正常显示
- [ ] N 键 → 笔记抽屉正常显示
- [ ] P 键 → 导出对话框正常显示
- [ ] S 键 → 演讲者窗口正常弹出

## Writing guide

See [references/authoring-guide.md](references/authoring-guide.md) for a
step-by-step walkthrough: file structure, naming, how to transform an outline
into a deck, how to choose layouts and themes per audience, how to do a
Chinese + English deck, and how to export.

## Catalogs (load when needed)

- [references/themes.md](references/themes.md) — all 36 themes with when-to-use.
- [references/layouts.md](references/layouts.md) — all 31 layout types.
- [references/animations.md](references/animations.md) — 27 CSS + 20 canvas FX animations.
- [references/full-decks.md](references/full-decks.md) — all 15 full-deck templates.
- [references/presenter-mode.md](references/presenter-mode.md) — **演讲者模式 + 逐字稿编写指南（技术分享/演讲必看）**.
- [references/authoring-guide.md](references/authoring-guide.md) — full workflow.
- [examples/export-reference/](examples/export-reference/index.html) — **P 键导出功能完整参考模板**，含导出对话 UI、SVG 生成、PDF 打印的完整实现。生成任何 deck 时以此为参考确保导出功能可正常工作。

## File structure

```
ostar-all-in-html-ppt/
├── SKILL.md                 (this file)
├── README.md                (中文 README)
├── README.EN.md             (English README)
├── references/              (detailed catalogs, load as needed)
├── assets/
│   ├── base.css             (tokens + primitives — do not edit per deck)
│   ├── fonts.css            (webfont imports)
│   ├── runtime.js           (keyboard + presenter + export + overview + theme cycle)
│   ├── themes/*.css         (36 token overrides, one per theme)
│   └── animations/
│       ├── animations.css   (27 named CSS entry animations)
│       ├── fx-runtime.js    (auto-init [data-fx] on slide enter)
│       └── fx/*.js          (20 canvas FX modules: particles/graph/fireworks…)
├── templates/
│   ├── deck.html                  (minimal 6-slide starter)
│   ├── theme-showcase.html        (36 slides, iframe-isolated per theme)
│   ├── layout-showcase.html       (iframe tour of all 31 layouts)
│   ├── animation-showcase.html    (20 FX + 27 CSS animation slides)
│   ├── full-decks-index.html      (gallery of all 15 full-deck templates)
│   ├── full-decks/<name>/         (15 scoped multi-slide deck templates)
│   └── single-page/*.html         (31 layout files with demo data)
├── scripts/
│   ├── new-deck.sh                (scaffold a deck from deck.html)
│   └── render.sh                  (headless Chrome → PNG)
├── examples/
│   ├── demo-deck/                  (complete working deck)
│   └── export-reference/           (P-key export reference template)
```

## Rendering to PNG

`scripts/render.sh` wraps headless Chrome at
`/Applications/Google Chrome.app/Contents/MacOS/Google Chrome`. For multi-slide
capture, runtime.js exposes `#/N` deep-links, and render.sh iterates 1..N.

```bash
./scripts/render.sh templates/single-page/kpi-grid.html        # single page
./scripts/render.sh examples/demo-deck/index.html 8 out-dir    # 8 slides, custom dir
```

## Keyboard cheat sheet

```
←  →  Space  PgUp  PgDn  Home  End    navigate
F                                       fullscreen
S                                       open presenter window (magnetic cards: current/next/script/timer)
P                                       export dialog — select slides → PDF via browser print (SVG) / PDF via PNG merge / SVG (.zip) / PNG (.zip)
N                                       quick notes drawer (bottom overlay)
R                                       reset timer (in presenter window)
?preview=N                              URL param — force preview-only mode (single slide, no chrome)
O                                       slide overview grid
T                                       cycle themes (reads data-themes attr)
A                                       cycle demo animation on current slide
#/N in URL                              deep-link to slide N
Esc                                     close all overlays
```

## License & author

MIT. Copyright (c) 2026 ostar999 &lt;ota1754@qq.com&gt;.
