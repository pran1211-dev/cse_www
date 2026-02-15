# Claude Code Guidelines — Hugo Small Business Website

## Project Overview

This is a small business website built with **Hugo** using the **PaperMod** theme. It is hosted on **GitHub Pages** with **GitHub Actions** handling builds and deployments. The site uses a **custom domain**. The goal is a clean, fast, professional web presence — not a web app. Keep it static, minimal, and performant.

## Business Context

**Business:** Cityscape Engineering
**Website:** https://cityscapeengineering.com
**Industry:** Structural engineering
**Service area:** New York (NY), New Jersey (NJ), and Connecticut (CT)
**Clients served:** All construction types — residential, commercial, industrial, and mixed-use

This context should inform all content, SEO, and copy decisions:

- Titles, descriptions, and headings should reference relevant service areas (NY, NJ, CT) and construction types where natural
- The audience is a mix of property owners, contractors, architects, and developers — write for a professional but non-engineer readership
- Structural engineering is a licensed, regulated profession — avoid overclaiming credentials or scope of work; flag anything that needs professional review
- Local SEO matters: service area signals (city names, state abbreviations) belong in page titles, descriptions, and body copy for service pages
- Industry terminology is appropriate but should be explained when it may not be familiar to a general contractor or property owner audience

## Tech Stack

- **Static site generator:** Hugo
- **Theme:** PaperMod (installed as a git submodule in `themes/PaperMod`)
- **Hosting:** GitHub Pages
- **CI/CD:** GitHub Actions (builds and deploys on push to `main`)
- **Domain:** Custom domain (configured via CNAME file and DNS)
- **JavaScript:** None unless absolutely necessary. Default to zero JS.

## Project Structure

```
.
├── .claude/
│   └── skills/
│       ├── hugo-theme-skill/    # Skill: custom theme creation and overrides
│       └── hugo-page-skill/     # Skill: new page creation with SEO
├── archetypes/          # Default front matter templates for new content
├── assets/              # Files processed by Hugo Pipes (SCSS, etc.)
├── content/             # All site content as Markdown files
│   ├── _index.md        # Homepage content
│   ├── about.md         # About page
│   ├── services/        # Services section
│   └── posts/           # Blog posts (if applicable)
├── data/                # Data files (JSON, YAML, TOML) for templates
├── layouts/             # Custom layout overrides (use sparingly)
│   ├── partials/        # Partial template overrides
│   └── shortcodes/      # Custom shortcodes
├── static/              # Static assets copied as-is (images, CNAME, etc.)
│   ├── images/
│   └── CNAME
├── themes/PaperMod/     # Theme submodule — NEVER edit directly
├── hugo.toml            # Site configuration
└── .github/workflows/   # GitHub Actions deployment workflow
```

## Skills

This project uses Claude Code skills for complex, domain-specific tasks. Always invoke the appropriate skill before starting work in these areas.

| Task | Skill to invoke |
|---|---|
| Create or modify theme templates, partials, CSS, or shortcodes | `skill: "hugo-theme-skill"` |
| Build a theme from a screenshot or existing theme reference | `skill: "hugo-theme-skill"` |
| Create a new page, post, or content file | `skill: "hugo-page-skill"` |
| Structure page content from a screenshot or mockup | `skill: "hugo-page-skill"` |
| Write or review front matter and SEO metadata | `skill: "hugo-page-skill"` |
| Update existing page content | No skill needed — follow the Content Updates section below |

**Skill boundary:** `hugo-theme-skill` owns everything in `layouts/`, `assets/`, and `themes/`. `hugo-page-skill` owns everything in `content/`. These do not overlap — if a task touches both, handle them separately with the appropriate skill for each part.

## Core Principles

### 1. Do Not Modify the Theme Directly
Never edit files inside `themes/PaperMod/`. Always override by placing files in the project root's `layouts/` or `assets/` directories. Hugo's lookup order will pick up the override automatically.

### 2. Keep It Static and Minimal
- No JavaScript unless there is a clear, unavoidable need. If JS is required, it must be vanilla JS — no frameworks, no npm, no build tools beyond Hugo itself.
- No external API calls, no client-side rendering, no SPAs.
- Rely on Hugo's built-in features: shortcodes, partials, Hugo Pipes for asset processing.

### 3. Performance Is Non-Negotiable
- Optimize all images before committing (use WebP where possible, compress PNGs/JPGs).
- Do not add external fonts unless explicitly requested. Prefer system font stacks.
- Do not add third-party CSS frameworks. PaperMod's styling is sufficient; extend it with small, scoped overrides if needed.
- Avoid external scripts and iframes. Every external request is a performance cost.

### 4. Content Lives in Markdown
- All pages and posts are Markdown files in `content/`.
- Use Hugo front matter (TOML format, matching `hugo.toml`) for metadata.
- Use shortcodes for reusable content patterns instead of raw HTML in Markdown.

## Configuration

- The primary config file is `hugo.toml` at the project root.
- PaperMod-specific params go under `[params]`. Refer to the PaperMod documentation for available options: https://github.com/adityatelange/hugo-PaperMod/wiki
- Use `hugo.toml` — not `config.toml`, `config.yaml`, or `config/` directory splitting — unless the config grows unmanageably large.

## News Page (Posts)

The news page (`/posts/`) is an **external link listing only**. It is not a blog.

- **No individual post pages are generated.** The `_index.md` cascade sets `_build.render = "never"` for all posts.
- Every post in `content/posts/` must have `external_url` in its front matter params, linking to the original article on an external site.
- All news links open in a new tab (`target="_blank"`).
- The Markdown body of a post is **not rendered** anywhere on the site — only `title`, `date`, `description`, `source`, `external_url`, and `cover` from front matter are used.
- The most recent post is displayed as the featured article with a large image. All others appear in a vertical list below.
- Do not create internal blog post content. The news page curates links to external press and industry articles.

### News Post Front Matter

```toml
+++
title = "Article Title"
date = 2026-02-14
description = "Brief summary for the listing."
draft = false

[params]
  source = "Publication Name"
  external_url = "https://example.com/article"
  cover = "images/optional-cover.jpg"
+++
```

## Content Conventions

### Front Matter Format
Use TOML (`+++` delimiters) for consistency:

```toml
+++
title = "Page Title"
date = 2026-02-11
draft = false
description = "A brief description for SEO and social sharing."
[params]
  cover = "images/cover.jpg"
+++
```

### Writing Style
- Write content in clear, professional language appropriate for a small business audience.
- Keep paragraphs short and scannable.
- Use headings (`##`, `###`) to structure longer pages.

## Content Updates

When editing existing pages in `content/`, follow these rules:

- **Never change the `slug`** — it breaks existing URLs and any SEO equity the page has built. If a slug genuinely needs to change, flag it and set up a redirect.
- **Update `lastmod`** in front matter whenever content changes meaningfully (rewrites, new sections, updated facts). Leave it alone for trivial fixes like typos.
- **Preserve existing structure** — do not reorganize headings, remove sections, or change shortcodes unless explicitly asked.
- **Do not add a `# H1` in the Markdown body** — PaperMod renders the front matter `title` as the H1. A second H1 breaks heading hierarchy and SEO.
- **Re-evaluate `title` and `description`** if the content direction changes significantly — stale meta that no longer matches the page content hurts click-through rate.
- **Scope edits tightly** — change what was asked, leave everything else as-is. Do not reformat, rewrite tone, or restructure unrelated sections.

## Layout Overrides

When customizing the theme's appearance:

1. Find the template you want to override inside `themes/PaperMod/layouts/`.
2. Copy it to the same relative path under the root `layouts/` directory.
3. Edit the copy — Hugo will use it instead of the theme's version.
4. Add a comment at the top of the overridden file noting what was changed and why.

Example:
```
# To override the footer:
# Copy themes/PaperMod/layouts/partials/footer.html
# To   layouts/partials/footer.html
# Then edit layouts/partials/footer.html
```

Keep overrides to a minimum. The more you override, the harder it is to update the theme.

## Custom CSS

If custom styles are needed, create `assets/css/extended/custom.css`. PaperMod automatically loads CSS files from this directory. Do not create separate stylesheets elsewhere.

Keep custom CSS small and scoped. Avoid `!important`. Use the browser inspector to understand PaperMod's existing class names and structure before writing overrides.

## Deployment

- Pushing to `main` triggers the GitHub Actions workflow in `.github/workflows/`.
- The workflow runs `hugo --minify` and deploys the `public/` output to GitHub Pages.
- The custom domain is configured via a `CNAME` file in `static/` and DNS records.
- **Never commit the `public/` directory.** It is generated during CI. Ensure it is in `.gitignore`.

## Git Practices

- Write clear, descriptive commit messages (e.g., `Add services page with pricing section`, not `update stuff`).
- Do not commit generated files, caches, or OS artifacts. The `.gitignore` should cover `public/`, `resources/`, `.hugo_build.lock`, and common OS files.
- When updating the PaperMod theme, update the git submodule reference and test locally before pushing.

## Testing Locally

Always verify changes with the local dev server before committing:

```bash
hugo server -D --disableFastRender
```

- `-D` includes draft content.
- `--disableFastRender` forces full page rebuilds to catch issues that incremental builds might miss.
- Check the site at `http://localhost:1313`.

## SEO and Metadata

- Every page must have a `title` and `description` in its front matter.
- Use meaningful, keyword-relevant titles.
- PaperMod handles Open Graph and Twitter card meta tags automatically from front matter — just fill them in.
- Ensure `baseURL` in `hugo.toml` is set to the production custom domain.

## Common Tasks

| Task | How |
|---|---|
| Add a new page | Invoke `hugo-page-skill`, then `hugo new content pagename.md` |
| Add a new blog post | Invoke `hugo-page-skill`, then `hugo new content posts/post-title.md` |
| Update existing page content | Follow the Content Updates section — no skill needed |
| Add an image | Place in `static/images/`, reference as `/images/filename.webp` |
| Customize CSS | Invoke `hugo-theme-skill`, add rules to `assets/css/extended/custom.css` |
| Override a template | Invoke `hugo-theme-skill`, copy from `themes/PaperMod/layouts/` to `layouts/` |
| Update the theme | `git submodule update --remote themes/PaperMod` then test |

## What NOT to Do

- Do not install npm packages or add a `package.json`.
- Do not add client-side frameworks (React, Vue, Alpine, etc.).
- Do not edit files inside `themes/PaperMod/`.
- Do not commit the `public/` directory.
- Do not add tracking scripts or analytics without explicit approval.
- Do not use Hugo modules if git submodules are already working for the theme.
- Do not over-engineer. This is a small business site, not a SaaS product.
