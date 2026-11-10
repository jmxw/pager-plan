# PATCH /v1/navigation/{navigationId}/move_to/{targetNavigationId}

> Controller: NavigationController

Move a navigation menu as a child of an other parent navigation menu.

**Stub endpoint** — the handler body is empty: nothing is moved and no service is called. This
doc records the declared surface; it is extended together with the implementation (same-PR sync
rule).

## Permission

[`PROJECT_DEV`](/doc/permission/PROJECT_DEV.md) — held by OWNER, ADMIN, and DEVELOPER.

## Request

### Path Parameters

| Name | Type | Description |
| --- | --- | --- |
| `navigationId` | Long | Id of the [navigation](/doc/tech_noun/TN0601_navigation.md) node to move. |
| `targetNavigationId` | Long | Id of the navigation node that becomes the new parent. |

### Query Parameters

N/A

### Body

N/A

## Response

DTO: `EmptyResponse`, returned inside the `PagerResponse` envelope as `data` — see the
[API index](/doc/api/README.md#response-envelope).

| Field | Type | Description |
| --- | --- | --- |
| — | — | `data` is an empty object. |

## Errors

| Case | HTTP Status | Error Code |
| --- | --- | --- |
| The caller's role does not hold `PROJECT_DEV` (CUSTOMER) | 400 | [20002 UserNotPermitted](/doc/backend_error/user/20002.md) |

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
