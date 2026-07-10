# Pager CMS — Product Overview

Pager CMS is a next-generation, fully cloud-based website-building tool. It aims to lower
development and deployment cost for website developers, improve site security, and improve the
end-user experience.

## Background

Sites for businesses or individuals are typically built one of three ways:

| Approach | Pros | Cons |
|---|---|---|
| Fully custom development (front + back end) | Tailored to special requirements | Long development time, high cost |
| Highly customizable CMS | Covers most needs; pages can be highly customized | Most CMSes are PHP (attack-prone), self-hosted by the developer, poor end-user experience, high ops risk (data loss); few solutions for special needs like WeChat Official Account sync |
| Cloud website services | Low cost, no ops, high reliability, hard to attack | Almost no customization beyond simple page tweaks; expensive; no control over the data |

Fully custom development is per-project and doesn't scale into a product. Cloud services are
dominated by incumbents (Alibaba, Tencent, …), leaving little room to enter. The CMS approach is
productizable because many small/medium firms in China build highly page-customized sites for
their clients and need a CMS that supports deep page customization — so a better product and
service can be built there and grown over time.

## Positioning

Pager (working name) is a **SaaS CMS** that addresses the weaknesses of existing PHP CMSes
(DedeCMS, WordPress, 帝国, pboot, …) — attack-prone old PHP, high ops cost, and poor end-user
experience from single-server hosting. Pager:

- Frees developers from buying/operating servers and databases — they buy Pager site resources.
- Generates static pages from templates and deploys them to a CDN, improving end-user experience.
- Serves dynamic needs (e.g. message boards) through shared, centralized microservices available
  to all end users.
- Provides online editing/development tools, so developers don't set up or maintain a local
  environment.
- Provides a template marketplace where developers pick semi-finished templates to build from.

## Pricing models

Two models, similar to GitHub's hosted vs. enterprise self-hosted offerings:

- **SaaS** — template sales; per-site build-tool fee (or free tooling); storage fees; traffic fees
  (possibly bandwidth-based).
- **Self-hosted** — annual license fee; deployment fee; ops fee.

## User groups

| User | Permissions | Notes |
|---|---|---|
| End user | Visit web pages; call microservices (e.g. submit to a message board) | Visits the CDN-hosted static site; submits requests to feature microservices, tagged with the site id |
| Site user (business or individual) | Access the CMS; access microservices (e.g. view the message board) | Sets article content and site content within the parts the developer allows |
| Website developer | Access the CMS; add users; edit templates; publish pages | Creates site projects, assigns users/permissions, edits page templates, and defines what site users may edit |
| Template developer | Access the CMS; create/edit templates; share templates | |

## Feature docs

- [Template tag reference](template-tags.md) — `{pager:list}`, `[list:*]`, `[page:*]`, `{content:*}`.
