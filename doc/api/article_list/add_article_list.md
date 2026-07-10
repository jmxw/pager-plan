# POST /v1/article_list

> Controller: ArticleListController

Add a new article list into a project.

Creates an [article list](/doc/tech_noun/TN0502_article_list.md) under a
[project](/doc/tech_noun/TN0301_project.md), bound to an existing
[template](/doc/tech_noun/TN0401_template.md). The template must be an HTML template that belongs to
the same project, and the identifier must be unique within the project. The project revision is
bumped so the change is picked up on the next deployment.

## Permission

[`ARTICLE_LIST_WRITE`](/doc/permission/ARTICLE_LIST_WRITE.md)

## Request

### Path Parameters

N/A

### Query Parameters

N/A

### Body

DTO: `ArticleListAddRequest`

| Field | Type | Required | Description |
| --- | --- | --- | --- |
| `projectId` | Long | Yes | Physical id of the project the article list is added to. |
| `templateId` | Long | Yes | Physical id of the template bound to render the article list. Must be an HTML template of the same project. |
| `identifier` | String | Yes | Identifier of the new article list, must be unique within the project. |
| `name` | String | Yes | Display name of the new article list. |

## Response

DTO: `EmptyResponse` — wrapped as `data` inside the standard `PagerResponse` envelope
(`{ code, timestamp, data }`). The `data` body is empty (`{}`).

| Field | Type | Description |
| --- | --- | --- |
| — | — | The response body is empty (`{}`). |

## Errors

| Case | HTTP Status | Error Code |
| --- | --- | --- |
| Caller's role lacks the required permission | 400 Bad Request | [20002 UserNotPermitted](/doc/backend_error/user/20002.md) |
| No project exists with the given id | 400 Bad Request | [40001 ProjectIdNotExist](/doc/backend_error/project/40001.md) |
| The project belongs to a different organization than the caller's | 400 Bad Request | [40003 ProjectNotBelongToOrganization](/doc/backend_error/project/40003.md) |
| Caller has no privilege on the project | 400 Bad Request | [40002 ProjectNoPrivilege](/doc/backend_error/project/40002.md) |
| Caller's user record does not exist | 400 Bad Request | [20001 UserIdNotExist](/doc/backend_error/user/20001.md) |
| An article list with the same identifier already exists in the project | 400 Bad Request | [70003 ArticleListIdentifierExist](/doc/backend_error/article_list/70003.md) |
| No template exists with the given id | 400 Bad Request | [60001 TemplateIdNotExist](/doc/backend_error/template/60001.md) |
| The template is not an HTML template | 400 Bad Request | [70005 ArticleListTemplateFormatError](/doc/backend_error/article_list/70005.md) |
| The template belongs to a different project | 400 Bad Request | [60003 TemplateNotBelongToProject](/doc/backend_error/template/60003.md) |

## Example

Request:

```json
{
  "projectId": 1,
  "templateId": 100,
  "identifier": "news",
  "name": "新闻列表"
}
```

Success response (the `data` payload):

```json
{}
```
