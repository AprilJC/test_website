# Static Website Implementation Plan

> **For agentic workers:** REQUIRED: Use superpowers:subagent-driven-development (if subagents available) or superpowers:executing-plans to implement this plan. Steps use checkbox (`- [ ]`) syntax for tracking.

**Goal:** Build a minimal single-page static website with two local images to verify Vercel deployment.

**Architecture:** A single `index.html` with inline CSS and a small inline JS clock, plus two SVG placeholder images in an `images/` directory. No build step, no dependencies.

**Tech Stack:** HTML5, CSS3 (inline), vanilla JS (inline), SVG

---

## Chunk 1: Project files

### Task 1: Create placeholder images

**Files:**
- Create: `images/image1.svg`
- Create: `images/image2.svg`

- [ ] **Step 1: Create `images/` directory and `image1.svg`**

```svg
<!-- images/image1.svg -->
<svg xmlns="http://www.w3.org/2000/svg" width="600" height="400" viewBox="0 0 600 400">
  <defs>
    <linearGradient id="g1" x1="0%" y1="0%" x2="100%" y2="100%">
      <stop offset="0%" style="stop-color:#6366f1;stop-opacity:1" />
      <stop offset="100%" style="stop-color:#a855f7;stop-opacity:1" />
    </linearGradient>
  </defs>
  <rect width="600" height="400" fill="url(#g1)" rx="12"/>
  <text x="300" y="210" font-family="sans-serif" font-size="28" fill="white"
        text-anchor="middle" dominant-baseline="middle">Image 1</text>
</svg>
```

- [ ] **Step 2: Create `images/image2.svg`**

```svg
<!-- images/image2.svg -->
<svg xmlns="http://www.w3.org/2000/svg" width="600" height="400" viewBox="0 0 600 400">
  <defs>
    <linearGradient id="g2" x1="0%" y1="0%" x2="100%" y2="100%">
      <stop offset="0%" style="stop-color:#06b6d4;stop-opacity:1" />
      <stop offset="100%" style="stop-color:#10b981;stop-opacity:1" />
    </linearGradient>
  </defs>
  <rect width="600" height="400" fill="url(#g2)" rx="12"/>
  <text x="300" y="210" font-family="sans-serif" font-size="28" fill="white"
        text-anchor="middle" dominant-baseline="middle">Image 2</text>
</svg>
```

---

### Task 2: Create `index.html`

**Files:**
- Create: `index.html`

- [ ] **Step 1: Write `index.html`**

```html
<!DOCTYPE html>
<html lang="zh-CN">
<head>
  <meta charset="UTF-8" />
  <meta name="viewport" content="width=device-width, initial-scale=1.0" />
  <title>Vercel Deploy Test</title>
  <style>
    *, *::before, *::after { box-sizing: border-box; margin: 0; padding: 0; }

    body {
      min-height: 100vh;
      display: flex;
      flex-direction: column;
      align-items: center;
      justify-content: center;
      background: #0f0f0f;
      color: #f5f5f5;
      font-family: system-ui, sans-serif;
      gap: 2rem;
      padding: 2rem;
    }

    h1 {
      font-size: clamp(2rem, 6vw, 4rem);
      font-weight: 700;
      letter-spacing: -0.02em;
    }

    .subtitle {
      color: #888;
      font-size: 1.1rem;
    }

    .clock {
      font-size: 1.4rem;
      font-variant-numeric: tabular-nums;
      color: #a3e635;
      letter-spacing: 0.05em;
    }

    .images {
      display: flex;
      gap: 1.5rem;
      flex-wrap: wrap;
      justify-content: center;
      width: 100%;
      max-width: 960px;
    }

    .images img {
      width: 45%;
      min-width: 260px;
      border-radius: 12px;
      border: 1px solid #2a2a2a;
    }
  </style>
</head>
<body>
  <h1>Hello, Vercel! 🚀</h1>
  <p class="subtitle">部署成功 — 静态资源加载测试</p>
  <p class="clock" id="clock"></p>
  <div class="images">
    <img src="images/image1.svg" alt="Image 1" />
    <img src="images/image2.svg" alt="Image 2" />
  </div>

  <script>
    function tick() {
      document.getElementById('clock').textContent = new Date().toLocaleTimeString('zh-CN');
    }
    tick();
    setInterval(tick, 1000);
  </script>
</body>
</html>
```

- [ ] **Step 2: Verify files exist**

```
website/
├── index.html
├── images/
│   ├── image1.svg
│   └── image2.svg
└── docs/...
```

- [ ] **Step 3: Open `index.html` in browser to verify locally**

Open file directly in browser. Confirm:
- Title shows "Hello, Vercel!"
- Clock ticks every second
- Both images render with gradient colors

---

## Deployment

- [ ] Go to [vercel.com](https://vercel.com) → New Project → drag the `website/` folder
- [ ] Confirm no build command is needed (Framework: Other)
- [ ] Deploy and verify the live URL shows the same result as local
