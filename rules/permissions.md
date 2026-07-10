# Permission Docs Rules

Each fine-grained access right of Pager is documented **in its own markdown file** so it can carry
the context the [`UserPermission`](/source/pager-backend/domain/src/main/kotlin/com/xwkj/pager/domain/model/enum/UserPermission.kt)
enum cannot: what the permission grants in business terms, which
[`UserRole`](/source/pager-backend/domain/src/main/kotlin/com/xwkj/pager/domain/model/enum/UserRole.kt)s
hold it, and which endpoints are gated by it (linked to the [API Docs](./api_docs.md), never
copied). The enum stays the raw, machine reference; the permission doc is its human-readable spec
and the reverse index of the API docs' `Permission` field.

## Positioning

- One permission doc = one **`UserPermission` enum value**.
- A permission doc **references, never copies**: the endpoints it gates link to their
  [API Docs](./api_docs.md), and the term itself links to its Tech Noun on first mention. Copied
  text drifts; links do not.
- The permission doc is the inverse of the API doc's `Permission` line: the API doc names which
  permission gates an endpoint (via `@Permitted(UserPermission.X)`); the permission doc lists every
  endpoint a permission gates. In Pager each endpoint declares **at most one** permission — there is
  no permission-combining mode — so an endpoint appears under exactly one permission doc.

## Role model (Pager-specific)

Permissions are **not** granted per user. Each [`UserRole`](/source/pager-backend/domain/src/main/kotlin/com/xwkj/pager/domain/model/enum/UserRole.kt)
(OWNER > ADMIN > DEVELOPER > CUSTOMER) maps to a fixed `Set<UserPermission>` in `UserRole.permissions`,
and `UserPermissionInterceptor` admits a request when the caller's role set contains the endpoint's
`@Permitted` value. Therefore every permission doc records **which roles hold it** (a `> Roles:`
field), read straight from `UserRole.permissions`. A permission that no role holds is flagged as
such — its gated endpoints are currently unreachable.

## Structure Rules

- The permission set is small, so the docs are kept **flat**: one file per permission directly
  under `doc/permission/`, **not** grouped into per-area subfolders.
- Every permission doc records its **area** (the resource it governs: Access, User, Access Key,
  Project, Navigation/Template, Article List, Article, Label) as a `> Area:` field, and the folder
  `README.md` indexes every permission with Area and Roles columns.

Permission docs live under `doc/permission/` — a sibling of `doc/api/`, `doc/backend_error/`, and
`doc/tech_noun/`; API and Tech Noun links use a root-absolute path from the repo root (e.g.
`/doc/api/...`, `/doc/tech_noun/...`).

```
doc/permission/
├── README.md                 # index of every permission (with its area + roles)
├── _format/                  # copyable templates (never edited in place)
├── <ENUM_CONSTANT>.md        # 1 permission = 1 file
├── <ENUM_CONSTANT>.md
└── ...
```

### Naming

- **File**: the `UserPermission` enum constant verbatim (e.g. `PROJECT_WRITE.md`). This keeps each
  doc traceable to exactly one enum value.

## Templates

- Place copyable md templates under `doc/permission/_format/` and copy them to create a new doc
  (never edit the templates in `_format/` directly). At minimum every permission doc includes the
  items below.

```markdown
# <ENUM_CONSTANT>

> Area: <area>
> Roles: <roles that hold it, or "None (no role grants this)">

<One-line summary of the access this permission grants.>

## Description

<What the permission controls, in business terms — the resources and operations it unlocks.>

## Gated Endpoints

| Method | Path | Endpoint Doc |
| --- | --- | --- |
| POST | `/v1/project` | [add_project.md](/doc/api/project/add_project.md) |

Write `N/A` when no endpoint declares this permission yet.
```

## Content Rules

1. The **enum constant** is copied **verbatim** from `UserPermission` — it must match the backend
   exactly, including any name that looks misspelled.
2. The **area** is the resource the permission governs; the **roles** are exactly the roles whose
   `UserRole.permissions` set contains this value.
3. **Description** explains what the permission grants in business terms, not how the code checks
   it. If a domain term is used, it is linked to its [Tech Noun](/doc/tech_noun/) on first mention
   and plain text afterward.
4. **Gated Endpoints are referenced, not copied.** Each row links to the matching
   [API Doc](./api_docs.md); the request/response shape is not duplicated. List only endpoints that
   actually declare `@Permitted(UserPermission.X)` for this permission — an endpoint with no
   `@Permitted` (e.g. login, register) is gated by no permission and appears in no permission doc.
   Mark `⬜` when a listed endpoint's API doc is not yet written.

## Update Rule (Important)

- When a `UserPermission` value is added, renamed, or removed — when a `UserRole.permissions` set
  changes which roles hold it — or when an endpoint's `@Permitted` changes which permission gates
  it — the matching permission doc (and the affected API doc's `Permission` line) is created,
  updated, or deleted in the **same PR**.
- Reason: the permission doc is the reviewable spec of an access right and the reverse index of the
  API docs. A doc that lags the enum becomes a wrong access-control spec, exactly as a stale
  Backend Error doc becomes a wrong Swagger entry.
