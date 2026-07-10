# DELETE /v1/navigation/{navigationId}

> Controller: NavigationController

Delete a [navigation](/doc/tech_noun/TN0601_navigation.md) menu.

The navigation menu is removed along with its type-specific binding (its
[navigation list](/doc/tech_noun/TN0604_navigation_list.md) or
[navigation article](/doc/tech_noun/TN0603_navigation_article.md) record). The menu can only be deleted
when it has no child navigations, and a `ROOT` navigation cannot be deleted.

## Permission

[`PROJECT_DEV`](/doc/permission/PROJECT_DEV.md)

## Request

### Path Parameters

| Name | Type | Description |
| --- | --- | --- |
| `navigationId` | Long | Physical id of the navigation menu to delete. |

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
| Navigation menu still has child navigations | 400 Bad Request | [90007 NavigationDeleteRejectWithChildren](/doc/backend_error/navigation/90007.md) |
| Navigation menu is a `ROOT` navigation | 400 Bad Request | [90008 NavigationRootDeleteReject](/doc/backend_error/navigation/90008.md) |

## Example

Request:

N/A

Success response (the `data` payload):

```json
{}
```
