# Online Resume — Cloudflare Deployment Guide

## Project structure

```
cv/
├── public/
│   ├── index.html     ← Full resume (HTML + CSS + JS in a single file)
│   ├── _headers       ← Security headers (Cloudflare Pages)
│   └── photo.jpg      ← Add your photo here
└── wrangler.toml      ← Cloudflare Workers config
```

---

## 1. Customize the content

Open `public/index.html` and replace all `[brackets]`:

| Placeholder           | Replace with                             |
|-----------------------|------------------------------------------|
| `[LastName]`          | Your last name                           |
| `[your@email.com]`    | Your email address                       |
| `[username]`          | Your GitHub username                     |
| `[profile]`           | Your LinkedIn profile                    |
| `[your-domain.com]`   | Your Cloudflare domain                   |
| `[Company Name]`      | Your companies (×4 languages)            |
| `[Location]`          | Your locations                           |
| `[N]`                 | Number of engineers managed              |
| `[Year]` / `[Degree]` | Your degrees and certifications          |

**Profile photo:**
1. Add `photo.jpg` to `public/`
2. In `index.html`, replace `<div class="avatar-placeholder">L</div>` with:
   ```html
   <img src="photo.jpg" alt="Your Name">
   ```

---

## 2. Deploy to Cloudflare Workers (recommended in 2025)

```bash
# 1. Install Wrangler (Cloudflare CLI)
npm install -g wrangler

# 2. Log in to Cloudflare
npx wrangler login

# 3. Deploy (from the cv/ folder)
npx wrangler deploy

# → Immediate preview URL: https://my-cv.[account].workers.dev
```

---

## 3. Connect your domain

In the **Cloudflare dashboard**:
1. Workers & Pages → your worker → Settings → Domains & Routes
2. Add your custom domain: `cv.your-domain.com` or `your-domain.com`
3. DNS is configured automatically if the domain is already on Cloudflare

---

## 4. Automatic deployment via Git (optional)

```bash
# Initialize a git repo
git init && git add . && git commit -m "init cv"

# Push to GitHub
gh repo create my-cv --public --push

# In the Cloudflare dashboard:
# Workers & Pages → Create → Pages → Connect to Git → my-cv
# Build command: (leave empty)
# Output directory: public
```
→ Every `git push` triggers an automatic redeployment.

---

## 5. Export to PDF

Open the resume in Chrome/Edge:
1. `Ctrl+P` (or `Cmd+P` on Mac)
2. Destination: **Save as PDF**
3. Paper size: A4
4. Margins: Minimum
5. ✅ Background graphics

The `@media print` CSS is already configured for clean rendering.

---

## 6. Add Google Analytics (optional)

In the `<head>` of `index.html`, after the title:
```html
<!-- Google Analytics -->
<script async src="https://www.googletagmanager.com/gtag/js?id=G-XXXXXXXXXX"></script>
<script>
  window.dataLayer = window.dataLayer || [];
  function gtag(){dataLayer.push(arguments);}
  gtag('js', new Date());
  gtag('config', 'G-XXXXXXXXXX');
</script>
```
