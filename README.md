# Giang Do — Portfolio Site

## File structure
```
portfolio/
├── index.html              ← Homepage
├── foreverfit.html         ← Case study (template — duplicate for others)
├── fashion-fiesta.html     ← (duplicate foreverfit.html, change bg color + content)
├── flexistay.html          ← (duplicate, use --cream background)
├── fantastic-plastic.html  ← (duplicate, use --green background)
├── design-system.html      ← (duplicate)
├── llm-biases.html         ← (duplicate, use --blue background)
├── ux-design.html          ← Nav section pages (create as needed)
├── ux-industry.html
├── graphic-design.html
├── cv.html
├── about.html
├── contact.html
└── assets/
    └── images/
        ├── foreverfit-thumb.jpg
        ├── fashion-fiesta-thumb.jpg
        ├── flexistay-thumb.jpg
        ├── fantastic-plastic-thumb.jpg
        ├── design-system-thumb.jpg
        ├── llm-thumb.jpg
        └── (case study images per project)
```

## How to duplicate a case study page
1. Copy `foreverfit.html` → rename (e.g. `fashion-fiesta.html`)
2. Change `body { background: var(--green) }` to your desired color:
   - Fashion Fiesta → `background: var(--yellow)` + `color: var(--green)`
   - FlexiStay → `background: var(--cream)` + `color: var(--green)`
   - LLM Biases → `background: var(--blue)` + `color: var(--green)`
3. Update the title, marquee text, meta info, and description
4. Replace placeholder divs with real `<img>` tags

## Color reference
- Yellow: #e8ef5a
- Forest green: #1b3d2b
- Cream: #f0ede6
- Powder blue: #d6eaf5

## Deploying to GitHub Pages
1. Create a GitHub account at github.com
2. Create a new repo named: `yourusername.github.io`
3. Upload all files (drag & drop in GitHub's web UI, or use GitHub Desktop)
4. Go to Settings → Pages → select `main` branch → Save
5. Site is live at: https://yourusername.github.io

## Connecting a custom domain (e.g. giangdo.com)
1. Buy domain at porkbun.com (~$9-11/year for .com)
2. In Porkbun DNS settings, add:
   - A record: 185.199.108.153
   - A record: 185.199.109.153
   - A record: 185.199.110.153
   - A record: 185.199.111.153
   - CNAME: www → yourusername.github.io
3. In GitHub repo Settings → Pages → Custom domain → enter your domain
4. Tick "Enforce HTTPS" (appears after ~10 min)
5. Done — your site is live at your custom domain!
