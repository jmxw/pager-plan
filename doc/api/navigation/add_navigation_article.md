# POST /v1/navigation/{navigationId}/article

> Controller: NavigationController

Add a new article type [navigation](/doc/tech_noun/TN0601_navigation.md) menu to a parent navigation menu.

A new [navigation article](/doc/tech_noun/TN0603_navigation_article.md) child is created under the parent
navigation identified by the path. The child binds an [article](/doc/tech_noun/TN0501_article.md) to a
[template](/doc/tech_noun/TN0401_template.md); the article's language and the template must both belong to
the same [project](/doc/tech_noun/TN0301_project.md) as the parent navigation. The child sequence is
appended after the parent's existing children, and a navigation title is created for the project's default
language.

## Permission

[`PROJECT_DEV`](/doc/permission/PROJECT_DEV.md)

## Request

### Path Parameters

| Name | Type | Description |
| --- | --- | --- |
| `navigationId` | Long | Physical id of the parent navigation menu the new child is added to. |

### Query Parameters

N/A

### Body

DTO: `NavigationAddArticleRequest`

| Field | Type | Required | Description |
| --- | --- | --- | --- |
| `identifier` | String | Yes | Identifier of the new navigation menu; must be unique among the parent's children. |
| `name` | String | Yes | Display name of the new navigation menu; also used as the default-language navigation title. |
| `articleId` | Long | Yes | Physical id of the article bound to the new navigation menu. |
| `templateId` | Long | Yes | Physical id of the template used to render the new navigation menu. |

## Response

DTO: `EmptyResponse` — wrapped as `data` inside the standard `PagerResponse` envelope
(`{ code, timestamp, data }`). The `data` payload is an empty object `{}`.

| Field | Type | Description |
| --- | --- | --- |
| — | — | The response body is empty (`{}`). |

## Errors

| Case | HTTP Status | Error Code |
| --- | --- | --- |
| Caller's role lacks the required permission | 400 Bad Request | [20002 UserNotPermitted](/doc/backend_error/user/20002.md) |
| Parent navigation menu does not exist | 400 Bad Request | [90001 NavigationIdNotExist](/doc/backend_error/navigation/90001.md) |
| Caller does not exist | 400 Bad Request | [20001 UserIdNotExist](/doc/backend_error/user/20001.md) |
| Caller has no privilege on the parent navigation's project | 400 Bad Request | [40002 ProjectNoPrivilege](/doc/backend_error/project/40002.md) |
| Identifier already used by another child of the parent navigation | 400 Bad Request | [90002 NavigationIdentifierDuplicated](/doc/backend_error/navigation/90002.md) |
| Article does not exist | 400 Bad Request | [80001 ArticleIdNotExist](/doc/backend_error/article/80001.md) |
| Article belongs to a different project than the parent navigation | 400 Bad Request | [90003 NavigationArticleMismatching](/doc/backend_error/navigation/90003.md) |
| Template does not exist | 400 Bad Request | [60001 TemplateIdNotExist](/doc/backend_error/template/60001.md) |
| Template belongs to a different project than the parent navigation | 400 Bad Request | [90005 NavigationTemplateMismatching](/doc/backend_error/navigation/90005.md) |
| Project has no default language configured | 500 (as -9999) | [50003 LanguageDefaultNotFound](/doc/backend_error/language/50003.md) |

## Example

Request:

```json
{
  "identifier": "about",
  "name": "关于我们",
  "articleId": 108,
  "templateId": 7
}
```

Success response (the `data` payload):

```json
{}
```
