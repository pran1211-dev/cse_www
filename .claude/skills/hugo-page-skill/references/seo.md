# SEO Reference for Hugo Pages

This document covers on-page SEO best practices for Hugo content pages. Every page created with the hugo-page-skill should be validated against this reference before it is considered complete.

## Table of Contents

1. [Title Tag](#title-tag)
2. [Meta Description](#meta-description)
3. [URL Slug](#url-slug)
4. [Heading Hierarchy](#heading-hierarchy)
5. [Internal Linking](#internal-linking)
6. [Image SEO](#image-seo)
7. [Structured Data (JSON-LD)](#structured-data-json-ld)
8. [Open Graph and Social Meta](#open-graph-and-social-meta)
9. [Content Quality Signals](#content-quality-signals)
10. [SEO Checklist](#seo-checklist)

---

## Title Tag

The `title` in front matter becomes the `<title>` tag in the HTML head. It is the single most important on-page SEO element.

**Rules:**
- 50–60 characters including the site name suffix (e.g., `Page Title | Business Name`)
- Lead with the primary keyword — don't bury it at the end
- Be specific and descriptive. "Plumbing Services in Austin, TX" outperforms "Our Services"
- Each page must have a unique title — never duplicate across pages
- Write for the human scanning search results, not just for algorithms

**Good examples:**
```
title = "Commercial Plumbing Services in Austin, TX"
title = "About Us | Green River Landscaping"
title = "How to Winterize Your Sprinkler System | Green River Landscaping"
```

**Bad examples:**
```
title = "Services"            # Too generic, no keyword
title = "Home"                # Wasted opportunity
title = "Page 1"              # Meaningless
```

---

## Meta Description

The `description` in front matter becomes the `<meta name="description">` tag. It directly affects click-through rate from search results.

**Rules:**
- 140–160 characters
- Include the primary keyword naturally
- Write it as a value proposition: what will the reader get from this page?
- Include a soft call to action where appropriate ("Learn how...", "Find out...", "Get a free quote...")
- Every page must have a unique description — never duplicate
- Do not stuff keywords — write one clear sentence or two short ones

**Good examples:**
```
description = "Professional commercial plumbing services in Austin, TX. Licensed plumbers available 24/7 for repairs, installations, and inspections. Call for a free estimate."

description = "Learn how to winterize your sprinkler system before the first freeze. Step-by-step guide from our certified irrigation specialists."
```

**Bad examples:**
```
description = "Services services services plumbing plumbing Austin plumber"  # Keyword stuffing
description = "This is our services page."                                    # No value, too short
```

---

## URL Slug

The slug is set via the `slug` param in front matter or derived from the filename. Keep it clean and keyword-rich.

**Rules:**
- All lowercase, words separated by hyphens
- Include the primary keyword
- Short and descriptive — 3-5 words is ideal
- No stop words unless necessary for readability (`the`, `a`, `and`, `of`)
- Never use underscores — always hyphens
- Never use dates in slugs for evergreen content (dates cause content to feel stale)

**Good examples:**
```
slug = "commercial-plumbing-austin"
slug = "winterize-sprinkler-system"
slug = "landscaping-services"
```

**Bad examples:**
```
slug = "page-1"
slug = "our_services_page"       # Underscores
slug = "2024-01-15-blog-post"    # Date in evergreen content slug
```

---

## Heading Hierarchy

Heading structure communicates page organization to both users and search engines. Violations hurt accessibility and SEO equally.

**Rules:**
- One H1 per page. In Hugo with most themes (including PaperMod), the `title` front matter value renders as the H1 — **do not add a `#` H1 in the Markdown body**.
- `##` (H2) for major sections of the page
- `###` (H3) for subsections within an H2
- Never skip levels (no H2 → H4 jumps)
- Include the primary or secondary keyword in at least one H2
- Headings should describe the content beneath them — not be clever or vague

**Good heading structure for a service page:**
```markdown
## What We Do                      <!-- H2: overview -->
### Residential Plumbing            <!-- H3: subcategory -->
### Commercial Plumbing             <!-- H3: subcategory -->
## Why Choose Us                   <!-- H2: differentiators -->
## Service Areas                   <!-- H2: local SEO signal -->
## Frequently Asked Questions      <!-- H2: captures long-tail queries -->
```

---

## Internal Linking

Internal links distribute authority across the site, help users navigate, and signal content relationships to search engines.

**Rules:**
- Every page should link to at least 1-2 other relevant pages on the site
- The contact page and primary service pages are high-value destinations — link to them from relevant content
- Use descriptive anchor text that reflects the destination page's topic
- Never use "click here", "read more", or "learn more" as the sole anchor text
- Blog posts should link to relevant service pages (this is a direct SEO benefit)
- Service pages should link to related services and the contact page

**Good internal links:**
```markdown
If you're ready to get started, [request a free plumbing estimate](/contact/).
We also offer [commercial HVAC maintenance](/services/hvac-maintenance/) for larger properties.
```

**Bad internal links:**
```markdown
[Click here](/contact/) to contact us.
For more information, [read more](/services/).
```

---

## Image SEO

Every image on a page must be properly attributed for accessibility and SEO.

**Rules:**
- Every `<img>` must have a descriptive `alt` attribute
- Alt text should describe what is in the image, including relevant keywords where natural
- Do not use `alt=""` unless the image is purely decorative
- Use descriptive filenames: `commercial-plumbing-installation.webp` not `IMG_2045.jpg`
- Reference images from `/images/` (static) or `assets/images/` (processed)
- Add `loading="lazy"` to images below the fold (handled by the theme in most cases)

**Good alt text:**
```markdown
![Licensed plumber installing commercial pipes in Austin TX](/images/commercial-plumbing-install.webp)
```

**Bad alt text:**
```markdown
![image](/images/photo1.jpg)
![](/images/photo1.jpg)
![plumbing plumber pipes Austin commercial residential](/images/photo1.jpg)  # Keyword stuffing
```

---

## Structured Data (JSON-LD)

Structured data helps search engines understand page content and can unlock rich results (star ratings, FAQ dropdowns, breadcrumbs) in search results.

For a Hugo small business site, the most valuable schema types are:

### Organization (site-wide, in head.html)
Should already be in the theme's `head.html` partial. Includes business name, URL, logo, contact info, social profiles.

### LocalBusiness (homepage or contact page)
```json
{
  "@context": "https://schema.org",
  "@type": "LocalBusiness",
  "name": "Business Name",
  "url": "https://example.com",
  "telephone": "+1-555-000-0000",
  "address": {
    "@type": "PostalAddress",
    "streetAddress": "123 Main St",
    "addressLocality": "Austin",
    "addressRegion": "TX",
    "postalCode": "78701"
  }
}
```

### Service (individual service pages)
```json
{
  "@context": "https://schema.org",
  "@type": "Service",
  "name": "Commercial Plumbing",
  "provider": {
    "@type": "LocalBusiness",
    "name": "Business Name"
  },
  "description": "Licensed commercial plumbing services in Austin, TX.",
  "areaServed": "Austin, TX"
}
```

### FAQPage (pages with FAQ sections)
```json
{
  "@context": "https://schema.org",
  "@type": "FAQPage",
  "mainEntity": [{
    "@type": "Question",
    "name": "How much does commercial plumbing cost?",
    "acceptedAnswer": {
      "@type": "Answer",
      "text": "Costs vary by job scope. Contact us for a free estimate."
    }
  }]
}
```

### Article / BlogPosting (blog posts)
```json
{
  "@context": "https://schema.org",
  "@type": "BlogPosting",
  "headline": "How to Winterize Your Sprinkler System",
  "datePublished": "2026-02-11",
  "author": {
    "@type": "Organization",
    "name": "Business Name"
  }
}
```

In Hugo, inject structured data via a page-level front matter param and a partial that renders it in the `<head>`. Flag this need when creating a page that warrants it.

---

## Open Graph and Social Meta

PaperMod handles Open Graph tags automatically from standard front matter fields. Ensure these are always set:

```toml
title = "Page Title"                        # → og:title
description = "Page description."          # → og:description

[params]
  cover = "images/og-image.jpg"            # → og:image (1200×630px recommended)
```

**Rules:**
- The `cover` image should be 1200×630px for correct display on all platforms
- If no cover is set, most themes fall back to a site-level default — set one in `hugo.toml`
- The description doubles as both the meta description and `og:description` in PaperMod

---

## Content Quality Signals

Search engines evaluate content quality beyond keywords. These factors matter:

- **Word count**: Service pages should be at least 300 words. Blog posts covering a topic substantively should be 600–1500 words. Thin content (under 200 words) may be penalized.
- **Readability**: Short sentences, common words, clear structure. Aim for a reading level accessible to a broad audience.
- **Topical completeness**: Cover the topic thoroughly. An FAQ section, a "how it works" section, and service area details all add depth to service pages.
- **Freshness**: Blog posts should have accurate `date` and `lastmod` values. Evergreen pages that are updated should have their `lastmod` updated.
- **No duplicate content**: Every page must have unique content. Do not copy-paste descriptions across service pages with minor changes.

---

## SEO Checklist

Run through this before every page is considered complete:

- [ ] `title` is 50–60 characters, leads with the primary keyword, is unique
- [ ] `description` is 140–160 characters, includes keyword naturally, has a soft CTA, is unique
- [ ] `slug` is lowercase, hyphenated, keyword-rich, and clean
- [ ] Heading hierarchy is logical — no H1 in Markdown body, H2s cover main sections
- [ ] At least one H2 contains a primary or secondary keyword
- [ ] Page includes 1–2 internal links with descriptive anchor text
- [ ] All images have descriptive `alt` text and keyword-relevant filenames
- [ ] Open Graph fields are set (`title`, `description`, `cover` image)
- [ ] Structured data type is identified and flagged if needed (LocalBusiness, Service, FAQ, BlogPosting)
- [ ] Word count is appropriate for the page type
- [ ] No duplicate content from other pages on the site
