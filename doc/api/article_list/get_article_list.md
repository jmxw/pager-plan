# GET /v1/article_list/{articleListId}

> Controller: ArticleListController

Get detail information of an article list.

Returns a single [article list](/doc/tech_noun/TN0502_article_list.md) together with the
[project](/doc/tech_noun/TN0301_project.md) it belongs to.

## Permission

[`ARTICLE_LIST_READ`](/doc/permission/ARTICLE_LIST_READ.md)

## Request

### Path Parameters

| Name | Type | Description |
| --- | --- | --- |
| `articleListId` | Long | Physical id of the article list to fetch. |

### Query Parameters

N/A

### Body

N/A

## Response

DTO: `ArticleListResponse` — wrapped as `data` inside the standard `PagerResponse` envelope
(`{ code, timestamp, data }`).

| Field | Type | Description |
| --- | --- | --- |
| `project` | ProjectEntity | The project the article list belongs to. |
| `articleList` | ArticleListEntity | The requested article list. |

The `articleList` object is an `ArticleListEntity`:

| Field | Type | Description |
| --- | --- | --- |
| `articleListId` | Long | Physical id of the article list. |
| `createAt` | Long | Creation timestamp in epoch milliseconds. |
| `updateAt` | Long | Last update timestamp in epoch milliseconds. |
| `identifier` | String | Identifier of the article list, unique within its project. |
| `name` | String | Display name of the article list. |
| `template` | TemplateEntity | The template bound to render the article list (`templateId`, `createAt`, `updateAt`, `type`, `name`, `layer`, `errorCount`). |

The `project` object is a `ProjectEntity` (`projectId`, `createAt`, `updateAt`, `name`,
`identifier`, `region`, `state`, `stagingRevision`, `productionRevision`, `rootTemplateId`,
`rootNavigationId`, `indexDocument`, `errorDocument`).

## Errors

| Case | HTTP Status | Error Code |
| --- | --- | --- |
| Caller's role lacks the required permission | 400 Bad Request | [20002 UserNotPermitted](/doc/backend_error/user/20002.md) |
| No article list exists with the given id | 400 Bad Request | [70001 ArticleListIdNotExist](/doc/backend_error/article_list/70001.md) |
| Caller has no privilege on the article list's project | 400 Bad Request | [40002 ProjectNoPrivilege](/doc/backend_error/project/40002.md) |
| Caller's user record does not exist | 400 Bad Request | [20001 UserIdNotExist](/doc/backend_error/user/20001.md) |

## Example

Request: `N/A` (no body).

Success response (the `data` payload):

```json
{
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
  "articleList": {
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
}
```
