# DELETE /v1/article_list/{articleListId}

> Controller: ArticleListController

Delete an article list.

## Permission

[`ARTICLE_LIST_WRITE`](/doc/permission/ARTICLE_LIST_WRITE.md) — held by every role (OWNER, ADMIN,
DEVELOPER, CUSTOMER), so the permission check cannot fail for a valid login.

## Request

### Path Parameters

| Name | Type | Description |
| --- | --- | --- |
| `articleListId` | Long | Id of the [article list](/doc/tech_noun/TN0502_article_list.md) to delete. |

### Query Parameters

N/A

### Body

N/A

## Response

DTO: `EmptyResponse`, returned inside the `PagerResponse` envelope as `data` — see the
[API index](/doc/api/README.md#response-envelope). The list and its items are removed; the
[articles](/doc/tech_noun/TN0501_article.md) themselves are kept.

| Field | Type | Description |
| --- | --- | --- |
| — | — | `data` is an empty object. |

## Errors

| Case | HTTP Status | Error Code |
| --- | --- | --- |
| No article list exists for `{articleListId}` | 400 | [70001 ArticleListIdNotExist](/doc/backend_error/article_list/70001.md) |
| The caller (DEVELOPER / CUSTOMER role) has no [privilege](/doc/tech_noun/TN0203_privilege.md) grant for the list's [project](/doc/tech_noun/TN0301_project.md) | 400 | [40002 ProjectNoPrivilege](/doc/backend_error/project/40002.md) |
| The calling user's account row no longer exists (privilege check) | 400 | [20001 UserIdNotExist](/doc/backend_error/user/20001.md) |
| The list is still referenced by a precompiled `{pager:list}` [tag](/doc/tech_noun/TN0403_pager_tag.md) in at least one deployed [template](/doc/tech_noun/TN0401_template.md) | 400 | [70010 ArticleDeleteRejectedDueToPagerListExisting](/doc/backend_error/article_list/70010.md) |

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
