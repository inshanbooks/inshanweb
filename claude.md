# inshanweb — Claude Project Guide

## Project Overview

A Next.js blogging and product showcase platform for **Inshan Books** and **Inshan Media** (CV Inshan Karya Permata), a publishing company based in Yogyakarta, Indonesia. Content is in Bahasa Indonesia.

Two primary content sections:
- **Berita** (News/Articles) — located in `data/berita/`
- **Produk** (Products/Books) — located in `data/produk/`

## Tech Stack

- **Next.js** v15 with React 18 (Pages Router)
- **Tailwind CSS** v3.4 (dark mode via class strategy, primary color: Teal)
- **MDX** via `mdx-bundler` for content (supports math via KaTeX, citations, code titles)
- **PostCSS** + Autoprefixer
- Deployed to **Netlify** via `@netlify/plugin-nextjs`

## Project Structure

```
inshanweb/
├── components/          # Reusable React components
├── css/                 # Tailwind entry + Prism syntax highlighting
├── data/                # Content (MDX), site config, nav links
│   ├── berita/          # News articles (MDX)
│   ├── produk/          # Product/book entries (MDX)
│   ├── authors/         # Author profiles (MDX)
│   ├── siteMetadata.js  # Central site config (title, social, analytics, newsletter)
│   └── headerNavLinks.js
├── layouts/             # Page layout templates (PostLayout, ListLayout, etc.)
├── lib/                 # MDX compilation, RSS, tags, custom remark/rehype plugins
│   └── utils/           # formatDate, kebabCase, file helpers, htmlEscaper
├── pages/               # Next.js file-based routes
│   ├── api/             # Newsletter API endpoints
│   ├── berita/          # News routes (listing, pagination, slug)
│   └── produk/          # Product routes (listing, pagination, slug)
├── public/              # Static assets (favicons, images)
├── scripts/             # compose (new post CLI), sitemap gen, remote watcher
├── netlify.toml         # Netlify build + plugin config
└── config files         # next.config.mjs, tailwind.config.js, .eslintrc.js, etc.
```

## Path Aliases (jsconfig.json)

```
@/components/*  →  components/
@/data/*        →  data/
@/layouts/*     →  layouts/
@/lib/*         →  lib/
@/css/*         →  css/
```

## Key Commands

```bash
npm run dev       # Start dev server
npm run build     # Production build (also generates sitemap)
npm run serve     # Serve production build
npm run lint      # ESLint with auto-fix
npm run analyze   # Bundle size analysis
```

## Node Requirement

- Node.js `>=18.0.0`

## Deployment (Netlify)

- `netlify.toml` configures the build command and installs `@netlify/plugin-nextjs` automatically.
- Set `NEXT_PUBLIC_*` environment variables (Giscus, newsletter keys, etc.) in the Netlify dashboard.
- API routes under `pages/api/` are handled as serverless functions by the plugin.

## Adding Content

### New News Article (`data/berita/`)
Use the interactive compose script or create an MDX file directly. Required frontmatter fields:
- `title`, `date`, `lastmod`, `tags`, `draft`, `summary`

### New Product (`data/produk/`)
Same MDX format in `data/produk/`. A dedicated `ProdukLayout` and `ListLayoutProduk` handle product-specific rendering.

## Code Style

- **Prettier**: single quotes, no semicolons, 100-char print width, 2-space indent
- **ESLint**: extends `eslint:recommended`, `next`, `prettier`
- Git hooks via Husky run lint-staged on commit

## Integrations (configured in `data/siteMetadata.js`)

| Feature      | Current Provider |
|--------------|-----------------|
| Analytics    | Google Analytics (G-DRHY3FW8PM) |
| Comments     | Giscus |
| Newsletter   | Buttondown |

Alternatives are wired up and can be swapped by changing `siteMetadata.js` values. API routes in `pages/api/` support Mailchimp, ConvertKit, and Klaviyo as well.

## Layout & Component Conventions

- Page layouts live in `layouts/` and are referenced in MDX frontmatter or page files.
- `components/LayoutWrapper.js` wraps every page with header, footer, and theme context.
- Dark mode is class-based; toggle lives in `ThemeSwitch.js`.
- Custom MDX components (images, code blocks, etc.) are registered in `components/MDXComponents.js`.
