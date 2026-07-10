# API Docs Rules

The Swagger-generated API doc is treated as a machine artifact, not the human spec. Each API is
documented **in its own markdown file** so it can carry the context Swagger cannot: the exact
request/response shape, the required permission, the errors that can be returned (linked to
[Backend Errors](./backend_errors.md)), and the domain models it operates on (linked inline to
[Tech Noun](./tech_noun.md) on first mention).

## Positioning

- One API doc = one **endpoint** (one HTTP method + path).
- The API doc is the human-readable spec of a single endpoint; Swagger stays as the raw,
  auto-generated reference.
- An API doc **references, never copies**: error codes link to their Backend Error docs, and a
  domain model is linked to its Tech Noun entry inline the first time it appears. Copied text
  drifts; links do not.

## Structure Rules

- API docs are grouped **by controller**. One backend controller class = one subfolder.
- Each endpoint of the controller = **one file** inside that subfolder.
- Each controller subfolder has its own `README.md` listing the endpoints it contains.

API docs live under `doc/api/` — a sibling of `doc/backend_error/` and `doc/tech_noun/`; error and
Tech Noun links use a root-absolute path from the repo root (e.g. `/doc/backend_error/...`).

```
doc/api/
├── README.md                       # controller index
├── _format/                        # copyable templates (never edited in place)
├── <ControllerName>/               # 1 controller class = 1 folder
│   ├── README.md                   # endpoint list for this controller
│   ├── <endpoint>.md               # 1 endpoint = 1 file
│   └── <endpoint>.md
└── <ControllerName>/
    └── ...
```

A full worked example lives at [doc/api/ticket_search_preset/](/doc/api/ticket_search_preset/README.md).

### Naming

- **Folder**: the controller class name **without the `Controller` suffix**, e.g.
  `TicketSearchPresetController` → `ticket_search_preset/` (snake_case).
- **File**: the controller method name in snake_case, e.g. `listTicketSearchPresets` →
  `list_ticket_search_presets.md`. This is an authoring convention that keeps each doc traceable to
  exactly one endpoint; the method name is **not** shown inside the doc (a reader confirms the API
  by its method + path, not by the Kotlin function name).

## Templates

- Place copyable md templates under [`doc/api/_format/`](/doc/api/_format/api.md) and copy them
  to create a new doc (never edit the templates in `_format/` directly). At minimum every API doc
  includes the items below.

```markdown
# <METHOD> <path>

> Controller: <ControllerName>

<One-line summary — the same text as the endpoint's `@Operation(summary = ...)`.>

## Permission

[`<UserPermission.X>`](/doc/permission/<UserPermission.X>.md) — link the
`@Permitted(UserPermission.X)` value to its permission doc. List each permission when several are
named and state the `PermittedMode` (`ONE_OF` / `ALL` / `UNREQUIRED`).
Write `N/A (authenticated)` when the endpoint has no `@Permitted` but still requires login;
write `N/A (public)` when the endpoint is unauthenticated.

## Request

### Path Parameters

| Name | Type | Description |
| --- | --- | --- |
| `id` | Long | <what it identifies> |

### Query Parameters

| Name | Type | Required | Description |
| --- | --- | --- | --- |
| `keyword` | String | ❌ | <what it filters> |

### Body

DTO: `<RequestDtoName>`

| Field | Type | Required | Description |
| --- | --- | --- | --- |
| `name` | String | ✅ | <field meaning> |

Write `N/A` for any of the three groups the endpoint does not use.

## Response

DTO: `<ResponseWrapper><EntityName>` (e.g. `OaItemResponse<TicketEntity>`)

| Field | Type | Description |
| --- | --- | --- |
| `item.id` | Long | <field meaning> |

## Errors

| Case | HTTP Status | Error Code |
| --- | --- | --- |
| <the condition, in words> | <4xx/5xx> | [<code> <EnumConstant>](/doc/backend_error/<category>/<code>.md) |

## Example

Request:

```json
{ "name": "..." }
```

Success response:

```json
{ "item": { "id": 1, "name": "..." } }
```
```

## Content Rules

1. **Method, path, and summary are copied verbatim** from the controller — the `@GetMapping`
   / `@PostMapping` path, the HTTP method, and the `@Operation(summary = ...)` text. These must
   match the implementation exactly.
2. **Permission** is the exact `@Permitted(UserPermission.X)` on the endpoint, **linked to its
   [permission doc](/doc/permission/README.md)** (`/doc/permission/<X>.md`) — the API doc is the forward reference and the permission doc is the reverse index of it.
   When the annotation names several permissions, link each and state the `PermittedMode`
   (`ONE_OF` any-one-grants, `ALL` all-required, `UNREQUIRED` optional/broadening). If the endpoint
   has no `@Permitted`, state whether it is authenticated-only or fully public (no link).
3. **Request** is split into path parameters, query parameters, and body — each documented as a
   field table (`Required` column: ✅ / ❌). Name the request DTO class. Groups that do not
   apply are `N/A`.
4. **Response** names the response wrapper + entity type (`OaItemResponse<T>`,
   `OaItemsResponse<T>`, `EmptyResponse`, …) and documents the meaningful fields. Only the
   **success** shape is described here — errors live in the Errors section.
5. **Errors are referenced, not copied.** Each row gives the case, the **HTTP status** the error
   returns, and a link to the matching [Backend Error](./backend_errors.md) doc; the error message
   and placeholders are **not** duplicated. The HTTP status is the `httpStatus` of the enum entry
   in `OaErrorMessage` (defaults to `400 BAD_REQUEST` when the enum omits it) — it matches the
   `HTTP Status` line of the linked Backend Error doc. List only the errors this endpoint can
   actually return, following the same "describe only the possible error cases" rule as the PRD
   Functions section.
6. **Tech Nouns are referenced inline, not redefined.** There is **no** separate Tech Nouns
   section. The **first time** a domain model appears in the doc — in a field description, a prose
   line, or the summary — its name is linked to the matching [Tech Noun](./tech_noun.md) entry
   right there; later mentions in the same doc are plain text. A model is never re-described inline.
   If a term has no Tech Noun yet, the Tech Noun is created first (Tech Noun is the source of all
   terminology).
7. **Example** shows one realistic request body (or `N/A` for bodyless endpoints) and one success
   response, in `json` fenced blocks. The example is illustrative and, unlike the method/path,
   is not required to be byte-identical to a live payload.

## Update Rule (Important)

- When a controller endpoint is added, changed, or removed — its path, method, permission,
  request/response DTO, or the set of errors it throws — the matching API doc is created,
  updated, or deleted in the **same PR**.
- Reason: the API doc is the reviewable spec of the endpoint. A doc that lags the controller
  becomes a wrong spec, exactly as a stale Backend Error doc becomes a wrong Swagger entry.
