# Plural Newspaper (Hugo Theme) Planning Document

This document translates the README goals and clarified requirements into an implementation plan that can later be split into GitHub issues.

## 1. Project Summary

Plural Newspaper is a Hugo theme for blogs and informational websites.

Primary goals:

- Accessible by default (baseline WCAG 2.1 AA)
- Fast and lightweight (minimal JS; functional without JS)
- Responsive (mobile and desktop)
- Clean newspaper-style layout
- Three color schemes (light, dark, high contrast), based on the treblesand color scheme

## 2. Constraints and Decisions (Confirmed)

- Hugo version: `>= 0.139.4`
- License: GNU Affero General Public License (AGPL)
- Easy language content: treat as "Einfache Sprache" (DE) or "pidgin English" (EN)
  - Implemented as a dedicated *section* (content section), not merely a styling toggle
- List layout options apply to:
  - Section list pages
  - Taxonomy list pages
- Masonry: pseudo-masonry is sufficient (CSS Grid based)
- Social sharing: simple share links are sufficient (no complex JS-driven UI)
- CI/testing:
  - Whole test suite runs on `push` to `main`

## 3. Baseline Requirements

### 3.1 Content Types and Site Features

- Support Markdown content.
- Support tags and categories.
- Provide templates for:
  - Homepage / landing page
  - Section list pages (with list/grid/pseudo-masonry variants)
  - Taxonomy list pages (with list/grid/pseudo-masonry variants)
  - Single post pages
  - Static pages
- Customizable header and footer.
- Ensure that content types can be extended later.

### 3.2 Required Elements Per Blog Post

Each blog post must support these elements:

- Description (summary/lead)
- Tags
- Reading time
- Glossary (post-level glossary)

Nice-to-have (can be staged later, but should be considered in templates):

- Table of contents (ToC)

### 3.3 Widgets (MVP)

Provide configurable widgets:

- ToC
- Social links
- Recent posts
- Other posts in this category
- Fixed links

Placement is a design decision, but should be configurable (see params baseline).

### 3.4 Shortcodes

- Unknown list: define an MVP list after reviewing example content needs.
- Planning includes a discovery step and a placeholder epic for implementation.

### 3.5 JavaScript Policy

- The site must be functional without JavaScript.
  - Navigation, reading, taxonomy browsing, pagination must work.
  - Not all enhancements must work (e.g., explicit theme switching UI can be JS-enhanced).
- Any JS included should be optional and progressively enhance.

## 4. Accessibility Baseline

### 4.1 Target

- Baseline target: WCAG 2.1 AA.

### 4.2 Accessibility Checklist (Requirements)

This checklist should be implemented and checked against `exampleSite/`.

- Semantics and landmarks
  - One `main` landmark per page
  - Proper use of `header`, `nav`, `footer`
  - Headings follow a logical order
- Navigation
  - Skip link available and visible on focus
  - Keyboard navigation works for all interactive elements
  - No keyboard traps
- Focus and visibility
  - Visible focus indicators in all color schemes
  - Focus order follows visual order
- Forms and controls (if present)
  - Proper labels
  - Error messaging accessible
- Images and media
  - Meaningful images require `alt` text
  - Decorative images are marked appropriately
- Color and contrast
  - All schemes meet WCAG AA contrast for text and interactive elements
  - High contrast scheme must be usable without relying on color alone
- Motion and preferences
  - Honors `prefers-reduced-motion`
  - Honors `prefers-color-scheme` for initial scheme selection
  - Consider `prefers-contrast` where possible (progressive)
- ARIA
  - Use ARIA only when necessary
  - No redundant roles (e.g., `nav` doesn’t need `role="navigation"`)
- Screen reader support
  - Verify core flows with at least one Windows and one Apple screen reader (exact tools to be decided in CI policy; manual test matrix documented)

Deliverables:

- `ACCESSIBILITY.md` (or section in README) documenting:
  - WCAG target
  - Manual testing matrix
  - Known limitations

## 5. Performance Baseline

### 5.1 Hard Baseline Requirement

- Local load times under 100ms.
  - Clarification needed: what this means precisely (see Open Questions).

### 5.2 Suggested Budgets (Initial)

These budgets should be validated on `exampleSite/`.

- JavaScript: 0 KB by default (recommended); if needed, keep below 5 KB gzip
- CSS: keep below 30 KB gzip for core bundle
- Fonts: default to system font stack (0 external font requests)
- Images: use responsive images and lazy-loading for content images where appropriate

### 5.3 Measurement Strategy

- Provide a reproducible local measurement approach (documented):
  - Hugo build
  - Local server
  - Lighthouse CI run against local server
  - Optional: WebPageTest only if feasible and stable (likely scheduled, not per PR)

## 6. Configuration (Baseline Params)

Define a minimal, stable `params` surface that can be extended later.

### 6.1 Proposed `params` (MVP)

Color schemes:

- `params.colorScheme.default`: `"auto" | "light" | "dark"`
- `params.colorScheme.enableHighContrast`: `true | false`
- `params.colorScheme.persistSelection`: `true | false` (requires JS; if false, rely on media queries)

Layout:

- `params.listLayout.default`: `"list" | "grid" | "masonry"`
- `params.listLayout.allowUserOverride`: `true | false` (optional enhancement)

Header/footer:

- `params.header.logoText`: string
- `params.header.showSearch`: `true | false` (default false; implement only if required later)
- `params.footer.fixedLinks`: list of `{ name, url }`

Widgets:

- `params.widgets.sidebar`: ordered list of widget IDs
  - Allowed IDs (MVP): `toc`, `social_links`, `recent_posts`, `category_posts`, `fixed_links`

Social:

- `params.social.links`: list of `{ name, url }` for profile links
- `params.social.share.enable`: `true | false`
- `params.social.share.networks`: list of strings (e.g. `"mastodon"`, `"x"`, `"facebook"`, `"linkedin"`, `"email"`) (exact list TBD)

Blog post elements:

- `params.posts.showReadingTime`: `true | false`
- `params.posts.showTags`: `true | false`
- `params.posts.showGlossary`: `true | false`

Easy language:

- `params.easyLanguage.section`: string (default `"easy"` or `"einfach"`, to be confirmed)
- `params.easyLanguage.showBadge`: `true | false`

Notes:

- Keep params minimal; avoid adding knobs without a known use-case.

## 7. Theme Architecture (Proposed)

### 7.1 Hugo Theme Structure

- `theme.toml`
- `layouts/_default/baseof.html`
- `layouts/_default/list.html`
- `layouts/_default/single.html`
- `layouts/index.html` (homepage)
- `layouts/partials/`
  - `header.html`, `footer.html`, `nav.html`
  - `widgets/*`
  - `post/*` (meta, glossary, share)
- `layouts/taxonomy/` and terms templates
- Render hooks:
  - `layouts/_default/_markup/render-image.html`
  - `layouts/_default/_markup/render-link.html`
- `assets/` for CSS (and optional small JS)
- `static/` for icons, etc.
- `exampleSite/` required

### 7.2 Styling

- CSS variables for tokens.
- Three schemes (light/dark/high contrast) implemented by token overrides.
- Respect `prefers-color-scheme` and `prefers-reduced-motion`.

### 7.3 Pseudo-Masonry Implementation (CSS Grid)

- Use a grid-based layout that approximates masonry.
- Document known limitations (reading order vs visual order, consistent card heights, etc.).

## 8. Developer Experience and CI

### 8.1 GitHub Actions (Requirements)

- On `push` to `main`:
  - Run the full test suite
  - Build `exampleSite/`
  - Run linting/format checks (as applicable)
  - Run accessibility/performance checks (at minimum Lighthouse CI)

### 8.2 Testing Strategy (Proposed)

Given Hugo theme development realities, define tests as:

- Build tests:
  - `hugo --minify` for `exampleSite/`
  - `hugo server` smoke test in CI (if needed for Lighthouse)
- Link checks:
  - Validate internal links and generated pages
- Snapshot-style checks (optional):
  - Ensure key pages render expected structure (could be done with HTML assertions)

Open: whether to implement Go-based tests or rely on build + HTML checks.

## 9. Implementation Plan (Issue-Ready Epics)

### Epic 1: Repo Setup and Licensing

- Add AGPL license file and headers guidance.
- Add theme metadata (`theme.toml`).
- Add contribution guidelines (optional but recommended).

### Epic 2: Create `exampleSite/`

- Add `exampleSite/config.toml` (or `hugo.toml`) demonstrating baseline params.
- Add sample content:
  - Posts with tags/categories
  - At least one post with glossary
  - Easy language section content
  - Taxonomy pages
- Add sample menus/fixed links.

### Epic 3: Base Templates and Partials

- Implement `baseof.html` with landmarks and skip link.
- Header/footer partials with configurable fixed links.
- Navigation with keyboard-friendly behavior.

### Epic 4: Content Templates

- Single post template:
  - Description/summary
  - Tags
  - Reading time
  - Glossary section
  - Simple share links (config-driven)
- Static page template.

### Epic 5: Lists and Taxonomies

- Section list template with list/grid/masonry variants.
- Taxonomy list templates with list/grid/masonry variants.
- Pagination.
- “Other posts in this category” widget.

### Epic 6: Widgets System

- Define widget IDs and sidebar composition via `params.widgets.sidebar`.
- Implement widgets:
  - ToC
  - Social links
  - Recent posts
  - Other posts in this category
  - Fixed links

### Epic 7: Styling and Color Schemes

- Implement token system with three schemes.
- Ensure focus styles and contrast compliance.
- Ensure responsive layout for all templates.

### Epic 8: Easy Language Section

- Define the easy language section name and URL structure.
- Provide templates that:
  - Keep sentence length and typography readable
  - Avoid visual clutter
  - Provide optional badge/labeling
- Add example content in `exampleSite/`.

### Epic 9: Accessibility Workstream

- Create `ACCESSIBILITY.md` with WCAG 2.1 AA baseline and checklist.
- Implement and verify:
  - Skip link
  - Keyboard navigation
  - Focus visibility
  - ARIA review
  - Screen reader smoke-test notes

### Epic 10: Performance Workstream

- Document performance measurement methodology.
- Implement performance budgets (initial) and CI checks.
- Keep default JS at zero (or document minimal enhancement JS if used).

### Epic 11: Shortcodes Discovery and MVP

- Discovery step:
  - List needed shortcodes based on `exampleSite/` content needs and documentation goals.
- Implement MVP shortcodes (to be defined).
- Document shortcode usage with examples.

### Epic 12: CI and Quality Gates

- Add GitHub Actions workflow(s) for:
  - Build
  - Checks (HTML/link)
  - Lighthouse CI
  - (Optional) scheduled WebPageTest
- Ensure the whole suite runs on push to main.

## 10. Open Questions

These should be answered before finalizing issues, or encoded as investigation tasks.

- Performance baseline definition:
  - "local load times under 100ms" for which metric (TTFB? FCP? LCP?) and under what environment (machine specs, cold vs warm cache)?
- Color scheme switching UX:
  - Is `auto` sufficient, or do you want an explicit user toggle?
  - If toggle exists, do we need persistence (localStorage) or is session-only fine?
- Social share networks:
  - Which networks must be supported?
  - Should Mastodon sharing be supported explicitly (instance selection is tricky)?
- Glossary format:
  - Where does glossary data live (front matter vs content markup vs data files)?
  - Should glossary terms auto-link, or only render as a section?
- Easy language section specifics:
  - Exact section name (`easy`, `einfach`, etc.)
  - Any required metadata or badges
- Testing approach definition:
  - What constitutes the "whole test suite" for this repo (since it's currently empty)?
  - Preferred tools for HTML/link checking.

## 11. Risks

- WCAG AA compliance risk if requirements, content, and testing matrix aren’t pinned down.
- Pseudo-masonry can introduce confusing visual order vs DOM order; needs careful implementation.
- Performance CI flakiness (Lighthouse variability) unless stabilized.
- Scope creep in params/widgets/shortcodes if not constrained by MVP.

## 12. Suggested Next Steps

- Decide and document:
  - Performance metric(s) for the 100ms baseline
  - Social sharing networks
  - Glossary data model
  - Easy language section name
- Then proceed with Epic 1 (repo scaffold + license) and Epic 2 (`exampleSite/`) to ground the rest of the work.
