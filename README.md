# RamahAnak Marketing Site

This repository contains the public-facing RamahAnak website built with Hugo.

Its role is to communicate the product, explain the decision-first positioning, and publish blog/content pieces that support awareness, trust, and conversion. Based on the product plan, RamahAnak is not a generic discovery platform or travel directory. It is a focused decision system for families who want a confident weekend plan with less uncertainty and less effort.

## What This Repo Is For

- marketing pages
- product positioning and messaging
- editorial/blog content
- lightweight static-site delivery

This repo is the content and communication layer for the brand.

## What This Repo Is Not For

- login or access control
- payments
- recommendation generation logic
- scoring engine implementation
- operational subscriber system

The actual system/app is prepared separately. That product layer is where protected workflows, decision generation, and future system behavior belong.

## Product Context

RamahAnak is designed around one core problem: families do not need more options, they need more decision confidence.

The broader product direction in `PLAN.md` describes RamahAnak as a decision infrastructure product that reduces weekend planning fatigue by turning uncertainty into a clear recommendation. This website supports that strategy from the outside by explaining the value proposition and publishing content around the problem space.

In the current competition-oriented flow:

- the homepage explains the product and previews available weekly editions
- the plan pages contain the full weekly operational recommendation

## Stack

- Hugo
- static content and templates
- GitHub-friendly deployment workflow

## Local Development

```bash
hugo server
```

Default local preview:

```text
http://localhost:1313/
```

## Private Content Repo

This repo is configured to render all page/article text from YAML stored in a separate private repository. During CI, GitHub Actions checks out that repo, copies `content/` into this site's `data/`, then builds with Hugo.

Expected content repo structure:

```text
content/
  articles/
    article-1.yaml
    article-2.yaml
  pages/
    landing_page.yaml
    blog.yaml
    example_page.yaml
```

During CI, `private-content/contents/` is copied into this repo's `data/`, so the private repo root should contain:

```text
content/
  articles/
  pages/
```

Required GitHub configuration in this repo:

- Repository variable `ENGINE_REPO`
  Example: `your-org/ramahanak-engine`
- Optional repository variable `CONTENT_REF`
  Example: `main`
- Repository secret `ENGINE_REPO_TOKEN`
  This should be a GitHub token with read access to the private content repo.

If you scope `ENGINE_REPO` or `ENGINE_REPO_TOKEN` to a GitHub Actions environment such as `github-pages`, any job that reads them must declare that same `environment`.

Workflow behavior:

- checks out this site repo
- checks out the private content repo
- copies `content/` from that repo into `data/`
- validates that `pages/landing_page.yaml`, `pages/blog.yaml`, `pages/example_page.yaml`, and at least one `articles/*.yaml` exist
- builds and deploys the public site with Hugo

For local development, sync `content/` from the private repo into this repo's `data/` before running `hugo server`.

## Content Direction

The site should present RamahAnak as:

- a marketing site for the brand
- a publishing surface for blog/editorial content
- a clear entry point to the separate product system
- a homepage that explains the product and links to current and past weekly editions
- plan pages that act as the actual weekly recommendation artifact

Messaging should stay aligned with the plan:

- emphasize decision support over discovery
- avoid framing the product as a directory or generic recommendation blog
- keep the public site focused on trust, education, and conversion

## Deployment

This is a static site and can be deployed through standard Hugo hosting flows such as GitHub Pages or any static hosting provider.
