# FORMA — Gallery

> A zero-dependency, pure HTML/CSS/JS minimalist art gallery. Every artwork is rendered entirely in CSS. No images. No frameworks. No build step.

[![Live Demo](https://img.shields.io/badge/Live%20Demo-GitHub%20Pages-black?style=flat-square)](https://cristiansifuentes.github.io/appClaudeCode/)
[![HTML](https://img.shields.io/badge/HTML5-E34F26?style=flat-square&logo=html5&logoColor=white)](https://developer.mozilla.org/en-US/docs/Web/HTML)
[![CSS](https://img.shields.io/badge/CSS3-1572B6?style=flat-square&logo=css3&logoColor=white)](https://developer.mozilla.org/en-US/docs/Web/CSS)
[![JS](https://img.shields.io/badge/JavaScript-ES6-F7DF1E?style=flat-square&logo=javascript&logoColor=black)](https://developer.mozilla.org/en-US/docs/Web/JavaScript)
[![Claude Code](https://img.shields.io/badge/Built%20with-Claude%20Code-6B4FBB?style=flat-square)](https://claude.ai/code)

---

## What is FORMA?

FORMA is a fictional contemporary art gallery website built as a showcase of advanced front-end techniques using only the browser's native capabilities. It features:

- A **full-screen animated loader**
- A **custom magnetic cursor** with lerp smoothing
- A **hero** with editorial typography and a pure-CSS animated target
- A **seamless infinite ticker**
- A **9-piece gallery** where every artwork is drawn entirely with CSS (no images)
- **Scroll reveal** animations via `IntersectionObserver`
- **Animated counters** with ease-out cubic easing
- **Parallax** on the hero artwork
- A **grain texture overlay** via inline SVG filter

Single file. 920 lines. Zero dependencies.

---

## Preview

```
┌──────────────────────────────────────────────────────────┐
│  FORMA                    Collection   About   Visit      │
│ ──────────────────────────────────────────────────────── │
│                                │                          │
│  Contemporary Art — Est. 2008  │      ◎                  │
│                                │   (CSS art)              │
│  Form                          │                          │
│  & Void                        │                          │
│                                │                          │
│  [ View Collection → ]         │                          │
└──────────────────────────────────────────────────────────┘
  Form — Space — Silence — Light — Shadow — Void — ...  >>>
┌──────────────────────────────────────────────────────────┐
│  Current Collection                      09 Works        │
│  ┌────────────────────┬─────────┐                        │
│  │  Equilibrium (2×1) │ Strata  │                        │
│  ├──────────┬─────────┤         │                        │
│  │ Meridian │Compress.│ Silence │                        │
│  ├──────────┼─────────┤ (2 tall)│                        │
│  │ Division │ Rhythm  │         │                        │
│  ├──────────┴─────────┼─────────┤                        │
│  │  Singularity (2×1) │Emergence│                        │
│  └────────────────────┴─────────┘                        │
└──────────────────────────────────────────────────────────┘
```

---

## CSS Artworks — Technical Breakdown

All nine pieces in the gallery are rendered with zero images, using only CSS properties.

| # | Name | Technique |
|---|------|-----------|
| 1 | **Equilibrium** | Two overlapping circles via `::before` (filled) and `::after` (outline) |
| 2 | **Strata** | `repeating-linear-gradient` horizontal hairlines |
| 3 | **Meridian** | Two `linear-gradient` lines crossing at center + a 7px circle `::after` |
| 4 | **Compression** | Stacked `box-shadow` rings on a square — concentric squares without extra DOM |
| 5 | **Silence** | Black background, single 7px dot, `radial-gradient` glow aura |
| 6 | **Division** | `display: grid` 2×2 with alternating black/white quadrants |
| 7 | **Rhythm** | `repeating-linear-gradient` vertical bars in irregular barcode-like rhythm |
| 8 | **Singularity** | `inset` shorthand to position nested `::before` border square and `::after` filled square |
| 9 | **Emergence** | Two oversized circles clipped by `overflow: hidden`, creating arc fragments |

### The `box-shadow` stacking trick (Compression & Target)

Instead of nesting 10 divs to create concentric rings, a single element with stacked box-shadows creates the effect:

```css
.compression-core {
  width: 22%;
  aspect-ratio: 1;
  background: var(--ink);
  box-shadow:
    0 0 0 11px var(--paper),   /* gap */
    0 0 0 22px var(--ink),     /* ring */
    0 0 0 33px var(--paper),   /* gap */
    0 0 0 44px var(--ink),     /* ring */
    0 0 0 55px var(--paper),   /* gap */
    0 0 0 66px rgba(10,10,10,0.3);
}
```

The spread radius of each shadow layer defines the ring thickness. No extra HTML needed.

---

## Design System

```css
:root {
  --ink:   #0a0a0a;           /* near-black — text, lines, fills */
  --paper: #f4f4f2;           /* warm white — backgrounds       */
  --rule:  rgba(10,10,10,0.09); /* hairline dividers             */
  --ease:  cubic-bezier(0.16, 1, 0.3, 1); /* expo-like ease-out */
}
```

**Typography scale**

| Use | Size | Weight | Family |
|-----|------|--------|--------|
| Labels / eyebrows | `10–11px` | 300–400 | Helvetica Neue |
| Body copy | `13–14px` | 300 | Helvetica Neue |
| Section titles | `30px` | 400 | Georgia (serif) |
| Hero headline | `clamp(60px, 7.2vw, 110px)` | 400 | Georgia |
| Stats | `58px` | 400 | Georgia |

---

## CSS Techniques — Master Reference

### 1. CSS Custom Properties (Variables)

Define once on `:root`, reference everywhere. Change one value to reskin the whole project.

```css
:root { --ease: cubic-bezier(0.16, 1, 0.3, 1); }
.element { transition: transform 0.5s var(--ease); }
```

### 2. `mix-blend-mode: difference`

The nav and custom cursor use this to automatically invert against any background — white on dark sections, dark on light sections — without any JavaScript color detection.

```css
nav    { mix-blend-mode: difference; }
#cursor { mix-blend-mode: difference; background: white; }
```

### 3. `clamp()` — Fluid Typography

Eliminates media query breakpoints for font sizes.

```css
font-size: clamp(60px, 7.2vw, 110px);
/* minimum  | fluid   | maximum */
```

### 4. CSS Grid — Asymmetric Spanning

Complex magazine-style layouts without a single line of JavaScript.

```css
.gallery {
  display: grid;
  grid-template-columns: repeat(3, 1fr);
  grid-template-rows: repeat(4, 310px);
  gap: 2px;
}
/* Wide piece spans 2 columns */
.gallery .piece:nth-child(1) { grid-column: 1 / 3; }
/* Tall piece spans 2 rows */
.gallery .piece:nth-child(5) { grid-column: 3; grid-row: 2 / 4; }
```

### 5. `aspect-ratio`

Maintains perfect squares and circles regardless of container size.

```css
.circle { width: 38%; aspect-ratio: 1; border-radius: 50%; }
```

### 6. `repeating-linear-gradient` for Textures

No image files needed. The Strata and Rhythm artworks are pure CSS textures.

```css
/* Horizontal hairlines */
background: repeating-linear-gradient(
  to bottom,
  #0a0a0a 0, #0a0a0a 1px,
  #f4f4f2 1px, #f4f4f2 13px
);
```

### 7. Grain Texture via Inline SVG Filter

An `feTurbulence` SVG filter embedded as a `data:` URL in `background-image` — no external file, no HTTP request.

```css
body::after {
  background-image: url("data:image/svg+xml,%3Csvg ...%3E
    %3Cfilter id='n'%3E
      %3CfeTurbulence type='fractalNoise' baseFrequency='0.85'/%3E
    %3C/filter%3E
  %3C/svg%3E");
  opacity: 0.025;
  pointer-events: none;
}
```

### 8. CTA Button — Fill Sweep

A sliding background fill with no JavaScript, using `translateX` on a pseudo-element.

```css
.cta::before {
  content: '';
  position: absolute;
  inset: 0;
  background: var(--ink);
  transform: translateX(-102%);      /* hidden off-left */
  transition: transform 0.5s var(--ease);
}
.cta:hover::before { transform: translateX(0); } /* slides in */
.cta:hover { color: var(--paper); }
```

---

## JavaScript Techniques — Master Reference

### 1. Custom Cursor with Lerp Smoothing

Instead of snapping to the mouse position instantly, the cursor _lerps_ (linearly interpolates) toward the target — creating a lag that feels premium.

```js
let mx = 0, my = 0, cx = 0, cy = 0;

document.addEventListener('mousemove', e => { mx = e.clientX; my = e.clientY; });

(function moveCursor() {
  cx += (mx - cx) * 0.14;   // 14% of remaining distance per frame
  cy += (my - cy) * 0.14;
  cur.style.left = cx + 'px';
  cur.style.top  = cy + 'px';
  requestAnimationFrame(moveCursor);
})();
```

**Formula:** `current += (target - current) * factor`
- Factor `0.14` = smooth lag. `1.0` = instant snap. `0.05` = very sluggish.

### 2. IntersectionObserver — Scroll Reveal

Replaces scroll event listeners (which fire hundreds of times per second) with a browser-native callback that fires only when an element enters the viewport.

```js
const io = new IntersectionObserver(entries => {
  entries.forEach(e => {
    if (!e.isIntersecting) return;
    e.target.classList.add('in');  // CSS handles the animation
    io.unobserve(e.target);        // stop watching after first reveal
  });
}, { threshold: 0.1 });

document.querySelectorAll('.reveal').forEach(el => io.observe(el));
```

### 3. Counter Animation with Ease-Out Cubic

Smooth number counting using `performance.now()` for frame-accurate timing.

```js
const target = 218;
const dur    = 1500; // ms
const t0     = performance.now();

(function tick(now) {
  const p = Math.min((now - t0) / dur, 1);       // linear 0→1
  const v = 1 - Math.pow(1 - p, 3);              // ease-out cubic
  el.textContent = Math.floor(v * target);
  if (p < 1) requestAnimationFrame(tick);
  else el.textContent = target;
})(t0);
```

**Easing formula:** `eased = 1 - (1 - progress)³`

### 4. Passive Event Listeners

The `{ passive: true }` flag tells the browser the scroll handler will never call `preventDefault()` — enabling GPU-accelerated scroll without waiting for JS.

```js
window.addEventListener('scroll', handler, { passive: true });
```

### 5. Seamless Infinite Ticker (CSS only)

Duplicate the content inside `.ticker` so the second copy seamlessly follows the first. The animation moves by exactly `50%` — when the first copy exits, the second is in its place.

```html
<div class="ticker">
  <span>Form</span> ... <span>Reduction</span>
  <!-- exact duplicate below -->
  <span>Form</span> ... <span>Reduction</span>
</div>
```
```css
@keyframes run {
  from { transform: translateX(0); }
  to   { transform: translateX(-50%); }  /* -50% of total (2x) width */
}
```

---

## Claude Code — Commands Used to Build This

This project was created entirely using [Claude Code](https://claude.ai/code), Anthropic's AI coding CLI. Below are the exact commands and workflows used.

### Install Claude Code

```bash
npm install -g @anthropic-ai/claude-code
claude
```

### Key Claude Code Slash Commands

| Command | What it does |
|---------|-------------|
| `/help` | Show all available commands |
| `/init` | Generate a `CLAUDE.md` file documenting your codebase |
| `/run` | Launch and drive the app to verify changes work |
| `/code-review` | Review the current diff for bugs and improvements |
| `/code-review ultra` | Deep multi-agent cloud review of the branch |
| `/simplify` | Suggest and apply reuse/efficiency cleanups |
| `/security-review` | Full security audit of pending changes |
| `/verify` | Confirm a fix works by running the actual app |
| `/config` | Open interactive settings (model, theme, etc.) |
| `/fast` | Toggle Fast mode (Opus with faster output) |
| `/clear` | Clear conversation context |
| `/compact` | Compress context to save tokens |
| `/memory` | View and manage persistent memory |
| `/cost` | Show token usage and cost for the session |

### How This Project Was Scaffolded with Claude Code

```
> Build a minimalist art gallery website. Pure HTML/CSS/JS, no frameworks.
  Use CSS-only artworks, custom cursor with lerp, scroll reveal,
  IntersectionObserver counters, and a mix-blend-mode nav.
```

Claude Code generated the entire `index.html` in a single pass. The workflow:

```bash
# 1. Start Claude Code in your project directory
cd my-project
claude

# 2. Describe what you want (natural language)
# Claude generates the file

# 3. Review the code
/code-review

# 4. Run and verify it works
/run

# 5. Push to GitHub (all from inside Claude Code)
```

### Creating the GitHub Repo from the CLI

No browser needed. All done through Claude Code running terminal commands:

```powershell
# Install GitHub CLI
winget install --id GitHub.cli --accept-source-agreements --accept-package-agreements

# Authenticate (one-time — opens browser for OAuth)
gh auth login --hostname github.com --git-protocol https --web

# Initialize git, commit, create repo, and push — one command
git init
git add .
git commit -m "Initial commit"
gh repo create appClaudeCode --public --source=. --remote=origin --push
```

### Claude Code Settings & Configuration

```bash
# View current config
/config

# Set default model
claude config set model claude-opus-4-8

# Enable auto-accept for trusted commands
claude --dangerously-skip-permissions   # use with caution

# Project-level instructions (read by Claude on every session)
# Create CLAUDE.md in your project root:
echo "# Project Rules\n- Use vanilla JS only\n- No frameworks" > CLAUDE.md
```

### Claude Code Hooks (Automated Behaviors)

Hooks run shell commands in response to Claude Code events. Configure in `.claude/settings.json`:

```json
{
  "hooks": {
    "PostToolUse": [
      {
        "matcher": "Edit|Write",
        "hooks": [{ "type": "command", "command": "npx prettier --write $FILE" }]
      }
    ],
    "Stop": [
      {
        "hooks": [{ "type": "command", "command": "notify-send 'Claude Code finished'" }]
      }
    ]
  }
}
```

**Available hook events:**
- `PreToolUse` — before any tool call
- `PostToolUse` — after any tool call
- `Notification` — when Claude sends a notification
- `Stop` — when Claude finishes a response

### MCP Servers (Model Context Protocol)

Claude Code supports MCP servers that add tools to the agent (Google Drive, GitHub, Slack, etc.):

```bash
# Add an MCP server
claude mcp add github --command npx --args @modelcontextprotocol/server-github

# List connected servers
claude mcp list

# Common servers
npx @modelcontextprotocol/server-filesystem   # file system access
npx @modelcontextprotocol/server-github       # GitHub API
npx @modelcontextprotocol/server-postgres     # PostgreSQL queries
```

---

## Frontend Mastery Checklist

Work through these to go from intermediate to expert in modern frontend:

### CSS

- [ ] CSS Custom Properties — cascade, inheritance, dynamic updates with JS
- [ ] CSS Grid — named lines, `subgrid`, `auto-fill` vs `auto-fit`
- [ ] CSS Flexbox — `flex-basis`, `flex-grow`, `align-content`
- [ ] `clamp()`, `min()`, `max()` — intrinsic fluid sizing
- [ ] `mix-blend-mode` and `isolation` — compositing contexts
- [ ] `aspect-ratio` — replaces padding-top hacks
- [ ] `inset` shorthand — replaces `top/right/bottom/left`
- [ ] `box-shadow` stacking — multiple shadows, spread radius tricks
- [ ] `repeating-linear-gradient` — CSS-only textures and patterns
- [ ] CSS `@keyframes` — `animation-fill-mode`, `animation-delay`, `will-change`
- [ ] `cubic-bezier()` — custom easing curves
- [ ] `IntersectionObserver` + CSS class toggling — performant scroll animations
- [ ] `::before` / `::after` pseudo-elements — artwork without extra DOM
- [ ] `overflow: hidden` + oversized children — clipping shapes
- [ ] SVG filters in CSS (`feTurbulence`, `feGaussianBlur`, `feColorMatrix`)

### JavaScript

- [ ] `requestAnimationFrame` — 60fps animation loops
- [ ] Linear interpolation (lerp) — smooth value following
- [ ] `IntersectionObserver` — viewport detection without scroll events
- [ ] `performance.now()` — frame-accurate timing
- [ ] Easing functions — ease-out cubic, ease-in-out, spring physics
- [ ] `{ passive: true }` event listeners — scroll performance
- [ ] IIFE (Immediately Invoked Function Expression) — encapsulation
- [ ] `data-*` attributes — HTML as state store
- [ ] `Set` for deduplication — prevent double-firing observers

### Design Principles Applied Here

| Principle | Implementation |
|-----------|---------------|
| **Negative space** | `opacity: 0.38` text, generous padding, invisible separators |
| **Typographic hierarchy** | 3 font sizes max, 2 families (sans + serif), weight contrast |
| **Monochromatic palette** | 2 colors (`--ink`, `--paper`) with opacity variations |
| **Motion restraint** | Long durations (0.95s), expo ease-out, subtle scale (1.03×) |
| **Editorial grid** | 56px gutters, 1px hairline rules, asymmetric column spanning |
| **Texture** | SVG grain at 2.5% opacity — perceived depth without color |

---

## Run Locally

No build step. Open the file.

```bash
# Option 1 — just open it
open index.html

# Option 2 — local dev server (auto-reload)
npx serve .
# or
python -m http.server 8000
```

---

## Deploy to GitHub Pages

```bash
# In your repo settings → Pages → Source: Deploy from branch → main / (root)
# Your site will be live at:
# https://<your-username>.github.io/<repo-name>/
```

Or from the CLI:

```bash
gh api repos/CristianSifuentes/appClaudeCode \
  --method PUT \
  --field source.branch=master \
  --field source.path=/
```

---

## File Structure

```
appClaudeCode/
└── index.html     # Everything. HTML + CSS + JS. 920 lines.
```

---

## License

MIT — use freely, learn from it, build on it.

---

> Built with [Claude Code](https://claude.ai/code) by Anthropic.
> No frameworks were harmed in the making of this gallery.
