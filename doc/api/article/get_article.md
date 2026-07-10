# GET /v1/article/{articleId}

> Controller: ArticleController

Get detail information of an [article](/doc/tech_noun/TN0501_article.md).

## Permission

[`ARTICLE_READ`](/doc/permission/ARTICLE_READ.md)

## Request

### Path Parameters

| Name | Type | Description |
| --- | --- | --- |
| `articleId` | Long | Physical id of the article to fetch. |

### Query Parameters

N/A

### Body

N/A

## Response

DTO: `ArticleResponse` — wrapped as `data` inside the standard `PagerResponse` envelope
(`{ code, timestamp, data }`).

| Field | Type | Description |
| --- | --- | --- |
| `article` | ArticleContentEntity | Full detail of the article. |

`ArticleContentEntity` fields:

| Field | Type | Description |
| --- | --- | --- |
| `articleId` | Long | Physical id of the article. |
| `createAt` | Long | Creation time (epoch ms). |
| `updateAt` | Long | Last update time (epoch ms). |
| `title` | String | Article title. |
| `createdAuthor` | UserEntity | Author who created the article. |
| `lastAuthor` | UserEntity | Author who last edited the article. |
| `cover` | String? | Presigned URL of the cover image, or `null` when no cover is set. |
| `description` | String | Short article description. |
| `content` | String | Article body content. |

## Errors

| Case | HTTP Status | Error Code |
| --- | --- | --- |
| Caller's role lacks the required permission | 400 Bad Request | [20002 UserNotPermitted](/doc/backend_error/user/20002.md) |
| Article does not exist | 400 Bad Request | [80001 ArticleIdNotExist](/doc/backend_error/article/80001.md) |
| Caller has no privilege on the owning project | 400 Bad Request | [40002 ProjectNoPrivilege](/doc/backend_error/project/40002.md) |
| Caller resolved during the privilege check does not exist | 400 Bad Request | [20001 UserIdNotExist](/doc/backend_error/user/20001.md) |
| The article has no stored content | 400 Bad Request | [80002 ArticleContentNotExist](/doc/backend_error/article/80002.md) |

## Example

Request: `N/A` (no body).

Success response (the `data` payload):

```json
{
  "article": {
    "articleId": 101,
    "createAt": 1710000000000,
    "updateAt": 1710050000000,
    "title": "公司新闻",
    "createdAuthor": {
      "userId": 1,
      "identifier": "editor01",
      "name": "张三",
      "mail": "editor01@example.com",
      "enabled": true
    },
    "lastAuthor": {
      "userId": 2,
      "identifier": "editor02",
      "name": "李四",
      "mail": "editor02@example.com",
      "enabled": true
    },
    "cover": "https://oss.example.com/metadata/article/101/cover/8f2c.jpg?Expires=1710100000&Signature=abc",
    "description": "最新的公司动态与新闻发布。",
    "content": "<p>正文内容</p>"
  }
}
```
