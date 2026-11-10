# DELETE /v1/article_list/{articleListId}/article/{articleId}

> Controller: ArticleListController

Delete and article from an article list.

## Permission

[`ARTICLE_LIST_WRITE`](/doc/permission/ARTICLE_LIST_WRITE.md) — held by every role (OWNER, ADMIN,
DEVELOPER, CUSTOMER), so the permission check cannot fail for a valid login.

## Request

### Path Parameters

| Name | Type | Description |
| --- | --- | --- |
| `articleListId` | Long | Id of the [article list](/doc/tech_noun/TN0502_article_list.md) the article is removed from. |
| `articleId` | Long | Id of the [article](/doc/tech_noun/TN0501_article.md) to remove. The article itself is kept. |

### Query Parameters

N/A

### Body

N/A

## Response

DTO: `EmptyResponse`, returned inside the `PagerResponse` envelope as `data` — see the
[API index](/doc/api/README.md#response-envelope). The membership item is removed and the list's
[revision](/doc/tech_noun/TN0102_revision.md) is incremented.

| Field | Type | Description |
| --- | --- | --- |
| — | — | `data` is an empty object. |

## Errors

| Case | HTTP Status | Error Code |
| --- | --- | --- |
| No article list exists for `{articleListId}` | 400 | [70001 ArticleListIdNotExist](/doc/backend_error/article_list/70001.md) |
| No article exists for `{articleId}` | 400 | [80001 ArticleIdNotExist](/doc/backend_error/article/80001.md) |
| The list and the article belong to different [projects](/doc/tech_noun/TN0301_project.md) | 400 | [70007 ArticleListArticleMismatching](/doc/backend_error/article_list/70007.md) |
| The caller (DEVELOPER / CUSTOMER role) has no [privilege](/doc/tech_noun/TN0203_privilege.md) grant for the project | 400 | [40002 ProjectNoPrivilege](/doc/backend_error/project/40002.md) |
| The calling user's account row no longer exists (privilege check) | 400 | [20001 UserIdNotExist](/doc/backend_error/user/20001.md) |
| The list does not contain the article | 400 | [70009 ArticleListArticleNotExist](/doc/backend_error/article_list/70009.md) |

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
