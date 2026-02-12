# Go Template Reference for Hugo

This document covers the Go template syntax and patterns you'll use when building Hugo themes. It focuses on practical, real-world usage rather than exhaustive documentation.

## Table of Contents

1. [Basics](#basics)
2. [Variables and Context](#variables-and-context)
3. [Conditionals](#conditionals)
4. [Loops and Ranges](#loops-and-ranges)
5. [Accessing Data](#accessing-data)
6. [Pipes and Functions](#pipes-and-functions)
7. [Partial Templates](#partial-templates)
8. [Common Patterns](#common-patterns)
9. [Common Pitfalls](#common-pitfalls)

---

## Basics

Go templates use `{{ }}` delimiters. Use `{{- -}}` (with hyphens) to trim surrounding whitespace — this keeps the generated HTML clean.

```go-html-template
{{- /* This is a comment */ -}}
{{- .Title -}}
```

---

## Variables and Context

The dot (`.`) is the current context. At the top level of a page template, `.` is the Page object.

```go-html-template
{{ .Title }}          {{/* Page title */}}
{{ .Content }}        {{/* Rendered Markdown content */}}
{{ .RelPermalink }}   {{/* Relative URL of the page */}}
{{ .Date }}           {{/* Page date */}}
{{ .Site.Title }}     {{/* Site title from config */}}
```

Custom variables use `$`:

```go-html-template
{{- $heading := .Title -}}
{{- $hasHero := .Param "hero_image" -}}
```

Inside `range` and `with` blocks, `.` changes to the scoped value. Use `$` to access the top-level page context:

```go-html-template
{{- range .Pages -}}
  {{/* . is now the current page in the range */}}
  <a href="{{ .RelPermalink }}">{{ .Title }}</a>
  {{/* $ refers to the top-level page context */}}
  <span>From: {{ $.Title }}</span>
{{- end -}}
```

---

## Conditionals

### if / else

```go-html-template
{{- if .Param "show_cta" -}}
  {{- partial "cta.html" . -}}
{{- end -}}

{{- if .Params.hero_image -}}
  {{/* has hero */}}
{{- else -}}
  {{/* no hero */}}
{{- end -}}
```

### with

`with` is like `if`, but also rebinds `.` to the value. It's the idiomatic way to handle optional values:

```go-html-template
{{- with .Param "description" -}}
  <meta name="description" content="{{ . }}">
{{- end -}}
```

This is safer and cleaner than `{{ if .Param "description" }}...{{ .Param "description" }}...{{ end }}`.

### Comparison operators

```go-html-template
{{- if eq .Section "blog" -}}         {{/* equals */}}
{{- if ne .Kind "taxonomy" -}}        {{/* not equals */}}
{{- if gt (len .Pages) 0 -}}          {{/* greater than */}}
{{- if and .Title .Description -}}    {{/* logical and */}}
{{- if or .Params.cta_text .Params.cta_link -}}  {{/* logical or */}}
{{- if not .Draft -}}                 {{/* logical not */}}
```

---

## Loops and Ranges

### Basic range

```go-html-template
{{- range .Pages -}}
  <article>
    <h2><a href="{{ .RelPermalink }}">{{ .Title }}</a></h2>
    <p>{{ .Summary }}</p>
  </article>
{{- end -}}
```

### Range with limits

```go-html-template
{{/* First 3 pages */}}
{{- range first 3 .Pages -}}
  ...
{{- end -}}

{{/* Pages ordered by weight */}}
{{- range .Pages.ByWeight -}}
  ...
{{- end -}}

{{/* Pages ordered by date, newest first */}}
{{- range .Pages.ByDate.Reverse -}}
  ...
{{- end -}}
```

### Range with index

```go-html-template
{{- range $index, $page := .Pages -}}
  <div class="item {{ if eq $index 0 }}item--featured{{ end }}">
    {{ $page.Title }}
  </div>
{{- end -}}
```

### Range over params (slices, maps)

```go-html-template
{{/* Range over a list in front matter */}}
{{- range .Params.features -}}
  <li>{{ . }}</li>
{{- end -}}

{{/* Range over a map */}}
{{- range $key, $value := .Params.social -}}
  <a href="{{ $value }}">{{ $key }}</a>
{{- end -}}
```

---

## Accessing Data

### Page variables

```go-html-template
{{ .Title }}             {{/* Title from front matter */}}
{{ .Description }}       {{/* Description from front matter */}}
{{ .Content }}           {{/* Rendered Markdown */}}
{{ .Summary }}           {{/* Auto-generated or manual summary */}}
{{ .WordCount }}         {{/* Word count of content */}}
{{ .ReadingTime }}       {{/* Estimated reading time in minutes */}}
{{ .Date }}              {{/* Page date */}}
{{ .Lastmod }}           {{/* Last modification date */}}
{{ .RelPermalink }}      {{/* Relative URL */}}
{{ .Permalink }}         {{/* Absolute URL */}}
{{ .Section }}           {{/* Top-level section name */}}
{{ .Kind }}              {{/* page, section, home, taxonomy, term */}}
{{ .IsHome }}            {{/* Boolean: is this the homepage? */}}
```

### Front matter params

```go-html-template
{{ .Params.hero_image }}       {{/* Direct access (nil if missing) */}}
{{ .Param "hero_image" }}      {{/* Falls back to site params if missing */}}
```

Always prefer `.Param` over `.Params` for values that might have site-level defaults.

### Site-level variables

```go-html-template
{{ .Site.Title }}               {{/* From hugo.toml */}}
{{ .Site.Params.description }}  {{/* From [params] in hugo.toml */}}
{{ .Site.BaseURL }}             {{/* Site base URL */}}
{{ .Site.Menus.main }}          {{/* Main menu from config */}}
```

### Site data files

Data from files in `data/` directory:

```go-html-template
{{/* data/testimonials.toml */}}
{{- range .Site.Data.testimonials -}}
  <blockquote>{{ .quote }}</blockquote>
  <cite>{{ .author }}</cite>
{{- end -}}
```

---

## Pipes and Functions

Go templates use Unix-style pipes:

```go-html-template
{{ .Title | upper }}                  {{/* UPPERCASE TITLE */}}
{{ .Title | truncate 50 }}            {{/* Truncate to 50 chars */}}
{{ .Date | time.Format "Jan 2, 2006" }}  {{/* Format date */}}
{{ .Summary | plainify }}             {{/* Strip HTML tags */}}
```

### Useful string functions

```go-html-template
{{ lower .Title }}
{{ upper .Title }}
{{ title .Title }}                    {{/* Title Case */}}
{{ truncate 100 .Summary }}
{{ replace .Title " " "-" }}
{{ htmlUnescape .Content }}
{{ markdownify "**bold** text" }}     {{/* Render Markdown inline */}}
{{ safeHTML .Params.embed_code }}     {{/* Mark as safe HTML */}}
```

### URL and path functions

```go-html-template
{{ relURL "css/main.css" }}           {{/* Relative to baseURL */}}
{{ absURL "images/logo.png" }}        {{/* Absolute URL */}}
{{ ref . "about" }}                   {{/* Internal link by path */}}
```

### Collection functions

```go-html-template
{{ len .Pages }}                      {{/* Count items */}}
{{ first 5 .Pages }}                  {{/* First N items */}}
{{ after 5 .Pages }}                  {{/* Skip first N */}}
{{ where .Pages "Section" "blog" }}   {{/* Filter by field */}}
{{ sort .Pages "Weight" }}            {{/* Sort */}}
{{ shuffle .Pages }}                  {{/* Randomize */}}
```

---

## Partial Templates

### Basic usage

```go-html-template
{{- partial "header.html" . -}}
```

### Passing custom context

```go-html-template
{{- partial "cta.html" (dict
  "heading" "Ready to get started?"
  "text" "Contact us today for a free consultation."
  "link" "/contact/"
  "button_text" "Get in Touch"
) -}}
```

Inside the partial, access with `.heading`, `.text`, etc.

### Partials that return values

```go-html-template
{{/* partials/has-hero.html */}}
{{- return (and .Params.hero_image .Params.hero_heading) -}}

{{/* Using it */}}
{{- if partial "has-hero.html" . -}}
  {{- partial "hero.html" . -}}
{{- end -}}
```

---

## Common Patterns

### Navigation menu

```go-html-template
<nav aria-label="Main navigation">
  <ul>
    {{- range .Site.Menus.main -}}
      <li>
        <a href="{{ .URL }}"
           {{ if $.IsMenuCurrent "main" . }}aria-current="page"{{ end }}>
          {{- .Name -}}
        </a>
      </li>
    {{- end -}}
  </ul>
</nav>
```

### Conditional CSS class

```go-html-template
<body class="{{ if .IsHome }}home{{ else }}inner{{ end }} page-{{ .Kind }}">
```

### Default values

```go-html-template
{{- $description := .Param "description" | default .Summary | default .Site.Params.description -}}
```

### Scratch pad (for accumulating values across scopes)

```go-html-template
{{- .Scratch.Set "count" 0 -}}
{{- range .Pages -}}
  {{- if .Params.featured -}}
    {{- $.Scratch.Add "count" 1 -}}
  {{- end -}}
{{- end -}}
<p>Featured items: {{ .Scratch.Get "count" }}</p>
```

---

## Common Pitfalls

### 1. Forgetting context changes inside `range` and `with`
Inside these blocks, `.` is rebound. Use `$` to reach the page context.

### 2. Whitespace in output
Use `{{- -}}` aggressively. Without it, you'll get unexpected blank lines in your HTML.

### 3. Nil values
Accessing a nonexistent param directly (`.Params.something`) returns nil. Piping nil into a function that expects a string will error. Use `with` or `default` to guard:

```go-html-template
{{/* Bad — errors if .Params.subtitle is nil */}}
<h2>{{ .Params.subtitle | upper }}</h2>

{{/* Good — only renders if subtitle exists */}}
{{- with .Params.subtitle -}}<h2>{{ . | upper }}</h2>{{- end -}}
```

### 4. Comparing types
Go templates are strictly typed. `eq .WordCount "100"` won't work because `.WordCount` is an int and `"100"` is a string. Use `eq .WordCount 100`.

### 5. Date formatting
Go uses a reference date format: `Mon Jan 2 15:04:05 MST 2006`. This specific date is the format key — it's not arbitrary. `"January 2, 2006"` means "full month name, day, four-digit year."

```go-html-template
{{ .Date | time.Format "January 2, 2006" }}  {{/* February 11, 2026 */}}
{{ .Date | time.Format "2006-01-02" }}        {{/* 2026-02-11 */}}
```
