# GET /v1/navigation/{navigationId}/children

> Controller: NavigationController

Get project, parent navigations, children navigations and the navigation itself.

## Permission

[`PROJECT_DEV`](/doc/permission/PROJECT_DEV.md) — held by OWNER, ADMIN, and DEVELOPER.

## Request

### Path Parameters

| Name | Type | Description |
| --- | --- | --- |
| `navigationId` | Long | Id of the [navigation](/doc/tech_noun/TN0601_navigation.md) node to expand. |

### Query Parameters

N/A

### Body

N/A

## Response

DTO: `NavigationChildrenResponse`, returned inside the `PagerResponse` envelope as `data` — see
the [API index](/doc/api/README.md#response-envelope). `parents` is the chain from the root down
to the direct parent; `children` are ordered by `sequence`. Every navigation node carries the
fields listed for `self`.

| Field | Type | Description |
| --- | --- | --- |
| `project` | ProjectEntity | The owning [project](/doc/tech_noun/TN0301_project.md) — fields as in [get_project.md](/doc/api/project/get_project.md). |
| `self.navigationId` | Long | Id of the navigation node. |
| `self.createAt` | Long | Creation timestamp (epoch milliseconds). |
| `self.updateAt` | Long | Last-update timestamp (epoch milliseconds). |
| `self.type` | NavigationType | `ROOT`, `LIST`, or `ARTICLE`. |
| `self.identifier` | String | The [identifier](/doc/tech_noun/TN0101_identifier.md) — the URL path segment of the node. |
| `self.sequence` | Int | 1-based position among the siblings. |
| `self.name` | String | Display name of the node. |
| `parents[]` | NavigationEntity | Parent chain of the node (same fields as `self`). |
| `children[]` | NavigationEntity | Direct children of the node, ordered by `sequence` (same fields as `self`). |

## Errors

| Case | HTTP Status | Error Code |
| --- | --- | --- |
| The caller's role does not hold `PROJECT_DEV` (CUSTOMER) | 400 | [20002 UserNotPermitted](/doc/backend_error/user/20002.md) |
| No navigation exists for `{navigationId}` | 400 | [90001 NavigationIdNotExist](/doc/backend_error/navigation/90001.md) |
| The caller (DEVELOPER role) has no [privilege](/doc/tech_noun/TN0203_privilege.md) grant for the project | 400 | [40002 ProjectNoPrivilege](/doc/backend_error/project/40002.md) |
| The calling user's account row no longer exists (privilege check) | 400 | [20001 UserIdNotExist](/doc/backend_error/user/20001.md) |

A missing or invalid JWT is answered with `401 Unauthorized` (empty body) before the handler runs.

## Example

Request: N/A (no body)

Success response:

```json
{
  "code": 200,
  "timestamp": 1752700000000,
  "data": {
    "project": {
      "projectId": 3,
      "createAt": 1752500000000,
      "updateAt": 1752650000000,
      "name": "Corporate site",
      "identifier": "corp-site",
      "region": { "value": "CN_HANGZHOU", "regionName": "华东1（杭州）", "regionId": "oss-cn-hangzhou" },
      "state": "TEMPLATE_READY",
      "stagingRevision": 4,
      "productionRevision": 2,
      "rootTemplateId": 100,
      "rootNavigationId": 200,
      "indexDocument": "index.html",
      "errorDocument": "error.html"
    },
    "self": {
      "navigationId": 200,
      "createAt": 1752500000000,
      "updateAt": 1752650000000,
      "type": "ROOT",
      "identifier": "corp-site",
      "sequence": 1,
      "name": "corp-site"
    },
    "parents": [],
    "children": [
      {
        "navigationId": 201,
        "createAt": 1752600000000,
        "updateAt": 1752650000000,
        "type": "LIST",
        "identifier": "news",
        "sequence": 1,
        "name": "News"
      }
    ]
  }
}
```
