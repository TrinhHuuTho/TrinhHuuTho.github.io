# Trinh Huu Tho — Personal Portfolio

A modern, fast, and fully responsive personal portfolio website. Built with vanilla HTML/CSS/JS and Jekyll — no heavy frameworks, GitHub Pages compatible out of the box.

🔗 **Live site**: [https://TrinhHuuTho.github.io](https://TrinhHuuTho.github.io)

---

## ✨ Features

- **Dark/Light mode** toggle with system preference detection and localStorage persistence
- **Responsive design** — mobile-first, works on all screen sizes
- **Smooth animations** via Intersection Observer API
- **Project filter** by category (LLM, NLP, Computer Vision, MLOps, Research)
- **Blog** powered by Jekyll — write posts in Markdown, published automatically
- **Guestbook & Blog Comments** powered by [Giscus](https://giscus.app) (GitHub Discussions)
- **Contact form** powered by [Formspree](https://formspree.io)
- **Centralized config** — Giscus IDs and Formspree ID stored in `_data/giscus.yml`
- **SEO ready** — meta tags, Open Graph, Twitter Card, sitemap.xml, robots.txt
- **RSS Feed** for blog posts
- **Accessibility** — semantic HTML, ARIA attributes, keyboard navigation

---

## 📁 Project Structure

```
TrinhHuuTho.github.io/
├── _config.yml             # Jekyll configuration (title, URL, permalink, etc.)
├── _data/
│   └── giscus.yml          # Centralized config: Giscus IDs + Formspree ID
├── _layouts/
│   ├── default.html        # Base layout (nav, footer)
│   └── post.html           # Blog post layout (with Giscus comments)
├── _posts/                 # Blog posts (Markdown, Jekyll-processed)
│   └── YYYY-MM-DD-title.md
├── index.html              # Homepage (hero, about, projects, blog preview)
├── about.html              # Full about page (timeline, skills, awards)
├── projects.html           # Projects grid with category filter
├── blog/
│   └── index.html          # Blog listing with search & tag filter
├── guestbook.html          # Guestbook with Giscus integration
├── contact.html            # Contact form (Formspree)
├── sitemap.xml             # SEO sitemap
├── robots.txt              # SEO robots file
├── feed.xml                # RSS feed for blog
├── css/
│   └── style.css           # Main stylesheet (dark mode, responsive, animations)
├── js/
│   └── main.js             # Dark mode, nav, animations, filters, search, form
└── assets/
    └── images/
        ├── avatar.svg      # Avatar image
        └── favicon.svg     # SVG favicon
```

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
# Open http://localhost:4000

# Without Jekyll (static pages only, blog posts won't render)
python -m http.server 8000
# Open http://localhost:8000
```

### 3. Deploy to GitHub Pages

1. Push to the `main` branch
2. Go to **Settings → Pages → Source → Deploy from a branch → main → / (root)**
3. Your site will be live at `https://YOUR_USERNAME.github.io`

---

## ⚙️ Customization Guide

### Personal Information

Search and replace across all HTML files:

| Placeholder | Replace with |
|-------------|-------------|
| `Trinh Huu Tho` | Your full name |
| `TrinhHuuTho` | Your GitHub username |
| `trinhuutho@gmail.com` | Your email address |
| `linkedin.com/in/tho-trinh` | Your LinkedIn profile URL |
| `Thu Duc City, Vietnam` | Your location |

### Avatar Photo

In `index.html`, find `.avatar-img` and replace the placeholder with a real image:

```html
<div class="avatar-img">
  <img src="assets/images/avatar.jpg" alt="Your Name" width="280" height="280" />
</div>
```

### Setting up Giscus (Guestbook + Blog Comments)

All Giscus IDs are centralized in **`_data/giscus.yml`** — edit only this one file:

```yaml
repo: "YOUR_USERNAME/YOUR_USERNAME.github.io"
repo_id: "YOUR_REPO_ID"

blog_category: "Blog Comments"
blog_category_id: "YOUR_BLOG_CATEGORY_ID"

guestbook_category: "Guestbook"
guestbook_category_id: "YOUR_GUESTBOOK_CATEGORY_ID"
```

To get your IDs:
1. Enable **Discussions** in your GitHub repo settings
2. Install the [Giscus GitHub App](https://github.com/apps/giscus)
3. Go to [giscus.app](https://giscus.app), enter your repo, and copy the generated IDs

### Setting up Contact Form (Formspree)

The Formspree form ID is also in **`_data/giscus.yml`**:

```yaml
formspree_id: "YOUR_FORMSPREE_ID"
```

To get your ID:
1. Create a free account at [formspree.io](https://formspree.io)
2. Create a new form and copy the form ID (e.g. `abcdefgh`)

### Adding New Blog Posts

Create a new Markdown file in `_posts/` following the naming convention:

```
_posts/YYYY-MM-DD-your-post-title.md
```

Minimal front matter:

```yaml
---
layout: post
title: "Your Post Title"
date: YYYY-MM-DD
category: NLP
tags: [nlp, python]
excerpt: "Short description shown in blog listing."
subtitle: "Longer subtitle shown in post header."
read_time: 5
cover_image: "https://your-image-url.jpg"
---

Your post content in Markdown here...
```

Jekyll will automatically include it in the blog listing and RSS feed.

### Adding New Projects

In `projects.html`, copy an existing `<article class="project-card">` block and update:
- `data-categories` — for the filter buttons (e.g. `"nlp"`, `"llm"`, `"cv"`)
- Project title, description, tech badges, and GitHub/demo links

### Dark Mode Colors

All theme colors are CSS custom properties in `css/style.css`:

```css
:root {
  --accent: #6c63ff;           /* Primary accent color */
  --accent-secondary: #ff6584; /* Secondary accent (used in gradients) */
}
```

---

## 🔧 Technologies Used

- **HTML5** — Semantic markup
- **CSS3** — Custom properties, Grid, Flexbox, animations
- **Vanilla JavaScript** — No build tools or npm required
- **[Jekyll](https://jekyllrb.com)** — Static site generator for blog posts
- **[Giscus](https://giscus.app)** — Comments & guestbook via GitHub Discussions
- **[Formspree](https://formspree.io)** — Contact form backend
- **[Highlight.js](https://highlightjs.org)** — Code syntax highlighting
- **[Google Fonts](https://fonts.google.com)** — Inter + Fira Code

---

## 📊 Performance

- ⚡ Pure HTML/CSS/JS — no heavy frameworks
- 🖼️ SVG favicon — tiny file size
- 📦 No npm dependencies
- 🌐 Hosted on GitHub Pages CDN
- ♿ WCAG 2.1 AA accessible

---

## 📄 License

This project is licensed under the [Apache License 2.0](LICENSE). 
This means you are completely free to use it for commercial projects, 
modify it, and distribute it without private restrictions.
