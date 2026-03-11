# Personal Blog Design Spec

Date: 2026-03-11

## Goal

A personal blog website built with Astro, deployed on Vercel. Two main sections: blog posts (Markdown-driven) and a personal introduction page.

## Tech Stack

- **Framework:** Astro (official blog template as starting point)
- **Content:** Markdown files with frontmatter via Astro Content Collections
- **Styling:** Custom CSS (no UI framework), warm color palette
- **Deployment:** Vercel (auto-deploy on git push, no manual config needed)

## Pages

### `/` — Blog Index
- Site name + one-line tagline at the top
- List of all blog posts: title, date, description (from frontmatter)
- Click a post to navigate to its detail page

### `/about` — About Me
- Avatar image (`public/avatar.jpg`)
- A short bio paragraph (written directly in the `.astro` file)
- Social links: GitHub, Twitter/X (configured in `astro.config.mjs` site metadata)

### `/blog/[slug]` — Post Detail
- Post title and date
- Rendered Markdown body (supports headings, paragraphs, code blocks, blockquotes, images, tables)
- "Back to blog" link at the bottom

## File Structure

```
src/
├── content/
│   └── blog/              ← Add .md files here to publish posts
│       ├── hello-world.md
│       └── on-writing.md
├── pages/
│   ├── index.astro        ← Blog list
│   ├── about.astro        ← About page
│   └── blog/
│       └── [slug].astro   ← Post detail (dynamic route)
├── layouts/
│   └── BaseLayout.astro   ← Shared nav + footer
└── styles/
    └── global.css         ← Warm color variables + markdown styles

public/
├── avatar.jpg             ← Profile photo
└── images/                ← Post images referenced as /images/photo.jpg
```

## Content: Frontmatter Format

```markdown
---
title: "On Writing Well"
date: 2026-03-10
description: "A few thoughts on clarity and craft."
---

Post body here...
```

## Rich Content Support

- **Images in posts:** Place in `public/images/`, reference with `![alt](/images/photo.jpg)`. Global CSS ensures `max-width: 100%`, border-radius, and vertical margin.
- **Tables:** Standard Markdown syntax. Global CSS styles table borders with `#ede8df`, adds light header background, and ensures readable spacing.

## Design System

### Color Palette
| Token | Value | Usage |
|---|---|---|
| `--bg` | `#fffbf5` | Page background |
| `--border` | `#ede8df` | Dividers, borders |
| `--text-heading` | `#1a1008` | Titles, headings |
| `--text-body` | `#4a3828` | Body text |
| `--text-muted` | `#a08060` | Dates, labels, nav |
| `--accent` | `#c07840` | Links, active state, dates |

### Typography
- **Headings & body text:** Georgia, serif
- **Navigation, dates, labels:** system-ui, sans-serif
- **Base font size:** 17px, line-height 1.75

### Components
- **Navigation bar:** Site name (serif bold, left) + links (sans-serif small, right), `#fffbf5` background, bottom border `#ede8df`
- **Post card:** Title (serif bold), date (accent color, uppercase), description (serif muted), "阅读全文 →" link
- **Post body:** Generous line-height, images full-width with rounded corners, tables with warm border styling

## Sample Content

Two example posts included on initial setup so the site looks complete immediately.

## Publishing New Posts

1. Create `src/content/blog/my-post.md` with frontmatter
2. Write content in Markdown
3. `git push` → Vercel auto-builds and deploys
