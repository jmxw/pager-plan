# POST /v1/article/search

> Controller: ArticleController

Search [articles](/doc/tech_noun/TN0501_article.md) in a [project](/doc/tech_noun/TN0301_project.md).

## Permission

[`ARTICLE_READ`](/doc/permission/ARTICLE_READ.md)

## Request

### Path Parameters

N/A

### Query Parameters

N/A

### Body

DTO: `ArticleSearchRequest`

| Field | Type | Required | Description |
| --- | --- | --- | --- |
| `projectId` | Long | Yes | Physical id of the project to search within. |
| `languageType` | LanguageType | No | [Language type](/doc/tech_noun/TN0302_language.md) filter; when omitted the project's default language is used. |
| `page` | Int | No | Zero-based page index (default `0`). |
| `size` | Int | No | Page size (default `10`). |
| `title` | String | No | Title keyword filter; empty (default `""`) matches all articles. |

Results are ordered by creation time, newest first.

## Response

DTO: `ArticleSearchResponse` — wrapped as `data` inside the standard `PagerResponse` envelope
(`{ code, timestamp, data }`).

| Field | Type | Description |
| --- | --- | --- |
| `page` | Int | Zero-based page index echoed from the request. |
| `size` | Int | Page size echoed from the request. |
| `total` | Long | Total number of matching articles across all pages. |
| `articles` | List\<ArticleEntity\> | The current page of matching articles. |

Each `ArticleEntity` carries `articleId` (Long), `title` (String), `createAt` (Long, epoch ms),
`updateAt` (Long, epoch ms), `createdAuthor` (UserEntity), and `lastAuthor` (UserEntity).

## Errors

| Case | HTTP Status | Error Code |
| --- | --- | --- |
| Caller's role lacks the required permission | 400 Bad Request | [20002 UserNotPermitted](/doc/backend_error/user/20002.md) |
| Project does not exist | 400 Bad Request | [40001 ProjectIdNotExist](/doc/backend_error/project/40001.md) |
| Project belongs to another organization | 400 Bad Request | [40003 ProjectNotBelongToOrganization](/doc/backend_error/project/40003.md) |
| Caller has no privilege on the project | 400 Bad Request | [40002 ProjectNoPrivilege](/doc/backend_error/project/40002.md) |
| Caller resolved during the privilege check does not exist | 400 Bad Request | [20001 UserIdNotExist](/doc/backend_error/user/20001.md) |
| The requested language type does not exist in the project | 400 Bad Request | [50002 LanguageNotExist](/doc/backend_error/language/50002.md) |

## Example

Request:

```json
{
  "projectId": 12,
  "languageType": "SIMPLIFIED_CHINESE",
  "page": 0,
  "size": 10,
  "title": "公司"
}
```

Success response (the `data` payload):

```json
{
  "page": 0,
  "size": 10,
  "total": 1,
  "articles": [
    {
      "articleId": 101,
      "title": "公司新闻",
      "createAt": 1710000000000,
      "updateAt": 1710050000000,
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
      }
    }
  ]
}
```
