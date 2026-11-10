# POST /v1/article/search

> Controller: ArticleController

Search articles in a project.

## Permission

[`ARTICLE_READ`](/doc/permission/ARTICLE_READ.md) — held by every role (OWNER, ADMIN, DEVELOPER,
CUSTOMER), so the permission check cannot fail for a valid login.

## Request

### Path Parameters

N/A

### Query Parameters

N/A

### Body

DTO: `ArticleSearchRequest`

| Field | Type | Required | Description |
| --- | --- | --- | --- |
| `projectId` | Long | ✅ | Id of the [project](/doc/tech_noun/TN0301_project.md) to search in. |
| `languageType` | LanguageType | ❌ | The [language](/doc/tech_noun/TN0302_language.md) to search in (enum values: see [get_project_available_language_types.md](/doc/api/project/get_project_available_language_types.md)). Defaults to the project's default language when omitted. |
| `page` | Int | ❌ | 0-based page number. Default `0`. |
| `size` | Int | ❌ | Page size. Default `10`. |
| `title` | String | ❌ | Title filter (contains match). Default `""` (no filter). |

## Response

DTO: `ArticleSearchResponse`, returned inside the `PagerResponse` envelope as `data` — see the
[API index](/doc/api/README.md#response-envelope). [Articles](/doc/tech_noun/TN0501_article.md)
are ordered by creation time, newest first.

| Field | Type | Description |
| --- | --- | --- |
| `page` | Int | The requested page number, echoed back. |
| `size` | Int | The requested page size, echoed back. |
| `total` | Long | Total number of matching articles across all pages. |
| `articles[].articleId` | Long | Id of the article. |
| `articles[].title` | String | Title of the article. |
| `articles[].createAt` | Long | Creation timestamp (epoch milliseconds). |
| `articles[].updateAt` | Long | Last-update timestamp (epoch milliseconds). |
| `articles[].createdAuthor` | UserEntity | The [user](/doc/tech_noun/TN0202_user.md) who created the article — fields as in [get_users.md](/doc/api/user/get_users.md). |
| `articles[].lastAuthor` | UserEntity | The user who last edited the article. |

## Errors

| Case | HTTP Status | Error Code |
| --- | --- | --- |
| No project exists for `projectId` | 400 | [40001 ProjectIdNotExist](/doc/backend_error/project/40001.md) |
| The project belongs to another organization | 400 | [40003 ProjectNotBelongToOrganization](/doc/backend_error/project/40003.md) |
| The caller (DEVELOPER / CUSTOMER role) has no [privilege](/doc/tech_noun/TN0203_privilege.md) grant for the project | 400 | [40002 ProjectNoPrivilege](/doc/backend_error/project/40002.md) |
| The calling user's account row no longer exists (privilege check) | 400 | [20001 UserIdNotExist](/doc/backend_error/user/20001.md) |
| `languageType` is given but not enabled for the project | 400 | [50002 LanguageNotExist](/doc/backend_error/language/50002.md) |
| `languageType` is omitted and the project has no default language (data inconsistency) — answered as `-9999 UnknownError` | 500 | [50003 LanguageDefaultNotFound](/doc/backend_error/language/50003.md) |

A missing or invalid JWT is answered with `401 Unauthorized` (empty body) before the handler runs.

## Example

Request:

```json
{
  "projectId": 3,
  "languageType": "ENGLISH",
  "page": 0,
  "size": 10,
  "title": "news"
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
    "articles": [
      {
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
    ]
  }
}
```
