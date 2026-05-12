# My Blog

Personal blog built with [Hugo](https://gohugo.io) and the custom **darktan** theme.

---

## Quick start

### 1. Install Hugo

**Mac:**
```bash
brew install hugo
```

**Windows:**
```bash
winget install Hugo.Hugo.Extended
```

**Linux:**
```bash
sudo apt install hugo
```

### 2. Run locally

```bash
cd myblog
hugo server -D
```

Open http://localhost:1313 — the site live-reloads as you edit.

### 3. Write a new post

```bash
hugo new posts/my-post-title.md
```

This creates `content/posts/my-post-title.md`. Open it and write in Markdown.
Set `draft: false` when ready to publish.

---

## Deploying to GitHub Pages (free)

### First time setup

1. Create a new GitHub repo named `yourusername.github.io`
2. Push this project to it:
   ```bash
   git init
   git add .
   git commit -m "initial commit"
   git remote add origin https://github.com/fred4impact/fred4impact.github.io.git
   git push -u origin
   ```
3. Go to your repo → **Settings** → **Pages** → set Source to **GitHub Actions**
4. Every push to `main` automatically deploys your site

Your blog will be live at `https://yourusername.github.io`

---

## Customise

- **Your name, bio, social links** → edit `hugo.toml`
- **Colours and fonts** → edit `themes/darktan/static/css/main.css`
- **Sidebar links** → edit `themes/darktan/layouts/partials/sidebar.html`
- **Custom domain** → add a `CNAME` file to `static/` with your domain name

---

## File structure

```
myblog/
├── hugo.toml              ← site config (name, bio, links)
├── content/
│   ├── posts/             ← your blog posts (Markdown)
│   └── about.md           ← about page
├── themes/darktan/
│   ├── layouts/           ← HTML templates
│   └── static/css/        ← all styles
└── .github/workflows/     ← auto-deploy to GitHub Pages
```
