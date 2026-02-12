---
name: hugo-page
description: Create and structure new pages and content within an existing Hugo site. Use this skill whenever the user asks to add a new page, blog post, service page, landing page, or any content file to the site. Also trigger when the user provides a screenshot or mockup as layout/content inspiration for a new page. Also trigger when the user asks about front matter, content organization, page SEO, meta descriptions, internal linking, or how to structure a specific type of page. This skill covers content creation only — Markdown files, front matter, and shortcode usage. It does not create or modify theme templates, partials, or CSS. If theme-level work is needed, use the hugo-theme-skill instead.
---

# Hugo Page Creator

This skill handles the creation and structuring of new content pages within an existing Hugo site. The output is always Markdown files with front matter — not template files, not CSS, not partials. The goal is well-structured, SEO-ready content that works with the existing theme.

## Before You Start

Read the relevant reference file(s) based on what you're working on:

- **Structuring a specific page type** (homepage, service, about, contact, blog post) → Read `references/content-patterns.md`
- **Writing or reviewing front matter** → Read `references/front-matter.md`
- **Applying SEO best practices** → Read `references/seo.md`

For any new page, read all three.

## Skill Boundary

This skill is strictly about **content** — what goes inside `content/`. It does not:

- Create or modify files in `layouts/`, `themes/`, or `assets/`
- Add new partials, shortcodes, or base templates
- Write CSS or change visual design

If the user's request requires new template or theme work, flag it clearly and recommend using the `hugo-theme-skill` for that portion.

## Core Principles

### 1. Content and Structure Over Presentation
Write content that is meaningful independent of its visual presentation. Headings create document hierarchy. Paragraphs communicate clearly. Shortcodes handle reusable content blocks. The theme handles how it looks — this skill handles what it says and how it is organized.

### 2. One Clear Purpose Per Page
Every page should have a single, clear purpose. A service page sells one service. A blog post covers one topic. A landing page drives one action. If a page is trying to do too many things, it should be split.

### 3. SEO Is Built In, Not Bolted On
Every page must have a strong `title`, a compelling `description`, a logical heading hierarchy, and relevant internal links. These are not optional — they are part of every page created with this skill. Read `references/seo.md` before writing front matter for any page.

### 4. Content for Real Users First
Write for the person reading the page, not for search engines. Clear language, short paragraphs, scannable structure, and a logical flow of information serve both users and SEO simultaneously. Keyword stuffing and unnatural phrasing are always wrong.

### 5. Shortcodes for Structure, Markdown for Content
Use Hugo shortcodes for structural content blocks (CTAs, testimonials, service cards, contact info). Use Markdown for prose. Never write raw HTML in Markdown files unless there is no shortcode available and the HTML is genuinely necessary.

## Page Creation Workflow

Follow this sequence for every new page:

1. **Identify the page type** — What is this page? (Service, about, blog post, landing page, contact.) Each type has conventions — see `references/content-patterns.md`.
2. **Define the SEO target** — What is the primary search intent this page serves? What keyword or phrase should it rank for? This informs the title, description, headings, and URL slug.
3. **Plan the content sections** — What sections does this page need? List them before writing anything. A service page might need: hero headline, problem statement, solution/offering, how it works, social proof, CTA.
4. **Write front matter** — Title, description, date, slug, and any page-specific params. See `references/front-matter.md`.
5. **Write the content** — Markdown prose with proper heading hierarchy. Use shortcodes for structural blocks.
6. **Review the SEO checklist** — Verify against `references/seo.md` before considering the page done.

## Screenshot → Page Workflow

When the user provides a screenshot or mockup as inspiration for a new page:

1. **Extract content sections, not visual design** — Identify what *content* exists on the page: headlines, body copy blocks, feature lists, testimonials, CTAs, images, forms. Ignore colors, fonts, and layout specifics — those belong to the theme.

2. **Inventory the sections** — List every distinct content block you see. Confirm the list with the user before writing anything.

3. **Identify shortcode opportunities** — Any repeated or structured content (testimonials, service cards, CTA buttons, icon+text pairs) should map to a shortcode rather than inline Markdown. Call out which shortcodes are needed — if they don't exist in the theme yet, flag them for the user to create via `hugo-theme-skill`.

4. **Map to page structure** — Translate the visual layout into a logical content hierarchy:
   - Big headline → `# H1` (or `title` in front matter)
   - Sub-sections → `## H2` headings
   - Supporting detail → `### H3` headings
   - Repeated items → shortcode invocations

5. **Draft the page** — Write the front matter and Markdown body. Use placeholder copy where the user hasn't provided real content, and mark every placeholder explicitly with `[PLACEHOLDER: description]`.

6. **Do not invent content** — Never fabricate business details, phone numbers, addresses, testimonial text, or service descriptions. If it isn't in the screenshot or provided by the user, use a placeholder.

## Content Writing Standards

- **Headings**: One `## H1` equivalent per page (handled via `title` in front matter — do not add a second `# H1` in the Markdown body unless the theme explicitly requires it). Use `##` for main sections, `###` for subsections.
- **Paragraphs**: 2–4 sentences. Short. Scannable.
- **Lists**: Use bullet lists for 3+ parallel items. Use numbered lists only for sequential steps.
- **Links**: Use descriptive anchor text. Never "click here" or "read more" as standalone link text.
- **Images**: Every image reference must include alt text. Reference images as `/images/filename.webp`. Do not embed images from external URLs.

## What NOT to Do

- Do not create or edit files outside of `content/`.
- Do not write raw HTML in Markdown unless absolutely necessary and no shortcode exists.
- Do not invent business-specific content — use placeholders.
- Do not create a page without a `title` and `description` in its front matter.
- Do not use `# H1` headings in the Markdown body if the theme renders the page `title` as the H1 (which PaperMod and most Hugo themes do).
- Do not add tracking parameters, affiliate links, or external scripts.
- Do not keyword-stuff titles or descriptions.
