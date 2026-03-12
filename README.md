# duanleks.space

Personal project hub — a launchpad for all tools and experiments at [duanleks.space](https://duanleks.space).

Built with SvelteKit + Tailwind, deployed on Vercel.

---

## Stack

| | |
|---|---|
| Framework | SvelteKit 2 (Svelte 5 runes) |
| Styling | Tailwind CSS 3 + scoped component styles |
| Fonts | Syne (display) + JetBrains Mono (mono) |
| Adapter | `@sveltejs/adapter-vercel` |
| Deploy | Vercel |

---

## Project structure

```
src/
├── app.css              # Global CSS variables (dark/light theme tokens)
├── app.html             # HTML shell
├── lib/
│   ├── assets/
│   │   └── favicon.svg  # DL monogram favicon
│   ├── projects.js      # Project data (name, url, logo, tags, color, status)
│   ├── theme.js         # Svelte store for dark/light theme state
│   ├── ProjectCard.svelte
│   ├── ProjectIcon.svelte  # Inline SVG icons for logo-less projects
│   └── StatusBadge.svelte
└── routes/
    ├── +layout.svelte
    └── +page.svelte

static/
└── logos/               # Project logo files served at /logos/*
    ├── duanleks.svg
    ├── LFranca.png
    ├── graphn.svg
    ├── mermaid.svg
    ├── mermaid_white.svg  # White variant for dark theme
    ├── mindmap.png
    ├── mindmap_white.png  # White variant for dark theme
    └── webui.png
```

---

## Dev

```sh
npm install
npm run dev
```

## Build & preview

```sh
npm run build
npm run preview
```

---

## Deploy (Vercel)

Push to GitHub, import the repo in Vercel. Framework is auto-detected as SvelteKit via `vercel.json`. No extra config needed.

```json
// vercel.json
{ "framework": "sveltekit" }
```

---

## Adding a project

Edit `src/lib/projects.js`:

```js
{
  name: "myapp",                        // used as card title + icon key
  description: "Short description",
  url: "https://myapp.duanleks.space",
  logo: "/logos/myapp.svg",            // null = uses ProjectIcon fallback
  tags: ["tag1", "tag2"],
  color: "#6ee7b7",                    // accent color for glow, tags, hover
  status: "active",                    // active | building | paused | archived
  story: "One-liner about why it exists."
}
```

If `logo` is `null`, add a matching SVG icon block in `src/lib/ProjectIcon.svelte`.

If the logo is dark/black and needs to flip white in dark mode, add it to `darkLogoMap` in `ProjectCard.svelte` and put a `*_white` variant in `static/logos/`.

---

## UI guidelines

### Theme

Two themes — dark (default) and light — toggled via a button in the hero. Theme state lives in `src/lib/theme.js` (Svelte store) and is applied as `data-theme="light"` on `<html>`. CSS variables in `app.css` handle all color switching.

### Colors

| Token | Dark | Light | Usage |
|---|---|---|---|
| `--bg-base` | `#0a0c0f` | `#f7f5f0` | Page background |
| `--bg-card` | `#13161d` | `#ffffff` | Card background |
| `--text-primary` | `#e8eaf0` | `#1a1c22` | Headings, body |
| `--text-secondary` | `#8891a4` | `#5a6070` | Descriptions |
| `--text-muted` | `#4a5260` | `#9aa0ae` | Meta, labels |
| `--accent` | `#e8c547` | `#c9a820` | Gold — active filter, arrow, glow |

Each project card also has its own `--accent` CSS variable (the `color` field) used for per-card glow and tag hover.

### Typography

- **Display** — `Syne 700/800` for the hero name and card titles
- **Mono** — `JetBrains Mono 400/500/600` for everything else (descriptions, tags, filters, labels)

Keep all UI text in JetBrains Mono. Only use Syne for large display text.

### Logos & icons

- Prefer SVG logos placed in `static/logos/`
- Dark/black logos need a `*_white` variant for dark theme — swap is handled in `ProjectCard.svelte` via `darkLogoMap`
- Projects without a logo file get an inline SVG icon from `ProjectIcon.svelte`, styled with the project's accent color on a tinted background

### Status badges

| Status | Color | Meaning |
|---|---|---|
| `active` | green (pulsing dot) | Live and running |
| `building` | yellow | In progress / migrating |
| `paused` | gray | On hold |
| `archived` | red | No longer maintained |
