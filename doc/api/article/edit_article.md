# PUT /v1/article/{articleId}

> Controller: ArticleController

Edit detail information of an [article](/doc/tech_noun/TN0501_article.md).

## Permission

[`ARTICLE_WRITE`](/doc/permission/ARTICLE_WRITE.md)

## Request

### Path Parameters

| Name | Type | Description |
| --- | --- | --- |
| `articleId` | Long | Physical id of the article to edit. |

### Query Parameters

N/A

### Body

DTO: `ArticleEditRequest`

| Field | Type | Required | Description |
| --- | --- | --- | --- |
| `title` | String | Yes | New article title. |
| `description` | String | Yes | New short article description. |
| `content` | String | Yes | New article body content; replaces the existing content. |

Saving bumps the article's revision and records the caller as the last author.

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
| The article has no stored content | 400 Bad Request | [80002 ArticleContentNotExist](/doc/backend_error/article/80002.md) |

## Example

Request:

```json
{
  "title": "公司新闻（更新）",
  "description": "更新后的公司动态描述。",
  "content": "<p>更新后的正文内容</p>"
}
```

Success response (the `data` payload):

```json
{}
```
