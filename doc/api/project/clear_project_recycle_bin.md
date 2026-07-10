# PATCH /v1/project/{projectId}/clear_recycle_bin

> Controller: ProjectController

> Status: not yet implemented (the backing service method `ProjectService.clearRecycleBin` is a `TODO()`).

Clears a [project](/doc/tech_noun/TN0301_project.md)'s recycle bin. The intended behavior is to
permanently remove the [templates](/doc/tech_noun/TN0401_template.md) held under the project's
recycle-bin template. The endpoint is guarded by the permission below; no service logic runs yet, so
the project-scoped checks documented below are the intended contract once the method is implemented.

## Permission

[`PROJECT_DEV`](/doc/permission/PROJECT_DEV.md)

## Request

### Path Parameters

| Name | Type | Description |
| --- | --- | --- |
| `projectId` | Long | Physical id of the project whose recycle bin is cleared. |

### Query Parameters

N/A

### Body

N/A

## Response

DTO: `EmptyResponse` — wrapped as `data` inside the standard `PagerResponse` envelope
(`{ code, timestamp, data }`). The `data` body is empty (`{}`).

| Field | Type | Description |
| --- | --- | --- |
| — | — | The response body is empty (`{}`). |

## Errors

| Case | HTTP Status | Error Code |
| --- | --- | --- |
| Caller's role lacks the required permission | 400 Bad Request | [20002 UserNotPermitted](/doc/backend_error/user/20002.md) |

The `@Permitted` gate above is the only check that runs today; the backing service method is
unimplemented, so no business error is raised beyond it yet.

## Example

Request:

N/A

Success response (the `data` payload):

```json
{}
```
