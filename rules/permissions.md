# Permission Docs Rules

Each fine-grained access right of the OA system is documented **in its own markdown file** so it
can carry the context the [`UserPermission`](/source/oa-backend/domain/src/main/kotlin/com/xwkj/oa/domain/enum/UserPermission.kt)
enum cannot: what the permission grants in business terms, and which endpoints are gated by it
(linked to the [API Docs](./api_docs.md), never copied). The enum stays the raw, machine reference;
the permission doc is its human-readable spec and the reverse index of the API docs' `Permission`
field.

## Positioning

- One permission doc = one **`UserPermission` enum value**.
- A permission doc **references, never copies**: the endpoints it gates link to their
  [API Docs](./api_docs.md), and the term itself links to
  [TN0106 User Permission](/doc/tech_noun/TN0106_user_permission.md) on first mention. Copied
  text drifts; links do not.
- The permission doc is the inverse of the API doc's `Permission` line: the API doc names which
  permission gates an endpoint; the permission doc lists every endpoint a permission gates. The
  `PermissionMode` used to combine multiple permissions is an **endpoint** concern and stays in the
  API doc — it is not restated here.

## Structure Rules

- The permission set is small, so the docs are kept **flat**: one file per permission directly
  under `doc/permission/`, **not** grouped into per-area subfolders.
- Every permission doc still records its **area** (the grouping used in
  [TN0106 User Permission](/doc/tech_noun/TN0106_user_permission.md): Device, Org / access,
  Customer, Project / finance, Task, Ticket, Other) as a `> Area:` field, and the folder
  `README.md` indexes every permission with an Area column, so the grouping stays visible without
  a directory per area.

Permission docs live under `doc/permission/` — a sibling of `doc/api/`,
`doc/backend_error/`, and `doc/tech_noun/`; API and Tech Noun links use a root-absolute path from
the repo root (e.g. `/doc/api/...`, `/doc/tech_noun/...`).

```
doc/permission/
├── README.md                 # index of every permission (with its area)
├── _format/                  # copyable templates (never edited in place)
├── <ENUM_CONSTANT>.md        # 1 permission = 1 file
├── <ENUM_CONSTANT>.md
└── ...
```

### Naming

- **File**: the `UserPermission` enum constant verbatim (e.g. `DEVICE_MANAGE.md`). This keeps each
  doc traceable to exactly one enum value.

## Templates

- Place copyable md templates under `doc/permission/_format/` and copy them to create a new doc
  (never edit the templates in `_format/` directly). At minimum every permission doc includes the
  items below.

```markdown
# <ENUM_CONSTANT>

> Area: <area>

<One-line summary of the access this permission grants.>

## Description

<What the permission controls, in business terms — the resources and operations it unlocks, and
any admin-level gating (`RoleType`) that complements it.>

## Gated Endpoints

| Method | Path | Endpoint Doc |
| --- | --- | --- |
| POST | `/v1/device` | [create_device.md](/doc/api/device/create_device.md) |

Write `N/A` when no documented endpoint requires this permission yet.
```

## Content Rules

1. The **enum constant** is copied **verbatim** from `UserPermission` — it must match the backend
   exactly, including any name that looks misspelled.
2. The **area** matches the grouping in
   [TN0106 User Permission](/doc/tech_noun/TN0106_user_permission.md); a new permission is added
   to the area its enum block belongs to (a new area is created only when the enum grows one).
3. **Description** explains what the permission grants in business terms, not how the code checks
   it. If the term `User Permission` (or `PermissionMode`, `RoleType`) is used, it is linked to its
   [Tech Noun](/doc/tech_noun/TN0106_user_permission.md) entry on first mention and plain text
   afterward.
4. **Gated Endpoints are referenced, not copied.** Each row links to the matching
   [API Doc](./api_docs.md); the request/response shape is not duplicated. List only endpoints that
   actually declare `@Permitted(UserPermission.X)` for this permission. When an endpoint combines
   several permissions via `PermissionMode`, it is still listed under each permission it names — the
   mode itself is documented only in the API doc.

## Update Rule (Important)

- When a `UserPermission` value is added, renamed, or removed — or when an endpoint's `@Permitted`
  changes which permission gates it — the matching permission doc (and the affected API doc's
  `Permission` line) is created, updated, or deleted in the **same PR**.
- Reason: the permission doc is the reviewable spec of an access right and the reverse index of the
  API docs. A doc that lags the enum becomes a wrong access-control spec, exactly as a stale
  Backend Error doc becomes a wrong Swagger entry.
