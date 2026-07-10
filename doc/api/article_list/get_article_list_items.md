# POST /v1/article_list/{articleListId}/items

> Controller: ArticleListController

Get all items of an article list.

Returns a page of the items of an [article list](/doc/tech_noun/TN0502_article_list.md) — each item
is an [article](/doc/tech_noun/TN0501_article.md) that has been added to the list — resolved for a
given [language type](/doc/tech_noun/TN0302_language.md), ordered by publish time descending and
optionally filtered by title.

## Permission

[`ARTICLE_LIST_READ`](/doc/permission/ARTICLE_LIST_READ.md)

## Request

### Path Parameters

| Name | Type | Description |
| --- | --- | --- |
| `articleListId` | Long | Physical id of the article list whose items are fetched. |

### Query Parameters

N/A

### Body

DTO: `ArticleListItemSearchRequest`

| Field | Type | Required | Description |
| --- | --- | --- | --- |
| `languageType` | LanguageType? | No | Language type to resolve the article titles in. When `null`, the project's default language is used. |
| `page` | Int | No | Zero-based page index. Defaults to `0`. |
| `size` | Int | No | Page size. Defaults to `10`. |
| `title` | String | No | Title filter matched against the article title. Defaults to `""` (no filter). |

## Response

DTO: `ArticleListItemSearchResponse` — wrapped as `data` inside the standard `PagerResponse`
envelope (`{ code, timestamp, data }`).

| Field | Type | Description |
| --- | --- | --- |
| `page` | Int | Echoed zero-based page index. |
| `size` | Int | Echoed page size. |
| `total` | Long | Total number of matching items. |
| `articleListItems` | List\<ArticleListItemEntity\> | The matching items for this page. |

Each `ArticleListItemEntity` element carries the following fields:

| Field | Type | Description |
| --- | --- | --- |
| `articleListItemId` | Long | Physical id of the article-list item (the membership record). |
| `publishAt` | Long | Timestamp in epoch milliseconds when the article was added to the list. |
| `article` | ArticleEntity | The article referenced by this item. |

Each `ArticleEntity` carries the following fields:

| Field | Type | Description |
| --- | --- | --- |
| `articleId` | Long | Physical id of the article. |
| `title` | String | Article title in the resolved language. |
| `createAt` | Long | Creation timestamp in epoch milliseconds. |
| `updateAt` | Long | Last update timestamp in epoch milliseconds. |
| `createdAuthor` | UserEntity | The user who created the article. |
| `lastAuthor` | UserEntity | The user who last edited the article. |

Each `UserEntity` carries `userId` (Long), `identifier` (String), `name` (String), `mail` (String),
and `enabled` (Boolean).

## Errors

| Case | HTTP Status | Error Code |
| --- | --- | --- |
| Caller's role lacks the required permission | 400 Bad Request | [20002 UserNotPermitted](/doc/backend_error/user/20002.md) |
| No article list exists with the given id | 400 Bad Request | [70001 ArticleListIdNotExist](/doc/backend_error/article_list/70001.md) |
| Caller has no privilege on the article list's project | 400 Bad Request | [40002 ProjectNoPrivilege](/doc/backend_error/project/40002.md) |
| Caller's user record does not exist | 400 Bad Request | [20001 UserIdNotExist](/doc/backend_error/user/20001.md) |
| The requested `languageType` is not configured for the project | 400 Bad Request | [50002 LanguageNotExist](/doc/backend_error/language/50002.md) |

## Example

Request:

```json
{
  "languageType": "SIMPLIFIED_CHINESE",
  "page": 0,
  "size": 10,
  "title": "发布"
}
```

Success response (the `data` payload):

```json
{
  "page": 0,
  "size": 10,
  "total": 1,
  "articleListItems": [
    {
      "articleListItemId": 30,
      "publishAt": 1710400000000,
      "article": {
        "articleId": 12,
        "title": "新版本发布公告",
        "createAt": 1710300000000,
        "updateAt": 1710350000000,
        "createdAuthor": {
          "userId": 2,
          "identifier": "editor",
          "name": "张编辑",
          "mail": "editor@example.com",
          "enabled": true
        },
        "lastAuthor": {
          "userId": 2,
          "identifier": "editor",
          "name": "张编辑",
          "mail": "editor@example.com",
          "enabled": true
        }
      }
    }
  ]
}
```
