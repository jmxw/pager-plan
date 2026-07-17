# DELETE /v1/navigation/{navigationId}

> Controller: NavigationController

Delete a navigation menu.

## Permission

[`PROJECT_DEV`](/doc/permission/PROJECT_DEV.md) — held by OWNER, ADMIN, and DEVELOPER.

## Request

### Path Parameters

| Name | Type | Description |
| --- | --- | --- |
| `navigationId` | Long | Id of the [navigation](/doc/tech_noun/TN0601_navigation.md) node to delete. Only a childless, non-root node can be deleted. |

### Query Parameters

N/A

### Body

N/A

## Response

DTO: `EmptyResponse`, returned inside the `PagerResponse` envelope as `data` — see the
[API index](/doc/api/README.md#response-envelope). The node's
[navigation list](/doc/tech_noun/TN0604_navigation_list.md) /
[navigation article](/doc/tech_noun/TN0603_navigation_article.md) binding is removed together
with the node.

| Field | Type | Description |
| --- | --- | --- |
| — | — | `data` is an empty object. |

## Errors

| Case | HTTP Status | Error Code |
| --- | --- | --- |
| The caller's role does not hold `PROJECT_DEV` (CUSTOMER) | 400 | [20002 UserNotPermitted](/doc/backend_error/user/20002.md) |
| No navigation exists for `{navigationId}` | 400 | [90001 NavigationIdNotExist](/doc/backend_error/navigation/90001.md) |
| The caller (DEVELOPER role) has no [privilege](/doc/tech_noun/TN0203_privilege.md) grant for the [project](/doc/tech_noun/TN0301_project.md) | 400 | [40002 ProjectNoPrivilege](/doc/backend_error/project/40002.md) |
| The calling user's account row no longer exists (privilege check) | 400 | [20001 UserIdNotExist](/doc/backend_error/user/20001.md) |
| The node still has children navigations | 400 | [90007 NavigationDeleteRejectWithChildren](/doc/backend_error/navigation/90007.md) |
| The node is the `ROOT` navigation of the project | 400 | [90008 NavigationRootDeleteReject](/doc/backend_error/navigation/90008.md) |

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
