---
name: ostar-all-in-html-ppt
description: ostar-all-in-html-ppt — 全能型 HTML PPT 工作室，内置 PDF/SVG 导出。创作专业的静态 HTML 演示文稿，支持 P 键导出为 PDF（浏览器打印）和 SVG（按页打包为 ZIP）。当用户需要演示文稿、PPT、幻灯片、Keynote、Deck、Slideshow、「幻灯片」「演讲稿」「做一份 PPT」「做一份 slides」、reveal 风格 HTML Deck、小红书图文，或任何需要美观、支持键盘导航的多页演讲/报告/分享文档时使用。触发词包括 "presentation"、"ppt"、"slides"、"deck"、"keynote"、"reveal"、"slideshow"、"幻灯片"、"演讲稿"、"分享稿"、"小红书图文"、"talk slides"、"pitch deck"、"tech sharing"、"technical presentation"。
---

# ostar-all-in-html-ppt — 全能型 HTML PPT 工作室

创作专业的 HTML 演示文稿，以静态文件形式交付。一个主题文件 = 一种外观。一个布局文件 = 一种页面类型。一个动画类 = 一种入场效果。所有页面共享 `assets/base.css` 中基于 Token 的设计系统。

## 安装

```bash
npx skills add https://github.com/ostar999/ostar-all-in-html-ppt.git
```

一条命令，无需构建。纯静态 HTML/CSS/JS，仅依赖 CDN 网页字体。

## 本技能提供的内容

- **36 个主题** (`assets/themes/*.css`) — minimal-white、editorial-serif、soft-pastel、sharp-mono、arctic-cool、sunset-warm、catppuccin-latte/mocha、dracula、tokyo-night、nord、solarized-light、gruvbox-dark、rose-pine、neo-brutalism、glassmorphism、bauhaus、swiss-grid、terminal-green、xiaohongshu-white、rainbow-gradient、aurora、blueprint、memphis-pop、cyberpunk-neon、y2k-chrome、retro-tv、japanese-minimal、vaporwave、midcentury、corporate-clean、academic-paper、news-broadcast、pitch-deck-vc、magazine-bold、engineering-whiteprint
- **15 个完整 Deck 模板** (`templates/full-decks/<name>/`) — 完整的多页 deck，带作用域 CSS 类 `.tpl-<name>`。其中 8 个从真实 deck 中提取（xhs-white-editorial、graphify-dark-graph、knowledge-arch-blueprint、hermes-cyber-terminal、obsidian-claude-gradient、testing-safety-alert、xhs-pastel-card、dir-key-nav-minimal），7 个场景脚手架（pitch-deck、product-launch、tech-sharing、weekly-report、xhs-post 3:4、course-module、**presenter-mode-reveal** — 演讲者模式专用）
- **31 个布局** (`templates/single-page/*.html`)，带真实演示数据
- **27 个 CSS 动画** (`assets/animations/animations.css`)，通过 `data-anim` 使用
- **20 个 Canvas 特效动画** (`assets/animations/fx/*.js`)，通过 `data-fx` 使用 — particle-burst、confetti-cannon、firework、starfield、matrix-rain、knowledge-graph（力导向图）、neural-net（脉冲）、constellation、orbit-ring、galaxy-swirl、word-cascade、letter-explode、chain-react、magnetic-field、data-stream、gradient-blob、sparkle-trail、shockwave、typewriter-multi、counter-explosion
- **键盘运行时** (`assets/runtime.js`) — 方向键、T（切换主题）、A（动画）、F/O、**S（演讲者模式：磁性卡片弹窗，包含 CURRENT / NEXT / SCRIPT / TIMER 四个卡片）**、**P（导出：缩略图选择器 → 通过浏览器打印导出 PDF (SVG) / 通过 PNG 合并导出 PDF / SVG ZIP / PNG ZIP 下载）**、N（备注抽屉）、R（演讲者窗口中重置计时器）
- **特效运行时** (`assets/animations/fx-runtime.js`) — 自动在幻灯片进入时初始化 `[data-fx]`，离开时清理
- **展示 Deck**：主题 / 布局 / 动画 / 完整 Deck 图库
- **无头 Chrome 渲染脚本**：用于 PNG 导出

## 何时使用

当用户需要任何类型的基于幻灯片的输出，或希望将文本/笔记转化为可演示的 Deck 时使用。优先使用本技能，而非从零开始构建。

### 🎤 演讲者模式（Presenter Mode + 逐字稿）

如果用户提及以下任何内容：**演讲 / 分享 / 讲稿 / 逐字稿 / speaker notes / presenter view / 演讲者视图 / 提词器**，或说「我要去给团队讲 xxx」「要做一场技术分享」「怕讲不流畅」「想要一份带逐字稿的 PPT」— **请使用 `presenter-mode-reveal` 完整 Deck 模板**，并在每页的 `<aside class="notes">` 中编写 150–300 字的逐字稿。

详见 [references/presenter-mode.md](references/presenter-mode.md) 获取完整的编写指南，包括讲稿编写的 3 条规则：
1. **不是讲稿，是提示信号** — 加粗核心词 + 过渡句独立成段
2. **每页 150–300 字** — 2–3 分钟/页的节奏
3. **用口语，不用书面语** —「因此」→「所以」，「该方案」→「这个方案」

所有完整 Deck 模板都支持 S 键演讲者模式（已内置在 `runtime.js` 中）。**按 S 键打开一个新弹窗，包含 4 个磁性卡片**：
- 🔵 **CURRENT** — 当前幻灯片的像素级 iframe 预览
- 🟣 **NEXT** — 下一张幻灯片的像素级 iframe 预览
- 🟠 **SPEAKER SCRIPT** — 大字逐字稿（可滚动）
- 🟢 **TIMER** — 已用时间 + 幻灯片计数器 + 上一页/下一页/重置按钮

每个卡片可以**通过标题栏拖动**，并**通过右下角手柄调整大小**。卡片位置/大小按 deck 持久化到 `localStorage`。「重置布局」按钮可恢复默认排列。

**为什么预览是像素级的**：每个预览是一个 `<iframe>`，通过 `?preview=N` 查询参数加载实际的 deck HTML；`runtime.js` 检测到此参数后仅渲染第 N 张幻灯片，不带任何 chrome。因此预览使用**与观众视图相同的 CSS、主题、字体和视口**——颜色和布局保证完全一致。

**平滑导航**：切换幻灯片时，演讲者窗口向每个 iframe 发送 `postMessage({type:'preview-goto', idx:N})`。iframe 只需在幻灯片之间切换 `.is-active`——**无需重新加载，无闪烁**。两个窗口还通过 `BroadcastChannel` 保持同步。

只有 `presenter-mode-reveal` 是从根本上围绕此功能设计的，每张幻灯片都有合适的示例逐字稿。

演讲者窗口键盘操作：`← →` 导航（同步观众窗口）· `R` 重置计时器 · `Esc` 关闭弹窗。
观众窗口键盘操作：`S` 打开演讲者 · `P` 导出 PDF(SVG)/PDF(PNG)/SVG/PNG · `T` 循环切换主题 · `← →` 导航（同步演讲者窗口）· `F` 全屏 · `O` 总览。

## 开始创作之前 — 务必先询问或推荐

**在理解以下三件事之前，不要开始写幻灯片。** 要么直接询问用户，要么——如果用户已经提供了丰富的内容——提出一个有品味的默认方案并确认。

1. **内容和受众。** 这个 Deck 关于什么，多少页，谁会看（工程师 / 高管 / 小红书读者 / 学生 / VC）？
2. **风格 / 主题。** 36 个主题中哪个合适？如果不确定，根据调性推荐 2–3 个候选：
   - 商业 / 投资人路演 → `pitch-deck-vc`、`corporate-clean`、`swiss-grid`
   - 技术分享 / 工程 → `tokyo-night`、`dracula`、`catppuccin-mocha`、`terminal-green`、`blueprint`
   - 小红书图文 → `xiaohongshu-white`、`soft-pastel`、`rainbow-gradient`、`magazine-bold`
   - 学术 / 报告 → `academic-paper`、`editorial-serif`、`minimal-white`
   - 前卫 / 赛博 / 产品发布 → `cyberpunk-neon`、`vaporwave`、`y2k-chrome`、`neo-brutalism`
3. **起点。** 用 15 个完整 Deck 模板之一，还是从零开始？指出最接近的 `templates/full-decks/<name>/`，并询问是否合适。如果用户的内容暗示了明显的选择（例如「我要做产品发布会」→ `product-launch`），可以自信地提出建议，而不是盲目询问。

一个好的开场白应该是这样的：

> 我可以给你做这份 PPT！先确认三件事：
> 1. 大致内容 / 页数 / 观众是谁？
> 2. 风格偏好？我建议从这 3 个主题里选一个：`tokyo-night`（技术分享默认好看）、`xiaohongshu-white`（小红书风）、`corporate-clean`（正式汇报）。
> 3. 要不要用我现成的 `tech-sharing` 全 deck 模板打底？

只有这些问题都搞清楚了，才开始搭建 Deck 并编写内容。

## 快速开始

1. **搭建新 Deck。** 从仓库根目录执行：
   ```bash
   ./scripts/new-deck.sh my-talk
   open examples/my-talk/index.html
   ```
2. **选择主题。** 打开 Deck 按 `T` 键循环切换。或者硬编码：
   ```html
   <link rel="stylesheet" id="theme-link" href="../assets/themes/aurora.css">
   ```
   主题目录见 [references/themes.md](references/themes.md)。
3. **选择布局。** 从 `templates/single-page/` 中复制 `<section class="slide">...</section>` 代码块到你的 Deck 中。替换演示数据。
   布局目录见 [references/layouts.md](references/layouts.md)。
4. **添加动画。** 在任何元素上添加 `data-anim="fade-up"`（或 `class="anim-fade-up"`）。在 `<ul>`/网格上，使用 `anim-stagger-list` 实现逐项渐显。
   对于 Canvas 特效，使用 `<div data-fx="knowledge-graph">...</div>` 并引入 `<script src="../assets/animations/fx-runtime.js"></script>`。
   动画目录见 [references/animations.md](references/animations.md)。
5. **使用完整 Deck 模板。** 将 `templates/full-decks/<name>/` 复制到 `examples/my-talk/` 作为起点。每个文件夹是自包含的，带作用域 CSS。
   目录见 [references/full-decks.md](references/full-decks.md)，图库在 `templates/full-decks-index.html`。
6. **渲染为 PNG。**
   ```bash
   ./scripts/render.sh templates/theme-showcase.html       # 单页
   ./scripts/render.sh examples/my-talk/index.html 12      # 12 页
   ```

## 🛑 强制自检（每次操作必执行）

**本节适用于每次操作：** 生成新 Deck、修改现有 Deck、切换主题/模板、添加动画，或对 `.html` 文件做任何更改。

在告知用户工作完成之前，你必须执行以下 3 步自检：

### 第 0 步：内联审查

- [ ] 所有 CSS 规则是否都在 `<style>` 标签中（而非外部 `<link>`）？
- [ ] 所有 JavaScript 是否都在 `<script>` 标签中（而非外部 `<script src>`）？
- [ ] 主题 Token 是否以 `:root[data-theme="xxx"]` 块的形式存在？
- [ ] `.overview` / `.notes-overlay` / `.export-overlay` CSS 块是否存在？
- [ ] `applyTheme()` 是否已修改为内联模式（不创建 `<link>` 元素）？

如果任何资源仍然使用外部引用，重新读取源文件并立即将其内联。不要交付带有外部依赖的 Deck。

### 第 1 步：键盘功能

| 按键 | 预期行为 | 检查 |
|-----|----------|-------|
| `←` `→` `Space` | 导航幻灯片 | ☐ |
| `F` | 切换全屏 | ☐ |
| `T` | 主题**可见地**切换 — 检查 `data-themes` 中的所有主题 | ☐ |
| `O` | 打开总览网格，显示缩略图 | ☐ |
| `N` | 打开备注抽屉 | ☐ |
| `S` | 打开演讲者窗口（检查 4 个磁性卡片） | ☐ |
| `P` | 打开导出对话框，显示缩略图 | ☐ |
| `A` | 在当前幻灯片上循环演示动画 | ☐ |
| `Esc` | 关闭所有浮层 | ☐ |

如果**任何**按键无响应，请停下来修复后再继续。
参见 [references/inline-errors.md](references/inline-errors.md)。

**🛑 生成的 HTML 中禁止出现紧凑键盘快捷键提醒栏。** Deck HTML 中不得包含
"S 演讲者视图·T切换主题·←→翻页·F全屏·O总览·P导出" 这类紧凑快捷键提醒栏。
这种提醒栏暗示按键仅被记录而非实际测试过。每个按键必须逐一验证 —
通过清单驱动的方式，而非在 UI 栏中汇总。如果 HTML 中存在此类提醒栏，
在声明 Deck 完成之前立即移除。

### 第 2 步：导出验证

1. 按 P → 导出对话框打开，所有缩略图样式正确。
2. 点击**每个**导出按钮 — 每个都必须有响应：
   - "全选" / "取消全选" — 缩略图切换选中
   - "导出 SVG (.zip)" — 开始下载
   - "导出 PNG (.zip)" — 开始下载
   - "导出 PDF (SVG)" — 打开打印对话框
   - "导出 PDF (PNG)" — 打开打印对话框
3. 打开至少一个下载的文件 → 验证：文字/图片可见，布局正确，颜色与浏览器匹配。

如果任何按钮无反应，参见 [references/export-pitfalls.md](references/export-pitfalls.md)（先检查 Pitfall 12）。

### 第 3 步：跨环境检查

- [ ] Deck 能通过 `file://` 协议正确打开（控制台无 CORS 错误）
- [ ] 浏览器控制台无 404 错误
- [ ] 幻灯片数量与预期一致
- [ ] 无 UI 元素重叠（页脚 vs 幻灯片内容）

**🛑 在上述所有复选框确认通过之前，不要告知用户工作已完成。** 预览中不可见的 bug（键盘/导出故障）是用户反馈问题的第一大来源。

## 创作规则（重要）

- **🛑 默认：单个自包含 HTML 文件。** 每个生成的 Deck 必须是一个单一的 `.html` 文件，包含所有 CSS 和 JS 内联 — 不使用外部 `<link>` 样式表，不使用 `<script src="...">` 引用 `assets/`。唯一的例外是 Google Fonts CDN 的 `@import`（字体是二进制文件，无法内联）。这意味着：
  - 读取以下源文件并内联到 HTML 中（路径相对于 skill 根目录）：
    - `assets/base.css` → 完整内容放入 `<style>`（布局原语、chrome、总览、备注、导出对话框、打印）
    - `assets/animations/animations.css` → 所有 `@keyframes` + 动画类放入同一个 `<style>`
    - `assets/themes/<name>.css` → `:root` Token 值放入同一个 `<style>`（或从模板的作用域样式中复制 Token）
    - `templates/full-decks/<name>/style.css` → 自定义组件样式放入同一个 `<style>`（去除 `.tpl-xxx` 作用域）
    - `assets/runtime.js` → 完整内容放入 `</body>` 前的 `<script>` 中
  - **不要**在 HTML 旁边创建 `style.css`、`theme.css` 或任何外部资源文件
  - **不要**创建指向 `assets/` 的符号链接
  - 除非用户明确说「用外联相对路径」或类似表述，否则不使用外部 `<link>` / `<script src>`
  - 当用户切换主题、模板或添加动画时，重新读取源文件并更新内联 `<style>` 和 `<script>` 内容 — 切勿混用内联和外部

- **🛑 分步生成 — 绝不要一次性写出完整 HTML。**
  一个包含内联 CSS + JS 的完整 Deck 文件很大。在单次 API 调用中写入会导致卡顿和输出截断。始终使用以下 3 步工作流：
  **第 1 步 — 写骨架。** 创建 HTML 文件，包含 `<style>`（所有 CSS 从源文件内联）+ `<script>`（runtime.js 内联）+ `<div class="deck"></div>` — 但不包含幻灯片。
  **第 2 步 — 插入幻灯片。** 使用 Edit 在 `<div class="deck">` 和 `</div>` 之间插入 `<section class="slide">...</section>` 代码块。如果 Deck 有很多页，分批插入而不是一次性全部插入。
  **第 3 步 — 验证。** 回读文件，检查幻灯片数量是否与预期一致。**🛑 强制要求：** 在声明 Deck「完成」之前测试键盘功能和导出。这不是可选的 — 每个 Deck 都必须通过以下检查：

  **3a. 键盘检查（每个键 — 首先执行）：**

  | 按键 | 预期行为 | 检查 |
  |-----|----------|-------|
  | `←` `→` | 导航幻灯片 | ☐ |
  | `Space` | 下一张幻灯片 | ☐ |
  | `F` | 切换全屏 | ☐ |
  | `T` | 主题**可见地**切换 | ☐ |
  | `O` | 打开总览网格 | ☐ |
  | `N` | 打开备注抽屉 | ☐ |
  | `S` | 打开演讲者窗口 | ☐ |
  | `P` | 打开导出对话框 | ☐ |
  | `Esc` | 关闭所有浮层 | ☐ |

  **🛑 T 键特别关注：** 这是内联 Deck 中最常见的静默故障键。如果按 T 键无反应：(a) 检查 `<html>` 上是否存在 `data-themes` 属性，(b) 检查内联的 `applyTheme()` 是否不创建 `<link>` 元素，(c) 检查 CSS 是否有 `:root[data-theme="xxx"]` 块。参见 inline-errors.md 中的 Pitfall 5。

  如果**任何**按键无响应，停下来修复后再测试导出。
  参见 [references/inline-errors.md](references/inline-errors.md) 获取常见键盘/运行时 bug 的完整目录及已验证的修复方法。

  **🛑 生成的 HTML 中禁止出现紧凑键盘快捷键提醒栏。** 绝不向 Deck HTML 中添加紧凑
  键盘快捷键提醒栏（如 "S演讲者·T主题·←→翻页·F全屏·O总览·P导出"）。
  这种提醒栏是用 UI 摘要替代了真正的逐键验证 — 每个按键必须逐个测试，
  而非在 UI 汇总中打广告。如果生成的 HTML 中存在此类提醒栏，立即移除。

  **3b. 导出检查（每个按钮 — 全部点击）：**

  1. 按 P → 导出对话框打开，所有幻灯片缩略图样式正确。
  2. **点击每个导出按钮** — "全选"、"取消全选"、"导出 SVG (.zip)"、"导出 PNG (.zip)"、"导出 PDF (SVG)"、"导出 PDF (PNG)" — 每个都必须有响应（开始下载、打开打印对话框或 UI 更新）。如果任何按钮无反应，参见 [references/export-pitfalls.md](references/export-pitfalls.md) 中的 Pitfall 12。
  3. 至少导出一张幻灯片为 PNG 和 PDF (SVG)，然后打开下载的文件验证：(a) 所有文字/卡片/图片可见 — 无缺失，(b) 布局与浏览器渲染一致，(c) 颜色和字体正确。

  如果有任何问题，查阅 [references/inline-errors.md](references/inline-errors.md) 解决键盘/运行时 bug，或 [references/export-pitfalls.md](references/export-pitfalls.md) 解决导出 bug。在告知用户 Deck 完成之前修复。不要跳过此步骤 — 键盘和导出 bug 在浏览器预览中不可见，只有在用户实际使用 Deck 时才会暴露。

- **始终从模板开始。** 不要从零开始创作幻灯片——先从 `templates/single-page/` 中复制最接近的布局，然后替换内容。
- **🛑 从 Markdown 或其他格式转换时必须保留图片。** 如果用户提供了 Markdown 文件（`.md`）
  或其他包含图片的文档格式（`![](path)`、`<img>` 等），生成的 HTML Deck 默认必须包含
  这些图片。不得丢弃、跳过或用占位符替换图片。从源文件中读取图片引用，并使用带有正确
  `src` 路径的 `<img>` 标签将其嵌入到对应的幻灯片中（酌情使用绝对或相对路径）。
  如果图片路径失效，告知用户 — 不要静默忽略。
- **使用 Token，而非字面颜色。** 每个颜色、圆角、阴影都应来自 `assets/base.css` 中定义的 CSS 变量，并由主题覆盖。
  好的写法：`color: var(--text-1)`。不好的写法：`color: #111`。
- **不要发明新的布局文件。** 优先组合现有的布局。只有在 31 个布局都不合适时才添加新的 `templates/single-page/*.html`。
- **尊重 Chrome 插槽。** `.deck-header`、`.slide-number` 和进度条由 `assets/base.css` + `runtime.js` 提供。
- **默认不要添加 `.deck-footer`。** `.deck-footer` 使用 `position:absolute`，可能与幻灯片内容重叠（尤其是居中布局如 Cover）。只有在用户明确要求页脚中显示标签或页码时才添加。如果添加了，你必须目视验证它在每一页上不与任何幻灯片内容重叠 — 在不同视口尺寸下测试。
- **键盘优先。** 始终在 `</body>` 前的 `<script>` 标签中内联 `assets/runtime.js` 内容，使 Deck 支持 ← → / T / A / F / P / S / O / hash 深度链接。不要使用 `<script src="...">` — 将 runtime.js 源代码复制到 HTML 文件中。
- **🛑 内联主题支持（T 键）。** 由于默认是单个自包含 HTML 文件，没有外部资源，内联的 `applyTheme()` 函数必须修改为在没有外部 CSS 文件的情况下工作。原始的 `runtime.js` 中的 `applyTheme()` 会创建指向外部 `.css` 文件的 `<link>` 元素 — 这在内联 Deck 中会导致 404 错误。

  **必需的 `applyTheme()` 修改（内联 runtime.js 时应用）：**
  ```js
  // ❌ 原始 — 创建指向不存在的外部文件的 <link>
  function applyTheme(name){
    let link=document.getElementById('theme-link');
    if(!link){link=document.createElement('link');...}  // ← 删除此部分
    link.href=themeBase+name+'.css';                      // ← 内联模式下 404
    root.setAttribute('data-theme',name);
  }

  // ✅ 修复后 — 适用于内联 Deck（同时向后兼容外部模式）
  function applyTheme(name){
    let link=document.getElementById('theme-link');
    if(link){link.href=themeBase+name+'.css';}  // 仅在存在时更新
    root.setAttribute('data-theme',name);         // 始终设置（内联 CSS 响应此属性）
  }
  ```

  **内联主题所需的 CSS 模式：**
  ```css
  /* 每个主题一个 :root[data-theme="xxx"] 块 — 所有 Token 变量必须存在 */
  :root, :root[data-theme="academic-paper"]{ --bg:#fdfcf8; --accent:#1a3a7a; /* ...所有 token */ }
  :root[data-theme="minimal-white"]{ --bg:#ffffff; --accent:#2563eb; /* ...所有 token */ }
  :root[data-theme="corporate-clean"]{ --bg:#f8fafc; --accent:#1e40af; /* ...所有 token */ }
  ```

  **必需的 HTML 属性：**
  ```html
  <html data-themes="academic-paper,minimal-white,corporate-clean" data-theme="academic-paper">
  ```

  **🛑 每次生成 Deck 后，按 T 键验证：** 每次按下时颜色/字体必须可见地变化。如果没有，参见 [references/inline-errors.md](references/inline-errors.md) Pitfall 5。
  同时测试所有其他按键（← → Space F O N S P Esc）— 参见下面的键盘检查清单。

- **每逻辑页一个 `.slide`。** `runtime.js` 使 `.slide.is-active` 可见；所有其他都被隐藏。
- **提供备注。** 在每张幻灯片内将演讲者备注包裹在 `<div class="notes">…</div>` 中。按 S 键打开浮层查看。
- **绝不要把仅供演讲者阅读的文字放在幻灯片本身上。** 描述性文字如「这一页展示了……」或「Speaker: 这里可以补充……」或面向演讲者的小号说明文字必须放在 `<div class="notes">` 中，而不是作为可见的 `<p>` / `<span>` 元素出现在幻灯片上。`.notes` 类默认是 `display:none` — 它只在 S 浮层中出现。幻灯片应只包含面向观众的内容（标题、要点、数据、图表、图片）。

### SVG/PNG 导出规则（关键 — 在编写自定义样式前阅读）

**🛑 强制要求：生成任何 Deck 后，你必须验证导出功能正常。** 在浏览器中打开 Deck，按 `P`，并确认：
1. 导出对话框打开，所有幻灯片缩略图可见且样式正确。
2. **点击每个导出按钮**（全选、取消全选、导出 SVG、导出 PNG、导出 PDF SVG、导出 PDF PNG）。每个按钮必须响应 — 静默/无响应的按钮意味着 `buildExportDialog()` 存在孤立 `querySelector`（参见 Pitfall 12）。
3. 点击「导出 PDF (SVG)」或「导出 SVG (.zip)」— 开始下载。
4. 打开下载的文件 — 布局、颜色、字体和终端块与浏览器渲染一致。

**📖 参见 [references/export-pitfalls.md](references/export-pitfalls.md) 获取已知导出 bug、根因和已验证修复方法的目录。** 这是从真实 Deck 调试中积累的经验 — 在排查任何 P 键导出问题之前请先阅读。**当导出按钮无响应时，始终先检查 Pitfall 12。**

如果上述任何一项失败，在告知用户 Deck 完成之前运行以下清单。
同样的清单适用于用户切换主题、模板或添加动画时 — 内联 `<style>` 必须与每次更改保持同步。

**导出修复清单（按顺序执行直到修复）：**

1. ☐ 所有关键 CSS 规则是否都在内联 `<style>` 标签中（而非外部 `<link>`）？
2. ☐ `.slide` 是否有显式的 `background:var(--bg);color:var(--text-1)`？
3. ☐ 作用域类（`.tpl-xxx`）是否已移除，或也添加到每个 `<section class="slide">` 上？
4. ☐ `.overview` / `.notes-overlay` / `.export-overlay` CSS 块是否存在？
5. ☐ `.export-bar::after` 是否包含耐心等待提示？
6. ☐ `.export-thumb.selected` 是否使用 `border-color: var(--accent-2)`（而非 `--accent`）？
7. ☐ 如果用户切换了主题：`:root` Token 值是否已更新为新主题？
8. ☐ 如果用户添加了动画：`@keyframes` 定义是否存在于内联 `<style>` 中？
9. ☐ **网格布局幻灯片**：打印 CSS 是否使用 `display:grid`（而非 `display:flex`）？`.slide.full` 是否有自己的 `display:flex` 规则？
10. ☐ **打印 CSS 中不在网格 `.slide` 上使用 `::before`** — 它会变成占据第一列的多余网格项。
11. ☐ **`._pdf_show_` 不得设置 `display`** — 它会覆盖 grid/flex 规则。仅用于 visibility/opacity/position。
12. ☐ **在 `slideToSVG()` 和 `slideToPNGBlob()` 中剥离 `backdrop-filter`** — SVG foreignObject 不支持它，会导致块状伪影。
13. ☐ **`@media print` 将 `.gradient-text` 重置**为纯色 `var(--accent)` — `background-clip:text` 在打印中失效，留下可见的渐变矩形和硬边缘。
14. ☐ **SVG PDF 导出文件名** — `exportToPDFSVG()` 必须在 `window.print()` 之前将 `document.title` 设置为包含 `_svg` 后缀。
15. ☐ **卡片/标注背景在无 `backdrop-filter` 时可见** — 半透明背景（`.06` 透明度）在模糊剥离后变得不可见。使用 `.72+` 透明度作为回退（参见 Pitfall 9）。
16. ☐ **导出克隆中移除 `anim-stagger-list`** — `slideToSVG()` 和 `slideToPNGBlob()` 必须移除 `anim-stagger-list` 类，否则子元素保持 `opacity:0`（参见 Pitfall 10）。
17. ☐ **装饰性背景在 `.slide` 上重复** — 仅应用于 body 的径向渐变/光斑不会出现在 SVG 导出中。将它们也复制到 `.slide` 本身上（参见 Pitfall 11）。

---

- **所有关键 CSS 必须内联在 `<style>` 标签中。** 不要将主题变量或基础布局样式放在外部 `<link>` 样式表中，并依赖运行时的 `fetch()` 在导出时加载它们。`fetch()` 在 `file://` 协议下静默失败（浏览器 CORS 策略），而没有这些样式渲染的 SVG 将显示异常。参见 `examples/export-reference/index.html` 获取规范模式：一个大 `<style>` 块包含 `:root` 主题 Token + 所有布局原语 + 自定义组件样式 + 导出对话框 CSS。

- **作用域类（`.tpl-<name>`）必须存在于 SVG DOM 中。** `slideToSVG()` 函数仅克隆 `<section class="slide">` 元素 — 它不包括外部的 `<body>` 或 `<div class="deck">`。如果你在 `<body>` 上使用了像 `.tpl-suno-tutorial` 这样的作用域类，并编写了像 `.tpl-suno-tutorial .code-block` 这样的选择器，这些选择器在 SVG 中将失败，因为 `.tpl-suno-tutorial` 祖先元素不存在。**修复方法**：将作用域类也添加到每个 `<section class="slide tpl-suno-tutorial">` 上，并更新以 `.slide` 为后代的选择器（例如，`.tpl-suno-tutorial .slide::before` 也必须改为 `.slide.tpl-suno-tutorial::before`）。

- **导出缩略图选中边框必须使用 `var(--accent-2)`，而非 `var(--accent)`。** 许多主题将 `--accent` 设置为白色（`#fff`），这与复选框背景（`.export-thumb.selected .export-check`）使用的颜色相同。当边框和复选框都是白色时，用户无法分辨哪些缩略图被选中。对 `.export-thumb.selected { border-color }` 使用 `var(--accent-2)` — 它始终与白色复选框明显不同。
  ```css
  /* ✅ 正确 */
  .export-thumb.selected { border-color: var(--accent-2); }
  /* ❌ 错误 — 在白色 accent 主题（如 blueprint）上不可见 */
  .export-thumb.selected { border-color: var(--accent); }
  ```

- **`.slide` 类必须显式设置 `background` 和 `color`。** 在浏览器中，这些属性通过 `base.css` 从 `<html>`/`<body>` 继承。在 SVG `foreignObject` 中，没有 `<html>` 或 `<body>`，因此幻灯片回退到透明/黑色。始终在内联 CSS 中为 `.slide` 添加 `background:var(--bg);color:var(--text-1)`。

- **SVG `foreignObject` 仅限浏览器。** 非浏览器工具（ImageMagick、Mac 预览、sharp 等）不渲染 `foreignObject` 内容 — 它们会产生空白图像。对于 PNG 输出，使用内置的 P 键 PNG 导出（SVG → data URL → canvas → PNG）或 `scripts/render.sh`（无头 Chrome）。内置 PNG 导出使用 `data:` URL（而非 `blob:` URL）以避免来自跨源字体引用的 canvas 污染。

- **对具有不同 HTML 属性的元素，`replace_all` 很脆弱。** 在进行批量替换（如为所有幻灯片添加类）时，带有额外属性的元素（例如 `<section class="slide center">`）不会匹配 `<section class="slide"`。在 `replace_all` 之后始终用 grep 搜索遗漏项。

- **导出 `updateCount()` 必须使用缓存的幻灯片总数，绝不要使用 `document.querySelectorAll('.slide').length`。** 导出对话框的 `renderThumbs()` 将每张幻灯片克隆到缩略图中 — 每个克隆都保留 `.slide` 类。克隆后，`querySelectorAll('.slide')` 返回的是真实数量的两倍（或更多）。在 `renderThumbs()` 内，于克隆循环之前将真实幻灯片数量缓存在模块级变量（`_slideTotal`）中，并在 `updateCount()` 中使用该缓存值。
  ```javascript
  // ✅ 正确
  var _slideTotal = 0;
  function renderThumbs(){
    grid.innerHTML = '';
    var slides = document.querySelectorAll('.deck .slide');
    _slideTotal = slides.length; // 克隆前缓存
    slides.forEach(function(s,i){ /* 克隆 .slide */ });
    updateCount();
  }
  function updateCount(){
    var n = dialog.querySelectorAll('.export-thumb.selected').length;
    dialog.querySelector('#exp-count').textContent = '已选 '+n+'/'+_slideTotal;
  }
  // ❌ 错误 — 也计算了克隆的缩略图 .slide 元素
  function updateCount(){
    var total = document.querySelectorAll('.slide').length; // 例如 28 而非 14
  }
  ```

- **每个 Deck 必须包含导出等待提示。** P 键导出对话框必须在操作按钮下方显示耐心等待消息，因为大型 Deck 需要明显的时间来渲染。将以下 CSS 规则添加到每个 Deck 的内联 `<style>` 中：
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
  `.export-bar`（`display:flex; flex-wrap:wrap`）上的 `::after` 伪元素占据按钮下方的整行。无需 JavaScript — 纯 CSS。

### 运行时 Chrome CSS（关键 — 当不加载 `base.css` 时）

如果你选择将所有 CSS 内联在 `<style>` 标签中（而不是链接 `assets/base.css`），你还必须包含**运行时创建的 chrome 元素**的 CSS。`runtime.js` 创建这些 DOM 元素，但它们没有内置样式 — 完全依赖 `base.css`：

- **`.overview` / `.overview.open` / `.overview .thumb`** — O 键幻灯片总览网格。缺失 = 按 O 键后只看到透明遮罩，看不到任何缩略图。
- **`.notes-overlay` / `.notes-overlay.open`** — N 键演讲备注底部抽屉。缺失 = 按 N 键后抽屉不显示。

**为什么会这样：** `runtime.js` 动态创建 `<div class="overview">` 并将其附加到 `<body>`，然后切换 `.open` 来显示它。如果没有 CSS 规则针对 `.overview.open`，该元素保持 `display:none`（或不可见且无布局）。用户按 O 键没有任何反应 — 但 DOM 是正确的。

**如何修复：** 将 `assets/base.css` 中的 `.overview` 和 `.notes-overlay` CSS 块复制到内联 `<style>` 中。最小集合：

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

**发布内联 CSS Deck 前的清单：**
- [ ] O 键 → 总览网格正常显示
- [ ] N 键 → 笔记抽屉正常显示
- [ ] P 键 → 导出对话框正常显示
- [ ] S 键 → 演讲者窗口正常弹出

## 写作指南

参见 [references/authoring-guide.md](references/authoring-guide.md) 获取逐步操作指南：文件结构、命名、如何将大纲转化为 Deck、如何根据受众选择布局和主题、如何制作中英双语 Deck，以及如何导出。

## 目录（需要时加载）

- [references/themes.md](references/themes.md) — 全部 36 个主题及使用场景。
- [references/layouts.md](references/layouts.md) — 全部 31 种布局类型。
- [references/animations.md](references/animations.md) — 27 个 CSS + 20 个 Canvas 特效动画。
- [references/full-decks.md](references/full-decks.md) — 全部 15 个完整 Deck 模板。
- [references/presenter-mode.md](references/presenter-mode.md) — **演讲者模式 + 逐字稿编写指南（技术分享/演讲必看）**。
- [references/authoring-guide.md](references/authoring-guide.md) — 完整工作流。
- [references/inline-errors.md](references/inline-errors.md) — **🛑 内联 HTML 键盘/运行时错误修复手册**，涵盖 T 键失效、JS 语法错误、主题变量冲突等常见 Bug 的根因和修复方法。**每次生成 Deck 后必须按此清单自检。**
- [references/export-pitfalls.md](references/export-pitfalls.md) — **P 键导出错误修复手册**，涵盖打印 CSS、SVG foreignObject、画布污染等 12 个已知导出 Bug。
- [examples/export-reference/](examples/export-reference/index.html) — **P 键导出功能完整参考模板**，含导出对话 UI、SVG 生成、PDF 打印的完整实现。生成任何 Deck 时以此为参考确保导出功能可正常工作。

## 文件结构

```
ostar-all-in-html-ppt/
├── SKILL.md                 （本文件）
├── README.md                （中文 README）
├── README.EN.md             （英文 README）
├── showcase.html            （可视化展示页：主题/布局/动效/模板总览）
├── references/              （详细目录，按需加载）
│   ├── themes.md            （36 个主题及使用场景）
│   ├── layouts.md           （31 种布局类型）
│   ├── animations.md        （27 CSS + 20 FX 目录）
│   ├── full-decks.md        （15 个完整 Deck 模板）
│   ├── presenter-mode.md    （🎤 演讲者模式 + 讲稿编写指南）
│   ├── authoring-guide.md   （完整工作流）
│   ├── inline-errors.md     （🛑 键盘/运行时错误参考手册）
│   └── export-pitfalls.md   （P 键导出错误参考手册）
├── assets/
│   ├── base.css             （Token + 原语 — 不要按 Deck 编辑）
│   ├── fonts.css            （网页字体导入）
│   ├── runtime.js           （键盘 + 演讲者 + 导出 + 总览 + 主题循环）
│   ├── themes/*.css         （36 个主题 Token 覆盖，每个主题一个文件）
│   └── animations/
│       ├── animations.css   （27 个命名的 CSS 入场动画）
│       ├── fx-runtime.js    （幻灯片进入时自动初始化 [data-fx]）
│       └── fx/*.js          （20 个 Canvas 特效模块：粒子/图谱/烟花…）
├── templates/
│   ├── deck.html                  （最小 6 页启动模板）
│   ├── theme-showcase.html        （36 页，每页 iframe 隔离主题）
│   ├── layout-showcase.html       （全部 31 个布局的 iframe 浏览）
│   ├── animation-showcase.html    （20 FX + 27 CSS 动画页）
│   ├── full-decks-index.html      （全部 15 个完整 Deck 模板图库）
│   ├── full-decks/<name>/         （15 个带作用域的完整多页 Deck 模板）
│   └── single-page/*.html         （31 个布局文件，带演示数据）
├── scripts/
│   ├── new-deck.sh                （从 deck.html 搭建新 Deck）
│   ├── render.sh                  （无头 Chrome → PNG）
│   └── verify-output/             （自测截图）
├── examples/
│   ├── demo-deck/                  （完整可用的 Deck）
│   ├── export-reference/           （P 键导出参考模板）
│   └── ganqian-peixun/            （培训示例 Deck）
└── docs/
    ├── readme/                    （README 图片）
    └── showcase/                  （展示页 CSS/JS/预览）
```

## 渲染为 PNG

`scripts/render.sh` 封装了位于 `/Applications/Google Chrome.app/Contents/MacOS/Google Chrome` 的无头 Chrome。对于多页捕获，runtime.js 暴露了 `#/N` 深度链接，render.sh 迭代 1..N。

```bash
./scripts/render.sh templates/single-page/kpi-grid.html        # 单页
./scripts/render.sh examples/demo-deck/index.html 8 out-dir    # 8 页，自定义目录
```

## 键盘速查表

```
←  →  Space  PgUp  PgDn  Home  End    导航幻灯片
F                                       切换全屏
S                                       打开演讲者窗口（磁性卡片：当前页/下一页/讲稿/计时器）
P                                       导出对话框 — 选择幻灯片 → 通过浏览器打印导出 PDF (SVG) / 通过 PNG 合并导出 PDF / SVG (.zip) / PNG (.zip)
N                                       快速备注抽屉（底部浮层）
R                                       重置计时器（在演讲者窗口中）
?preview=N                              URL 参数 — 强制仅预览模式（单张幻灯片，无 chrome）
O                                       幻灯片总览网格
T                                       循环切换主题（读取 data-themes 属性）
A                                       在当前幻灯片上循环演示动画
#/N in URL                              深度链接到第 N 张幻灯片
Esc                                     关闭所有浮层
```

## 许可与作者

MIT。版权所有 (c) 2026 ostar999 &lt;ota1754@qq.com&gt;。
