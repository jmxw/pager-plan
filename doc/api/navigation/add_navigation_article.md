# POST /v1/navigation/{navigationId}/article

> Controller: NavigationController

Add a new article type navigation menu to a parent navigation menu.

## Permission

[`PROJECT_DEV`](/doc/permission/PROJECT_DEV.md) — held by OWNER, ADMIN, and DEVELOPER.

## Request

### Path Parameters

| Name | Type | Description |
| --- | --- | --- |
| `navigationId` | Long | Id of the parent [navigation](/doc/tech_noun/TN0601_navigation.md) node the new `ARTICLE` node is appended to (as the last child). |

### Query Parameters

N/A

### Body

DTO: `NavigationAddArticleRequest`

| Field | Type | Required | Description |
| --- | --- | --- | --- |
| `identifier` | String | ✅ | The [identifier](/doc/tech_noun/TN0101_identifier.md) of the new node — the URL path segment of its page; unique among the parent's children. |
| `name` | String | ✅ | Display name of the node; also stored as its [navigation title](/doc/tech_noun/TN0602_navigation_title.md) for the project's default [language](/doc/tech_noun/TN0302_language.md). |
| `articleId` | Long | ✅ | Id of the [article](/doc/tech_noun/TN0501_article.md) rendered on this node — stored as the node's [navigation article](/doc/tech_noun/TN0603_navigation_article.md) binding. Must belong to the same [project](/doc/tech_noun/TN0301_project.md). |
| `templateId` | Long | ✅ | Id of the [template](/doc/tech_noun/TN0401_template.md) that renders the article page. Must belong to the same project. |

## Response

DTO: `EmptyResponse`, returned inside the `PagerResponse` envelope as `data` — see the
[API index](/doc/api/README.md#response-envelope).

| Field | Type | Description |
| --- | --- | --- |
| — | — | `data` is an empty object. |

## Errors

| Case | HTTP Status | Error Code |
| --- | --- | --- |
| The caller's role does not hold `PROJECT_DEV` (CUSTOMER) | 400 | [20002 UserNotPermitted](/doc/backend_error/user/20002.md) |
| No navigation exists for `{navigationId}` | 400 | [90001 NavigationIdNotExist](/doc/backend_error/navigation/90001.md) |
| The caller (DEVELOPER role) has no [privilege](/doc/tech_noun/TN0203_privilege.md) grant for the project | 400 | [40002 ProjectNoPrivilege](/doc/backend_error/project/40002.md) |
| The calling user's account row no longer exists (privilege check) | 400 | [20001 UserIdNotExist](/doc/backend_error/user/20001.md) |
| `identifier` is already used by another child of the parent node | 400 | [90002 NavigationIdentifierDuplicated](/doc/backend_error/navigation/90002.md) |
| No article exists for `articleId` | 400 | [80001 ArticleIdNotExist](/doc/backend_error/article/80001.md) |
| The article belongs to another project | 400 | [90003 NavigationArticleMismatching](/doc/backend_error/navigation/90003.md) |
| No template exists for `templateId` | 400 | [60001 TemplateIdNotExist](/doc/backend_error/template/60001.md) |
| The template belongs to another project | 400 | [90005 NavigationTemplateMismatching](/doc/backend_error/navigation/90005.md) |
| The project has no default language (data inconsistency) — answered as `-9999 UnknownError` | 500 | [50003 LanguageDefaultNotFound](/doc/backend_error/language/50003.md) |

A missing or invalid JWT is answered with `401 Unauthorized` (empty body) before the handler runs.

## Example

Request:

```json
{
  "identifier": "about",
  "name": "About us",
  "articleId": 11,
  "templateId": 122
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
