# DELETE /v1/article/{articleId}

> Controller: ArticleController

Delete an [article](/doc/tech_noun/TN0501_article.md).

## Permission

[`ARTICLE_WRITE`](/doc/permission/ARTICLE_WRITE.md)

## Request

### Path Parameters

| Name | Type | Description |
| --- | --- | --- |
| `articleId` | Long | Physical id of the article to delete. |

### Query Parameters

N/A

### Body

N/A

Deleting an article also removes its content and its membership in any
[article list](/doc/tech_noun/TN0502_article_list.md), and bumps the revision of each article list
that referenced it.

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
| Article does not exist | 400 Bad Request | [80001 ArticleIdNotExist](/doc/backend_error/article/80001.md) |
| Caller has no privilege on the owning project | 400 Bad Request | [40002 ProjectNoPrivilege](/doc/backend_error/project/40002.md) |
| Caller resolved during the privilege check does not exist | 400 Bad Request | [20001 UserIdNotExist](/doc/backend_error/user/20001.md) |

## Example

Request: `N/A` (no body).

Success response (the `data` payload):

```json
{}
```
