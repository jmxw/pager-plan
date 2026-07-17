# GET /v1/article_list/{articleListId}

> Controller: ArticleListController

Get detail information of an article list.

## Permission

[`ARTICLE_LIST_READ`](/doc/permission/ARTICLE_LIST_READ.md) — held by every role (OWNER, ADMIN,
DEVELOPER, CUSTOMER), so the permission check cannot fail for a valid login.

## Request

### Path Parameters

| Name | Type | Description |
| --- | --- | --- |
| `articleListId` | Long | Id of the [article list](/doc/tech_noun/TN0502_article_list.md) to read. |

### Query Parameters

N/A

### Body

N/A

## Response

DTO: `ArticleListResponse`, returned inside the `PagerResponse` envelope as `data` — see the
[API index](/doc/api/README.md#response-envelope).

| Field | Type | Description |
| --- | --- | --- |
| `project` | ProjectEntity | The owning [project](/doc/tech_noun/TN0301_project.md) — fields as in [get_project.md](/doc/api/project/get_project.md). |
| `articleList.articleListId` | Long | Id of the article list. |
| `articleList.createAt` | Long | Creation timestamp (epoch milliseconds). |
| `articleList.updateAt` | Long | Last-update timestamp (epoch milliseconds). |
| `articleList.identifier` | String | The [identifier](/doc/tech_noun/TN0101_identifier.md) referenced by `{pager:list id="…"}` [tags](/doc/tech_noun/TN0403_pager_tag.md). |
| `articleList.name` | String | Display name of the list. |
| `articleList.template` | TemplateEntity | The [template](/doc/tech_noun/TN0401_template.md) that renders one article of this list — fields as in [read_directory.md](/doc/api/template/read_directory.md). |

## Errors

| Case | HTTP Status | Error Code |
| --- | --- | --- |
| No article list exists for `{articleListId}` | 400 | [70001 ArticleListIdNotExist](/doc/backend_error/article_list/70001.md) |
| The caller (DEVELOPER / CUSTOMER role) has no [privilege](/doc/tech_noun/TN0203_privilege.md) grant for the list's project | 400 | [40002 ProjectNoPrivilege](/doc/backend_error/project/40002.md) |
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
    "articleList": {
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
  }
}
```
