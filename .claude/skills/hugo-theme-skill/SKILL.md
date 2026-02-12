---
name: hugo-theme
description: Build custom Hugo themes from scratch for small business websites. Use this skill whenever the user asks to create, modify, or extend a Hugo theme, design a page layout, add a new template or partial, create shortcodes, style the site, or work on any file inside the theme directory. Also trigger when the user provides a screenshot, mockup, or image of a website design and wants it built as a Hugo theme. Also trigger when the user references an existing theme or live website as a visual reference for building a new theme. Also trigger when the user asks about Hugo templating, Go template syntax, Hugo's lookup order, or how to structure content types. This skill covers everything from initial theme scaffolding to polished, production-ready templates with responsive design, accessibility, and performance baked in.
---

# Hugo Custom Theme Builder

This skill guides the creation of custom Hugo themes for small business websites. The goal is to produce clean, semantic, performant HTML with well-organized Go templates — a theme that is easy to maintain, visually professional, and uniquely branded.

## Before You Start

Read the relevant reference file(s) based on what you're working on:

- **Scaffolding a new theme or understanding structure** → Read `references/theme-structure.md`
- **Writing or editing Go templates** → Read `references/go-templates.md`
- **Working on CSS, typography, or visual design** → Read `references/styling.md`

If you're starting a theme from scratch, read all three.

## Core Principles

### 1. Semantic HTML First
Write clean, meaningful HTML. Use `<header>`, `<nav>`, `<main>`, `<article>`, `<section>`, `<aside>`, `<footer>` — not a sea of `<div>`s. Every element should earn its place. Good HTML structure is the foundation of accessibility, SEO, and maintainability.

### 2. Progressive Enhancement
The site must work perfectly without JavaScript. Start with HTML and CSS. If interactivity is needed (e.g., a mobile nav toggle), add minimal vanilla JS as an enhancement layer — the content should still be accessible if JS fails to load.

### 3. Mobile-First Responsive Design
Write CSS mobile-first: base styles for small screens, then use `min-width` media queries to layer on styles for larger viewports. Every template must look great on phones, tablets, and desktops. Test layouts mentally at 320px, 768px, and 1200px widths.

### 4. Performance by Default
Every decision should favor speed. This means no external CSS frameworks, no web fonts unless the brand requires them (and then only 1-2 weights via `font-display: swap`), images served in modern formats, and minimal DOM depth. Hugo's built-in asset pipeline (Hugo Pipes) should handle CSS bundling and minification — never rely on external build tools.

### 5. Accessibility Is Not Optional
Every page must be navigable by keyboard and screen reader. This means proper heading hierarchy (one `<h1>` per page, logical nesting), descriptive `alt` text on images, ARIA labels where semantic HTML isn't sufficient, visible focus styles, and sufficient color contrast (WCAG AA minimum, 4.5:1 for body text).

## Theme Development Workflow

When building or extending the theme, follow this sequence:

1. **Understand the need** — What page, component, or feature is being requested? What content will it display?
2. **Plan the template** — Decide which template type (baseof, single, list, partial, shortcode) and where it fits in Hugo's lookup order.
3. **Write the HTML structure** — Semantic markup first, no styling. Verify the Go template logic pulls the right content.
4. **Style it** — Add CSS in the theme's stylesheet. Mobile-first. Use CSS custom properties for brand tokens (colors, fonts, spacing).
5. **Test locally** — `hugo server -D --disableFastRender` and check multiple viewport sizes.
6. **Refine** — Check accessibility, performance, and code cleanliness.

## Content Architecture for Small Business Sites

A typical small business Hugo site has these content types:

```
content/
├── _index.md              # Homepage (uses layouts/index.html)
├── about/
│   └── _index.md          # About page
├── services/
│   ├── _index.md          # Services landing/list page
│   ├── service-one.md     # Individual service
│   └── service-two.md     # Individual service
├── blog/
│   ├── _index.md          # Blog list page
│   ├── first-post.md
│   └── second-post.md
└── contact/
    └── _index.md          # Contact page
```

Design templates to handle all of these. The homepage is typically a custom layout; services and blog use list + single templates; about and contact are standalone single pages.

## Front Matter Conventions

Use TOML format (`+++` delimiters) consistently. Define a sensible set of front matter params that templates expect:

```toml
+++
title = "Page Title"
date = 2026-02-11
draft = false
description = "SEO and social sharing description."
weight = 1  # For ordering in lists

[params]
  hero_image = "images/hero.jpg"
  hero_heading = "Custom heading for hero section"
  show_cta = true
  cta_text = "Get in Touch"
  cta_link = "/contact/"
+++
```

Document which front matter params each template uses in a comment block at the top of the template file.

## Go Template Conventions

- Always use `{{- -}}` (trimmed whitespace delimiters) to keep generated HTML clean.
- Use `with` blocks instead of `if` for optional values — it's more idiomatic and handles nil safely.
- Prefer `.Param "key"` over `.Params.key` for accessing page params, as it falls back to site params automatically.
- Comment complex template logic with `{{/* explanation */}}`.
- Never put business logic in templates. If something needs computation, use Hugo's built-in functions or a data file.

## CSS Architecture

Use a single main stylesheet processed through Hugo Pipes. Organize with CSS custom properties for theming:

```css
:root {
  /* Brand colors */
  --color-primary: #1a365d;
  --color-secondary: #e53e3e;
  --color-bg: #ffffff;
  --color-text: #1a202c;
  --color-text-muted: #718096;

  /* Typography */
  --font-heading: 'Brand Font', Georgia, serif;
  --font-body: 'Body Font', system-ui, sans-serif;
  --font-size-base: 1rem;
  --line-height-base: 1.6;

  /* Spacing scale */
  --space-xs: 0.25rem;
  --space-sm: 0.5rem;
  --space-md: 1rem;
  --space-lg: 2rem;
  --space-xl: 4rem;
  --space-2xl: 8rem;

  /* Layout */
  --max-width: 72rem;
  --content-width: 40rem;
}
```

Do NOT use Tailwind, Bootstrap, or any CSS framework. Write lean, purposeful CSS that only includes what the site needs. BEM-style naming (`.block__element--modifier`) is encouraged for component-scoped styles but not mandatory — consistency within the project is what matters.

## Shortcodes for Reusable Content

Build shortcodes for content patterns the business will reuse. Common ones for a small business site:

- **CTA button/section** — Reusable call-to-action block
- **Testimonial** — Customer quote with attribution
- **Service card** — Summary card used in grids
- **Image with caption** — Responsive image with proper sizing and alt text
- **Contact info** — Phone, email, address block

Each shortcode should be a self-contained partial in `layouts/shortcodes/`. Keep them simple — a shortcode that needs more than ~30 lines of template logic is probably doing too much.

## Image Handling

Use Hugo's built-in image processing wherever possible:

```go-html-template
{{- $image := resources.Get .Params.hero_image -}}
{{- with $image -}}
  {{- $small := .Resize "600x webp" -}}
  {{- $medium := .Resize "1200x webp" -}}
  {{- $large := .Resize "1800x webp" -}}
  <picture>
    <source srcset="{{ $large.RelPermalink }}" media="(min-width: 1200px)">
    <source srcset="{{ $medium.RelPermalink }}" media="(min-width: 768px)">
    <img src="{{ $small.RelPermalink }}" alt="{{ $.Params.hero_alt | default $.Title }}" loading="lazy" decoding="async">
  </picture>
{{- end -}}
```

Place source images in `assets/images/` (not `static/`) so Hugo Pipes can process them. Only fall back to `static/` for files that don't need processing (like favicons).

## SEO and Meta

The `<head>` partial must include:
- `<title>` with page title and site name
- `<meta name="description">` from front matter
- Open Graph tags (`og:title`, `og:description`, `og:image`, `og:url`)
- Twitter card meta tags
- Canonical URL
- Structured data (JSON-LD) for the business (Organization schema at minimum)

Use Hugo's built-in `.Site.Title`, `.Title`, `.Description`, `.Permalink` variables. Build a reusable `head.html` partial that handles all of this.

## Visual Reference Workflow

When the user provides a screenshot, mockup, or image of a website:

1. **Inventory the components** — Before writing any code, list every visible UI section: navigation, hero, feature/service blocks, testimonials, CTA sections, footer, etc. Confirm the list with the user if anything is ambiguous.

2. **Extract design tokens** — Identify and document:
   - **Colors**: primary, secondary, background, text, muted text, border. Note approximate hex values from the image.
   - **Typography**: serif vs. sans-serif for headings and body, approximate size scale, font weight usage.
   - **Spacing**: how generous or compact the layout feels — map to a spacing scale.
   - **Border radius**: sharp corners, subtle rounding, or pill shapes.
   - **Shadows and depth**: flat design, subtle elevation, or heavy card shadows.

3. **Map components to Hugo files** — For each component identified, decide:
   - Is it a partial? (header, footer, hero, cta, testimonial-list)
   - Is it a shortcode? (testimonial, service-card, cta-button)
   - Is it a page-level template? (index.html, services/list.html)

4. **Build in order** — Follow this sequence:
   1. Design tokens in `assets/css/main.css` (`:root` custom properties)
   2. `baseof.html` — the shell
   3. `head.html`, `header.html`, `footer.html` partials
   4. Homepage (`index.html`) assembling all hero/section partials
   5. Section templates (services, blog) — list then single
   6. Shortcodes for reusable content blocks
   7. Remaining partials

5. **Do not guess at brand specifics** — If the screenshot doesn't clearly show a color value, font name, or content, use a reasonable placeholder and call it out explicitly so the user can fill it in.

## Existing Theme Analysis Workflow

When the user provides an existing Hugo theme directory as a visual or structural reference:

1. **Read `baseof.html` first** — Understand the page shell: what blocks are defined, what partials are called, what the HTML structure looks like.

2. **Read the CSS custom properties** — Find the `:root` block in the main stylesheet. This is the design token system — colors, fonts, spacing. This is what you'll adapt, not copy verbatim.

3. **Inventory the partials** — List every file in `layouts/partials/`. Understand what each one does before writing anything.

4. **Note the content architecture** — What sections and content types does the theme support? Match or extend this for the new theme.

5. **Identify what to replicate vs. what to change** — Be explicit about which visual patterns are being carried over (layout structure, spacing feel, type scale) versus what is being replaced (colors, fonts, specific components).

6. **Never copy theme files directly** — Reproduce the intent and visual style in freshly written, clean code. Do not paste existing theme code as a starting point.

## What NOT to Do

- Do not add npm, webpack, PostCSS, or any Node.js tooling. Hugo Pipes handles asset processing.
- Do not use CSS frameworks (Tailwind, Bootstrap, Bulma, etc.).
- Do not add JavaScript unless there is a clear, unavoidable need — and then only vanilla JS.
- Do not over-abstract templates. A small business site has maybe 5-8 page types. Don't build a "framework" — build exactly what's needed.
- Do not use Hugo Modules for theme dependencies. Keep the theme self-contained.
- Do not hardcode business-specific content (phone numbers, addresses, company name) into templates. Always pull from site config or front matter so the theme remains configurable.
