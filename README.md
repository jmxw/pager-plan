# Pager Plan

Design and implementation plans for **Pager** — a SaaS, cloud-based website builder / CMS.

Pager lets website developers build sites from templates, render them to static pages served
from a CDN, and back the dynamic parts (message boards, forms, …) with shared microservices —
so developers don't buy or operate their own servers. Content editing, template authoring, and
publishing all happen in an online CMS.

This repository holds **specifications only** — there is no application code here, nothing
to build, and nothing to run. Each Markdown file is a self-contained plan that describes a
single feature in enough detail to implement it directly: scope, data model, API endpoints,
module-by-module changes, and implementation order. The plans are authored against the
`pager-backend` and `pager-frontend` codebases (linked under `source/`, see below) so they can
be written, reviewed, and tracked independently of the code.

## Reading the plans

The plans are published as a searchable site via GitHub Pages —
**https://jmxw.github.io/pager-plan/** — with a sidebar grouped by release, full-text search,
and rendered mermaid diagrams. It rebuilds automatically on every push to `master`
(see [the docs workflow](https://github.com/jmxw/pager-plan/blob/master/.github/workflows/docs.yml)).

To preview the site locally, in a virtual environment:

```bash
python3 -m venv .venv
source .venv/bin/activate          # Windows: .venv\Scripts\activate
pip install -r requirements-docs.txt
mkdocs serve                       # serves http://127.0.0.1:8000
```

`.venv/` is gitignored. Next time just `source .venv/bin/activate && mkdocs serve`.

The site is built with [MkDocs Material](https://squidfunk.github.io/mkdocs-material/) from
`mkdocs.yml`. MkDocs follows symlinks while scanning, so the build is scoped to a small
`docs/` folder of symlinks to `plan/` (which holds `release/` and `common/`), the `rules/` and
`doc/` folders, plus the top-level docs — which keeps it from crawling the large `source/`
code-repo symlinks. You can also just browse the files on GitHub, which renders Markdown and
mermaid in the file view.

## What the plans target

The plans describe changes to a two-part application:

- **Backend** — Kotlin + Spring Boot, organized as a multi-module project:
  `domain` (JPA entities, repositories, DAOs, services, security, RabbitMQ/OSS components) ·
  `app/cms` (REST API for content management, port 8080) · `app/consumer` (RabbitMQ worker that
  processes deployment events — unpacks ZIP artifacts, precompiles content, deploys to OSS,
  port 8081) · `lib/oss` (Aliyun OSS wrapper). Persistence is MySQL (JPA/Hibernate); async work
  runs over RabbitMQ; generated static sites and assets live in Aliyun OSS. Auth is JWT with a
  custom `@Permitted(UserPermission.X)` annotation; the role hierarchy is
  OWNER > ADMIN > DEVELOPER > CUSTOMER. Package base: `com.xwkj.pager` (domain / consumer),
  `com.xwkj.cms` (CMS app).
- **Frontend** — Vue 3 (Composition API) + Vuex 4 + Vue Router 4 + Element Plus, built with
  Vue CLI, and an OpenAPI-generated API client (`make openapi`). Built on the VAB
  (Vue Admin Beautiful / admin-plus) template framework. UI copy is bilingual via Vue I18n
  (`zh` default, `en`).

A backend plan and its frontend counterpart are written as separate files (see naming below).

## Layout

```
pager-plan/
├── plan/                Plan content; the folder the code repos symlink in (see below)
│   ├── common/          Shared reference docs cited by multiple plans (e.g. the Pager
│   │                    template-tag reference: {pager:list}, [list:*], [page:*], …)
│   └── release/         Plans grouped by the release they ship in
│       ├── 1.0/            Initial feature set
│       └── feature/        Parking lot for plans not yet assigned to a release
├── rules/               Doc-authoring rules (how to write these specs) — see rules/README.md
│   ├── api_docs.md         How to document API endpoints
│   ├── backend_errors.md   How to document backend error codes
│   ├── common_rules.md     Cross-cutting authoring rules (linking, voice, …)
│   ├── permissions.md      How to document UserPermission values
│   ├── tech_noun.md        Domain-term terminology rules
│   ├── validation.md       How to document field validation
│   └── spec/               Spec templates & examples (template.md, functions.md, design.md, …)
└── doc/                 Reference docs, cross-linked from plans (root-absolute `/doc/...` links)
    ├── api/             One doc per API endpoint, grouped by controller
    ├── backend_error/   One doc per backend error code, grouped by category
    ├── permission/      One doc per `UserPermission` value
    ├── security/        Security audit findings (backend / frontend)
    ├── spec/            Spec index — the master catalog of Pager CMS screens
    └── tech_noun/       Domain-term definitions — the source of terminology for all docs
```

`rules/` holds the **authoring conventions** for these specs (how to write an API doc, an error
doc, a permission doc, a spec — the templates and the linking/voice rules). `doc/` holds the
**cross-cutting reference material itself** (the actual API endpoints, backend error codes,
permissions, the security audit, and the domain tech-noun glossary) produced by following those
rules. Plans and docs link into `doc/` with **root-absolute** paths (`/doc/...`), the same
convention used for the `source/` code links; both `rules/` and `doc/` render on the site through
their own `docs/` symlinks.

All plans live under `plan/` so that the code repos can symlink **just that subfolder**
rather than the whole repository. The repo root also holds the `source/pager-backend` and
`source/pager-frontend` symlinks back to the code repos (see below); nesting everything under
`plan/` keeps those out of what the code repos see, which avoids a symlink reference cycle
(`source/pager-backend → pager-backend → plan → pager-plan → source/pager-backend`).

## File naming conventions

Within each release folder, one feature usually spans two files:

| Suffix | Meaning |
|---|---|
| `<feature>.md` or `<feature>-be.md` | Backend plan |
| `<feature>-fe.md` | Frontend plan |

Some older plans use `-backend` / `-frontend` instead of `-be` / `-fe`. Multi-step features
use an umbrella file plus numbered steps (e.g. `deploy.md` + `deploy-step1.md` … `-stepN.md`);
the umbrella tracks per-step status. Schema migrations are embedded in the backend plan, not
added as separate `.sql` files.

## Plan structure

Plans are not rigidly templated, but most follow this shape:

- **Overview / Goal** — what the feature does and why.
- **Scope** — operations and constraints, often as tables.
- **API Endpoints** — method, path, function name, permission, description.
- **Module Changes** — concrete changes per backend module (error codes, DAOs, DTOs,
  service methods, controller endpoints) with Kotlin snippets; or per frontend file
  (API modules, routes, views) with snippets and wireframes.
- **Implementation Order** and **Summary of New / Modified Files**.

## Code repo setup

Plans reference real modules, files, endpoints, and error codes in the backend and frontend
codebases, so it helps to have both checked out alongside this repo and reachable via local
symlinks under `source/` (`source/pager-backend`, `source/pager-frontend`). The whole `source/`
folder is gitignored.

```bash
# 1. Clone the code repos to any local paths you prefer
git clone https://github.com/jmxw/pager.git       ~/Documents/IdeaProjects/pager-backend
git clone https://github.com/jmxw/pager-front.git  ~/Documents/WebstormProjects/pager-frontend

# 2. From the root of this repo, create the symlinks under source/ (use the paths from step 1)
mkdir -p source
ln -s ~/Documents/IdeaProjects/pager-backend     source/pager-backend
ln -s ~/Documents/WebstormProjects/pager-frontend source/pager-frontend

# 3. Verify
ls source/pager-backend    # should show the backend project (app/, domain/, lib/, …)
ls source/pager-frontend   # should show the frontend project (src/, package.json, …)
```

> The backend GitHub repo is named `pager` and the frontend repo is named `pager-front`; the
> local checkout directories (`pager-backend` / `pager-frontend`) and the `source/` symlink names
> are what the plans reference throughout.

The two code repos do the inverse — each surfaces this repo's `plan/` subfolder at their own
`plan/` via the same symlink trick (`ln -s ~/Documents/Projects/pager-plan/plan plan`), so paths
like `plan/release/1.0/...` resolve from either side. They symlink the `plan/` **subfolder**,
not the repo root — that's what avoids the `source/` ↔ `plan` reference cycle described above.

## Working in this repo

- Add a new plan to the folder for the release it ships in (or `plan/release/feature/` if
  undecided), following the naming conventions above.
- Keep backend and frontend concerns in separate files.
- Plans should be detailed enough to implement against the `pager-backend` / `pager-frontend`
  codebases without further design work — name real modules, files, endpoints, and error codes.
- Follow the authoring conventions in [rules/README.md](rules/README.md).

See [CLAUDE.md](CLAUDE.md) for guidance aimed at Claude Code.
