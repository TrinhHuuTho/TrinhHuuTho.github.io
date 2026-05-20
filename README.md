# Trinh Huu Tho — Personal Portfolio

> A modern, fast, and fully responsive personal portfolio — built with vanilla HTML/CSS/JS and Jekyll. No heavy frameworks, GitHub Pages compatible out of the box.

[![License](https://img.shields.io/github/license/TrinhHuuTho/TrinhHuuTho.github.io?style=flat-square)](LICENSE)
[![Last Commit](https://img.shields.io/github/last-commit/TrinhHuuTho/TrinhHuuTho.github.io?style=flat-square)](https://github.com/TrinhHuuTho/TrinhHuuTho.github.io/commits/main)
[![Repo Size](https://img.shields.io/github/repo-size/TrinhHuuTho/TrinhHuuTho.github.io?style=flat-square)](https://github.com/TrinhHuuTho/TrinhHuuTho.github.io)
[![Stars](https://img.shields.io/github/stars/TrinhHuuTho/TrinhHuuTho.github.io?style=flat-square)](https://github.com/TrinhHuuTho/TrinhHuuTho.github.io/stargazers)
[![Forks](https://img.shields.io/github/forks/TrinhHuuTho/TrinhHuuTho.github.io?style=flat-square)](https://github.com/TrinhHuuTho/TrinhHuuTho.github.io/network/members)
[![Issues](https://img.shields.io/github/issues/TrinhHuuTho/TrinhHuuTho.github.io?style=flat-square)](https://github.com/TrinhHuuTho/TrinhHuuTho.github.io/issues)
[![PRs Welcome](https://img.shields.io/badge/PRs-welcome-brightgreen?style=flat-square)](https://github.com/TrinhHuuTho/TrinhHuuTho.github.io/pulls)
[![GitHub Pages](https://img.shields.io/badge/GitHub%20Pages-deployed-brightgreen?style=flat-square&logo=github)](https://TrinhHuuTho.github.io)

---

## 📸 Preview

<!-- Replace the image below with a real screenshot (e.g. assets/images/preview.png) -->
<!-- Recommended: capture a full-page screenshot at 1440px width -->
<p align="center">
  <img src="assets/images/preview.png" alt="Portfolio preview — light and dark mode side by side" width="900" />
</p>

<!-- Optional: add a short screen-recording GIF (≤ 3 MB) showing dark/light toggle, project filter, and blog -->
<!-- <p align="center">
  <img src="assets/images/demo.gif" alt="Demo: dark/light toggle, project filter, blog navigation" width="900" />
</p> -->

<!-- Optional: link a YouTube / Loom video demo -->
<!-- [![Watch the demo](https://img.shields.io/badge/▶%20Watch%20Demo-YouTube-red?style=flat-square&logo=youtube)](https://youtu.be/YOUR_VIDEO_ID) -->

🔗 **Live site**: [https://TrinhHuuTho.github.io](https://TrinhHuuTho.github.io)

---

## 📑 Table of Contents

1. [Preview](#-preview)
2. [Features](#-features)
3. [Why this template?](#-why-this-template)
4. [Tech Stack](#-tech-stack)
5. [Quick Start](#-quick-start)
6. [Customization Guide](#️-customization-guide)
7. [Project Structure](#-project-structure)
8. [Contributing](#-contributing)
9. [Roadmap](#️-roadmap)
10. [License](#-license)

---

## ✨ Features

| Feature | Details |
|---------|---------|
| 🌗 **Dark / Light mode** | System preference detection + `localStorage` persistence |
| 📱 **Responsive design** | Mobile-first, adapts to all screen sizes |
| 🎞️ **Smooth animations** | Powered by Intersection Observer API |
| 🗂️ **Project filter** | Filter by LLM · NLP · Computer Vision · MLOps · Research |
| ✍️ **Blog** | Jekyll-powered — write in Markdown, published automatically |
| 💬 **Comments & Guestbook** | [Giscus](https://giscus.app) via GitHub Discussions |
| 📬 **Contact form** | [Formspree](https://formspree.io) — zero-backend |
| 🔧 **Centralized config** | All IDs in one file: `_data/giscus.yml` |
| 🔍 **SEO ready** | Open Graph, Twitter Card, `sitemap.xml`, `robots.txt` |
| 📡 **RSS feed** | Auto-generated blog feed |
| ♿ **Accessible** | Semantic HTML, ARIA attributes, keyboard navigation (WCAG 2.1 AA) |

---

## 💡 Why this template?

Most portfolio templates require Node.js, npm, or complex build pipelines. This one doesn't.

- **Zero build tools** — edit HTML/CSS/JS and push. Done.
- **No npm** — no `node_modules`, no version conflicts.
- **Jekyll only for the blog** — and even that is optional (GitHub Pages runs it automatically).
- **One config file** to set up Giscus comments and the contact form.
- **Fork-and-go** — personalize with a simple find-and-replace across 5 fields.

---

## 🛠️ Tech Stack

<p>
  <img src="https://img.shields.io/badge/HTML5-E34F26?style=flat-square&logo=html5&logoColor=white" alt="HTML5" />
  <img src="https://img.shields.io/badge/CSS3-1572B6?style=flat-square&logo=css3&logoColor=white" alt="CSS3" />
  <img src="https://img.shields.io/badge/JavaScript-F7DF1E?style=flat-square&logo=javascript&logoColor=black" alt="JavaScript" />
  <img src="https://img.shields.io/badge/Jekyll-CC0000?style=flat-square&logo=jekyll&logoColor=white" alt="Jekyll" />
  <img src="https://img.shields.io/badge/GitHub%20Pages-222222?style=flat-square&logo=github&logoColor=white" alt="GitHub Pages" />
  <img src="https://img.shields.io/badge/Giscus-1F6FEB?style=flat-square&logo=github&logoColor=white" alt="Giscus" />
  <img src="https://img.shields.io/badge/Formspree-E7532D?style=flat-square" alt="Formspree" />
  <img src="https://img.shields.io/badge/Highlight.js-F0DB4F?style=flat-square" alt="Highlight.js" />
</p>

---

## 🚀 Quick Start

### 1. Fork / Clone

```bash
git clone https://github.com/TrinhHuuTho/TrinhHuuTho.github.io.git
cd TrinhHuuTho.github.io
```

### 2. Run locally

```bash
# With Jekyll (recommended — renders blog posts correctly)
bundle install
bundle exec jekyll serve
# → Open http://localhost:4000

# Without Jekyll (static pages only; blog posts won't render)
python -m http.server 8000
# → Open http://localhost:8000
```

### 3. Deploy to GitHub Pages

1. Push to the `main` branch.
2. Go to **Settings → Pages → Source → Deploy from a branch → `main` → `/ (root)`**.
3. Your site goes live at `https://YOUR_USERNAME.github.io`.

---

## ⚙️ Customization Guide

### Personal Information

Find-and-replace these five values across all HTML files:

| Placeholder | Replace with |
|-------------|-------------|
| `Trinh Huu Tho` | Your full name |
| `TrinhHuuTho` | Your GitHub username |
| `trinhuutho@gmail.com` | Your email address |
| `linkedin.com/in/tho-trinh` | Your LinkedIn URL |
| `Thu Duc City, Vietnam` | Your location |

### Avatar Photo

In `index.html`, find `.avatar-img` and point to your image:

```html
<div class="avatar-img">
  <img src="assets/images/avatar.jpg" alt="Your Name" width="280" height="280" />
</div>
```

### Giscus (Comments & Guestbook)

Edit **`_data/giscus.yml`** — this is the only file you need to touch:

```yaml
repo: "YOUR_USERNAME/YOUR_USERNAME.github.io"
repo_id: "YOUR_REPO_ID"
blog_category: "Blog Comments"
blog_category_id: "YOUR_BLOG_CATEGORY_ID"
guestbook_category: "Guestbook"
guestbook_category_id: "YOUR_GUESTBOOK_CATEGORY_ID"
```

To get your IDs:
1. Enable **Discussions** in your GitHub repo settings.
2. Install the [Giscus GitHub App](https://github.com/apps/giscus).
3. Go to [giscus.app](https://giscus.app), enter your repo name, and copy the generated IDs.

### Contact Form (Formspree)

Also in `_data/giscus.yml`:

```yaml
formspree_id: "YOUR_FORMSPREE_ID"
```

Create a free form at [formspree.io](https://formspree.io) and paste the ID.

### Adding Blog Posts

Create `_posts/YYYY-MM-DD-your-title.md` with this front matter:

```yaml
---
layout: post
title: "Your Post Title"
date: YYYY-MM-DD
category: NLP
tags: [nlp, python]
excerpt: "Short description shown in the blog listing."
subtitle: "Longer subtitle shown in the post header."
read_time: 5
cover_image: "https://your-image-url.jpg"
---

Your content in Markdown here…
```

### Adding Projects

In `projects.html`, duplicate an `<article class="project-card">` block and update:
- `data-categories` — e.g. `"nlp"`, `"llm"`, `"cv"`
- Title, description, tech badges, GitHub/demo links

### Theme Colors

```css
/* css/style.css */
:root {
  --accent: #6c63ff;           /* Primary accent */
  --accent-secondary: #ff6584; /* Used in gradients */
}
```

---

## 📁 Project Structure

```
TrinhHuuTho.github.io/
├── _config.yml             # Jekyll config
├── _data/
│   └── giscus.yml          # ⭐ Central config: Giscus IDs + Formspree ID
├── _layouts/
│   ├── default.html        # Base layout (nav, footer)
│   └── post.html           # Blog post layout (with comments)
├── _posts/                 # Blog posts (Markdown)
├── index.html              # Homepage
├── about.html              # About page (timeline, skills, awards)
├── projects.html           # Projects grid with filter
├── blog/index.html         # Blog listing (search + tag filter)
├── guestbook.html          # Guestbook
├── contact.html            # Contact form
├── sitemap.xml             # SEO sitemap
├── robots.txt              # SEO robots
├── feed.xml                # RSS feed
├── css/style.css           # Main stylesheet
├── js/main.js              # All JS (dark mode, nav, filters, search, form)
└── assets/images/
    ├── avatar.svg          # Avatar
    ├── favicon.svg         # Favicon
    └── preview.png         # README preview screenshot (add yours here)
```

---

## 🤝 Contributing

Contributions, bug reports, and feature suggestions are welcome!

### Ways to contribute

- 🐛 **Report a bug** — open an [issue](https://github.com/TrinhHuuTho/TrinhHuuTho.github.io/issues/new?labels=bug)
- 💡 **Suggest a feature** — open an [issue](https://github.com/TrinhHuuTho/TrinhHuuTho.github.io/issues/new?labels=enhancement)
- 🔧 **Fix a bug / implement a feature** — open a PR (see below)
- 📝 **Improve docs** — typos, clarity, translations
- 🎨 **UI polish** — spacing, colors, animations

### Setup for development

```bash
git clone https://github.com/TrinhHuuTho/TrinhHuuTho.github.io.git
cd TrinhHuuTho.github.io
bundle install
bundle exec jekyll serve   # → http://localhost:4000
```

### Branch naming

| Type | Pattern | Example |
|------|---------|---------|
| Bug fix | `fix/short-description` | `fix/mobile-nav-overflow` |
| New feature | `feat/short-description` | `feat/project-search` |
| Docs | `docs/short-description` | `docs/update-readme` |
| Style / UI | `style/short-description` | `style/dark-mode-contrast` |

### Commit style

Follow [Conventional Commits](https://www.conventionalcommits.org/):

```
feat: add project search bar
fix: resolve mobile nav overflow on iOS
docs: update Giscus setup instructions
style: improve dark mode contrast for code blocks
```

### Pull request checklist

- [ ] Tested locally with `bundle exec jekyll serve`
- [ ] Works on mobile (check at 320px–375px width)
- [ ] Dark mode and light mode both look correct
- [ ] No broken links or console errors
- [ ] PR description explains *what* and *why*

---

## 🗺️ Roadmap

Ideas and improvements planned for the future — great starting points for contributors!

- [ ] **i18n support** — bilingual EN/VI content switcher
- [ ] **Project detail pages** — individual case-study pages per project
- [ ] **Reading progress bar** — visual scroll indicator on blog posts
- [ ] **Back-to-top button** — smooth scroll to top
- [ ] **Copy code button** — one-click copy on code blocks
- [ ] **Open Graph image generator** — auto-generate OG images for blog posts
- [ ] **Improved print stylesheet** — clean printable resume view
- [ ] **More project categories** — Data Engineering, MLOps, etc.

> Have an idea not listed here? [Open a feature request!](https://github.com/TrinhHuuTho/TrinhHuuTho.github.io/issues/new?labels=enhancement)

---

## 📄 License

Licensed under the [Apache License 2.0](LICENSE) — free to use for personal and commercial projects, modify, and distribute.
