# Content Patterns for Hugo Pages

This document covers the standard content structure for each page type a small business Hugo site typically needs. Use these as the starting point for any new page, then adapt based on the specific business and user's requirements.

## Table of Contents

1. [Homepage](#homepage)
2. [Service Page (Individual)](#service-page-individual)
3. [Services Landing Page](#services-landing-page)
4. [About Page](#about-page)
5. [Contact Page](#contact-page)
6. [Blog Post](#blog-post)
7. [Landing Page (Campaign)](#landing-page-campaign)
8. [FAQ Page](#faq-page)

---

## Homepage

**File:** `content/_index.md`
**Template used:** `layouts/index.html`
**Purpose:** First impression. Communicate what the business does, for whom, and why they should stay.

**Section order:**
1. **Hero** — Headline, subheadline, primary CTA button. The headline should answer: "What do you do and for whom?" in one sentence.
2. **Value proposition / intro** — 2–3 sentences expanding on the hero. Address the customer's problem and hint at the solution.
3. **Services overview** — 3–6 cards summarizing the main service offerings, each linking to its service page.
4. **Why choose us / differentiators** — 3–4 key differentiators. Short, specific, and credible (not "we're the best").
5. **Social proof** — Testimonials, client logos, or a review count. At least one real quote.
6. **Secondary CTA** — A second prompt to contact or get a quote, positioned after social proof.
7. **Service area / local signal** — One short paragraph or list of service areas. Valuable for local SEO.

**Content notes:**
- The homepage headline is the highest-value copywriting on the site. Make it specific.
- "Welcome to our website" is never an acceptable headline.
- Every section should flow logically into the next.

**Example structure:**
```markdown
+++
title = "Austin's Trusted Commercial Plumber"
description = "Green River Plumbing provides licensed commercial plumbing services across Austin, TX. Available 24/7 for repairs, installations, and inspections."
[params]
  hero_heading = "Commercial Plumbing That Doesn't Let You Down"
  hero_subheading = "Licensed, insured, and available 24/7 across Austin and surrounding areas."
  show_cta = true
  cta_text = "Get a Free Estimate"
  cta_link = "/contact/"
+++

[Services overview shortcode or Markdown section]

## Why Austin Businesses Choose Us

- **Licensed and insured** — All work meets Texas state code.
- **24/7 emergency response** — We pick up the phone.
- **Upfront pricing** — No surprise invoices.
- **10+ years serving Austin** — We know local codes and conditions.

[Testimonials shortcode]

## Serving Austin and Beyond

We provide commercial plumbing services throughout Austin, Round Rock, Cedar Park, and the greater Central Texas area.

{{< cta heading="Ready to Get Started?" text="Contact us today for a free, no-obligation estimate." link="/contact/" button="Request a Quote" >}}
```

---

## Service Page (Individual)

**File:** `content/services/service-name.md`
**Template used:** `layouts/services/single.html` or `layouts/_default/single.html`
**Purpose:** Convert a visitor who is considering this specific service.

**Section order:**
1. **Service headline** — Set via `title` in front matter. Specific and keyword-rich.
2. **Opening paragraph** — What is this service, and who needs it? 2–3 sentences.
3. **The problem** — Briefly describe the pain point or challenge this service solves. Makes the visitor feel understood.
4. **What we offer** — The actual service details. What does it include? What does the process look like? Use a list or `### subsections` for clarity.
5. **How it works** — Numbered steps if the service has a clear process (inspection → quote → work → follow-up).
6. **Why choose us for this service** — 2–4 differentiators specific to this service.
7. **FAQ** — 3–5 questions the customer is likely to have. Each Q&A adds word count and captures long-tail search queries.
8. **Social proof** — One or two testimonials relevant to this service if available.
9. **CTA** — Clear, specific ask. "Request a free [service name] estimate."

**Content notes:**
- Minimum 400 words. A thorough service page with FAQs should reach 600–900 words.
- Do not describe every service on one page — each service gets its own page.
- Link to related services internally.

**Example structure:**
```markdown
+++
title = "Commercial Pipe Repair in Austin, TX"
description = "Fast, reliable commercial pipe repair in Austin. Licensed plumbers available 24/7 for burst pipes, leaks, and emergency repairs. Get a free estimate."
slug = "commercial-pipe-repair"
date = 2026-02-11
draft = false
[params]
  cover = "images/services/pipe-repair.webp"
+++

Whether it's a slow leak or a burst pipe, plumbing failures disrupt your business and cost you money. Our licensed commercial plumbers respond fast and fix it right the first time.

## The Cost of Ignoring Pipe Problems

[PLACEHOLDER: 2–3 sentences about consequences of delayed repairs — water damage, liability, downtime.]

## Our Commercial Pipe Repair Services

- Emergency burst pipe response
- Leak detection and repair
- Pipe relining and replacement
- Water damage assessment
- Code compliance inspections

## How It Works

1. **Call or contact us** — We respond within [X hours] for non-emergencies, faster for emergencies.
2. **On-site assessment** — A licensed plumber inspects the issue and provides a written quote.
3. **Repair** — Work is completed to Texas code with minimal disruption to your operations.
4. **Follow-up** — We walk you through the completed work and answer any questions.

## Why Green River Plumbing

- Licensed master plumbers on every commercial job
- Upfront written quotes — no surprise charges
- Available 24/7 including weekends and holidays
- Fully insured — protects you and your property

## Frequently Asked Questions

### How quickly can you respond to an emergency?
[PLACEHOLDER: Response time commitment.]

### Do you provide written quotes?
[PLACEHOLDER: Quote process description.]

### Are your plumbers licensed for commercial work in Texas?
[PLACEHOLDER: Licensing details.]

{{< cta heading="Need a Pipe Repaired?" text="Contact us for a free estimate. We respond within 24 hours." link="/contact/" button="Get a Free Estimate" >}}

**Related services:** [Commercial Drain Cleaning](/services/drain-cleaning/) · [HVAC Plumbing Integration](/services/hvac-plumbing/)
```

---

## Services Landing Page

**File:** `content/services/_index.md`
**Template used:** `layouts/services/list.html`
**Purpose:** Overview of all services — entry point for visitors who don't yet know which specific service they need.

**Section order:**
1. **Opening paragraph** — What category of services does the business offer? Who are they for?
2. **Services list/grid** — Rendered by the template from child pages. The Markdown body provides context above and below the list.
3. **Brief differentiators** — 2–3 sentences on why to choose this business, not a competitor.
4. **CTA** — Contact or get a quote.

**Content notes:**
- Keep this page relatively brief — the detail lives on each individual service page.
- The template renders child pages automatically; this page's Markdown is the framing content around that list.

```markdown
+++
title = "Commercial Plumbing Services in Austin, TX"
description = "Full-service commercial plumbing in Austin. Pipe repair, drain cleaning, water heater installation, and more. Licensed, insured, available 24/7."
+++

We offer a full range of commercial plumbing services for businesses, property managers, and contractors across Austin and Central Texas. Every job is handled by licensed, insured master plumbers — no subcontracting.

[Service cards rendered by template]

Not sure which service you need? [Contact us](/contact/) and we'll point you in the right direction.

{{< cta heading="Ready to Get a Quote?" text="Reach out and we'll get back to you within one business day." link="/contact/" button="Contact Us" >}}
```

---

## About Page

**File:** `content/about/_index.md` or `content/about.md`
**Template used:** `layouts/_default/single.html`
**Purpose:** Build trust and credibility. Help visitors feel confident choosing this business.

**Section order:**
1. **Who we are** — Brief founding story or mission statement. 2–3 sentences.
2. **Our story** — How the business started, why, what drives it. Authentic and specific beats generic.
3. **What makes us different** — Real, specific differentiators. Avoid clichés like "we're passionate about our work."
4. **The team** — Names, roles, brief bios if appropriate. A photo reference (placeholder) if the theme supports it.
5. **Credentials and certifications** — Licenses, associations, awards. Adds credibility.
6. **Service area** — Brief mention.
7. **CTA** — Invite them to reach out.

**Content notes:**
- Write in first or second person, not third ("we believe" or "our team", not "the company believes").
- Specific details build trust. Years of experience, number of projects, specific certifications.
- Avoid buzzwords: "passionate", "world-class", "innovative", "cutting-edge."

---

## Contact Page

**File:** `content/contact/_index.md`
**Template used:** `layouts/_default/single.html` or custom contact template
**Purpose:** Make it as easy as possible to get in touch.

**Section order:**
1. **Brief intro** — One sentence inviting contact. Set expectations (response time).
2. **Contact details** — Phone, email, address. Use a shortcode if the theme has one.
3. **Contact form** — If the theme supports one; handled at the theme level.
4. **Hours** — Business hours if relevant.
5. **Service area map or list** — Optional but useful for local businesses.

**Content notes:**
- This page should be short. Remove friction, don't add it.
- Make phone number and email prominent and linkable (`tel:` and `mailto:` links).
- Set clear expectations: "We respond within one business day" reduces anxiety.

```markdown
+++
title = "Contact Green River Plumbing"
description = "Get in touch with Green River Plumbing in Austin, TX. Call, email, or fill out our form for a free estimate. We respond within one business day."
+++

Have a plumbing issue or want a free estimate? Reach out — we respond within one business day.

{{< contact-info phone="+1-512-000-0000" email="hello@example.com" address="123 Main St, Austin, TX 78701" >}}

## Business Hours

Monday–Friday: 7am–6pm
Saturday: 8am–2pm
Sunday: Emergency calls only

## Service Area

We serve Austin, Round Rock, Cedar Park, Pflugerville, and surrounding Central Texas communities.
```

---

## Blog Post

**File:** `content/posts/post-slug.md`
**Template used:** `layouts/blog/single.html` or `layouts/_default/single.html`
**Purpose:** Attract organic search traffic, demonstrate expertise, support service pages with internal links.

**Section order:**
1. **Introduction** — What is this post about and why should the reader care? 2–3 sentences. No "In this blog post, we will discuss..."
2. **Body sections** — `## H2` headings for each major point. Each section: 100–250 words.
3. **Key takeaways or summary** — Brief recap for scanners.
4. **CTA** — Link to a relevant service page or contact. Every post should drive somewhere.

**Content notes:**
- Target one specific search query per post. "How to winterize a sprinkler system" beats "Plumbing tips."
- The post `title` should match the search query closely.
- Minimum 600 words for a post worth indexing. Thin posts should be drafts.
- End every post with a natural internal link to a service page.
- Use `tags` in front matter to group related posts.

**Example structure:**
```markdown
+++
title = "How to Winterize Your Sprinkler System Before the First Freeze"
description = "Protect your irrigation system from freeze damage with these step-by-step winterization tips from Austin's irrigation specialists."
date = 2026-02-11
draft = false
tags = ["irrigation", "winterization", "how-to"]
[params]
  cover = "images/posts/sprinkler-winterization.webp"
+++

When temperatures drop below freezing, an un-winterized sprinkler system can crack pipes and damage heads — repairs that run into the hundreds. Here's how to do it right before the cold arrives.

## When to Winterize in Central Texas

[PLACEHOLDER: Timing guidance specific to the region.]

## Step-by-Step: Manual Drain Method

[PLACEHOLDER: Step-by-step instructions.]

## Step-by-Step: Blowout Method

[PLACEHOLDER: Step-by-step instructions.]

## Common Mistakes to Avoid

[PLACEHOLDER: 3–4 common errors.]

## When to Call a Professional

If your system is complex, older, or you're not comfortable with the process, [our irrigation team](/services/irrigation/) can winterize your system quickly and correctly.

{{< cta heading="Need a Hand?" text="We winterize sprinkler systems across Austin every fall. Book your appointment before the rush." link="/contact/" button="Schedule Winterization" >}}
```

---

## Landing Page (Campaign)

**File:** `content/landing/campaign-name.md`
**Template used:** Custom single template (may require theme work)
**Purpose:** Drive one specific conversion action from a targeted traffic source (ad, email, direct outreach).

**Section order:**
1. **Headline** — Specific offer or value prop. Must match the ad or link the visitor clicked.
2. **Subheadline** — Reinforce the headline with a supporting detail.
3. **Key benefits** — 3–5 bullet points. What does the visitor get?
4. **Social proof** — One strong testimonial or trust signal.
5. **Form or CTA** — The conversion action. One action only — no navigation, no distractions.
6. **Trust signals** — License numbers, insurance, associations, guarantee language.

**Content notes:**
- Landing pages often strip navigation to remove distractions. Flag this for the theme if needed.
- One CTA only. Every additional link or option reduces conversions.
- The headline must match the message that brought the visitor here (ad headline, email subject).
- Set `noindex: true` in front matter if the page is for a paid campaign and shouldn't rank organically.

---

## FAQ Page

**File:** `content/faq/_index.md`
**Template used:** `layouts/_default/single.html`
**Purpose:** Answer common questions, reduce support burden, and capture long-tail search queries.

**Content notes:**
- Group questions by topic using `## H2` headings.
- Each question is an `### H3`. Answer immediately beneath — no preamble.
- Write questions as the customer would ask them, not as the business would frame them.
- Mark this page for `FAQPage` structured data in front matter.
- Link from FAQ answers to relevant service pages.

```markdown
+++
title = "Frequently Asked Questions | Green River Plumbing"
description = "Answers to common questions about our commercial plumbing services in Austin, TX — pricing, response times, licensing, and more."
[params]
  schema_type = "FAQPage"
+++

## Pricing and Estimates

### How much does commercial plumbing cost?
[PLACEHOLDER: Honest answer about pricing variables, offer free estimate.]

### Do you charge for estimates?
[PLACEHOLDER: Estimate policy.]

## Response Times

### How quickly can you respond to an emergency?
[PLACEHOLDER: SLA or commitment.]

## Licensing and Insurance

### Are your plumbers licensed in Texas?
[PLACEHOLDER: Licensing details and how to verify.]

### Are you insured?
[PLACEHOLDER: Insurance coverage details.]
```
