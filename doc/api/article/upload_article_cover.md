# POST /v1/article/{articleId}/cover

> Controller: ArticleController

Upload cover image for an [article](/doc/tech_noun/TN0501_article.md).

## Permission

[`ARTICLE_WRITE`](/doc/permission/ARTICLE_WRITE.md)

## Request

### Path Parameters

| Name | Type | Description |
| --- | --- | --- |
| `articleId` | Long | Physical id of the article whose cover is uploaded. |

### Query Parameters

N/A

### Body

DTO: `N/A` — the request is `multipart/form-data`, not JSON.

| Form field | Type | Required | Description |
| --- | --- | --- | --- |
| `cover` | File (multipart) | Yes | The cover image file to store for the article. |

Any previously uploaded cover is removed from storage before the new file is saved; the article's
cover reference is replaced and its revision is bumped.

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
| The uploaded cover file could not be saved | 400 Bad Request | [80003 ArticleCoverSaveError](/doc/backend_error/article/80003.md) |

## Example

Request: `multipart/form-data` with a single `cover` file part.

```
Content-Type: multipart/form-data; boundary=----PagerBoundary

------PagerBoundary
Content-Disposition: form-data; name="cover"; filename="cover.jpg"
Content-Type: image/jpeg

<binary image bytes>
------PagerBoundary--
```

Success response (the `data` payload):

```json
{}
```
