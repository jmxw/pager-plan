# CLAUDE.md

Guidance for Claude Code when working in this repository.

## What this repo is

This is a **planning repository for Pager** — a SaaS, cloud-based website builder / CMS.
It holds Markdown specifications only. There is **no application code here**: nothing to
compile, no test suite, no dev server. Do not look for a build/lint/test command and do not
try to "run" anything. Work here means **reading, writing, and refining plans**.

Pager builds websites from templates, renders them to static pages served from a CDN, and backs
the dynamic parts (message boards, forms, …) with shared microservices. Content editing,
template authoring, and publishing happen in an online CMS. The plans target a two-part app
(the code lives elsewhere, linked under `source/`):

- **Backend** (`pager-backend`, GitHub repo `jmxw/pager`) — Kotlin + Spring Boot, multi-module:
  `domain` → `app/cms` (REST API, port 8080) and `app/consumer` (RabbitMQ deployment worker,
  port 8081); `lib/oss` wraps Aliyun OSS. MySQL for persistence (JPA/Hibernate, DDL auto-update),
  RabbitMQ for async deployment events, Aliyun OSS for generated sites/assets. Auth is Spring
  Security + JWT with a custom `@Permitted(UserPermission.X)` annotation; role hierarchy
  OWNER > ADMIN > DEVELOPER > CUSTOMER. Package base `com.xwkj.pager` (domain / consumer),
  `com.xwkj.cms` (CMS app).
- **Frontend** (`pager-frontend`, GitHub repo `jmxw/pager-front`) — Vue 3 (Composition API) +
  Vuex 4 + Vue Router 4 + Element Plus, built with Vue CLI on the VAB (admin-plus) template
  framework, with an OpenAPI-generated API client (regenerated with `make openapi`). UI strings
  are bilingual via Vue I18n (`zh` default, `en`).

## Repository layout

All plan content lives under `plan/` (so the code repos can symlink just that subfolder and
avoid a `source/` ↔ `plan` reference cycle — the code-repo symlinks back here live under
`source/pager-backend` and `source/pager-frontend`; see the README):

- `plan/common/` — shared reference docs cited by plans (e.g. the Pager template-tag reference:
  `{pager:list}`, `[list:*]`, `[page:*]`, `{content:*}`).
- `plan/release/<version>/` — plans grouped by the release that ships them. `plan/release/feature/`
  is a parking lot for plans not yet assigned to a release.

Two more top-level folders render on the site but are not plans:

- `rules/` — the **authoring conventions** for these specs (how to write an API doc, an error
  doc, a permission doc, a spec; the templates and the linking/voice rules). Read
  [rules/README.md](rules/README.md) before authoring reference docs.
- `doc/` — the **cross-cutting reference material itself** produced by following those rules:
  `api/` (one doc per endpoint), `backend_error/` (one per error code), `permission/` (one per
  `UserPermission`), `security/` (audit findings), `spec/` (screen catalog), `tech_noun/`
  (domain glossary). Plans link into it with **root-absolute** `/doc/...` paths.

## Versioning — never edit a previous release's plan

Plans under `plan/release/<version>/` for an **already-released** version are immutable
history. Do **not** edit them to reflect a later decision — not even a rename, a key/error-code
tweak, or a "fix". If a new decision changes something an older plan described, **create a new
plan in the latest (in-progress) release folder** that owns the change and supersedes the old
behavior, and reference the old plan rather than rewriting it. Only the current, not-yet-shipped
release's plans are editable. The same applies to a plan already parked in
`plan/release/feature/` — leave it alone unless that parked feature is what you're actively
developing. (This rule extends to the corresponding shipped code: don't retroactively rewrite
`pager-backend` / `pager-frontend` to match a new decision except as the implementation of a new
latest-version plan.)

## Naming conventions (follow these for new files)

- Backend plan: `<feature>.md` (or `<feature>-be.md`).
- Frontend plan: `<feature>-fe.md`.
- DB migration: **do not** create a separate `.sql` file — embed the SQL in the BE plan
  (see "Schema migrations live in the BE plan" below).
- Keep backend and frontend in **separate files** — do not merge them.
- Multi-step features use an umbrella file plus numbered steps (e.g. `deploy.md` +
  `deploy-step1.md` …); the umbrella holds the data model, architecture, and a step-status table.
- A few older plans use `-backend` / `-frontend`; match the suffix style already present in
  the same release folder when extending an existing feature.

## When writing or editing a plan

Match the conventions of the existing plans and the `rules/` folder rather than inventing a new
format. A plan should be:

- Concrete and implementation-ready: name real modules, files, endpoints, DAO methods,
  request/response DTOs, error codes, and permissions. Plans should be implementable without
  further design.
- Backend endpoints carry a `@Permitted(UserPermission.X)` and an `@Operation` summary; follow
  the controller / request-DTO / service-method patterns in `source/pager-backend` (CMS app
  package `com.xwkj.cms`, domain in `com.xwkj.pager`).
- Frontend plans assume the OpenAPI client is regenerated after backend changes (note it as a
  prerequisite via `make openapi`), add routes in `src/router/index.js`
  (`constantRoutes` / `asyncRoutes`), place API modules under `src/api/`, and keep Chinese to the
  literal on-screen UI copy — everything else in the plan is English (see the hard rule below).
- Not a dump of full source: include the key parts only (API definition, model definition, key
  logic), not entire implementations.
- **Diagrams:** draw UI/UX wireframes & mockups with **PlantUML Salt** (` ```plantuml ` +
  `@startsalt … @endsalt`), and flowcharts / sequence / state diagrams with **Mermaid**
  (` ```mermaid `). Both render with no local Java/`plantuml.jar`: the published site renders
  PlantUML via the `plantuml-markdown` extension against the public server (configured in
  `mkdocs.yml`, dep in `requirements-docs.txt`), and VSCode Markdown Preview Enhanced is pointed
  at an online server in `.vscode/settings.json`. Install/refresh docs deps with
  `pip install -r requirements-docs.txt`.

## Implementing a plan end-to-end (BE + FE)

When asked to **implement** a plan (not just write it), do the whole loop **autonomously** —
implement → restart BE → `make openapi` → restart FE → verify — without pausing for approval to
run/kill/restart steps. BE: from `source/pager-backend`, `./gradlew :app:cms:bootRun` (default
profile, port 8080; the consumer is `:app:consumer:bootRun` on 8081) and wait for the Swagger
API docs endpoint to return 200. FE: `make openapi` (BE must be up), then restart `npm run serve`
(port 8080 dev server — note the CMS API also uses 8080, so run them on different hosts/ports or
point the dev proxy at the running API). Verify by compiling both sides.

## Conventions that are easy to get wrong

- Honor existing API field names verbatim even when they look misspelled. Do not "fix" them in a
  plan unless the change is the point of the plan.
- The only build tooling here is the MkDocs Material docs site (`mkdocs.yml`,
  `requirements-docs.txt`, `docs/` symlinks, `.github/workflows/docs.yml`), which publishes
  the plans to GitHub Pages. Don't add application build/test tooling — the code lives in the
  `pager-backend` / `pager-frontend` repos, not here. `docs/` symlinks the whole `plan/` folder,
  the `rules/` folder, **and the `doc/` folder**, so anything added under any of them appears on
  the site automatically; only a new top-level folder would need its own symlink in `docs/`.
- **Plans are written in English — prose, headings, and code comments alike. Chinese is ONLY for
  literal user-facing UI copy.** This is a hard rule, not a preference. Everything you author in a
  plan document — section headings, body/explanatory prose, table cells, bullet points, list
  items, diagram captions/narration, and code comments (both in `pager-backend` / `pager-frontend`
  and in any fenced code snippet inside a plan) — is **English**. The **only** thing that stays
  Chinese is the *literal string that appears on screen*: a button/label/menu/toast text, a
  placeholder, and the wording drawn inside a Salt/mockup as the actual UI. Everything else is a
  description of that UI and must be English.
  - **Prose that names a UI label:** keep the Chinese label as the reference, then gloss it in
    English on first use — e.g. "the 保存 (save) action". Do **not** narrate in Chinese.
  - Before finishing a plan, scan for stray Chinese outside on-screen strings
    (`grep -nP '[\x{4e00}-\x{9fff}]' <plan>.md`) and confirm every hit is a genuine UI string, not
    prose, a heading, a caption, or a comment.
- File references in prose should use clickable relative Markdown links.
- To link a file in the code repos, go through the `source/pager-backend` / `source/pager-frontend`
  symlinks using a **root-absolute** path — `/source/pager-frontend/src/...` — not a `../../`
  relative climb and not the real `WebstormProjects/` / `IdeaProjects/` location.

## Writing release notes

A release's `plan/release/<version>/release-notes.md` summarizes what shipped — match the format of
the existing ones: an intro paragraph, then **Backend** / **Frontend** bullet lists, **Database
Migrations**, and a **Deployment checklist**.

In the **Backend** and **Frontend** bullet lists, keep each bullet **brief**: **one short
sentence** naming what function/feature changed, then the plan-doc link — and nothing else. Do
**not** pack a bullet with semicolon-chained clauses; that detail already lives in the linked plan.
Likewise do **not** list, per bullet, the schema/DB changes, new/modified file/table/component/DTO
names, or migration / `make openapi` / dependency boilerplate — that belongs in the **Database
Migrations** and **Deployment checklist** sections, not repeated on every bullet.

The **Database Migrations** section must **contain the actual migration SQL** (not just describe or
link it), merged into **one single ```sql fenced block that runs once, top-to-bottom** — not one
block per feature. Order the statements so the whole script executes in a single pass, use comment
banners to delimit each feature, and **de-duplicate/consolidate** where features touch the same
object. Call out any statement that isn't runnable verbatim. List the owning backend plans once,
below the block.

## Schema migrations live in the BE plan

Do **not** add a separate SQL file for a schema migration. Merge the migration SQL directly
into the backend plan (e.g. a "Migration" / "Schema Changes" section with a ```sql fenced
block) rather than creating a sibling `<feature>-migration.sql`. Keeping the DDL alongside
the DTOs, DAO methods, and endpoints it supports keeps the whole backend change reviewable in
one file. Note the backend uses JPA/Hibernate with DDL auto-update, so keep migration DDL
consistent with the entity mapping changes described in the same plan.
