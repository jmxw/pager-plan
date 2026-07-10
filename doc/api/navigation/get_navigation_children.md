# GET /v1/navigation/{navigationId}/children

> Controller: NavigationController

Get [project](/doc/tech_noun/TN0301_project.md), parent [navigations](/doc/tech_noun/TN0601_navigation.md), children navigations and the navigation itself.

The navigation identified by the path is resolved together with its owning project, the ordered chain of
its ancestor navigations, and its direct children (ordered by sequence).

## Permission

[`PROJECT_DEV`](/doc/permission/PROJECT_DEV.md)

## Request

### Path Parameters

| Name | Type | Description |
| --- | --- | --- |
| `navigationId` | Long | Physical id of the navigation menu whose context is requested. |

### Query Parameters

N/A

### Body

N/A

## Response

DTO: `NavigationChildrenResponse` — wrapped as `data` inside the standard `PagerResponse` envelope
(`{ code, timestamp, data }`).

| Field | Type | Description |
| --- | --- | --- |
| `project` | ProjectEntity | The project that owns the navigation menu. |
| `self` | NavigationEntity | The navigation menu identified by the path. |
| `parents` | NavigationEntity[] | Ancestor navigation menus, ordered from the outermost ancestor down to the direct parent. |
| `children` | NavigationEntity[] | Direct child navigation menus, ordered by sequence. |

`NavigationEntity` fields:

| Field | Type | Description |
| --- | --- | --- |
| `navigationId` | Long | Physical id of the navigation menu. |
| `createAt` | Long | Creation time, epoch milliseconds. |
| `updateAt` | Long | Last update time, epoch milliseconds. |
| `type` | NavigationType | Navigation type, one of `ROOT`, `LIST`, `ARTICLE`. |
| `identifier` | String | Identifier of the navigation menu, unique among its siblings. |
| `sequence` | Int | Order of the navigation menu among its siblings. |
| `name` | String | Display name of the navigation menu. |

`ProjectEntity` fields:

| Field | Type | Description |
| --- | --- | --- |
| `projectId` | Long | Physical id of the project. |
| `createAt` | Long | Creation time, epoch milliseconds. |
| `updateAt` | Long | Last update time, epoch milliseconds. |
| `name` | String | Display name of the project. |
| `identifier` | String | Identifier of the project. |
| `region` | RegionEntity | OSS region descriptor (`value`, `regionName`, `regionId`). |
| `state` | ProjectState | Current project state. |
| `stagingRevision` | Long | Revision currently deployed to staging. |
| `productionRevision` | Long | Revision currently deployed to production. |
| `rootTemplateId` | Long? | Physical id of the root template, or null when unset. |
| `rootNavigationId` | Long? | Physical id of the root navigation, or null when unset. |
| `indexDocument` | String | Index document path of the deployed site. |
| `errorDocument` | String | Error document path of the deployed site. |

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
{
  "project": {
    "projectId": 3,
    "createAt": 1704067200000,
    "updateAt": 1704153600000,
    "name": "公司官网",
    "identifier": "corp-site",
    "region": {
      "value": "CN_HANGZHOU",
      "regionName": "华东1（杭州）",
      "regionId": "oss-cn-hangzhou"
    },
    "state": "DEPLOYED",
    "stagingRevision": 12,
    "productionRevision": 11,
    "rootTemplateId": 7,
    "rootNavigationId": 1,
    "indexDocument": "index.html",
    "errorDocument": "error.html"
  },
  "self": {
    "navigationId": 5,
    "createAt": 1704067200000,
    "updateAt": 1704153600000,
    "type": "LIST",
    "identifier": "news",
    "sequence": 2,
    "name": "新闻列表"
  },
  "parents": [
    {
      "navigationId": 1,
      "createAt": 1704067200000,
      "updateAt": 1704067200000,
      "type": "ROOT",
      "identifier": "root",
      "sequence": 1,
      "name": "根导航"
    }
  ],
  "children": [
    {
      "navigationId": 8,
      "createAt": 1704067200000,
      "updateAt": 1704153600000,
      "type": "ARTICLE",
      "identifier": "about",
      "sequence": 1,
      "name": "关于我们"
    }
  ]
}
```
