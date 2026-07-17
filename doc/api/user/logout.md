# GET /v1/user/logout

> Controller: UserController

User logout

## Permission

N/A (authenticated) — the method declares no `@Permitted`; any logged-in role may call it.

## Request

### Path Parameters

N/A

### Query Parameters

N/A

### Body

N/A

## Response

DTO: `EmptyResponse`, returned inside the `PagerResponse` envelope as `data` — see the
[API index](/doc/api/README.md#response-envelope). Authentication is a stateless JWT, so nothing
is invalidated server-side — the handler only acknowledges; the client discards its token.

| Field | Type | Description |
| --- | --- | --- |
| — | — | `data` is an empty object. |

## Errors

N/A — no business error is raised by this endpoint.

A missing or invalid JWT is answered with `401 Unauthorized` (empty body) before the handler runs.

## Example

Request: N/A (no body)

Success response:

```json
{
  "code": 200,
  "timestamp": 1752700000000,
  "data": {}
}
```
