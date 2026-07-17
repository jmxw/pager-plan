# POST /v1/article_list/{articleListId}/items

> Controller: ArticleListController

Get all items of an article list.

## Permission

[`ARTICLE_LIST_READ`](/doc/permission/ARTICLE_LIST_READ.md) — held by every role (OWNER, ADMIN,
DEVELOPER, CUSTOMER), so the permission check cannot fail for a valid login.

## Request

### Path Parameters

| Name | Type | Description |
| --- | --- | --- |
| `articleListId` | Long | Id of the [article list](/doc/tech_noun/TN0502_article_list.md) whose items are read. |

### Query Parameters

N/A

### Body

DTO: `ArticleListItemSearchRequest`

| Field | Type | Required | Description |
| --- | --- | --- | --- |
| `languageType` | LanguageType | ❌ | The [language](/doc/tech_noun/TN0302_language.md) to filter the item [articles](/doc/tech_noun/TN0501_article.md) by (enum values: see [get_project_available_language_types.md](/doc/api/project/get_project_available_language_types.md)). Defaults to the project's default language when omitted. |
| `page` | Int | ❌ | 0-based page number. Default `0`. |
| `size` | Int | ❌ | Page size. Default `10`. |
| `title` | String | ❌ | Article title filter (contains match). Default `""` (no filter). |

## Response

DTO: `ArticleListItemSearchResponse`, returned inside the `PagerResponse` envelope as `data` —
see the [API index](/doc/api/README.md#response-envelope). Items are ordered by publish time,
newest first.

| Field | Type | Description |
| --- | --- | --- |
| `page` | Int | The requested page number, echoed back. |
| `size` | Int | The requested page size, echoed back. |
| `total` | Long | Total number of matching items across all pages. |
| `articleListItems[].articleListItemId` | Long | Id of the list membership row. |
| `articleListItems[].publishAt` | Long | Timestamp the article was published into the list (epoch milliseconds). |
| `articleListItems[].article` | ArticleEntity | The member article — fields as in [search_articles.md](/doc/api/article/search_articles.md). |

## Errors

| Case | HTTP Status | Error Code |
| --- | --- | --- |
| No article list exists for `{articleListId}` | 400 | [70001 ArticleListIdNotExist](/doc/backend_error/article_list/70001.md) |
| The caller (DEVELOPER / CUSTOMER role) has no [privilege](/doc/tech_noun/TN0203_privilege.md) grant for the list's [project](/doc/tech_noun/TN0301_project.md) | 400 | [40002 ProjectNoPrivilege](/doc/backend_error/project/40002.md) |
| The calling user's account row no longer exists (privilege check) | 400 | [20001 UserIdNotExist](/doc/backend_error/user/20001.md) |
| `languageType` is given but not enabled for the project | 400 | [50002 LanguageNotExist](/doc/backend_error/language/50002.md) |
| `languageType` is omitted and the project has no default language (data inconsistency) — answered as `-9999 UnknownError` | 500 | [50003 LanguageDefaultNotFound](/doc/backend_error/language/50003.md) |

A missing or invalid JWT is answered with `401 Unauthorized` (empty body) before the handler runs.

## Example

Request:

```json
{
  "languageType": "ENGLISH",
  "page": 0,
  "size": 10,
  "title": ""
}
```

Success response:

```json
{
  "code": 200,
  "timestamp": 1752700000000,
  "data": {
    "page": 0,
    "size": 10,
    "total": 1,
    "articleListItems": [
      {
        "articleListItemId": 21,
        "publishAt": 1752650000000,
        "article": {
          "articleId": 11,
          "title": "Company news",
          "createAt": 1752600000000,
          "updateAt": 1752650000000,
          "createdAuthor": {
            "userId": 2,
            "identifier": "alice",
            "name": "Alice",
            "mail": "alice@example.com",
            "enabled": true
          },
          "lastAuthor": {
            "userId": 2,
            "identifier": "alice",
            "name": "Alice",
            "mail": "alice@example.com",
            "enabled": true
          }
        }
      }
    ]
  }
}
```
