# POST /v1/article/{articleId}/cover

> Controller: ArticleController

Upload cover image for an article.

## Permission

[`ARTICLE_WRITE`](/doc/permission/ARTICLE_WRITE.md) — held by every role (OWNER, ADMIN, DEVELOPER,
CUSTOMER), so the permission check cannot fail for a valid login.

## Request

### Path Parameters

| Name | Type | Description |
| --- | --- | --- |
| `articleId` | Long | Id of the [article](/doc/tech_noun/TN0501_article.md) the cover belongs to. |

### Query Parameters

N/A

### Body

`multipart/form-data` upload (no JSON DTO):

| Part | Type | Required | Description |
| --- | --- | --- | --- |
| `cover` | file | ✅ | The cover image file. A previously uploaded cover is deleted from OSS and replaced. |

## Response

DTO: `EmptyResponse`, returned inside the `PagerResponse` envelope as `data` — see the
[API index](/doc/api/README.md#response-envelope). The cover is stored on the metadata OSS
bucket and the article's [revision](/doc/tech_noun/TN0102_revision.md) is incremented; the
presigned cover URL is returned by [get_article.md](get_article.md).

| Field | Type | Description |
| --- | --- | --- |
| — | — | `data` is an empty object. |

## Errors

| Case | HTTP Status | Error Code |
| --- | --- | --- |
| No article exists for `{articleId}` | 400 | [80001 ArticleIdNotExist](/doc/backend_error/article/80001.md) |
| The caller (DEVELOPER / CUSTOMER role) has no [privilege](/doc/tech_noun/TN0203_privilege.md) grant for the article's [project](/doc/tech_noun/TN0301_project.md) | 400 | [40002 ProjectNoPrivilege](/doc/backend_error/project/40002.md) |
| The calling user's account row no longer exists (privilege check) | 400 | [20001 UserIdNotExist](/doc/backend_error/user/20001.md) |
| The uploaded file cannot be written to the local cache | 400 | [80003 ArticleCoverSaveError](/doc/backend_error/article/80003.md) |

A missing or invalid JWT is answered with `401 Unauthorized` (empty body) before the handler runs.

## Example

Request: N/A (`multipart/form-data` with a single `cover` file part)

Success response:

```json
{
  "code": 200,
  "timestamp": 1752700000000,
  "data": {}
}
```
