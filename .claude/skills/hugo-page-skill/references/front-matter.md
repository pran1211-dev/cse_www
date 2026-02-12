# Front Matter Reference for Hugo Pages

This document covers front matter conventions, required and optional fields, and page-type-specific params for the Hugo site. All front matter uses TOML format with `+++` delimiters.

## Table of Contents

1. [Required Fields](#required-fields)
2. [Common Optional Fields](#common-optional-fields)
3. [Page Params](#page-params)
4. [Taxonomy Fields](#taxonomy-fields)
5. [Front Matter by Page Type](#front-matter-by-page-type)
6. [Front Matter Checklist](#front-matter-checklist)

---

## Required Fields

Every page must have these fields. No exceptions.

```toml
+++
title = "Page Title Here"
description = "140–160 character meta description for SEO and social sharing."
date = 2026-02-11
draft = false
+++
```

| Field | Purpose | Rules |
|---|---|---|
| `title` | Page title, H1, `<title>` tag | 50–60 chars, lead with keyword, unique per page |
| `description` | Meta description and og:description | 140–160 chars, unique per page, soft CTA |
| `date` | Publication date | ISO 8601 format: `YYYY-MM-DD` |
| `draft` | Exclude from builds | Always `false` for published content |

---

## Common Optional Fields

Add these when relevant.

```toml
+++
title = "Page Title"
description = "Page description."
date = 2026-02-11
draft = false

slug = "custom-url-slug"         # Override the URL. Use when filename isn't ideal.
weight = 1                        # Controls ordering in lists. Lower = earlier.
lastmod = 2026-02-11             # Last modified date. Update when content changes.
+++
```

| Field | Purpose | When to use |
|---|---|---|
| `slug` | Override the URL path | When the filename doesn't make a clean URL |
| `weight` | Ordering in list pages | Services, nav items, any ordered content |
| `lastmod` | Last modification date | Updated evergreen pages — signals freshness to search engines |

---

## Page Params

Additional configuration under `[params]` that templates may use.

### Cover / Hero Image

```toml
[params]
  cover = "images/page-cover.webp"       # og:image and page hero image
  cover_alt = "Descriptive alt text"     # Alt text for the cover image
```

- Image should be 1200×630px for correct Open Graph display
- Always include `cover_alt` when setting `cover`
- Place the image in `static/images/` (or `assets/images/` if Hugo Pipes processing is needed)

### Hero Section Params

```toml
[params]
  hero_heading = "Custom hero headline"   # Override title for the visual hero
  hero_subheading = "Supporting line"     # Hero subheadline
  hero_image = "images/hero.webp"         # Background or foreground hero image
  hero_image_alt = "Alt text"
```

Use `hero_heading` when the SEO title and the visual headline should differ. The `title` is optimized for search; the `hero_heading` can be more conversational.

### Call to Action Params

```toml
[params]
  show_cta = true
  cta_text = "Get a Free Estimate"        # Button label
  cta_link = "/contact/"                  # Destination URL
  cta_heading = "Ready to Get Started?"  # Optional CTA section heading
  cta_subtext = "We respond within 24 hours."  # Optional supporting line
```

### Schema / Structured Data

```toml
[params]
  schema_type = "Service"         # Signals which JSON-LD schema to render
                                  # Options: Service, FAQPage, BlogPosting, LocalBusiness
```

### No-Index (Campaign Landing Pages)

```toml
[params]
  noindex = true    # Prevents search engine indexing — use for paid campaign pages
```

---

## Taxonomy Fields

Used for blog posts and content that benefits from categorization.

```toml
tags = ["plumbing", "maintenance", "how-to"]
categories = ["Guides"]
```

**Rules:**
- `tags` are granular topics. Use 2–5 per post.
- `categories` are broad groupings. Use sparingly — 1 per post is usually enough.
- Keep tags consistent across posts. Don't create near-duplicate tags (`how-to`, `how to`, `howto`).
- Tags become archive pages in Hugo — they have SEO value if used consistently.

---

## Front Matter by Page Type

### Homepage (`content/_index.md`)

```toml
+++
title = "Business Name | Primary Keyword"
description = "140–160 char description of what the business does and where."
date = 2026-02-11
draft = false

[params]
  hero_heading = "Customer-facing headline"
  hero_subheading = "Supporting line"
  hero_image = "images/hero.webp"
  hero_image_alt = "Descriptive alt text"
  show_cta = true
  cta_text = "Get a Free Estimate"
  cta_link = "/contact/"
  cover = "images/og-home.webp"
  cover_alt = "Alt text for social sharing image"
+++
```

### Service Page (`content/services/service-name.md`)

```toml
+++
title = "Service Name in Location | Business Name"
description = "140–160 char description targeting the service and location."
date = 2026-02-11
draft = false
slug = "service-name-location"
weight = 1

[params]
  cover = "images/services/service-name.webp"
  cover_alt = "Alt text"
  schema_type = "Service"
  show_cta = true
  cta_text = "Get a Free Estimate"
  cta_link = "/contact/"
+++
```

### Services Landing Page (`content/services/_index.md`)

```toml
+++
title = "Services | Business Name"
description = "Overview description of the service categories offered."
date = 2026-02-11
draft = false

[params]
  cover = "images/services-overview.webp"
  cover_alt = "Alt text"
+++
```

### About Page (`content/about/_index.md`)

```toml
+++
title = "About Us | Business Name"
description = "Learn about Business Name — who we are, our story, and why clients trust us."
date = 2026-02-11
draft = false

[params]
  cover = "images/about-team.webp"
  cover_alt = "The Business Name team"
+++
```

### Contact Page (`content/contact/_index.md`)

```toml
+++
title = "Contact Us | Business Name"
description = "Get in touch with Business Name. Call, email, or fill out our form. We respond within one business day."
date = 2026-02-11
draft = false

[params]
  schema_type = "LocalBusiness"
+++
```

### Blog Post (`content/posts/post-title.md`)

```toml
+++
title = "Post Title Matching Search Query"
description = "140–160 char description. What will the reader learn or get from this post?"
date = 2026-02-11
lastmod = 2026-02-11
draft = false
slug = "post-url-slug"
tags = ["tag1", "tag2"]

[params]
  cover = "images/posts/post-slug.webp"
  cover_alt = "Descriptive alt text for the post image"
  schema_type = "BlogPosting"
+++
```

### FAQ Page (`content/faq/_index.md`)

```toml
+++
title = "FAQ | Business Name"
description = "Answers to frequently asked questions about [service area] — pricing, availability, licensing, and more."
date = 2026-02-11
draft = false

[params]
  schema_type = "FAQPage"
+++
```

### Campaign Landing Page (`content/landing/campaign-name.md`)

```toml
+++
title = "Offer or Campaign Headline"
description = "Campaign-specific description matching the ad or email that drove traffic here."
date = 2026-02-11
draft = false
slug = "campaign-slug"

[params]
  noindex = true
  show_cta = true
  cta_text = "Claim Offer"
  cta_link = "/contact/"
+++
```

---

## Front Matter Checklist

Before finalizing front matter for any page:

- [ ] `title` is set, 50–60 characters, leads with primary keyword
- [ ] `description` is set, 140–160 characters, unique to this page
- [ ] `date` is set in `YYYY-MM-DD` format
- [ ] `draft` is `false` for published content
- [ ] `slug` is set if the filename doesn't produce a clean URL
- [ ] `weight` is set if the page appears in an ordered list
- [ ] `cover` image is referenced and `cover_alt` is filled in
- [ ] `schema_type` is set for pages that warrant structured data
- [ ] `tags` are set for blog posts (2–5 consistent tags)
- [ ] `noindex = true` is set for campaign landing pages that should not be indexed
