# Personal Blog Implementation Plan

> **For agentic workers:** REQUIRED: Use superpowers:subagent-driven-development (if subagents available) or superpowers:executing-plans to implement this plan. Steps use checkbox (`- [ ]`) syntax for tracking.

**Goal:** Build a warm-styled personal blog with Astro (blog + about page), replacing the existing test site, deployable to Vercel.

**Architecture:** Scaffold Astro with the official blog template, replace all default styling with the warm color system, customize pages for our layout (index = blog list, /about = bio + social, /blog/[slug] = post detail). Content is Markdown files in `src/content/blog/`.

**Tech Stack:** Astro 4+, Markdown (Content Collections), custom CSS, Node 18+, Vercel

---

## Chunk 1: Scaffold & Configure

### Task 1: Initialize Astro project

**Files:**
- Create: All Astro scaffold files in `/Users/april/Documents/ClaudeCode/website/`

- [ ] **Step 1: Remove old test site files**

```bash
cd /Users/april/Documents/ClaudeCode/website
rm -f index.html
rm -rf images
```

- [ ] **Step 2: Scaffold Astro 4 blog template in-place**

```bash
cd /Users/april/Documents/ClaudeCode/website
npm create astro@4 . -- --template blog --no-git --skip-houston
```

When prompted "Initialize a new git repository?" → No
When prompted about install → Yes (install dependencies)

> We pin `astro@4` to use `src/content/config.ts`. Astro 5 moves the config to `src/content.config.ts` (root level) — if you see that layout after scaffolding, update the path in Task 3 accordingly.

- [ ] **Step 3: Verify build passes**

```bash
cd /Users/april/Documents/ClaudeCode/website
npm run build
```

Expected: `dist/` directory created, no errors.

- [ ] **Step 4: Verify dev server starts**

```bash
npm run dev
```

Expected: `http://localhost:4321` opens, shows Astro blog template.

---

### Task 2: Configure site metadata and consts

**Files:**
- Modify: `src/consts.ts`

- [ ] **Step 1: Update `src/consts.ts`**

```typescript
export const SITE_TITLE = "My Blog";
export const SITE_DESCRIPTION = "写字·记录·思考";
export const SOCIAL_LINKS = {
  github: "https://github.com/yourusername",
  twitter: "https://twitter.com/yourusername",
};
```

> User should replace `yourusername` with their real handles before deploying.

---

### Task 3: Update content collection schema to use `date` field

**Files:**
- Modify: `src/content/config.ts`

- [ ] **Step 1: Update schema to use `date` instead of `pubDate`**

```typescript
import { defineCollection, z } from 'astro:content';

const blog = defineCollection({
  type: 'content',
  schema: z.object({
    title: z.string(),
    description: z.string(),
    date: z.coerce.date(),
    updatedDate: z.coerce.date().optional(),
  }),
});

export const collections = { blog };
```

- [ ] **Step 2: Verify build still passes**

```bash
npm run build
```

Expected: no errors (existing sample posts may fail until updated in Task 9 — that's fine, we'll fix them there).

---

## Chunk 2: Design System

### Task 4: Replace global.css with warm palette

**Files:**
- Modify: `src/styles/global.css`

- [ ] **Step 1: Replace entire `src/styles/global.css` with warm design system**

```css
/* ── Design tokens ── */
:root {
  --bg: #fffbf5;
  --bg-subtle: #f5ede0;
  --border: #ede8df;
  --text-heading: #1a1008;
  --text-body: #4a3828;
  --text-muted: #a08060;
  --accent: #c07840;
  --accent-hover: #9a5e2c;
  --font-serif: Georgia, 'Times New Roman', serif;
  --font-sans: system-ui, -apple-system, sans-serif;
  --max-width: 680px;
}

/* ── Reset ── */
*, *::before, *::after { box-sizing: border-box; margin: 0; padding: 0; }

/* ── Base ── */
html { font-size: 17px; }

body {
  background: var(--bg);
  color: var(--text-body);
  font-family: var(--font-serif);
  line-height: 1.75;
  min-height: 100vh;
  display: flex;
  flex-direction: column;
}

/* ── Typography ── */
h1, h2, h3, h4, h5, h6 {
  font-family: var(--font-serif);
  color: var(--text-heading);
  line-height: 1.3;
  margin-top: 2rem;
  margin-bottom: 0.5rem;
}
h1 { font-size: 2rem; }
h2 { font-size: 1.5rem; }
h3 { font-size: 1.2rem; }

a {
  color: var(--accent);
  text-decoration: none;
}
a:hover { color: var(--accent-hover); text-decoration: underline; }

p { margin-bottom: 1.2rem; }
p:last-child { margin-bottom: 0; }

/* ── Layout wrapper ── */
.wrapper {
  max-width: var(--max-width);
  margin: 0 auto;
  padding: 0 1.5rem;
  width: 100%;
}

/* ── Main content ── */
main {
  flex: 1;
  padding: 3rem 0;
}

/* ── Markdown prose styles ── */
.prose h1, .prose h2, .prose h3,
.prose h4, .prose h5, .prose h6 {
  margin-top: 2.5rem;
}

.prose p { margin-bottom: 1.4rem; }

.prose ul, .prose ol {
  padding-left: 1.5rem;
  margin-bottom: 1.4rem;
}
.prose li { margin-bottom: 0.3rem; }

.prose blockquote {
  border-left: 3px solid var(--accent);
  margin: 1.5rem 0;
  padding: 0.5rem 0 0.5rem 1.25rem;
  color: var(--text-muted);
  font-style: italic;
}

.prose code {
  background: var(--bg-subtle);
  border: 1px solid var(--border);
  border-radius: 4px;
  font-size: 0.88em;
  padding: 0.15em 0.4em;
  font-family: ui-monospace, 'Cascadia Code', monospace;
  color: var(--text-heading);
}

.prose pre {
  background: #2d1f10;
  border-radius: 8px;
  padding: 1.25rem 1.5rem;
  overflow-x: auto;
  margin: 1.5rem 0;
}
.prose pre code {
  background: none;
  border: none;
  padding: 0;
  color: #f0e0c8;
  font-size: 0.875rem;
}

/* ── Images in prose ── */
.prose img {
  max-width: 100%;
  height: auto;
  border-radius: 8px;
  margin: 1.5rem 0;
  border: 1px solid var(--border);
  display: block;
}

/* ── Tables in prose ── */
.prose table {
  width: 100%;
  border-collapse: collapse;
  margin: 1.5rem 0;
  font-size: 0.95rem;
}
.prose th {
  background: var(--bg-subtle);
  border: 1px solid var(--border);
  padding: 0.6rem 0.9rem;
  text-align: left;
  font-family: var(--font-sans);
  font-size: 0.8rem;
  letter-spacing: 0.04em;
  text-transform: uppercase;
  color: var(--text-muted);
}
.prose td {
  border: 1px solid var(--border);
  padding: 0.6rem 0.9rem;
}
.prose tr:nth-child(even) td { background: #fdf8f1; }

/* ── Divider ── */
hr {
  border: none;
  border-top: 1px solid var(--border);
  margin: 2.5rem 0;
}
```

- [ ] **Step 2: Verify dev server renders warm background**

```bash
npm run dev
```

Expected: page background is `#fffbf5` (warm off-white).

---

## Chunk 3: Layout & Navigation

### Task 5: Rewrite Header component with warm nav

**Files:**
- Modify: `src/components/Header.astro`

- [ ] **Step 1: Replace `src/components/Header.astro`**

```astro
---
import { SITE_TITLE } from '../consts';

const { pathname } = Astro.url;
const isAbout = pathname.startsWith('/about');
---

<header>
  <nav class="wrapper">
    <a href="/" class="site-title">{SITE_TITLE}</a>
    <div class="nav-links">
      <a href="/" class:list={[{ active: !isAbout }]}>博客</a>
      <a href="/about" class:list={[{ active: isAbout }]}>关于我</a>
    </div>
  </nav>
</header>

<style>
  header {
    background: var(--bg);
    border-bottom: 1px solid var(--border);
    padding: 1rem 0;
    position: sticky;
    top: 0;
    z-index: 10;
  }

  nav {
    display: flex;
    align-items: center;
    justify-content: space-between;
  }

  .site-title {
    font-family: var(--font-serif);
    font-size: 1.1rem;
    font-weight: 700;
    color: var(--text-heading);
    text-decoration: none;
  }
  .site-title:hover { color: var(--accent); text-decoration: none; }

  .nav-links {
    display: flex;
    gap: 1.5rem;
    font-family: var(--font-sans);
    font-size: 0.875rem;
  }

  .nav-links a {
    color: var(--text-muted);
    text-decoration: none;
    letter-spacing: 0.02em;
    transition: color 0.15s;
  }
  .nav-links a:hover,
  .nav-links a.active {
    color: var(--accent);
    text-decoration: none;
  }
</style>
```

---

### Task 6: Rewrite Footer component

**Files:**
- Modify: `src/components/Footer.astro`

- [ ] **Step 1: Replace `src/components/Footer.astro`**

```astro
---
import { SITE_TITLE } from '../consts';

const year = new Date().getFullYear();
---

<footer>
  <div class="wrapper">
    <span>© {year} {SITE_TITLE}</span>
  </div>
</footer>

<style>
  footer {
    border-top: 1px solid var(--border);
    padding: 1.5rem 0;
    font-family: var(--font-sans);
    font-size: 0.8rem;
    color: var(--text-muted);
  }
</style>
```

---

### Task 7: Rewrite BaseLayout (formerly BlogPost layout)

**Files:**
- Modify: `src/layouts/BlogPost.astro`

- [ ] **Step 1: Replace `src/layouts/BlogPost.astro`**

```astro
---
import type { CollectionEntry } from 'astro:content';
import BaseHead from '../components/BaseHead.astro';
import Header from '../components/Header.astro';
import Footer from '../components/Footer.astro';
import { SITE_TITLE, SITE_DESCRIPTION } from '../consts';

type Props = CollectionEntry<'blog'>['data'] & { title?: string; description?: string };

const { title, description, date } = Astro.props;

const formattedDate = date
  ? date.toLocaleDateString('zh-CN', { year: 'numeric', month: 'long', day: 'numeric' })
  : null;
---

<!doctype html>
<html lang="zh-CN">
  <head>
    <BaseHead title={title ?? SITE_TITLE} description={description ?? SITE_DESCRIPTION} />
  </head>
  <body>
    <Header />
    <main>
      <div class="wrapper">
        {formattedDate && (
          <div class="post-meta">
            <time>{formattedDate}</time>
          </div>
        )}
        <article class="prose">
          <slot />
        </article>
        <div class="back-link">
          <a href="/">← 返回博客</a>
        </div>
      </div>
    </main>
    <Footer />
  </body>
</html>

<style>
  .post-meta {
    font-family: var(--font-sans);
    font-size: 0.8rem;
    color: var(--accent);
    letter-spacing: 0.06em;
    text-transform: uppercase;
    margin-bottom: 0.75rem;
  }

  .prose :global(h1) {
    font-size: 2rem;
    margin-top: 0;
    margin-bottom: 1.5rem;
    color: var(--text-heading);
  }

  .back-link {
    margin-top: 3rem;
    padding-top: 1.5rem;
    border-top: 1px solid var(--border);
    font-family: var(--font-sans);
    font-size: 0.875rem;
  }
  .back-link a { color: var(--text-muted); }
  .back-link a:hover { color: var(--accent); text-decoration: none; }
</style>
```

- [ ] **Step 2: Verify build**

```bash
npm run build
```

Expected: no errors.

---

## Chunk 4: Blog Index Page

### Task 8: Rewrite blog index page

**Files:**
- Modify: `src/pages/index.astro`

- [ ] **Step 1: Replace `src/pages/index.astro`**

```astro
---
import BaseHead from '../components/BaseHead.astro';
import Header from '../components/Header.astro';
import Footer from '../components/Footer.astro';
import { SITE_TITLE, SITE_DESCRIPTION } from '../consts';
import { getCollection } from 'astro:content';

const posts = (await getCollection('blog')).sort(
  (a, b) => b.data.date.valueOf() - a.data.date.valueOf()
);
---

<!doctype html>
<html lang="zh-CN">
  <head>
    <BaseHead title={SITE_TITLE} description={SITE_DESCRIPTION} />
  </head>
  <body>
    <Header />
    <main>
      <div class="wrapper">
        <div class="site-header">
          <h1>{SITE_TITLE}</h1>
          <p class="tagline">{SITE_DESCRIPTION}</p>
        </div>
        <ul class="post-list">
          {posts.map((post) => {
            const date = post.data.date.toLocaleDateString('zh-CN', {
              year: 'numeric', month: 'long', day: 'numeric'
            });
            return (
              <li>
                <a href={`/blog/${post.slug}/`} class="post-card">
                  <div class="post-date">{date}</div>
                  <div class="post-title">{post.data.title}</div>
                  {post.data.description && (
                    <div class="post-desc">{post.data.description}</div>
                  )}
                  <div class="post-read">阅读全文 →</div>
                </a>
              </li>
            );
          })}
        </ul>
        {posts.length === 0 && (
          <p class="empty">还没有文章，快去 <code>src/content/blog/</code> 新建一篇吧。</p>
        )}
      </div>
    </main>
    <Footer />
  </body>
</html>

<style>
  .site-header {
    padding: 2.5rem 0 2rem;
    border-bottom: 1px solid var(--border);
    margin-bottom: 2rem;
  }
  .site-header h1 { margin-top: 0; font-size: 1.75rem; }
  .tagline {
    color: var(--text-muted);
    font-family: var(--font-sans);
    font-size: 0.95rem;
    margin: 0.25rem 0 0;
  }

  .post-list {
    list-style: none;
    display: flex;
    flex-direction: column;
    gap: 0;
  }

  .post-card {
    display: block;
    padding: 1.5rem 0;
    border-bottom: 1px solid var(--border);
    text-decoration: none;
    transition: background 0.1s;
  }
  .post-card:hover { text-decoration: none; }
  .post-card:hover .post-title { color: var(--accent); }

  .post-date {
    font-family: var(--font-sans);
    font-size: 0.75rem;
    color: var(--accent);
    letter-spacing: 0.06em;
    text-transform: uppercase;
    margin-bottom: 0.35rem;
  }

  .post-title {
    font-family: var(--font-serif);
    font-size: 1.2rem;
    font-weight: 700;
    color: var(--text-heading);
    margin-bottom: 0.4rem;
    transition: color 0.15s;
  }

  .post-desc {
    color: var(--text-muted);
    font-size: 0.95rem;
    line-height: 1.6;
    margin-bottom: 0.5rem;
  }

  .post-read {
    font-family: var(--font-sans);
    font-size: 0.8rem;
    color: var(--text-muted);
  }

  .empty {
    color: var(--text-muted);
    font-family: var(--font-sans);
    font-size: 0.95rem;
    padding: 2rem 0;
  }
</style>
```

- [ ] **Step 2: Delete the now-redundant `src/pages/blog/index.astro` if it exists**

```bash
rm -f /Users/april/Documents/ClaudeCode/website/src/pages/blog/index.astro
```

- [ ] **Step 3: Verify dev server**

```bash
npm run dev
```

Expected: `http://localhost:4321` shows warm post list.

---

## Chunk 5: Post Detail Page

### Task 9: Rewrite post detail dynamic route

**Files:**
- Modify: `src/pages/blog/[...slug].astro`

- [ ] **Step 1: Replace `src/pages/blog/[...slug].astro`**

```astro
---
import { getCollection } from 'astro:content';
import BlogPost from '../../layouts/BlogPost.astro';

export async function getStaticPaths() {
  const posts = await getCollection('blog');
  return posts.map((post) => ({
    params: { slug: post.slug },
    props: post,
  }));
}

type Props = Awaited<ReturnType<typeof getCollection<'blog'>>>[number];

const post = Astro.props;
const { Content } = await post.render();
---

<BlogPost {...post.data}>
  <h1>{post.data.title}</h1>
  <Content />
</BlogPost>
```

---

## Chunk 6: About Page

### Task 10: Rewrite About page

**Files:**
- Modify: `src/pages/about.astro`

- [ ] **Step 1: Replace `src/pages/about.astro`**

```astro
---
import BaseHead from '../components/BaseHead.astro';
import Header from '../components/Header.astro';
import Footer from '../components/Footer.astro';
import { SITE_TITLE, SOCIAL_LINKS } from '../consts';
---

<!doctype html>
<html lang="zh-CN">
  <head>
    <BaseHead title={`关于我 · ${SITE_TITLE}`} description="个人介绍" />
  </head>
  <body>
    <Header />
    <main>
      <div class="wrapper">
        <div class="about">
          <div class="avatar-wrap">
            <img src="/avatar.jpg" alt="头像" class="avatar" />
          </div>
          <h1>关于我</h1>
          <div class="bio prose">
            <p>
              你好，我是 <strong>Your Name</strong>。在这里我写一些关于
              <!-- 在这里修改你的自我介绍 -->
              技术、生活和思考的文字。
            </p>
            <p>
              这个博客是我记录想法、分享观察的地方。没有固定主题，
              只要是让我觉得值得写下来的，都会出现在这里。
            </p>
          </div>
          <div class="social">
            {SOCIAL_LINKS.github && (
              <a href={SOCIAL_LINKS.github} target="_blank" rel="noopener">
                GitHub
              </a>
            )}
            {SOCIAL_LINKS.twitter && (
              <a href={SOCIAL_LINKS.twitter} target="_blank" rel="noopener">
                Twitter / X
              </a>
            )}
          </div>
        </div>
      </div>
    </main>
    <Footer />
  </body>
</html>

<style>
  .about {
    max-width: 560px;
    padding: 2.5rem 0;
  }

  .avatar-wrap {
    margin-bottom: 1.5rem;
  }

  .avatar {
    width: 88px;
    height: 88px;
    border-radius: 50%;
    object-fit: cover;
    border: 3px solid var(--border);
    display: block;
  }

  h1 {
    margin-top: 0;
    font-size: 1.75rem;
    margin-bottom: 1rem;
  }

  .bio {
    margin-bottom: 2rem;
  }

  .social {
    display: flex;
    gap: 1.25rem;
    font-family: var(--font-sans);
    font-size: 0.9rem;
  }

  .social a {
    color: var(--text-muted);
    border-bottom: 1px solid var(--border);
    padding-bottom: 1px;
    text-decoration: none;
  }
  .social a:hover {
    color: var(--accent);
    border-color: var(--accent);
    text-decoration: none;
  }
</style>
```

- [ ] **Step 2: Create SVG avatar placeholder so About page renders without a broken image**

Write this content to `/Users/april/Documents/ClaudeCode/website/public/avatar.svg`:

```svg
<svg xmlns="http://www.w3.org/2000/svg" width="200" height="200" viewBox="0 0 200 200">
  <circle cx="100" cy="100" r="100" fill="#e8d5b7"/>
  <circle cx="100" cy="82" r="32" fill="#c4a882"/>
  <ellipse cx="100" cy="160" rx="52" ry="40" fill="#c4a882"/>
</svg>
```

- [ ] **Step 3: Update `about.astro` to reference the SVG placeholder**

In `src/pages/about.astro`, change:
```
<img src="/avatar.jpg" alt="头像" class="avatar" />
```
to:
```
<img src="/avatar.svg" alt="头像" class="avatar" />
```

> To use a real photo later: drop your image as `public/avatar.jpg` (or any format) and update this src attribute.

---

## Chunk 7: Sample Content & Final Verification

### Task 11: Write two sample blog posts

**Files:**
- Delete: existing template posts in `src/content/blog/`
- Create: `src/content/blog/hello-world.md`
- Create: `src/content/blog/on-writing.md`

- [ ] **Step 1: Remove template posts**

```bash
rm -f /Users/april/Documents/ClaudeCode/website/src/content/blog/*.md
```

- [ ] **Step 2: Create `src/content/blog/hello-world.md`**

```markdown
---
title: "Hello, World"
date: 2026-03-01
description: "第一篇文章，说说为什么开始写博客，以及这里会写些什么。"
---

## 为什么开始写

很长一段时间里，我一直把想法存在备忘录里，零散而无序。有一天翻出三年前写的一段话，发现自己已经完全不记得当时的语境了——那段文字像是别人写的。

那一刻我意识到，写作不只是记录，更是一种整理思维的方式。把模糊的感受变成具体的句子，需要真正想清楚才能写清楚。

所以我决定开始写博客。

## 会写什么

没有固定主题。可能是读到某本书的感想，某个技术问题的解法，或者只是某天下午的一点观察。

不追求系统，不追求完整，只要觉得值得写下来就写。

---

> 路虽远，行则将至。
```

- [ ] **Step 3: Create `src/content/blog/on-writing.md`**

```markdown
---
title: "关于写作这件事"
date: 2026-03-10
description: "一些关于如何写得清楚、写得真实的想法。"
---

写作最难的不是文采，而是诚实。

## 清楚比优美更重要

读过很多文章，最让人读不下去的不是文字普通，而是不知道作者想说什么。一段话转了三个弯，最后也没有落点。

清楚意味着：你知道这句话的主语是谁，动作是什么，为什么重要。

## 一个有用的检查方法

写完一段之后，问自己：如果把这段删掉，读者会失去什么？

如果答案是「什么都不会失去」，那就删掉。

## 关于风格

风格不是刻意为之的东西。你读了足够多，写了足够多，自然会有。

不必模仿谁的语气，不必追求「有文学感」。把话说清楚，本身就是一种风格。

---

| 好的写作 | 不好的写作 |
|---|---|
| 说具体的事 | 堆砌抽象概念 |
| 一句话一个重点 | 一段话讲五件事 |
| 删掉能删的 | 保留一切「感觉有用」的 |
```

> Note: The second post includes a table to demonstrate table rendering.

- [ ] **Step 4: Create `public/images/` directory**

```bash
mkdir -p /Users/april/Documents/ClaudeCode/website/public/images
```

Create a README placeholder so the directory is tracked:

```bash
echo "# Post images\nDrop images here and reference them in markdown as ![alt](/images/filename.jpg)" > /Users/april/Documents/ClaudeCode/website/public/images/README.md
```

---

### Task 12: Final build verification

- [ ] **Step 1: Run full build**

```bash
cd /Users/april/Documents/ClaudeCode/website
npm run build
```

Expected: `dist/` built with no errors. Output should include:
- `dist/index.html`
- `dist/about/index.html`
- `dist/blog/hello-world/index.html`
- `dist/blog/on-writing/index.html`

- [ ] **Step 2: Preview the built site**

```bash
npm run preview
```

Open `http://localhost:4321`. Verify:
- Home page shows 2 posts in warm style
- Clicking a post loads the detail page with proper formatting
- The "on-writing" post shows a rendered table
- `/about` page shows avatar, bio, and social links
- Navigation highlights active page

- [ ] **Step 3: Verify Vercel auto-detection**

Check `package.json` has the standard Astro scripts:

```json
{
  "scripts": {
    "dev": "astro dev",
    "build": "astro build",
    "preview": "astro preview"
  }
}
```

Vercel will automatically detect Astro and run `npm run build` with output dir `dist/`.

---

## Deployment

After all tasks pass:

1. Push to GitHub (or drag `website/` folder to Vercel dashboard)
2. Vercel detects Astro → sets Build Command: `npm run build`, Output: `dist/`
3. Live URL is ready

## Adding New Posts Later

```bash
# Create a new post
cat > src/content/blog/my-new-post.md << 'EOF'
---
title: "文章标题"
date: 2026-03-15
description: "一句话摘要"
---

正文...
EOF

git add . && git commit -m "post: add my-new-post"
git push  # Vercel auto-deploys
```
