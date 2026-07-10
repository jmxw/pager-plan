# <METHOD> <path>

> Controller: <ControllerName>

<One-line summary — the `@Operation(summary = ...)` text copied verbatim (or a faithful one-liner
when the method has none). Link the primary domain model to its Tech Noun (`/doc/tech_noun/...`) on
first mention, plain text after. Cross-doc links use a root-absolute path from the repo root
(`/doc/permission/`, `/doc/tech_noun/`, `/doc/backend_error/`, `/source/pager-backend/`).>

## Permission

[`<CONSTANT>`](/doc/permission/<CONSTANT>.md) — the exact `@Permitted(UserPermission.X)` on the
method. Write `N/A (authenticated)` (login required, no `@Permitted`) or `N/A (public)` (whitelisted
in `SecurityConfiguration`) instead of a link where that applies.

## Request

### Path Parameters

| Name | Type | Description |
| --- | --- | --- |
| `id` | Long | <what it identifies> |

### Query Parameters

| Name | Type | Required | Description |
| --- | --- | --- | --- |

### Body

DTO: `<RequestDtoName>`

| Field | Type | Required | Description |
| --- | --- | --- | --- |

<Write `N/A` for any of the three groups the endpoint does not use.>

## Response

DTO: `<ReturnDtoName>` — wrapped as `data` inside the standard `PagerResponse` envelope
(`{ code, timestamp, data }`).

| Field | Type | Description |
| --- | --- | --- |

<`EmptyResponse` → the body is `{}`. Document only the success shape here; errors live below.>

## Errors

| Case | HTTP Status | Error Code |
| --- | --- | --- |
| <the condition, in words> | 400 Bad Request | [<code> <Constant>](/doc/backend_error/<category>/<code>.md) |

<Write `N/A` when the endpoint raises no documented business error. List only errors this endpoint
can actually return, traced from its service method.>

## Example

Request:

```json
{ }
```

Success response (the `data` payload):

```json
{ }
```
