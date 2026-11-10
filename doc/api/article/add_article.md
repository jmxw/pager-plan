# POST /v1/article

> Controller: ArticleController

Add a new article into a project.

## Permission

[`ARTICLE_WRITE`](/doc/permission/ARTICLE_WRITE.md) — held by every role (OWNER, ADMIN, DEVELOPER,
CUSTOMER), so the permission check cannot fail for a valid login.

## Request

### Path Parameters

N/A

### Query Parameters

N/A

### Body

DTO: `ArticleAddRequest`

| Field | Type | Required | Description |
| --- | --- | --- | --- |
| `projectId` | Long | ✅ | Id of the [project](/doc/tech_noun/TN0301_project.md) the [article](/doc/tech_noun/TN0501_article.md) is created in. |
| `title` | String | ✅ | Title of the article. |
| `description` | String | ✅ | Short description of the article. |
| `articleListIds` | Set&lt;Long&gt; | ✅ | Ids of the [article lists](/doc/tech_noun/TN0502_article_list.md) the new article is published into. May be empty. Ids that do not exist are silently ignored; ids of lists in another project are rejected (see Errors). |
| `languageType` | LanguageType | ❌ | The [language](/doc/tech_noun/TN0302_language.md) the article is written in (enum values: see [get_project_available_language_types.md](/doc/api/project/get_project_available_language_types.md)). Defaults to the project's default language when omitted. |

## Response

DTO: `EmptyResponse`, returned inside the `PagerResponse` envelope as `data` — see the
[API index](/doc/api/README.md#response-envelope). The article is created with an empty body
content; the body is filled by a later [edit_article.md](edit_article.md) call.

| Field | Type | Description |
| --- | --- | --- |
| — | — | `data` is an empty object. |

## Errors

| Case | HTTP Status | Error Code |
| --- | --- | --- |
| The calling user's account row no longer exists | 400 | [20001 UserIdNotExist](/doc/backend_error/user/20001.md) |
| No project exists for `projectId` | 400 | [40001 ProjectIdNotExist](/doc/backend_error/project/40001.md) |
| The project belongs to another organization | 400 | [40003 ProjectNotBelongToOrganization](/doc/backend_error/project/40003.md) |
| The caller (DEVELOPER / CUSTOMER role) has no [privilege](/doc/tech_noun/TN0203_privilege.md) grant for the project | 400 | [40002 ProjectNoPrivilege](/doc/backend_error/project/40002.md) |
| `languageType` is given but not enabled for the project | 400 | [50002 LanguageNotExist](/doc/backend_error/language/50002.md) |
| `languageType` is omitted and the project has no default language (data inconsistency) — answered as `-9999 UnknownError` | 500 | [50003 LanguageDefaultNotFound](/doc/backend_error/language/50003.md) |
| An id in `articleListIds` names an article list of another project | 400 | [70006 ArticleListProjectMismatching](/doc/backend_error/article_list/70006.md) |

A missing or invalid JWT is answered with `401 Unauthorized` (empty body) before the handler runs.

## Example

Request:

```json
{
  "projectId": 3,
  "title": "Company news",
  "description": "Latest company news.",
  "articleListIds": [5],
  "languageType": "ENGLISH"
}
```

Success response:

```json
{
  "code": 200,
  "timestamp": 1752700000000,
  "data": {}
}
```
