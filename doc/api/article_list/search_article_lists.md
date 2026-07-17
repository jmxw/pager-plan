# POST /v1/article_list/search

> Controller: ArticleListController

Search article lists in a project.

## Permission

[`ARTICLE_READ`](/doc/permission/ARTICLE_READ.md) — held by every role (OWNER, ADMIN, DEVELOPER,
CUSTOMER), so the permission check cannot fail for a valid login.

## Request

### Path Parameters

N/A

### Query Parameters

N/A

### Body

DTO: `ArticleListSearchRequest`

| Field | Type | Required | Description |
| --- | --- | --- | --- |
| `projectId` | Long | ✅ | Id of the [project](/doc/tech_noun/TN0301_project.md) to search in. |
| `page` | Int | ❌ | 0-based page number. Default `0`. |
| `size` | Int | ❌ | Page size. Default `20`. |
| `name` | String | ❌ | Name filter (contains match). Default `""` (no filter). |

## Response

DTO: `ArticleListSearchResponse`, returned inside the `PagerResponse` envelope as `data` — see the
[API index](/doc/api/README.md#response-envelope).
[Article lists](/doc/tech_noun/TN0502_article_list.md) are ordered by creation time, newest first.

| Field | Type | Description |
| --- | --- | --- |
| `page` | Int | The requested page number, echoed back. |
| `size` | Int | The requested page size, echoed back. |
| `total` | Long | Total number of matching article lists across all pages. |
| `project` | ProjectEntity | The searched project — fields as in [get_project.md](/doc/api/project/get_project.md). |
| `articleLists[]` | ArticleListEntity | The matching article lists — fields as in [get_article_list.md](get_article_list.md). |

## Errors

| Case | HTTP Status | Error Code |
| --- | --- | --- |
| No project exists for `projectId` | 400 | [40001 ProjectIdNotExist](/doc/backend_error/project/40001.md) |
| The project belongs to another organization | 400 | [40003 ProjectNotBelongToOrganization](/doc/backend_error/project/40003.md) |
| The caller (DEVELOPER / CUSTOMER role) has no [privilege](/doc/tech_noun/TN0203_privilege.md) grant for the project | 400 | [40002 ProjectNoPrivilege](/doc/backend_error/project/40002.md) |
| The calling user's account row no longer exists (privilege check) | 400 | [20001 UserIdNotExist](/doc/backend_error/user/20001.md) |

A missing or invalid JWT is answered with `401 Unauthorized` (empty body) before the handler runs.

## Example

Request:

```json
{
  "projectId": 3,
  "page": 0,
  "size": 20,
  "name": "news"
}
```

Success response:

```json
{
  "code": 200,
  "timestamp": 1752700000000,
  "data": {
    "page": 0,
    "size": 20,
    "total": 1,
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
    "articleLists": [
      {
        "articleListId": 5,
        "createAt": 1752600000000,
        "updateAt": 1752650000000,
        "identifier": "news",
        "name": "News list",
        "template": {
          "templateId": 120,
          "createAt": 1752600000000,
          "updateAt": 1752650000000,
          "type": "HTML",
          "name": "news.html",
          "layer": 2,
          "errorCount": 0
        }
      }
    ]
  }
}
```
