# Static Website Design — Vercel Deployment Test

Date: 2026-03-10

## Goal

A minimal static website to verify the Vercel deployment pipeline, including static asset (image) path resolution.

## File Structure

```
website/
├── index.html
├── images/
│   ├── image1.svg
│   └── image2.svg
└── docs/
    └── superpowers/specs/...
```

## Page Content

- Large heading: "Hello, Vercel!"
- Subtitle with deployment success message
- Live clock (JS, updates every second)
- Two local images displayed side by side (45% width each, rounded corners)
- Inline CSS, dark background, centered layout, no external dependencies

## Images

Two SVG placeholder images stored in `images/`. Can be replaced with real images later.

## Deployment

Drag the `website/` folder to vercel.com — no build command or configuration file needed.
