# <METHOD> <path>

> Controller: <ControllerName>

<One-line summary — the same text as the endpoint's `@Operation(summary = ...)`, copied verbatim.>

## Permission

[`<UserPermission.X>`](/doc/permission/<UserPermission.X>.md) — held by <roles>. Write
`N/A (authenticated)` when the endpoint has no `@Permitted` but still requires login;
write `N/A (public)` when the path is whitelisted in `SecurityConfiguration` (no login required).

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

Write `N/A` for any of the three groups the endpoint does not use. For a multipart upload,
document the form part(s) in the Body table instead of a DTO.

## Response

DTO: `<ResponseDtoName>` (e.g. `ProjectGetResponse`, `EmptyResponse`), returned inside the
`PagerResponse` envelope as `data` — see the [API index](/doc/api/README.md#response-envelope).
Field paths in the table are relative to `data`.

| Field | Type | Description |
| --- | --- | --- |
| `project.projectId` | Long | <field meaning> |

## Errors

| Case | HTTP Status | Error Code |
| --- | --- | --- |
| <the condition, in words> | 400 | [<code> <EnumConstant>](/doc/backend_error/<category>/<code>.md) |

List only the errors this endpoint can actually return. For an authenticated endpoint, add the
standard note that a missing/invalid JWT is answered with `401 Unauthorized` (empty body) before
the handler runs. Write `N/A` when no business error is raised.

## Example

Request:

```json
{ "name": "..." }
```

Success response:

```json
{
  "code": 200,
  "timestamp": 1752700000000,
  "data": { "...": "..." }
}
```
