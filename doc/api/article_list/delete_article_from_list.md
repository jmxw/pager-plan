# DELETE /v1/article_list/{articleListId}/article/{articleId}

> Controller: ArticleListController

Delete and article from an article list.

Removes an [article](/doc/tech_noun/TN0501_article.md) from an
[article list](/doc/tech_noun/TN0502_article_list.md). The article and the article list must belong
to the same [project](/doc/tech_noun/TN0301_project.md), and the article must currently be an item
of the list. The article list revision is bumped so the change is picked up on the next deployment.

## Permission

[`ARTICLE_LIST_WRITE`](/doc/permission/ARTICLE_LIST_WRITE.md)

## Request

### Path Parameters

| Name | Type | Description |
| --- | --- | --- |
| `articleListId` | Long | Physical id of the article list to remove from. |
| `articleId` | Long | Physical id of the article to remove. |

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
| No article exists with the given id | 400 Bad Request | [80001 ArticleIdNotExist](/doc/backend_error/article/80001.md) |
| The article and the article list belong to different projects | 400 Bad Request | [70007 ArticleListArticleMismatching](/doc/backend_error/article_list/70007.md) |
| Caller has no privilege on the article's project | 400 Bad Request | [40002 ProjectNoPrivilege](/doc/backend_error/project/40002.md) |
| Caller's user record does not exist | 400 Bad Request | [20001 UserIdNotExist](/doc/backend_error/user/20001.md) |
| The article is not an item of the article list | 400 Bad Request | [70009 ArticleListArticleNotExist](/doc/backend_error/article_list/70009.md) |

## Example

Request: `N/A` (no body).

Success response (the `data` payload):

```json
{}
```
