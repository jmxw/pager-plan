# GET /v1/article/{articleId}

> Controller: ArticleController

Get detail information of an article.

## Permission

[`ARTICLE_READ`](/doc/permission/ARTICLE_READ.md) — held by every role (OWNER, ADMIN, DEVELOPER,
CUSTOMER), so the permission check cannot fail for a valid login.

## Request

### Path Parameters

| Name | Type | Description |
| --- | --- | --- |
| `articleId` | Long | Id of the [article](/doc/tech_noun/TN0501_article.md) to read. |

### Query Parameters

N/A

### Body

N/A

## Response

DTO: `ArticleResponse`, returned inside the `PagerResponse` envelope as `data` — see the
[API index](/doc/api/README.md#response-envelope). The `article` field carries the full
`ArticleContentEntity`, including the body content.

| Field | Type | Description |
| --- | --- | --- |
| `article.articleId` | Long | Id of the article. |
| `article.createAt` | Long | Creation timestamp (epoch milliseconds). |
| `article.updateAt` | Long | Last-update timestamp (epoch milliseconds). |
| `article.title` | String | Title of the article. |
| `article.createdAuthor` | UserEntity | The [user](/doc/tech_noun/TN0202_user.md) who created the article — fields as in [get_users.md](/doc/api/user/get_users.md). |
| `article.lastAuthor` | UserEntity | The user who last edited the article. |
| `article.cover` | String (nullable) | Presigned OSS URL of the cover image, or `null` when no cover has been uploaded. |
| `article.description` | String | Short description of the article. |
| `article.content` | String | Body content of the article. |

## Errors

| Case | HTTP Status | Error Code |
| --- | --- | --- |
| No article exists for `{articleId}` | 400 | [80001 ArticleIdNotExist](/doc/backend_error/article/80001.md) |
| The caller (DEVELOPER / CUSTOMER role) has no [privilege](/doc/tech_noun/TN0203_privilege.md) grant for the article's [project](/doc/tech_noun/TN0301_project.md) | 400 | [40002 ProjectNoPrivilege](/doc/backend_error/project/40002.md) |
| The calling user's account row no longer exists (privilege check) | 400 | [20001 UserIdNotExist](/doc/backend_error/user/20001.md) |
| No content row exists for the article (data inconsistency) | 400 | [80002 ArticleContentNotExist](/doc/backend_error/article/80002.md) |

A missing or invalid JWT is answered with `401 Unauthorized` (empty body) before the handler runs.

## Example

Request: N/A (no body)

Success response:

```json
{
  "code": 200,
  "timestamp": 1752700000000,
  "data": {
    "article": {
      "articleId": 11,
      "createAt": 1752600000000,
      "updateAt": 1752650000000,
      "title": "Company news",
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
      },
      "cover": "https://pager-metadata.oss-cn-hangzhou.aliyuncs.com/article/11/cover/uuid?Expires=...",
      "description": "Latest company news.",
      "content": "<p>The news body.</p>"
    }
  }
}
```
