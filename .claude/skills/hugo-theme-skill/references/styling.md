# Styling Reference for Hugo Themes

This document covers CSS architecture, typography, responsive design, and visual design patterns for custom Hugo themes.

## Table of Contents

1. [CSS Architecture](#css-architecture)
2. [Custom Properties (Design Tokens)](#custom-properties-design-tokens)
3. [Typography](#typography)
4. [Layout Patterns](#layout-patterns)
5. [Responsive Design](#responsive-design)
6. [Component Styling Patterns](#component-styling-patterns)
7. [Hugo Pipes for CSS](#hugo-pipes-for-css)
8. [Performance Considerations](#performance-considerations)

---

## CSS Architecture

Use a single `main.css` file in `assets/css/`. For small business sites, this is typically enough. If the file grows beyond ~500 lines, split into logical sections imported with `@import` (Hugo Pipes can concatenate them).

Organize the stylesheet in this order:

```css
/* 1. Custom properties (design tokens) */
/* 2. Reset / base styles */
/* 3. Typography */
/* 4. Layout utilities */
/* 5. Components (header, footer, hero, cards, etc.) */
/* 6. Page-specific overrides */
/* 7. Media queries (or inline with each component, mobile-first) */
```

---

## Custom Properties (Design Tokens)

Define all brand values as CSS custom properties on `:root`. This is the single source of truth for the site's visual identity.

```css
:root {
  /* ---- Colors ---- */
  --color-primary: #1a365d;
  --color-primary-light: #2a4a7f;
  --color-primary-dark: #0f2440;
  --color-secondary: #e53e3e;
  --color-secondary-light: #fc5c5c;

  --color-bg: #ffffff;
  --color-bg-alt: #f7f8fa;
  --color-text: #1a202c;
  --color-text-muted: #718096;
  --color-border: #e2e8f0;

  /* ---- Typography ---- */
  --font-heading: Georgia, 'Times New Roman', serif;
  --font-body: system-ui, -apple-system, 'Segoe UI', sans-serif;
  --font-mono: 'SFMono-Regular', Consolas, monospace;

  --text-xs: 0.75rem;     /* 12px */
  --text-sm: 0.875rem;    /* 14px */
  --text-base: 1rem;      /* 16px */
  --text-lg: 1.125rem;    /* 18px */
  --text-xl: 1.25rem;     /* 20px */
  --text-2xl: 1.5rem;     /* 24px */
  --text-3xl: 1.875rem;   /* 30px */
  --text-4xl: 2.25rem;    /* 36px */
  --text-5xl: 3rem;       /* 48px */

  --leading-tight: 1.2;
  --leading-normal: 1.6;
  --leading-relaxed: 1.8;

  /* ---- Spacing ---- */
  --space-1: 0.25rem;     /* 4px */
  --space-2: 0.5rem;      /* 8px */
  --space-3: 0.75rem;     /* 12px */
  --space-4: 1rem;        /* 16px */
  --space-6: 1.5rem;      /* 24px */
  --space-8: 2rem;        /* 32px */
  --space-12: 3rem;       /* 48px */
  --space-16: 4rem;       /* 64px */
  --space-24: 6rem;       /* 96px */
  --space-32: 8rem;       /* 128px */

  /* ---- Layout ---- */
  --max-width: 72rem;     /* 1152px */
  --content-width: 42rem; /* 672px — ideal for reading */
  --gutter: var(--space-6);

  /* ---- Effects ---- */
  --radius-sm: 0.25rem;
  --radius-md: 0.5rem;
  --radius-lg: 1rem;
  --shadow-sm: 0 1px 2px rgba(0, 0, 0, 0.05);
  --shadow-md: 0 4px 6px rgba(0, 0, 0, 0.07);
  --shadow-lg: 0 10px 25px rgba(0, 0, 0, 0.1);
  --transition: 200ms ease;
}
```

When the brand calls for it, replace these defaults. The point is that every visual value is centralized here.

---

## Typography

### Base type styles

```css
html {
  font-size: 100%; /* Respect user's browser settings */
}

body {
  font-family: var(--font-body);
  font-size: var(--text-base);
  line-height: var(--leading-normal);
  color: var(--color-text);
}

h1, h2, h3, h4, h5, h6 {
  font-family: var(--font-heading);
  line-height: var(--leading-tight);
  color: var(--color-text);
  margin-top: 0;
}

h1 { font-size: var(--text-4xl); margin-bottom: var(--space-6); }
h2 { font-size: var(--text-3xl); margin-bottom: var(--space-4); }
h3 { font-size: var(--text-2xl); margin-bottom: var(--space-3); }

p {
  margin-top: 0;
  margin-bottom: var(--space-4);
}

a {
  color: var(--color-primary);
  text-decoration: underline;
  text-underline-offset: 2px;
  transition: color var(--transition);
}

a:hover {
  color: var(--color-primary-light);
}

a:focus-visible {
  outline: 2px solid var(--color-primary);
  outline-offset: 2px;
}
```

### Web fonts

Only add web fonts if the brand requires them. When you do:

1. Self-host the font files (don't rely on Google Fonts CDN).
2. Use `woff2` format only — it has universal support and best compression.
3. Load at most 2 weights (regular and bold). A third weight is a luxury.
4. Always set `font-display: swap` to prevent invisible text during load.

```css
@font-face {
  font-family: 'BrandFont';
  src: url('/fonts/brandfont-regular.woff2') format('woff2');
  font-weight: 400;
  font-style: normal;
  font-display: swap;
}

@font-face {
  font-family: 'BrandFont';
  src: url('/fonts/brandfont-bold.woff2') format('woff2');
  font-weight: 700;
  font-style: normal;
  font-display: swap;
}
```

Place font files in `static/fonts/`.

---

## Layout Patterns

### Container

```css
.container {
  width: 100%;
  max-width: var(--max-width);
  margin-inline: auto;
  padding-inline: var(--gutter);
}

.container--narrow {
  max-width: var(--content-width);
}
```

### Section spacing

```css
.section {
  padding-block: var(--space-16);
}

.section--compact {
  padding-block: var(--space-8);
}
```

### Grid layouts

Use CSS Grid for page-level layouts and card grids:

```css
.grid {
  display: grid;
  gap: var(--space-8);
}

.grid--2 {
  grid-template-columns: repeat(auto-fit, minmax(min(100%, 20rem), 1fr));
}

.grid--3 {
  grid-template-columns: repeat(auto-fit, minmax(min(100%, 16rem), 1fr));
}
```

The `minmax(min(100%, Xrem), 1fr)` pattern is responsive without media queries — columns wrap naturally based on available space.

### Flexbox for component layouts

```css
.flex {
  display: flex;
  gap: var(--space-4);
}

.flex--between {
  justify-content: space-between;
  align-items: center;
}
```

---

## Responsive Design

### Mobile-first approach

Write base styles for the smallest screen, then add complexity for larger viewports:

```css
/* Base: single column, smaller type */
.hero__title {
  font-size: var(--text-3xl);
}

/* Tablet and up */
@media (min-width: 48em) { /* 768px */
  .hero__title {
    font-size: var(--text-4xl);
  }
}

/* Desktop */
@media (min-width: 75em) { /* 1200px */
  .hero__title {
    font-size: var(--text-5xl);
  }
}
```

### Breakpoint convention

Use `em` units for media queries (they respect user zoom settings):

```css
/* Small: default (no query) */
/* Medium: @media (min-width: 48em)   — 768px */
/* Large:  @media (min-width: 64em)   — 1024px */
/* XL:     @media (min-width: 75em)   — 1200px */
```

### Responsive images

Always constrain images:

```css
img {
  max-width: 100%;
  height: auto;
  display: block;
}
```

---

## Component Styling Patterns

### Header / Navigation

```css
.site-header {
  padding-block: var(--space-4);
  border-bottom: 1px solid var(--color-border);
}

.site-nav {
  display: flex;
  align-items: center;
  justify-content: space-between;
  gap: var(--space-6);
}

.site-nav__links {
  display: flex;
  list-style: none;
  margin: 0;
  padding: 0;
  gap: var(--space-6);
}

.site-nav__link {
  text-decoration: none;
  color: var(--color-text);
  font-weight: 500;
  transition: color var(--transition);
}

.site-nav__link:hover,
.site-nav__link[aria-current="page"] {
  color: var(--color-primary);
}
```

### Cards

```css
.card {
  background: var(--color-bg);
  border: 1px solid var(--color-border);
  border-radius: var(--radius-md);
  padding: var(--space-6);
  transition: box-shadow var(--transition);
}

.card:hover {
  box-shadow: var(--shadow-md);
}

.card__title {
  font-size: var(--text-xl);
  margin-bottom: var(--space-2);
}

.card__text {
  color: var(--color-text-muted);
  margin-bottom: var(--space-4);
}
```

### Buttons

```css
.btn {
  display: inline-flex;
  align-items: center;
  gap: var(--space-2);
  padding: var(--space-3) var(--space-6);
  font-family: var(--font-body);
  font-size: var(--text-base);
  font-weight: 600;
  text-decoration: none;
  border: 2px solid transparent;
  border-radius: var(--radius-sm);
  cursor: pointer;
  transition: background-color var(--transition), color var(--transition);
}

.btn--primary {
  background: var(--color-primary);
  color: #fff;
}

.btn--primary:hover {
  background: var(--color-primary-light);
}

.btn--outline {
  background: transparent;
  color: var(--color-primary);
  border-color: var(--color-primary);
}

.btn--outline:hover {
  background: var(--color-primary);
  color: #fff;
}

.btn:focus-visible {
  outline: 2px solid var(--color-primary);
  outline-offset: 2px;
}
```

### Hero section

```css
.hero {
  padding-block: var(--space-24);
  text-align: center;
}

.hero--with-bg {
  background-size: cover;
  background-position: center;
  color: #fff;
  position: relative;
}

.hero--with-bg::before {
  content: '';
  position: absolute;
  inset: 0;
  background: rgba(0, 0, 0, 0.5);
}

.hero__content {
  position: relative;
  z-index: 1;
  max-width: var(--content-width);
  margin-inline: auto;
}
```

---

## Hugo Pipes for CSS

In the `head.html` partial, load the stylesheet through Hugo Pipes:

```go-html-template
{{- $css := resources.Get "css/main.css" -}}
{{- if hugo.IsProduction -}}
  {{- $css = $css | minify | fingerprint -}}
{{- end -}}
<link rel="stylesheet" href="{{ $css.RelPermalink }}"
  {{- with $css.Data.Integrity }} integrity="{{ . }}"{{ end -}}>
```

This gives you:
- **Development**: Unminified CSS for easy debugging
- **Production**: Minified with content hash in the filename for cache busting

---

## Performance Considerations

1. **No CSS frameworks.** You'll write maybe 300-500 lines of CSS for a small business site. That's tiny. A framework adds 10-50x more CSS than you need.

2. **Minimize specificity.** Use class selectors. Avoid nesting deeper than 2 levels. Never use `!important` unless overriding third-party styles you can't control.

3. **Use modern CSS.** `gap` in flexbox, `aspect-ratio`, `clamp()` for fluid typography, `inset` shorthand, logical properties (`margin-inline`, `padding-block`) — these reduce code size and improve readability.

4. **Fluid typography with clamp.** Instead of multiple breakpoints for font sizes:

```css
.hero__title {
  font-size: clamp(var(--text-3xl), 5vw, var(--text-5xl));
}
```

5. **Prefers-reduced-motion.** Respect users who opt out of animations:

```css
@media (prefers-reduced-motion: reduce) {
  *, *::before, *::after {
    animation-duration: 0.01ms !important;
    transition-duration: 0.01ms !important;
  }
}
```

6. **Print styles.** Optional but professional:

```css
@media print {
  .site-header, .site-footer, .btn { display: none; }
  body { font-size: 12pt; color: #000; }
  a { text-decoration: underline; color: #000; }
}
```
