# Inline-HTML Errors — Lessons Learned

This document catalogs real-world bugs discovered when generating standalone
single-file HTML decks. These errors are specific to the inline-CSS/JS approach
required by the skill's authoring rules. **Read this before debugging any
keyboard or runtime functionality issue.**

---

## Pitfall 1: Theme cycling (T key) completely broken — functions & handlers missing

**Symptom:** Pressing T key has no effect. Theme does not change. Console may
show `ReferenceError: cycleTheme is not defined`.

**Root cause:** When inlining `runtime.js` into a `<script>` tag, the developer
may accidentally omit the theme cycling code. The original `runtime.js` contains
~80 lines of theme-related code (variables, `applyTheme()`, `cycleTheme()`,
keyboard binding, and BroadcastChannel sync) that are easy to miss during
manual inlining. Without this code, pressing T silently does nothing.

**The 5 required components (ALL must be present):**

1. **Theme variables** — inserted after `var total = slides.length;`:
   ```javascript
   var root=document.documentElement;
   var themesAttr=root.getAttribute('data-themes')||document.body.getAttribute('data-themes');
   var themes=themesAttr?themesAttr.split(',').map(function(s){return s.trim()}).filter(Boolean):[];
   var themeIdx=0;
   var themeBase=root.getAttribute('data-theme-base');
   if(!themeBase){
     var existingLink=document.getElementById('theme-link');
     if(existingLink){var rawHref=existingLink.getAttribute('href')||'';var lastSlash=rawHref.lastIndexOf('/');themeBase=lastSlash>=0?rawHref.substring(0,lastSlash+1):'assets/themes/';}
     else{themeBase='assets/themes/';}
   }
   ```

2. **`applyTheme()` function** — inserted before the keyboard handler:
   ```javascript
   function applyTheme(name){
     var link=document.getElementById('theme-link');
     if(!link){link=document.createElement('link');link.rel='stylesheet';link.id='theme-link';document.head.appendChild(link);}
     link.href=themeBase+name+'.css';
     root.setAttribute('data-theme',name);
   }
   ```

3. **`cycleTheme()` function** — inserted next to `applyTheme()`:
   ```javascript
   function cycleTheme(fromRemote){
     if(!themes.length)return;
     themeIdx=(themeIdx+1)%themes.length;
     var name=themes[themeIdx];
     applyTheme(name);
     if(!fromRemote&&bc)bc.postMessage({type:'theme',name:name});
   }
   ```

4. **T key handler** — inside `document.addEventListener('keydown', ...)`:
   ```javascript
   case 't':case 'T':cycleTheme();break;
   ```

5. **BroadcastChannel theme sync** — inside `bc.onmessage` handler:
   ```javascript
   else if(e.data.type==='theme'&&e.data.name){
     var i=themes.indexOf(e.data.name);if(i>=0)themeIdx=i;applyTheme(e.data.name);
   }
   ```

**Why it's especially bad:** Silent failure — no error visible to the user, no
console message (if function is missing, the `case` line silently falls through
because the switch doesn't match). The user presses T and nothing happens, with
no indication of what went wrong.

**How to verify before shipping (MANDATORY for EVERY deck):**
1. Open the deck in a browser.
2. Press **T** — theme must visibly change (colors, fonts, etc.).
3. Press **T** again — should cycle to the next theme.
4. Press **← →** — navigation must work.
5. Press **F** — fullscreen toggles.
6. Press **O** — overview grid opens.
7. Press **N** — notes drawer opens.
8. Press **S** — presenter window opens (check popup blocker).
9. Press **P** — export dialog opens with all thumbnails.

If ANY key is unresponsive, check the keyboard handler switch statement and
verify all functions are defined in scope.

---

## Pitfall 2: `<\/script>` inside JavaScript string breaks entire runtime

**Symptom:** No keyboard shortcuts work at all (← →, F, S, T, P, O, N — all
dead). Page loads but slides don't change. Console shows `SyntaxError: Invalid
or unexpected token` or similar.

**Root cause:** The `buildPresenterHTML()` function in the inlined `runtime.js`
generates a complete HTML document as a JavaScript string. This string contains
`<\/script>` which the HTML parser interprets as the closing `</script>` tag
of the **outer** `<script>` block. The parser does NOT parse JavaScript — it
scans for the literal byte sequence `</script>` and closes the script element
at that point, regardless of whether it's inside a string. Everything after
that point (including all keyboard handlers, theme functions, and the rest of
the runtime) is treated as plain text and never executed.

**Affected code location:**
```javascript
// ❌ BROKEN — HTML parser sees </script> and closes the script tag
'...})();<\/script></body></html>';
```

**Fix:** Break the string to prevent the literal `</script>` sequence:
```javascript
// ✅ FIXED — string concatenation prevents </script> from appearing
'...})();<'+'/script></body></html>';
```

**Why it's especially bad:** This is a **catastrophic** failure — the entire
runtime is dead. All keyboard shortcuts, all navigation, all export
functionality — everything silently fails. The page loads visually but is
completely non-functional.

**How to verify before shipping:**
1. Press ← or → immediately after opening the deck — if nothing happens, this
   is your culprit.
2. Open browser DevTools Console (F12) and look for syntax errors.
3. Search the file for `<\/script>` (grep pattern: `\\<\\\\/script>`) — any
   match inside the `<script>` tag is a bug waiting to happen.

**Checklist for any inline JS that generates HTML strings:**
- [ ] No literal `</script>` anywhere in JavaScript strings.
- [ ] Always use `</` + `script>` or `<` + `/script>` concatenation.
- [ ] Test ← → navigation BEFORE testing any other key — it's the canary.

---

## Pitfall 3: Inline `:root` CSS variables override external theme `<link>`

**Symptom:** T key cycles through theme names (you can verify in DevTools:
`document.documentElement.getAttribute('data-theme')` changes), but the
**visual appearance does not change**. Colors, fonts, and radii stay the same
regardless of which theme is selected.

**Root cause:** When building a standalone single-file deck, the author puts
ALL CSS into a single `<style>` tag. This includes the `:root{}` block with
CSS custom properties (colors, radii, shadows, fonts). However, the T key
works by changing the `href` of `<link id="theme-link">`, which loads an
external `.css` file. In the CSS cascade, the inline `<style>` tag comes
**AFTER** the `<link>` tag, so its `:root` variables have **higher priority**
and override the external theme's variables. The result: theme switching
appears to work but has zero visual effect.

**Example of the problem:**
```html
<head>
  <!-- 1. External theme — loaded first, LOWER priority -->
  <link id="theme-link" href="path/to/corporate-clean.css">
  <!-- 2. Inline style — loaded second, HIGHER priority, overrides theme -->
  <style>
    :root{
      --bg:#ffffff;        /* ← THIS overrides the theme's --bg */
      --accent:#0a2540;    /* ← THIS overrides the theme's --accent */
      /* ... all other color variables */
    }
  </style>
</head>
```

**Fix:** Split the `:root` variables into two groups:

1. **Color/visual variables** — REMOVE from inline `<style>`. Let them come
   exclusively from the external `<link id="theme-link">`. These include:
   `--bg`, `--bg-soft`, `--surface`, `--surface-2`, `--border`,
   `--border-strong`, `--text-1`, `--text-2`, `--text-3`, `--accent`,
   `--accent-2`, `--accent-3`, `--good`, `--warn`, `--bad`, `--grad`,
   `--grad-soft`, `--radius`, `--radius-sm`, `--radius-lg`, `--shadow`,
   `--shadow-lg`, `--font-sans`, `--font-display`.

2. **Structural/non-visual variables** — KEEP in inline `<style>`. These are
   NOT defined in theme files and would be missing otherwise:
   `--font-serif`, `--font-mono`, `--letter-tight`, `--letter-normal`,
   `--ease` (from base.css), `--anim-dur`, `--anim-ease` (from animations.css).

**Correct structure:**
```html
<head>
  <!-- External theme provides ALL color/visual variables -->
  <link rel="stylesheet" id="theme-link" href="path/to/themes/corporate-clean.css">
  <style>
    /* Only structural variables that themes don't define */
    :root{
      --font-serif:'Playfair Display','Noto Serif SC',Georgia,serif;
      --font-mono:'JetBrains Mono','SF Mono',monospace;
      --letter-tight:-.03em;--letter-normal:-.01em;
      --ease:cubic-bezier(.4,0,.2,1);
      --anim-dur:.7s;--anim-ease:cubic-bezier(.4,0,.2,1);
    }
    /* ... rest of base.css, animations.css, custom styles ... */
  </style>
</head>
```

**Important:** Also add `<html data-themes="..." data-theme-base="..." data-theme="...">` with:
- `data-themes`: comma-separated list of theme names to cycle through
- `data-theme-base`: relative path to the themes directory (calculated from
  the deck's location to `<skill-root>/assets/themes/`)
- `data-theme`: current active theme name

**How to verify before shipping:**
1. Press T → colors/fonts must VISIBLY change.
2. Press T again → another visible change.
3. Press T 8+ times → cycles back to original theme.
4. If `data-theme` attribute changes but visuals don't, Pitfall 3 is active.

---

## Pitfall 4: Runtime JavaScript completely missing (incomplete inlining)

**Symptom:** Entire keyboard functionality dead. No navigation, no export,
no presenter mode. Slides are all visible at once or none are.

**Root cause:** When inlining `assets/runtime.js`, the developer copies the
file content into a `<script>` tag but accidentally truncates or omits large
portions. `runtime.js` is ~1344 lines — it's the largest single source file
to inline. The most commonly missed sections are:
- Theme cycling code (~80 lines, end of file)
- Presenter mode HTML generator (~200 lines)
- Export dialog & PNG/SVG generation (~300 lines)
- Keyboard handler (must be the last major block)

**Fix:** Use a systematic verification approach:
1. Count `function` declarations in the inlined `<script>` — should match
   the original `runtime.js`.
2. Verify each keyboard shortcut works (see checklist below).
3. Check that ALL `case` branches in the keyboard switch statement have
   corresponding functions defined in scope.

**How to verify:** Run the full keyboard checklist (see Pitfall 1).

---

## Mandatory pre-ship checklist for every inline-HTML deck

**Run these checks in order BEFORE telling the user the deck is done:**

### Keyboard functionality (every key must work)

| Key | Expected behavior | Check |
|-----|-------------------|-------|
| `←` `→` | Navigate between slides | ☐ |
| `Space` | Next slide | ☐ |
| `F` | Toggle fullscreen | ☐ |
| `T` | Cycle theme (visual change!) | ☐ |
| `O` | Open overview grid | ☐ |
| `N` | Open notes drawer | ☐ |
| `S` | Open presenter window | ☐ |
| `P` | Open export dialog | ☐ |
| `Esc` | Close all overlays | ☐ |

### Export functionality (see export-pitfalls.md for details)

| Button | Expected behavior | Check |
|--------|-------------------|-------|
| 全选 | Select all thumbnails | ☐ |
| 取消全选 | Deselect all | ☐ |
| 导出 SVG (.zip) | Download starts | ☐ |
| 导出 PNG (.zip) | Download starts | ☐ |
| 导出 PDF (SVG) | Print dialog opens | ☐ |
| 导出 PDF (PNG) | Print dialog opens | ☐ |

### Structure integrity

- [ ] `grep -c '<section class="slide'` matches expected slide count
- [ ] No `<\/script>` literal inside `<script>` tag
- [ ] `<html>` has `data-themes`, `data-theme-base`, `data-theme` attributes
- [ ] `<head>` has `<link id="theme-link" href="...">` before `<style>`
- [ ] Inline `:root` only has structural variables (no colors/radii/shadows)
- [ ] All 5 theme-components present (variables, applyTheme, cycleTheme, key handler, bc sync)

### Quick console check

Open DevTools Console (F12) after loading the page:
- [ ] No red errors on page load
- [ ] `$('.slide.is-active')` returns exactly one element
- [ ] `$('.slide').length` matches expected slide count

---

## Summary: When generating a deck, always check

| # | Check | Where |
|---|-------|-------|
| 1 | T key works (all 5 components present) | Inline `<script>` |
| 2 | No `<\/script>` in JS strings | Inline `<script>` presenter HTML |
| 3 | `:root` color vars NOT in inline `<style>` | Inline `<style>` |
| 4 | `<html>` has theme attributes | `<html>` tag |
| 5 | `<link id="theme-link">` exists before `<style>` | `<head>` |
| 6 | All keyboard shortcuts functional | Full deck test |
| 7 | Export buttons all respond | P-key dialog test |
