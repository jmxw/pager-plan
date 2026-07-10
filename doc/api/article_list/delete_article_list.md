# DELETE /v1/article_list/{articleListId}

> Controller: ArticleListController

Delete an article list.

Deletes an [article list](/doc/tech_noun/TN0502_article_list.md) together with its items. Deletion
is rejected when the article list is still referenced by precompiled template content, so any
template using the list must be updated first.

## Permission

[`ARTICLE_LIST_WRITE`](/doc/permission/ARTICLE_LIST_WRITE.md)

## Request

### Path Parameters

| Name | Type | Description |
| --- | --- | --- |
| `articleListId` | Long | Physical id of the article list to delete. |

### Query Parameters

N/A

### Body

N/A

## Response

DTO: `EmptyResponse` — wrapped as `data` inside the standard `PagerResponse` envelope
(`{ code, timestamp, data }`). The `data` body is empty (`{}`).

| Field | Type | Description |
| --- | --- | --- |
| — | — | The response body is empty (`{}`). |

## Errors

| Case | HTTP Status | Error Code |
| --- | --- | --- |
| Caller's role lacks the required permission | 400 Bad Request | [20002 UserNotPermitted](/doc/backend_error/user/20002.md) |
| No article list exists with the given id | 400 Bad Request | [70001 ArticleListIdNotExist](/doc/backend_error/article_list/70001.md) |
| Caller has no privilege on the article list's project | 400 Bad Request | [40002 ProjectNoPrivilege](/doc/backend_error/project/40002.md) |
| Caller's user record does not exist | 400 Bad Request | [20001 UserIdNotExist](/doc/backend_error/user/20001.md) |
| The article list is still referenced by precompiled template content | 400 Bad Request | [70010 ArticleDeleteRejectedDueToPagerListExisting](/doc/backend_error/article_list/70010.md) |

## Example

Request: `N/A` (no body).

Success response (the `data` payload):

```json
{}
```
