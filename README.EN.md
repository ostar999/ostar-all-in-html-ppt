# ostar-all-in-html-ppt · All-in-One HTML PPT Studio

> Cross-platform AI Skill. Any agent that supports Agent Skill (Claude Code / Codex / Cursor / OpenClaw etc.) can use this to create PPT-style HTML.

**中文文档:** [README.md](README.md)

## One-Command Install

```bash
npx skills add https://github.com/ostar999/ostar-all-in-html-ppt.git
```

- AI generates professional HTML PPTs for you: **36 themes, 15 full-deck templates, 31 page layouts, 47 animations (27 CSS + 20 Canvas FX), presenter mode (pixel-perfect previews + speaker script + timer), and built-in PDF(SVG)/PDF(PNG)/SVG/PNG export**.
- Pure static HTML/CSS/JS, no build required.

## About This Project

- Forked from [lewislulu/html-ppt-skill](https://github.com/lewislulu/html-ppt-skill) with gratitude to [lewis](https://github.com/lewislulu) and [Gao Mingfei](https://github.com/g199209).

- New features added by **ostar999** &lt;ota1754@qq.com&gt;:
  - P-key one-click PDF/SVG/PNG export.
  - Visual showcase HTML page (`showcase.html`) for browsing all themes, layouts, animations, and templates.

![html-ppt — cover with live previews](docs/readme/hero-v2.png)

---

## 💬 How to Use: Talking to AI

You don't need to write code — just describe what you want in natural language. Once this Skill is installed, the AI will recognize your PPT needs automatically.

### Trigger Keywords

The AI activates when you use words like: `PPT` `slides` `deck` `presentation` `pitch deck` `tech sharing` `幻灯片` `演讲稿` `小红书图文` `做一份`.

If you have other HTML/PPT agent skills installed, use `/ostar-all-in-html-ppt` or explicitly request the ostar-all-in-html-ppt skill.

### Conversation Template: 3 Things at Once

The AI needs to know 3 things before starting. Say them all at once for the best results:

| | You provide | Example |
|------|----------|------|
| ① | **Content & Audience** | What topic? How many slides? Need speaker notes? Engineers / investors / 小红书 readers / students? |
| ② | **Style Preference** | Dark tech / minimal white / 小红书 style / VC pitch? Let AI recommend if unsure |
| ③ | **Template Choice** | Tech sharing / weekly report / product launch / pitch deck? Or start from scratch |

### Example Dialogues

**⭐ Example 1: Recommended Workflow**

```
👤 You: /ostar-all-in-html-ppt @talk-script.md (optional: need speaker notes)

🤖 AI: I read your talk-script.md, it's about microservice migration for backend engineers.

Recommendations:
  • Theme: tokyo-night (dark, code-friendly) or dracula
  • Template: tech-sharing (8 slides · GitHub-dark · Agenda+Q&A)
  • ~9 slides: Cover→Agenda→3 content slides→Code comparison→Summary→Q&A

Does this work for you?

👤 You: Yes, tokyo-night please

🤖 AI: Generating now. I'll map your Markdown structure: # → cover, ## → section breaks, - → bullet slides, ``` → code slides...
```

> 💡 **Core workflow:** ① Save content as `.md` → ② Type `/ostar-all-in-html-ppt @doc.md` → ③ Skill reads content and recommends theme + template → ④ Confirm or adjust in natural language.

---

**Example 2: Tech Sharing (manual preferences)**

```
👤 Make me a tech sharing PPT on "Monolith to Microservices Migration",
   8-10 slides, for internal backend engineers. Dark theme, code blocks. Use tech-sharing template.

🤖 Got it:
   1. Content: Monolith→Microservices, 8-10 slides, backend engineers
   2. Style: tokyo-night or dracula (dark + code-friendly)
   3. Template: tech-sharing (GitHub-dark + terminal blocks + Agenda+Q&A)
   Starting now.
```

**Example 3: 小红书 Post**

```
👤 Make a 小红书 post: "5 Obsidian Plugins for Productivity", 3:4 portrait, warm pastel magazine style. Use xhs-post template.

🤖 Xiaohongshu post config:
   1. Content: 5 Obsidian plugins
   2. Ratio: 3:4 @ 810×1080 (portrait)
   3. Style: xiaohongshu-white or soft-pastel (warm pastel)
   4. Template: xhs-post (9 slides · pastel sticker cards)
   Generating.
```

**Example 4: Talk with Speaker Notes**

```
👤 I'm giving a 30-min talk on "AI-Assisted Programming Best Practices" next week.
   I need speaker notes — 150-300 words per slide, conversational tone. Use presenter-mode-reveal.

🤖 Presenter mode setup:
   1. Content: AI-assisted programming, 30min ≈ 10-12 slides
   2. Template: presenter-mode-reveal (presenter mode optimized)
   3. Speaker notes: 150-300 words/slide, conversational, key terms bolded
   Press S for presenter window with teleprompter + timer + next-slide preview.
```

**Example 5: Let AI Recommend**

```
👤 Make a pitch deck for VCs, about 10 slides. Not sure about style — you recommend.

🤖 VC pitch recommendations:
   1. Style: pitch-deck-vc (YC-style white+blue-purple gradient) or corporate-clean (navy, more conservative)
   2. Template: pitch-deck (10 slides · big numbers · growth charts · team+competition+TAM)
   Which style do you prefer? Or I can start with pitch-deck-vc?
```

### Conversation Tips

| ✅ Do | ❌ Don't |
|-------|----------|
| 🥇 **Best practice:** Save as `.md` → `/ostar-all-in-html-ppt @doc.md (optional: need speaker notes)` → AI recommends | Just say "make me a PPT" — AI can only guess |
| Say content + audience + style + template all at once | Switch themes/templates mid-generation |
| Use scenario keywords ("小红书 style", "VC pitch") not CSS terms | Describe pixel positions — this is not a design mockup |
| Mention slide count or duration | Forget to mention who the audience is |
| Let AI recommend 2-3 candidates if unsure | — |
| Say "need speaker notes" for talks | — |

---

## 📄 Self-Contained Single File — Generate and Deliver

The output is a **single `.html` file** that works anywhere.

The Skill automatically **inlines** everything during generation: reads `assets/base.css`, `assets/runtime.js`, `assets/animations/animations.css`, theme CSS, template CSS, and writes all content into `<style>` and `<script>` tags. The result is **one independent `.html` file**.

| Stage | Description |
|------|------|
| Before | Deck HTML references shared resources via `<link>` and `<script src>` |
| After | All CSS/JS inlined — **no external dependencies** (only Google Fonts CDN remains; fonts are binary, can't be inlined) |
| Result | Copy the `.html` file **to any device, any path** — email it, USB drive, cloud drive, WeChat — recipient double-clicks to present |

---

## 📂 Project Structure

```
ostar-all-in-html-ppt/
├── SKILL.md                      agent entry point
├── README.md                     Chinese README
├── README.EN.md                  English README (this file)
├── references/                   detailed docs
│   ├── themes.md                 36 themes with when-to-use
│   ├── layouts.md                31 layout types
│   ├── animations.md             27 CSS + 20 FX catalog
│   ├── full-decks.md             15 full-deck templates
│   ├── presenter-mode.md         🎤 presenter mode + speaker script guide
│   └── authoring-guide.md        full workflow
├── assets/
│   ├── base.css                  shared tokens + primitives
│   ├── fonts.css                 webfont imports
│   ├── runtime.js                keyboard + presenter + export + overview
│   ├── themes/*.css              36 theme token files
│   └── animations/
│       ├── animations.css        27 named CSS animations
│       ├── fx-runtime.js         auto-init [data-fx] on slide enter
│       └── fx/*.js               20 canvas FX modules
├── templates/
│   ├── deck.html                 minimal starter
│   ├── theme-showcase.html       iframe-isolated theme tour
│   ├── layout-showcase.html      all 31 layouts
│   ├── animation-showcase.html   47 animation slides
│   ├── full-decks-index.html     15-deck gallery
│   ├── full-decks/<name>/        15 scoped multi-slide decks
│   └── single-page/*.html        31 layout files with demo data
├── scripts/
│   ├── new-deck.sh               scaffold
│   ├── render.sh                 headless Chrome → PNG
│   └── verify-output/            56 self-test screenshots
├── examples/
│   ├── demo-deck/                 complete working deck
│   └── export-reference/          P-key export reference template
└── docs/
    └── readme/                    README images
```

### Concept Hierarchy: From Parts to Product

These 5 concepts build on each other:

```
Token System       → override :root →
assets/base.css    30+ CSS variables

Themes             +
assets/themes/     36 · controls "how it looks"
<name>.css

Layouts            → compose →
templates/         31 · controls "how it's arranged"
single-page/

Deck Templates     +
templates/         15 · controls "the complete storyline"
full-decks/

Animations         → runtime →
data-anim/         47 · controls "how it moves"
data-fx

runtime.js         =
assets/runtime.js  keyboard · presenter · export

                   A complete, ready-to-use HTML PPT
                   Single file, open in any browser
```

### Resource Path Reference

| Resource | Path | Count | Notes |
|----------|------|------|-------|
| 🎨 Themes | `assets/themes/<name>.css` | 36 | ~20 lines each, overrides `:root` only |
| 📐 Layouts | `templates/single-page/<name>.html` | 31 | Standalone HTML, `<body class="single">` |
| 📦 Full Decks | `templates/full-decks/<name>/` | 15 | One folder per template: index.html + style.css + README.md |
| ✨ CSS Anims | `assets/animations/animations.css` | 27 | All @keyframes + .anim-* classes in one file |
| 🎆 Canvas FX | `assets/animations/fx/<name>.js` | 20+1 | _util.js is shared dependency |
| ⚙️ FX Runtime | `assets/animations/fx-runtime.js` | 1 | Dynamic FX loading + slide lifecycle |
| ⌨️ Keyboard | `assets/runtime.js` | 1 | All shortcuts + presenter + export + overview |
| 🎯 Token System | `assets/base.css` | 1 | `:root` defaults + `.card`/`.grid`/`.slide` primitives |
| 🔤 Webfonts | `assets/fonts.css` | 1 | Google Fonts CDN @import (4 families) |
| 📖 Skill Def | `SKILL.md` | 1 | AI authoring rules + keyboard/export checklists |
| 📚 References | `references/*.md` | 8 | Catalogs + guides + bug fix manuals |

### Key Config Files

| File | Role |
|------|------|
| `assets/base.css` | **Foundation of the entire design system.** 30+ CSS variables + all layout primitives. |
| `assets/runtime.js` | **Required in every deck.** Keyboard nav, T theme, O overview, S presenter, P export. |
| `SKILL.md` | **AI generation guide.** Self-contained rules, 3-step workflow, keyboard/export checklists. |
| `references/inline-errors.md` | **Keyboard bug reference.** T key failure, JS errors, overview CSS missing. |
| `references/export-pitfalls.md` | **Export bug reference.** backdrop-filter removal, slide background loss, etc. |

---

## 🎨 36 Themes

### What is a "Theme"?

A theme is a set of **CSS variables (tokens)** defining the visual foundation: background, text colors, accent colors, card styles, shadows, border-radius, fonts — 20+ properties.

| | Description |
|------|------|
| Analogy | Like PowerPoint's **color scheme + font scheme** |
| Effect | Swap one theme CSS file = entire deck changes look instantly; content unaffected |
| Usage | Press `T` to cycle, or replace `<link id="theme-link">` href in HTML |

![36 Themes Preview (1/4)](docs/readme/themes-p1.png)
![36 Themes Preview (2/4)](docs/readme/themes-p2.png)
![36 Themes Preview (3/4)](docs/readme/themes-p3.png)
![36 Themes Preview (4/4)](docs/readme/themes-p4.png)

### Theme Quick Reference

| Category | Themes | Best For |
|----------|--------|----------|
| Soft & Light | `minimal-white` `editorial-serif` `soft-pastel` `xiaohongshu-white` `solarized-light` `catppuccin-latte` | Minimal reports, brand stories, 小红书, teaching |
| Bold & Statement | `sharp-mono` `neo-brutalism` `bauhaus` `swiss-grid` `memphis-pop` | Startup pitches, design talks, youth brands |
| Cool & Dark | `catppuccin-mocha` `dracula` `tokyo-night` `nord` `gruvbox-dark` `rose-pine` `arctic-cool` | Tech talks, developer sharing, finance |
| Warm & Vibrant | `sunset-warm` | Lifestyle, awards |
| Effect-Heavy | `glassmorphism` `aurora` `rainbow-gradient` `blueprint` `terminal-green` | Keynotes, hero slides, CLI demos |
| Professional Light | `corporate-clean` `pitch-deck-vc` `academic-paper` `japanese-minimal` `engineering-whiteprint` | Boardroom, VC pitch, academic, API docs |
| Editorial | `magazine-bold` `news-broadcast` `midcentury` `retro-tv` | Columns, data broadcast, vintage |
| Dramatic | `cyberpunk-neon` `vaporwave` `y2k-chrome` | Cyber talks, art, Gen-Z |

---

## 📐 31 Page Layouts

### What is a "Layout"?

A layout is an **HTML skeleton for a single slide** — it determines content arrangement: cover design, bullet lists, chart placement, code blocks. Each layout is a `<section class="slide">` block with demo data.

| | Description |
|------|------|
| Analogy | Like PowerPoint's **slide layouts / master slides** |
| Relation to Themes | Theme controls color → Layout controls structure → they compose independently. Same "two-column" layout with `tokyo-night` = dark tech; with `xiaohongshu-white` = 小红书 style |

![31 Layouts Preview](docs/readme/layouts-1.png)
![31 Layouts Preview](docs/readme/layouts-2.png)
![31 Layouts Preview](docs/readme/layouts-3.png)
![31 Layouts Preview](docs/readme/layouts-4.png)

### Layout Categories

| Category | Layouts | Use |
|----------|---------|-----|
| Openers & Transitions | `cover` `toc` `section-divider` | Deck cover, TOC, section breaks |
| Text-Centric | `bullets` `two-column` `three-column` `big-quote` | Bullet lists, side-by-side, pull quotes |
| Data & Charts | `stat-highlight` `kpi-grid` `table` `chart-bar` `chart-line` `chart-pie` `chart-radar` | Big numbers, KPIs, charts |
| Code & Terminal | `code` `diff` `terminal` | Syntax highlighting, diffs, terminal |
| Diagrams & Flows | `flow-diagram` `arch-diagram` `process-steps` `mindmap` | Pipelines, architecture, mind maps |
| Plans & Comparisons | `timeline` `roadmap` `gantt` `comparison` `pros-cons` `todo-checklist` | Timelines, roadmaps, before/after |
| Visuals | `image-hero` `image-grid` | Full-bleed images, bento grids |
| Closers | `cta` `thanks` | Call-to-action, thank you |

### How to Use: 3 Steps

Using `stat-highlight.html` as an example:

**Step 1: Open the layout source** and find `<section class="slide">`

```html
<!-- templates/single-page/stat-highlight.html (source) -->
<section class="slide center" data-title="Stat">
  <div style="font-size:260px;line-height:1;font-weight:900;letter-spacing:-.05em">
    <span class="counter gradient-text" data-to="92">0</span>
    <span class="gradient-text">%</span>
  </div>
  <h3>of prep time saved</h3>
  <p class="lede">Across 10 real projects, average deck creation time dropped from 4 hours to 20 minutes with html-ppt.</p>
</section>
```

**Step 2: Copy into your deck**, inside `<div class="deck">`

```html
<div class="deck">
  <!-- existing slides -->
  <section class="slide" data-title="Cover">...</section>

  <!-- 👇 paste here -->
  <section class="slide center" data-title="Stat">...</section>
  <!-- 👆 paste here -->
</div>
```

**Step 3: Replace demo data** with your content

```html
<section class="slide center" data-title="Revenue">
  <div style="font-size:260px;line-height:1;font-weight:900;letter-spacing:-.05em">
    <span class="counter gradient-text" data-to="58">0</span>
    <span class="gradient-text">%</span>
  </div>
  <h3>YoY revenue growth</h3>
  <p class="lede">Q3 revenue exceeded ¥24M, driven by new product line launches and market expansion.</p>
</section>
```

> 💡 **Key tip:** Keep `class="slide"` and `data-title` unchanged (runtime.js depends on them). Keep animation attributes (`counter`, `data-to`, `gradient-text`). Only replace text and numbers. All 31 layouts follow the same rule.

---

## ✨ 47 Animations

### What are "Animations"?

Two types: **CSS entry animations** (fire once when element enters) and **Canvas FX** (run continuously while slide is active).

| Type | Usage | Best For |
|------|-------|----------|
| **CSS (27)** | `data-anim="name"` on any element, runtime.js re-triggers on each page visit | Title entrances, staggered lists, counters |
| **Canvas FX (20)** | `data-fx="name"` on container div, fx-runtime.js manages lifecycle | Cover backgrounds, data viz, celebration pages |

> 💡 Design rule: max 1-2 animation types per slide. Stagger-list + one hero entrance = clean rhythm.

![Animations Preview](docs/readme/animations-1.png)

### CSS Entry Animations (27)

| Category | Animations | Effect |
|----------|------------|--------|
| Directional | `fade-up` `fade-down` `fade-left` `fade-right` | Translate + fade from each direction |
| Dramatic | `rise-in` `drop-in` `zoom-pop` `blur-in` `glitch-in` | Rise/drop/zoom/blur/glitch |
| Text FX | `typewriter` `neon-glow` `shimmer-sweep` `gradient-flow` | Typewriter/glow/shimmer/gradient |
| Lists & Numbers | `stagger-list` `counter-up` | Children appear sequentially / number counts up |
| SVG | `path-draw` `morph-shape` | Stroke draw / path morph |
| 3D | `parallax-tilt` `card-flip-3d` `cube-rotate-3d` `page-turn-3d` `perspective-zoom` | Tilt/flip/rotate/page turn/perspective |
| Ambient | `marquee-scroll` `kenburns` `confetti-burst` `spotlight` `ripple-reveal` | Scroll/zoom/confetti/spotlight/ripple |

### Canvas FX (20)

![Animations Preview](docs/readme/animations-2.png)

| FX | Effect | Best For |
|----|--------|----------|
| `particle-burst` | Particles explode from center | Reveal moments |
| `confetti-cannon` | Confetti from bottom corners | Thank you / success |
| `firework` | Fireworks launching upward | Celebration / launch |
| `starfield` | 3D starfield flying outward | Sci-fi backgrounds |
| `matrix-rain` | Falling katakana + hex | Cyber / security |
| `knowledge-graph` | Force-directed graph, 28 nodes | Knowledge / RAG |
| `neural-net` | 4-6-6-3 feedforward net | ML / architecture |
| `constellation` | Drifting connected points | Ambient hero backgrounds |
| `orbit-ring` | 5 concentric rings at different speeds | System / layered concepts |
| `galaxy-swirl` | ~800 logarithmic spiral particles | Covers / intros |
| `word-cascade` | Words fall from top | Vocabulary / concept clouds |
| `letter-explode` | Letters fly in from random directions | Hero titles |
| `chain-react` | Domino pulse across 8 circles | Pipeline / sequential |
| `magnetic-field` | Bezier curve particle trails | Energy / flow |
| `data-stream` | Scrolling hex/binary text | Data / API / security |
| `gradient-blob` | 4 drifting radial gradients | Soft hero backgrounds |
| `sparkle-trail` | Pointer-driven sparkles | Interactive hover |
| `shockwave` | Expanding rings from center | Impact / launch / alert |
| `typewriter-multi` | 3 lines typing concurrently | Terminal / boot logs |
| `counter-explosion` | Number counts up + particles | KPI reveals |

---

## 📦 15 Full-Deck Templates

### What is a "Deck Template"?

A deck template is a **complete multi-slide file** (usually 6-10 slides) with pre-combined theme + layouts + animations. It's not a part — it's a **ready-to-use product**.

| | Description |
|------|------|
| Analogy | Like PowerPoint's **template (.potx)** — cover, TOC, content, charts, closing all ready |
| vs Layouts/Themes | Theme = color scheme → Layout = single-page structure → **Deck template = complete multi-page composition** |
| Origin | 8 extracted from real projects, 7 generic scenario scaffolds |

![15 Templates Preview](docs/readme/templates-1.png)
![15 Templates Preview](docs/readme/templates-2.png)

### Template Quick Reference

| # | Template | Style | Slides | Best For |
|---|----------|-------|--------|----------|
| 1 | `xhs-white-editorial` | White magazine | 7+ | 小红书 + horizontal decks, Chinese-first |
| 2 | `graphify-dark-graph` | Dark knowledge graph | 7+ | CLI / knowledge graph / data viz launches |
| 3 | `knowledge-arch-blueprint` | Blueprint architecture | 7+ | System architecture, printable |
| 4 | `hermes-cyber-terminal` | Cyber terminal | 7+ | CLI / Agent / dev tool reviews |
| 5 | `obsidian-claude-gradient` | GitHub dark purple | 7+ | Developer workflows / MCP / Agent tutorials |
| 6 | `testing-safety-alert` | Red-amber safety | 7+ | Security / risk / incident postmortems |
| 7 | `xhs-pastel-card` | Soft pastel lifestyle | 7+ | Lifestyle / personal growth |
| 8 | `dir-key-nav-minimal` | 8-color minimal | 8 | Keynote minimal talks |
| 9 | `pitch-deck` | YC-style VC pitch | 10 | Fundraising / seed round |
| 10 | `product-launch` | Product launch | 8 | Product launch keynotes |
| 11 | `tech-sharing` | Tech sharing | 8 | Internal tech talks |
| 12 | `weekly-report` | Weekly report | 7 | Team status updates |
| 13 | `xhs-post` | 小红书 post | 9 | 小红书 / Instagram carousel (3:4) |
| 14 | `course-module` | Course module | 7 | Online courses / workshops |
| 15 | `presenter-mode-reveal` 🎤 | Presenter mode | 6 | Talks / lectures — needs speaker notes |

---

## ⚡ Features

### ⌨️ Keyboard Controls

| Key | Action |
|-----|--------|
| `←` `→` `Space` `PgUp` `PgDn` `Home` `End` | Navigate slides |
| `F` | Fullscreen |
| `S` | Presenter mode (magnetic-card popup: CURRENT / NEXT / SCRIPT / TIMER) |
| `P` | Export dialog — select slides → PDF(SVG) / PDF(PNG) / SVG(.zip) / PNG(.zip) |
| `N` | Bottom notes drawer |
| `R` | Reset timer (in presenter window) |
| `O` | Slide overview grid |
| `T` | Cycle themes (auto-syncs to presenter window) |
| `A` | Cycle demo animation on current slide |
| `Esc` | Close all overlays |
| `#/N` (URL) | Deep-link to slide N |
| `?preview=N` (URL) | Preview mode (single slide, no chrome) |

### 🎤 Presenter Mode (press `S`)

Press `S` on any deck to open a dedicated presenter window with 4 **draggable, resizable magnetic cards**: current slide, next slide preview, speaker script (逐字稿), and timer. Two windows sync via `BroadcastChannel`.

| | Description |
|------|------|
| Analogy | Like PowerPoint's "Presenter View" — audience sees fullscreen, speaker sees notes + next slide + timer |
| Core tech | `<aside class="notes">` with 150–300 words per slide. Press `S` for large-font teleprompter. Current/next slides use iframe pixel-level previews |
| Cards are draggable | 4 magnetic cards' positions and sizes saved to localStorage per deck |

| Card | Content |
|------|---------|
| 🔵 CURRENT | Current slide — pixel-perfect iframe preview |
| 🟣 NEXT | Next slide preview — prepare transitions |
| 🟠 SPEAKER SCRIPT | Teleprompter — large font, scrollable, 150-300 words/slide |
| 🟢 TIMER | Elapsed time + slide counter — `←` `→` navigate · `R` reset |

![Presenter mode with 4 magnetic cards](docs/readme/presenter.png)

**Why previews are pixel-perfect:** Each card is an `<iframe>` loading the same deck HTML with `?preview=N`. The runtime renders only slide N with no chrome — so previews use the **same CSS, theme, fonts, and viewport** as the audience view.

**Smooth navigation (zero flicker):** On slide change, the presenter window sends `postMessage({type:'preview-goto', idx:N})` to iframes. The iframe just toggles `.is-active` — **no reload, no flicker**.

**Speaker script rules (3 golden):**

1. **Prompt signals, not lines to read** — bold keywords, separate transitions
2. **150–300 words per slide** — ~2-3 min/page pace
3. **Write it like you speak** — "so" not "therefore", "this" not "the aforementioned"

See [`references/presenter-mode.md`](https://github.com/lewislulu/html-ppt-skill/blob/main/references/presenter-mode.md), or copy the ready-made `templates/full-decks/presenter-mode-reveal/` template.

### 📦 One-Click Export (press `P`)

| Format | Description |
|--------|-------------|
| PDF (SVG) | Browser print → vector PDF |
| PDF (PNG) | Canvas render → raster PDF |
| SVG (.zip) | Per-slide SVG in archive |
| PNG (.zip) | Per-slide PNG in archive |

---

## Design Philosophy

- **Token-driven design system.** All color, radius, shadow, font decisions live in `assets/base.css` + the current theme file.
- **Iframe isolation for previews.** Every showcase preview is a real, independent render.
- **Zero build.** Pure static HTML/CSS/JS. Webfonts via Google Fonts CDN.
- **Senior-designer defaults.** Opinionated type scale, spacing rhythm, gradients.
- **First-class Chinese + English support.** Noto Sans SC / Noto Serif SC pre-imported.

---

## License

- Inherits MIT from [lewislulu/html-ppt-skill](https://github.com/lewislulu/html-ppt-skill).
- MIT · Copyright (c) 2026 ostar999 &lt;ota1754@qq.com&gt;
