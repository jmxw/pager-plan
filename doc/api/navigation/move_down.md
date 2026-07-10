# PATCH /v1/navigation/{navigationId}/down

> Controller: NavigationController

Move [navigation](/doc/tech_noun/TN0601_navigation.md) menu down

The navigation menu is swapped in sequence with the sibling immediately after it under the same parent.
When the navigation menu has no parent or is already last, the request succeeds with no change.

## Permission

[`PROJECT_DEV`](/doc/permission/PROJECT_DEV.md)

## Request

### Path Parameters

| Name | Type | Description |
| --- | --- | --- |
| `navigationId` | Long | Physical id of the navigation menu to move down. |

### Query Parameters

N/A

### Body

N/A

## Response

DTO: `EmptyResponse` — wrapped as `data` inside the standard `PagerResponse` envelope
(`{ code, timestamp, data }`). The `data` payload is an empty object `{}`.

| Field | Type | Description |
| --- | --- | --- |
| — | — | The response body is empty (`{}`). |

## Errors

| Case | HTTP Status | Error Code |
| --- | --- | --- |
| Caller's role lacks the required permission | 400 Bad Request | [20002 UserNotPermitted](/doc/backend_error/user/20002.md) |
| Navigation menu does not exist | 400 Bad Request | [90001 NavigationIdNotExist](/doc/backend_error/navigation/90001.md) |
| Caller does not exist | 400 Bad Request | [20001 UserIdNotExist](/doc/backend_error/user/20001.md) |
| Caller has no privilege on the navigation's project | 400 Bad Request | [40002 ProjectNoPrivilege](/doc/backend_error/project/40002.md) |

## Example

Request:

N/A

Success response (the `data` payload):

```json
{}
```
