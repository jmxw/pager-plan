# PUT /v1/article_list/{articleListId}

> Controller: ArticleListController

Edit an article list.

## Permission

[`ARTICLE_LIST_WRITE`](/doc/permission/ARTICLE_LIST_WRITE.md) — held by every role (OWNER, ADMIN,
DEVELOPER, CUSTOMER), so the permission check cannot fail for a valid login.

## Request

### Path Parameters

| Name | Type | Description |
| --- | --- | --- |
| `articleListId` | Long | Id of the [article list](/doc/tech_noun/TN0502_article_list.md) to edit. |

### Query Parameters

N/A

### Body

DTO: `ArticleListEditRequest`

| Field | Type | Required | Description |
| --- | --- | --- | --- |
| `templateId` | Long | ❌ | Id of the new rendering [template](/doc/tech_noun/TN0401_template.md); `null` keeps the current template. Unlike [add_article_list.md](add_article_list.md), the edit path does **not** re-validate the template's type or project. |
| `name` | String | ❌ | New display name; `null` keeps the current name. |

## Response

DTO: `EmptyResponse`, returned inside the `PagerResponse` envelope as `data` — see the
[API index](/doc/api/README.md#response-envelope). The list's
[revision](/doc/tech_noun/TN0102_revision.md) is incremented.

| Field | Type | Description |
| --- | --- | --- |
| — | — | `data` is an empty object. |

## Errors

| Case | HTTP Status | Error Code |
| --- | --- | --- |
| No article list exists for `{articleListId}` | 400 | [70001 ArticleListIdNotExist](/doc/backend_error/article_list/70001.md) |
| The caller (DEVELOPER / CUSTOMER role) has no [privilege](/doc/tech_noun/TN0203_privilege.md) grant for the list's [project](/doc/tech_noun/TN0301_project.md) | 400 | [40002 ProjectNoPrivilege](/doc/backend_error/project/40002.md) |
| The calling user's account row no longer exists (privilege check) | 400 | [20001 UserIdNotExist](/doc/backend_error/user/20001.md) |
| `templateId` is given but no template exists for it | 400 | [60001 TemplateIdNotExist](/doc/backend_error/template/60001.md) |

A missing or invalid JWT is answered with `401 Unauthorized` (empty body) before the handler runs.

## Example

Request:

```json
{
  "templateId": 121,
  "name": "News list v2"
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
