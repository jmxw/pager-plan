# POST /v1/article_list

> Controller: ArticleListController

Add a new article list into a project.

## Permission

[`ARTICLE_LIST_WRITE`](/doc/permission/ARTICLE_LIST_WRITE.md) — held by every role (OWNER, ADMIN,
DEVELOPER, CUSTOMER), so the permission check cannot fail for a valid login.

## Request

### Path Parameters

N/A

### Query Parameters

N/A

### Body

DTO: `ArticleListAddRequest`

| Field | Type | Required | Description |
| --- | --- | --- | --- |
| `projectId` | Long | ✅ | Id of the [project](/doc/tech_noun/TN0301_project.md) the [article list](/doc/tech_noun/TN0502_article_list.md) is created in. |
| `templateId` | Long | ✅ | Id of the [template](/doc/tech_noun/TN0401_template.md) that renders one article of this list. Must be an HTML file of the same project. |
| `identifier` | String | ✅ | The [identifier](/doc/tech_noun/TN0101_identifier.md) of the list, referenced by `{pager:list id="…"}` [tags](/doc/tech_noun/TN0403_pager_tag.md); unique inside the project. |
| `name` | String | ✅ | Display name of the list. |

## Response

DTO: `EmptyResponse`, returned inside the `PagerResponse` envelope as `data` — see the
[API index](/doc/api/README.md#response-envelope).

| Field | Type | Description |
| --- | --- | --- |
| — | — | `data` is an empty object. |

## Errors

| Case | HTTP Status | Error Code |
| --- | --- | --- |
| No project exists for `projectId` | 400 | [40001 ProjectIdNotExist](/doc/backend_error/project/40001.md) |
| The project belongs to another organization | 400 | [40003 ProjectNotBelongToOrganization](/doc/backend_error/project/40003.md) |
| The caller (DEVELOPER / CUSTOMER role) has no [privilege](/doc/tech_noun/TN0203_privilege.md) grant for the project | 400 | [40002 ProjectNoPrivilege](/doc/backend_error/project/40002.md) |
| The calling user's account row no longer exists (privilege check) | 400 | [20001 UserIdNotExist](/doc/backend_error/user/20001.md) |
| `identifier` is already used by another article list of the project | 400 | [70003 ArticleListIdentifierExist](/doc/backend_error/article_list/70003.md) |
| No template exists for `templateId` | 400 | [60001 TemplateIdNotExist](/doc/backend_error/template/60001.md) |
| The template is not an HTML file | 400 | [70005 ArticleListTemplateFormatError](/doc/backend_error/article_list/70005.md) |
| The template belongs to another project | 400 | [60003 TemplateNotBelongToProject](/doc/backend_error/template/60003.md) |

A missing or invalid JWT is answered with `401 Unauthorized` (empty body) before the handler runs.

## Example

Request:

```json
{
  "projectId": 3,
  "templateId": 120,
  "identifier": "news",
  "name": "News list"
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
