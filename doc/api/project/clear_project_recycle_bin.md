# PATCH /v1/project/{projectId}/clear_recycle_bin

> Controller: ProjectController

Clear the recycle bin of a project.

**Not implemented** — the controller calls `ProjectService.clearRecycleBin`, whose body is a
Kotlin `TODO()`. Until it is implemented, every permitted call fails with
`-9999 UnknownError` (see Errors). This doc records the declared surface; it is extended
together with the implementation (same-PR sync rule).

## Permission

[`PROJECT_DEV`](/doc/permission/PROJECT_DEV.md) — held by OWNER, ADMIN, and DEVELOPER.

## Request

### Path Parameters

| Name | Type | Description |
| --- | --- | --- |
| `projectId` | Long | Id of the [project](/doc/tech_noun/TN0301_project.md) whose recycle-bin [templates](/doc/tech_noun/TN0401_template.md) are deleted permanently. |

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
| The service method is not implemented (`TODO()`) — currently every permitted call fails this way | 500 | [-9999 UnknownError](/doc/backend_error/unknown/-9999.md) |

A missing or invalid JWT is answered with `401 Unauthorized` (empty body) before the handler runs.

## Example

Request: N/A (no body)

Success response (unreachable until the service is implemented):

```json
{
  "code": 200,
  "timestamp": 1752700000000,
  "data": {}
}
```
