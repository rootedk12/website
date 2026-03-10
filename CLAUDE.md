# CLAUDE.md — RootED K-12 Website

This file provides guidance for AI assistants (Claude and others) working on this repository.

---

## Project Overview

This is the marketing website for **RootED K-12**, an educational technology consulting firm that provides workshops, strategic planning, and IT operations services to K-12 school districts.

- **Live site:** https://www.rootedk12.com
- **Hosting:** GitHub Pages (automatic deployment on push to `main`)
- **Contact:** info@rootedk12.com

---

## Tech Stack

This is a **static single-page website** — no build tool, no framework, no npm.

| Layer      | Technology                                    |
|------------|-----------------------------------------------|
| Markup     | HTML5 (`index.html`, 874 lines)               |
| Styling    | Tailwind CSS (CDN) + custom CSS in `<style>`  |
| JavaScript | Vanilla JS in `<script>` tag — no frameworks  |
| Icons      | Font Awesome 6.4.0 (CDN)                      |
| Fonts      | Google Fonts: Libre Baskerville + Lexend      |
| Analytics  | Google Tag Manager                            |
| Scheduling | Calendly embed                                |
| Podcast    | rss2json.com API (K-12 Tech Talk RSS feed)    |

All dependencies are loaded via CDN. There is no `package.json`, `node_modules`, or build process.

---

## Repository Structure

```
website/
├── index.html          # Entire website — HTML, CSS, and JS all in one file
├── 404.html            # Custom 404 error page
├── readme.md           # Human-readable project overview
├── CLAUDE.md           # This file — AI assistant guidance
├── robots.txt          # SEO: allow all crawlers, reference sitemap
├── sitemap.xml         # Single-URL sitemap for SEO
├── CNAME               # GitHub Pages custom domain: www.rootedk12.com
├── .github/
│   └── dependabot.yml  # Auto-updates for npm and GitHub Actions (weekly/monthly)
└── images/
    ├── hero.png        # Hero section background (855 KB)
    ├── Operations.png  # Service card icon
    ├── PD.png          # Service card icon
    ├── Strategy.png    # Service card icon
    ├── podcast.png     # Podcast section image
    ├── dayofai.png     # Partner/event image
    ├── k12techpro.webp # Partner logo
    ├── amesbury.jpg    # District partner logo
    ├── brockton.png    # District partner logo
    ├── clever.jpg      # Partner logo
    ├── lunenburg.png   # District partner logo
    ├── malden.png      # District partner logo
    ├── milwaukee.jpg   # District partner logo
    └── wakefield.png   # District partner logo
```

---

## Development Workflow

### Making Changes

1. Edit `index.html` directly (all content, styles, and scripts live here)
2. Commit to your working branch with a clear message
3. Merge/push to `main` to deploy
4. Changes go live on https://www.rootedk12.com within ~60 seconds

### No Build Step Required

There is nothing to install or compile. Open `index.html` in a browser to preview locally. All CDN resources require an internet connection.

### Deployment

GitHub Pages automatically deploys from the `main` branch. There are no GitHub Actions workflow files — GitHub Pages handles this natively.

---

## index.html Structure

The entire site is a single HTML file. Sections appear in this order:

| Section ID     | Content                                                    |
|----------------|------------------------------------------------------------|
| `#main-nav`    | Navigation bar with logo, links, mobile hamburger          |
| *(hero)*       | Full-bleed hero with headline and CTAs                     |
| `#partners`    | Infinite-scroll logo carousel of district/partner logos    |
| `#services`    | Three service cards (PD, Strategy, IT Operations)          |
| `#insights`    | Podcast section — dynamically fetches latest episodes      |
| `#about`       | Team bios (Mark Racine, Sarah Racine, Milo)                |
| `#contact`     | Contact CTA with email and Calendly link                   |
| *(footer)*     | Copyright                                                  |
| *(modal)*      | Hidden "Rooted Philosophy" modal (toggled by JS)           |

---

## CSS Conventions

### Brand Color Variables (defined in `<style>`)

```css
--brand-green: #2D5A27   /* Primary green — headers, accents */
--brand-sage:  #8FBC8F   /* Lighter green — backgrounds, secondary */
--brand-cream: #FDFBF7   /* Off-white background */
--brand-gold:  #D4AF37   /* Gold — highlights, borders */
```

### Typography

- **Headings (h1–h4):** `font-family: 'Libre Baskerville', serif` — weights 400–700
- **Body / UI text:** `font-family: 'Lexend', sans-serif` — weights 300–600
- **Buttons:** All caps (`text-transform: uppercase`), letter-spacing `0.05em`

### Key CSS Classes

| Class              | Purpose                                          |
|--------------------|--------------------------------------------------|
| `.fade-in-up`      | Scroll-triggered fade-in (via Intersection Observer) |
| `.service-card`    | Service section cards with hover effects         |
| `.btn-primary`     | Dark green filled button                         |
| `.btn-outline-white` | White outlined button (for dark backgrounds)   |
| `.btn-invert`      | Inverted button style                            |
| `.btn-solutions`   | Solutions/CTA button variant                     |
| `.nav-scrolled`    | Applied to nav on scroll — shrinks padding/logo  |
| `.slider`          | Infinite-scroll animation for partner logos      |
| `.sr-only`         | Visually hidden but accessible to screen readers |
| `.modal`           | Hidden modal overlay (philosophy section)        |

---

## JavaScript Conventions

All JavaScript is vanilla, inline in a `<script>` tag at the bottom of `index.html`.

### Key Functions

| Function                  | Purpose                                               |
|---------------------------|-------------------------------------------------------|
| `togglePhilosophyModal()` | Opens/closes the "Rooted Philosophy" modal            |
| `toggleMenu()`            | Toggles the mobile navigation hamburger menu          |
| `fetchPodcastFeed()`      | Async fetch from rss2json.com for podcast episodes    |
| `populatePodcastSection()`| Renders fetched podcast data into the DOM             |
| `gtag()`                  | Google Tag Manager event tracking                     |

### Patterns Used

- **Intersection Observer** — Used for `.fade-in-up` scroll animations on entry
- **Async/await** — Used for podcast RSS feed fetching with try/catch error handling
- **Event delegation** — Direct event listeners on specific elements (not global)
- **No state management** — No framework; DOM is the source of truth

---

## SEO Setup

- `<meta>` description and Open Graph tags are in the `<head>` of `index.html`
- JSON-LD structured data is embedded as a `<script type="application/ld+json">` for organization schema
- `robots.txt` allows all crawlers and references `sitemap.xml`
- `sitemap.xml` lists the single homepage URL with priority 1.00
- Images use descriptive `alt` attributes

---

## Important Constraints

- **Do not add a build step** — Keep this as a plain static site unless explicitly requested
- **Do not introduce npm or node** — All dependencies must remain CDN-based
- **Keep everything in `index.html`** — Avoid splitting into separate CSS/JS files unless explicitly asked
- **Preserve existing brand colors** — Use the CSS variables defined in `<style>`, not arbitrary colors
- **Do not upgrade CDN versions** without testing — Tailwind CDN, Font Awesome, and Google Fonts are version-pinned
- **Calendly and Google Tag Manager IDs** are production — do not modify them
- **Footer year** is currently hardcoded as `2025` — update if asked

---

## Dependabot Configuration

`.github/dependabot.yml` runs Dependabot for:
- **npm** ecosystem — weekly, max 5 open PRs
- **github-actions** ecosystem — monthly, max 5 open PRs

Note: Since there is no `package.json`, the npm configuration targets any future packages added to a root `package.json`.

---

## Branching Model

- `main` — Live production branch; pushes auto-deploy to GitHub Pages
- `claude/...` — Claude AI session branches for AI-generated changes (e.g., `claude/claude-md-mml7ngw1wqudgqec-pCIsA`)

When working as Claude Code, always develop on the designated `claude/...` branch and open a PR to `main`.
