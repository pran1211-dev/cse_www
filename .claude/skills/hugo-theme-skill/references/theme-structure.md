# Hugo Theme Structure Reference

This document covers the directory layout of a Hugo theme, what every file does, and how to scaffold a new theme from scratch.

## Table of Contents

1. [Directory Layout](#directory-layout)
2. [Scaffolding a New Theme](#scaffolding-a-new-theme)
3. [Template Types Explained](#template-types-explained)
4. [Hugo's Lookup Order](#hugos-lookup-order)
5. [The Base Template Pattern](#the-base-template-pattern)
6. [Partials](#partials)
7. [Static Assets vs Hugo Pipes](#static-assets-vs-hugo-pipes)

---

## Directory Layout

A complete custom theme for a small business site looks like this:

```
themes/my-theme/
├── archetypes/
│   └── default.md                 # Default front matter for `hugo new`
├── assets/
│   ├── css/
│   │   └── main.css               # Main stylesheet (processed by Hugo Pipes)
│   ├── js/
│   │   └── main.js                # Optional JS (only if needed)
│   └── images/                    # Images processed by Hugo Pipes
├── layouts/
│   ├── _default/
│   │   ├── baseof.html            # Base template — wraps every page
│   │   ├── single.html            # Default template for single pages
│   │   └── list.html              # Default template for list/section pages
│   ├── index.html                 # Homepage template
│   ├── partials/
│   │   ├── head.html              # <head> with meta, CSS, SEO tags
│   │   ├── header.html            # Site header and navigation
│   │   ├── footer.html            # Site footer
│   │   ├── hero.html              # Hero/banner section (reusable)
│   │   ├── cta.html               # Call-to-action section
│   │   ├── seo.html               # Open Graph and structured data
│   │   └── pagination.html        # Blog pagination
│   ├── shortcodes/
│   │   ├── cta-button.html        # Call-to-action button
│   │   ├── testimonial.html       # Customer testimonial
│   │   └── service-card.html      # Service summary card
│   ├── services/
│   │   ├── single.html            # Individual service page layout
│   │   └── list.html              # Services listing page layout
│   ├── blog/
│   │   ├── single.html            # Blog post layout
│   │   └── list.html              # Blog listing page layout
│   └── 404.html                   # Custom 404 page
├── static/
│   ├── favicon.ico
│   └── robots.txt
└── theme.toml                     # Theme metadata
```

Every file has a purpose. Do not add files "just in case." If the site doesn't have a blog yet, don't create blog templates until it does.

---

## Scaffolding a New Theme

Hugo provides a scaffolding command:

```bash
hugo new theme my-theme
```

This creates a minimal skeleton. However, the generated files are nearly empty, so you'll replace their contents entirely. The value of the command is getting the directory structure right.

After scaffolding, immediately:

1. Write `theme.toml` with the theme name and basic metadata.
2. Build out `baseof.html` as the foundation.
3. Create `head.html`, `header.html`, and `footer.html` partials.
4. Create `index.html` for the homepage.
5. Create `_default/single.html` and `_default/list.html` as fallbacks.
6. Add the main CSS file in `assets/css/main.css`.

---

## Template Types Explained

### baseof.html
The shell that wraps every page. Defines the `<!DOCTYPE html>`, `<html>`, `<head>`, and `<body>` structure. Uses `block` directives to define slots that child templates fill.

```go-html-template
<!DOCTYPE html>
<html lang="{{ .Site.LanguageCode | default "en" }}">
<head>
  {{- partial "head.html" . -}}
</head>
<body>
  {{- partial "header.html" . -}}
  <main id="main-content">
    {{- block "main" . }}{{- end }}
  </main>
  {{- partial "footer.html" . -}}
  {{- block "scripts" . }}{{- end }}
</body>
</html>
```

### single.html
Renders a single piece of content (a page, blog post, service). It "defines" the `main` block from baseof.

```go-html-template
{{- define "main" -}}
<article>
  <h1>{{ .Title }}</h1>
  <div class="content">
    {{ .Content }}
  </div>
</article>
{{- end -}}
```

### list.html
Renders a section page that lists its children (e.g., /services/ listing all services, /blog/ listing all posts).

```go-html-template
{{- define "main" -}}
<section>
  <h1>{{ .Title }}</h1>
  {{ .Content }}
  {{- range .Pages -}}
    <article class="summary">
      <h2><a href="{{ .RelPermalink }}">{{ .Title }}</a></h2>
      <p>{{ .Summary }}</p>
    </article>
  {{- end -}}
</section>
{{- end -}}
```

### index.html
The homepage template. This is typically the most custom layout — it assembles multiple partials (hero, services preview, testimonials, CTA) into a unique composition.

### Section-specific templates
Templates in `layouts/blog/` or `layouts/services/` override the defaults for that content section. Hugo picks them automatically based on the content's directory.

---

## Hugo's Lookup Order

Hugo resolves templates from most specific to least specific. For a single page in the `services` section:

1. `layouts/services/single.html` — section-specific
2. `layouts/_default/single.html` — default fallback

For the homepage:

1. `layouts/index.html`

For a list page in `blog`:

1. `layouts/blog/list.html`
2. `layouts/_default/list.html`

This means you only need to create section-specific templates when they differ from the defaults. If your blog list and services list look the same, one `_default/list.html` handles both.

---

## The Base Template Pattern

The `baseof.html` + `define "main"` pattern is the foundation of Hugo theming. Every page template should define the `main` block. Other optional blocks (like `scripts` or `head-extra`) can be defined for pages that need extra resources.

Rules:
- Every page template (`single.html`, `list.html`, `index.html`) must start with `{{ define "main" }}` and end with `{{ end }}`.
- `baseof.html` is the only template that outputs the full HTML document structure.
- Never duplicate `<html>`, `<head>`, or `<body>` tags in child templates.

---

## Partials

Partials are reusable template fragments. They receive a context (usually `.`, the current page) and can return values.

Guidelines:
- Keep partials focused — one job per partial.
- Name them descriptively: `hero.html`, not `section1.html`.
- Pass the minimum context needed. Usually `.` is fine, but for generic partials, consider passing a dict: `{{ partial "cta.html" (dict "heading" "Ready to start?" "link" "/contact/") }}`.
- Partials are the right tool for repeated UI components. Shortcodes are the right tool for content authors to embed components in Markdown.

---

## Static Assets vs Hugo Pipes

**`static/` directory**: Files copied verbatim to the output. No processing. Use for favicons, `robots.txt`, `CNAME`, and any file that must not be modified.

**`assets/` directory**: Files available for processing by Hugo Pipes. Use for CSS (fingerprinting, minification), JS (bundling, minification), and images (resizing, format conversion).

To process CSS through Hugo Pipes:

```go-html-template
{{- $css := resources.Get "css/main.css" -}}
{{- $css = $css | minify | fingerprint -}}
<link rel="stylesheet" href="{{ $css.RelPermalink }}" integrity="{{ $css.Data.Integrity }}">
```

This gives you cache-busting hashes and minified output without any build tools.
