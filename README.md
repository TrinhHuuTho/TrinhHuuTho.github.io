# 🤖 Trinh Huu Tho — AI Engineer Portfolio

A modern, fast, and fully responsive personal portfolio website for an AI/ML Engineer. Built with vanilla HTML, CSS, and JavaScript — no frameworks needed, GitHub Pages compatible out of the box.

🔗 **Live site**: [https://TrinhHuuTho.github.io](https://TrinhHuuTho.github.io)

---

## ✨ Features

- **Dark/Light mode** toggle with system preference detection and localStorage persistence
- **Responsive design** — mobile-first, works on all screen sizes
- **Smooth animations** via Intersection Observer API
- **Project filter** by category (LLM, NLP, Computer Vision, MLOps, Research)
- **Blog** with search, tag filters, and syntax-highlighted code snippets (Highlight.js)
- **Guestbook** powered by [Giscus](https://giscus.app) (GitHub Discussions)
- **Contact form** powered by [Formspree](https://formspree.io)
- **SEO ready** — meta tags, Open Graph, Twitter Card, sitemap.xml, robots.txt
- **RSS Feed** for blog posts
- **Accessibility** — semantic HTML, ARIA attributes, keyboard navigation

---

## 📁 Project Structure

```
TrinhHuuTho.github.io/
├── index.html              # Homepage (hero, about, projects, blog, contact previews)
├── about.html              # Full about page with timeline & certifications
├── projects.html           # Projects grid with category filter
├── guestbook.html          # Guestbook with Giscus integration
├── contact.html            # Contact form (Formspree)
├── sitemap.xml             # SEO sitemap
├── robots.txt              # SEO robots file
├── feed.xml                # RSS feed for blog
├── blog/
│   ├── index.html          # Blog listing with search & tag filter
│   ├── post-1-building-llm-chatbot.html
│   ├── post-2-computer-vision-yolov8.html
│   └── post-3-rag-pipeline.html
├── css/
│   └── style.css           # Main stylesheet (dark mode, responsive, animations)
├── js/
│   └── main.js             # Dark mode, nav, animations, filters, search, form
└── assets/
    └── images/
        └── favicon.svg     # SVG favicon
```

---

## 🚀 Quick Start

### 1. Fork / Clone

```bash
git clone https://github.com/TrinhHuuTho/TrinhHuuTho.github.io.git
cd TrinhHuuTho.github.io
```

### 2. Open locally

```bash
# Option A: Python built-in server
python -m http.server 8000
# Then open http://localhost:8000

# Option B: Node.js live-server
npx live-server
```

### 3. Deploy to GitHub Pages

1. Push to the `main` branch
2. Go to **Settings → Pages → Source → Deploy from a branch → main → / (root)**
3. Your site will be live at `https://YOUR_USERNAME.github.io`

---

## ⚙️ Customization Guide

### Personal Information

Search and replace these placeholders across all HTML files:

| Placeholder | Replace with |
|-------------|-------------|
| `Trinh Huu Tho` | Your full name |
| `TrinhHuuTho` | Your GitHub username |
| `trinhuutho@email.com` | Your email address |
| `linkedin.com/in/trinhuutho` | Your LinkedIn profile |
| `twitter.com/trinhuutho` | Your Twitter/X handle |
| `Ho Chi Minh City, Vietnam` | Your location |

### Avatar Photo

Replace the placeholder initials in `index.html` with a real `<img>` tag inside `.avatar-img`:

```html
<!-- In index.html, find .avatar-img and replace the placeholder div: -->
<div class="avatar-img">
  <img src="assets/images/avatar.jpg" alt="Trinh Huu Tho" width="280" height="280" />
</div>
```

### Setting up Giscus (Guestbook + Blog Comments)

1. Enable **Discussions** in your GitHub repo settings
2. Install the [Giscus GitHub App](https://github.com/apps/giscus) on your repo
3. Go to [giscus.app](https://giscus.app) and fill in your repo info
4. Copy the generated `data-repo-id` and `data-category-id`
5. Replace `YOUR_REPO_ID` and `YOUR_CATEGORY_ID` in:
   - `guestbook.html`
   - `blog/post-1-building-llm-chatbot.html`
   - `blog/post-2-computer-vision-yolov8.html`
   - `blog/post-3-rag-pipeline.html`

### Setting up Contact Form (Formspree)

1. Create a free account at [formspree.io](https://formspree.io)
2. Create a new form and copy the form ID
3. In `contact.html`, replace `YOUR_FORMSPREE_ID` in the `<form action="...">` attribute

### Adding New Blog Posts

1. Copy an existing blog post file in `blog/`
2. Update the content, meta tags, title, date, and read-time
3. Add a card for the new post in `blog/index.html`
4. Add a preview card in `index.html` (Latest Articles section)
5. Add the URL to `sitemap.xml` and an `<item>` to `feed.xml`

### Adding New Projects

In `projects.html`, copy a project card `<article>` and update:
- `data-categories` attribute (for filter functionality)
- Project title, description, tech stack, and links

### Dark Mode Customization

All colors are CSS custom properties in `css/style.css`:

```css
:root {
  --accent: #6c63ff;        /* Primary accent color */
  --accent-secondary: #ff6584; /* Secondary accent (gradients) */
  /* ... */
}
```

---

## 🔧 Technologies Used

- **HTML5** — Semantic markup
- **CSS3** — Custom properties, CSS Grid, Flexbox, animations
- **Vanilla JavaScript** — No dependencies (except CDN libs for code highlighting)
- **[Giscus](https://giscus.app)** — Comments & guestbook via GitHub Discussions
- **[Formspree](https://formspree.io)** — Contact form backend
- **[Highlight.js](https://highlightjs.org)** — Code syntax highlighting in blog posts
- **[Google Fonts](https://fonts.google.com)** — Inter + Fira Code

---

## 📊 Performance

- ⚡ Pure HTML/CSS/JS — no heavy frameworks
- 🖼️ SVG favicon — tiny file size
- 📦 No npm dependencies to install
- 🌐 Hosted on GitHub Pages CDN
- ♿ WCAG 2.1 AA accessible

---

## 📄 License

MIT License — feel free to use this as a template for your own portfolio!