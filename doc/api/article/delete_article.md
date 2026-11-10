# DELETE /v1/article/{articleId}

> Controller: ArticleController

Delete an article.

## Permission

[`ARTICLE_WRITE`](/doc/permission/ARTICLE_WRITE.md) — held by every role (OWNER, ADMIN, DEVELOPER,
CUSTOMER), so the permission check cannot fail for a valid login.

## Request

### Path Parameters

| Name | Type | Description |
| --- | --- | --- |
| `articleId` | Long | Id of the [article](/doc/tech_noun/TN0501_article.md) to delete. |

### Query Parameters

N/A

### Body

N/A

## Response

DTO: `EmptyResponse`, returned inside the `PagerResponse` envelope as `data` — see the
[API index](/doc/api/README.md#response-envelope). The article, its content, and its
[article list](/doc/tech_noun/TN0502_article_list.md) memberships are removed; each affected
list's [revision](/doc/tech_noun/TN0102_revision.md) is incremented so the next deployment
re-renders it.

| Field | Type | Description |
| --- | --- | --- |
| — | — | `data` is an empty object. |

## Errors

| Case | HTTP Status | Error Code |
| --- | --- | --- |
| No article exists for `{articleId}` | 400 | [80001 ArticleIdNotExist](/doc/backend_error/article/80001.md) |
| The caller (DEVELOPER / CUSTOMER role) has no [privilege](/doc/tech_noun/TN0203_privilege.md) grant for the article's [project](/doc/tech_noun/TN0301_project.md) | 400 | [40002 ProjectNoPrivilege](/doc/backend_error/project/40002.md) |
| The calling user's account row no longer exists (privilege check) | 400 | [20001 UserIdNotExist](/doc/backend_error/user/20001.md) |

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
