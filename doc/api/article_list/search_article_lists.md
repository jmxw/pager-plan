# POST /v1/article_list/search

> Controller: ArticleListController

Search article lists in a project.

Returns a page of the [article lists](/doc/tech_noun/TN0502_article_list.md) that belong to a
[project](/doc/tech_noun/TN0301_project.md), ordered by creation time descending and optionally
filtered by name.

## Permission

[`ARTICLE_READ`](/doc/permission/ARTICLE_READ.md)

## Request

### Path Parameters

N/A

### Query Parameters

N/A

### Body

DTO: `ArticleListSearchRequest`

| Field | Type | Required | Description |
| --- | --- | --- | --- |
| `projectId` | Long | Yes | Physical id of the project whose article lists are searched. |
| `page` | Int | No | Zero-based page index. Defaults to `0`. |
| `size` | Int | No | Page size. Defaults to `20`. |
| `name` | String | No | Name filter matched against the article list name. Defaults to `""` (no filter). |

## Response

DTO: `ArticleListSearchResponse` — wrapped as `data` inside the standard `PagerResponse` envelope
(`{ code, timestamp, data }`).

| Field | Type | Description |
| --- | --- | --- |
| `page` | Int | Echoed zero-based page index. |
| `size` | Int | Echoed page size. |
| `total` | Long | Total number of matching article lists. |
| `project` | ProjectEntity | The project the article lists belong to. |
| `articleLists` | List\<ArticleListEntity\> | The matching article lists for this page. |

Each `ArticleListEntity` element carries the following fields:

| Field | Type | Description |
| --- | --- | --- |
| `articleListId` | Long | Physical id of the article list. |
| `createAt` | Long | Creation timestamp in epoch milliseconds. |
| `updateAt` | Long | Last update timestamp in epoch milliseconds. |
| `identifier` | String | Identifier of the article list, unique within its project. |
| `name` | String | Display name of the article list. |
| `template` | TemplateEntity | The template bound to render the article list. |

Each `TemplateEntity` carries the following fields:

| Field | Type | Description |
| --- | --- | --- |
| `templateId` | Long | Physical id of the template. |
| `createAt` | Long | Creation timestamp in epoch milliseconds. |
| `updateAt` | Long | Last update timestamp in epoch milliseconds. |
| `type` | TemplateType | Template type (e.g. `HTML`, `HTM`, `CSS`). |
| `name` | String | File name of the template. |
| `layer` | Int | Directory depth of the template. |
| `errorCount` | Int | Number of precompile errors recorded against the template. |

The `project` object is a `ProjectEntity`:

| Field | Type | Description |
| --- | --- | --- |
| `projectId` | Long | Physical id of the project. |
| `createAt` | Long | Creation timestamp in epoch milliseconds. |
| `updateAt` | Long | Last update timestamp in epoch milliseconds. |
| `name` | String | Display name of the project. |
| `identifier` | String | Identifier of the project. |
| `region` | RegionEntity | OSS region descriptor (`value`, `regionName`, `regionId`). |
| `state` | ProjectState | Current project state. |
| `stagingRevision` | Long | Revision currently published to staging. |
| `productionRevision` | Long | Revision currently published to production. |
| `rootTemplateId` | Long? | Physical id of the root template, or `null`. |
| `rootNavigationId` | Long? | Physical id of the root navigation, or `null`. |
| `indexDocument` | String | Index document file name. |
| `errorDocument` | String | Error document file name. |

## Errors

| Case | HTTP Status | Error Code |
| --- | --- | --- |
| Caller's role lacks the required permission | 400 Bad Request | [20002 UserNotPermitted](/doc/backend_error/user/20002.md) |
| No project exists with the given id | 400 Bad Request | [40001 ProjectIdNotExist](/doc/backend_error/project/40001.md) |
| The project belongs to a different organization than the caller's | 400 Bad Request | [40003 ProjectNotBelongToOrganization](/doc/backend_error/project/40003.md) |
| Caller has no privilege on the project | 400 Bad Request | [40002 ProjectNoPrivilege](/doc/backend_error/project/40002.md) |
| Caller's user record does not exist | 400 Bad Request | [20001 UserIdNotExist](/doc/backend_error/user/20001.md) |

## Example

Request:

```json
{
  "projectId": 1,
  "page": 0,
  "size": 20,
  "name": "新闻"
}
```

Success response (the `data` payload):

```json
{
  "page": 0,
  "size": 20,
  "total": 1,
  "project": {
    "projectId": 1,
    "createAt": 1710000000000,
    "updateAt": 1710500000000,
    "name": "官方网站",
    "identifier": "official-site",
    "region": {
      "value": "CN_HANGZHOU",
      "regionName": "华东1（杭州）",
      "regionId": "oss-cn-hangzhou"
    },
    "state": "DEPLOYED",
    "stagingRevision": 12,
    "productionRevision": 8,
    "rootTemplateId": 100,
    "rootNavigationId": 200,
    "indexDocument": "index.html",
    "errorDocument": "error.html"
  },
  "articleLists": [
    {
      "articleListId": 5,
      "createAt": 1710000000000,
      "updateAt": 1710400000000,
      "identifier": "news",
      "name": "新闻列表",
      "template": {
        "templateId": 100,
        "createAt": 1709000000000,
        "updateAt": 1709500000000,
        "type": "HTML",
        "name": "news-list.html",
        "layer": 0,
        "errorCount": 0
      }
    }
  ]
}
```
