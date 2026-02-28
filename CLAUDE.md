# CLAUDE.md — AI Assistant Guide for Youth 4 A Better Future PH Website

## Project Overview

This is the official website for **Youth 4 A Better Future PH** (also known as KPSMK), a nonprofit organization dedicated to empowering Filipino youth through financial literacy education. The site is hosted at **kpsmk.org** via GitHub Pages.

The entire website is a **single static HTML file** (`index.html`) with no build tools, frameworks, or package managers. Everything — HTML structure, CSS styles, and JavaScript logic — lives in one file.

---

## Repository Structure

```
/
├── index.html   # The entire website (HTML + CSS + JS in one file)
├── CNAME        # GitHub Pages custom domain: kpsmk.org
└── CLAUDE.md    # This file
```

There are no `node_modules`, `package.json`, build configs, or test files. This is intentional — the site is pure vanilla web technology.

---

## Technology Stack

| Layer      | Technology                              |
|------------|----------------------------------------|
| Markup     | HTML5 (semantic elements)              |
| Styles     | CSS3 (inline in `<style>` block)       |
| Scripts    | Vanilla JavaScript (inline in `<script>`) |
| Fonts      | Google Fonts CDN (Playfair Display, Inter) |
| Hosting    | GitHub Pages                           |
| Domain     | kpsmk.org (via CNAME)                  |

No external JavaScript libraries. No CSS preprocessors. No bundlers.

---

## index.html Architecture

### Overall File Structure

```
index.html
├── <head>              — meta tags, Google Fonts import, inline <style>
├── <header>            — sticky navigation bar with logo + links
├── <main>              — five page sections (only one visible at a time)
│   ├── #home           — hero / landing section
│   ├── #about          — mission, vision, team, story, beliefs
│   ├── #work           — programs, curriculum, delivery methods
│   ├── #involved       — volunteer, partner, sponsor opportunities
│   └── #contact        — contact form + email info
├── <footer>            — copyright and tagline
└── <script>            — vanilla JS for SPA navigation and form handling
```

### Single-Page Application (SPA) Pattern

The site uses a simple DOM-based SPA pattern. All five sections exist in the HTML at all times, but only one is shown using a `.hidden` CSS class:

```js
// Show a section by ID, hide all others
function showSection(sectionId) { ... }
```

Navigation links call `showSection()` directly via `onclick` attributes:
```html
<a href="#" onclick="showSection('about')">About</a>
```

On `DOMContentLoaded`, the home section is shown by default.

### Mobile Navigation

A hamburger menu button (`.menu-btn`) appears on small screens. Clicking it calls `toggleMenu()`, which toggles the `.open` class on the `<nav>` element to show/hide mobile nav links.

### Contact Form

The contact form calls `handleSubmit(event)` on submit. It currently shows a `window.alert()` confirmation and resets the form. There is **no backend integration** — form submissions are not stored or emailed anywhere.

---

## Design System

### Color Palette (CSS Custom Properties)

```css
--primary-navy:    #001F54   /* Main brand color, backgrounds */
--primary-red:     #CE1126   /* Accent color, CTAs, highlights */
--dark-bg:         #0a0e1a   /* Page background */
--card-bg:         #0d1526   /* Card backgrounds */
--text-primary:    #f9fafb   /* Main body text */
--text-secondary:  #d1d5db   /* Muted/secondary text */
--text-muted:      #9ca3af   /* Subtle labels */
--border-color:    rgba(255,255,255,0.08) /* Card/element borders */
```

Always use these CSS variables rather than hardcoding color values.

### Typography

- **Headings** (`h1`–`h6`): Playfair Display, serif, weights 700–900
- **Body text**: Inter, sans-serif, weights 400–700
- `h1`: ~5.5rem (hero), section titles: ~4.5rem
- Body: 1rem–1.1rem with 1.7–1.8 line-height

### Component Patterns

**Cards** — standard pattern throughout the site:
```html
<div class="card">...</div>
<!-- Variants: blue-card, yellow-card, white-card -->
```
Cards use glassmorphism (`backdrop-filter: blur`), hover `translateY(-8px)` transforms, and box-shadow transitions.

**CTA Buttons**:
```html
<a href="#" class="cta-button" onclick="showSection('contact')">Get In Touch</a>
```
Gradient background with `transform: translateY(-2px)` on hover.

**Section Structure**:
```html
<section id="work" class="page-section hidden">
  <div class="container">
    <div class="section-header">
      <h2 class="section-title">How We Work</h2>
      <p class="section-subtitle">...</p>
    </div>
    <!-- content -->
  </div>
</section>
```

**Grids**: Use CSS Grid with `auto-fit` and `minmax()` for responsive layouts:
```css
grid-template-columns: repeat(auto-fit, minmax(300px, 1fr));
```

---

## Development Workflow

### Making Changes

Since there is no build step:

1. Edit `index.html` directly
2. Open `index.html` in a browser to preview changes
3. Commit and push to deploy via GitHub Pages

```bash
# Preview locally (simple option)
open index.html
# Or serve with any static file server:
python3 -m http.server 8080
```

### Deployment

GitHub Pages automatically deploys on push to the default branch (`master`). There is no CI/CD pipeline. After pushing, the live site at **kpsmk.org** updates within a few minutes.

```bash
git add index.html
git commit -m "descriptive commit message"
git push origin master
```

### Branch Strategy

- `master` — production branch; changes here go live immediately on kpsmk.org
- `claude/*` branches — used by Claude Code for proposed changes; require review before merging to `master`

---

## Key Conventions

### HTML
- Use semantic elements (`<header>`, `<main>`, `<section>`, `<footer>`, `<nav>`)
- All page sections must have `class="page-section hidden"` and a unique `id`
- Keep the existing heading hierarchy (`h1` only in hero, `h2` for section titles, `h3` for subsections)
- Image `alt` attributes are required

### CSS
- All CSS lives in the `<style>` block in `<head>` — do not use external stylesheets
- Use CSS custom properties (`var(--primary-navy)`) instead of hardcoded colors
- Responsive breakpoints use `@media (max-width: Npx)` — primary breakpoints at 768px and 480px
- Animations use `@keyframes` with `animation` shorthand; prefer `opacity` and `transform` for performance
- Glassmorphism: `backdrop-filter: blur(20px)` with `background: rgba(...)` — keep consistent

### JavaScript
- All JS lives in the `<script>` block at the bottom of `<body>` — no external scripts
- Keep the existing SPA functions (`showSection`, `toggleMenu`, `handleSubmit`) intact
- Event handlers are attached via HTML `onclick` attributes — maintain this pattern for consistency
- No jQuery, no frameworks, no external libraries

### Content & Copy
- Organization name: **Youth 4 A Better Future PH** (full) or **KPSMK** (short form)
- Contact emails: `volunteer@kpsmk.org`, `partnerships@kpsmk.org`, `info@kpsmk.org`
- Social media: LinkedIn, Instagram, TikTok (links are in the footer/contact section)
- Tone: Inspirational, mission-driven, professional but approachable

---

## Common Tasks

### Add a new section

1. Add a nav link calling `showSection('new-section-id')`
2. Add `<section id="new-section-id" class="page-section hidden">` to `<main>`
3. Style within the existing `<style>` block following existing patterns

### Update team member info

Find the `#about` section and locate the `.team-grid` container. Team cards follow this pattern:
```html
<div class="card">
  <h3>Name</h3>
  <p class="role">Title</p>
  <p>Bio text...</p>
</div>
```

### Change colors

Update the CSS custom property values in the `:root` block at the top of the `<style>` section. Do not hardcode colors elsewhere.

### Add a new program or card

Follow the existing card HTML pattern and place inside the appropriate section's grid container. The CSS handles responsive layout automatically via `auto-fit`.

---

## Things to Avoid

- **Do not** introduce a build system, package manager, or bundler unless explicitly requested
- **Do not** add external JavaScript libraries or CSS frameworks (Bootstrap, Tailwind, etc.)
- **Do not** split the single `index.html` into multiple files without explicit approval
- **Do not** add backend logic — this is a static site; form handling stays client-side only
- **Do not** push directly to `master` from a Claude branch — propose changes for review
- **Do not** hardcode colors outside CSS custom properties
- **Do not** break the SPA navigation pattern (`showSection` + `.hidden` class toggle)
