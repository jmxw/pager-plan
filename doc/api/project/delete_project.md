# DELETE /v1/project/{projectId}

> Controller: ProjectController

Delete a project.

## Permission

[`PROJECT_WRITE`](/doc/permission/PROJECT_WRITE.md) — held by OWNER and ADMIN.

## Request

### Path Parameters

| Name | Type | Description |
| --- | --- | --- |
| `projectId` | Long | Id of the [project](/doc/tech_noun/TN0301_project.md) to delete. |

### Query Parameters

N/A

### Body

N/A

## Response

DTO: `EmptyResponse`, returned inside the `PagerResponse` envelope as `data` — see the
[API index](/doc/api/README.md#response-envelope). The whole project is removed:
[templates](/doc/tech_noun/TN0401_template.md) and [template files](/doc/tech_noun/TN0402_template_file.md),
[navigations](/doc/tech_noun/TN0601_navigation.md) and their bindings,
[article lists](/doc/tech_noun/TN0502_article_list.md), [articles](/doc/tech_noun/TN0501_article.md)
with their contents, [labels](/doc/tech_noun/TN0303_label.md),
[languages](/doc/tech_noun/TN0302_language.md), [privileges](/doc/tech_noun/TN0203_privilege.md),
precompiled tag rows — and finally the project's OSS [bucket](/doc/tech_noun/TN0702_bucket.md).

| Field | Type | Description |
| --- | --- | --- |
| — | — | `data` is an empty object. |

## Errors

| Case | HTTP Status | Error Code |
| --- | --- | --- |
| The caller's role does not hold `PROJECT_WRITE` (DEVELOPER, CUSTOMER) | 400 | [20002 UserNotPermitted](/doc/backend_error/user/20002.md) |
| No project exists for `{projectId}` | 400 | [40001 ProjectIdNotExist](/doc/backend_error/project/40001.md) |
| The project belongs to another organization | 400 | [40003 ProjectNotBelongToOrganization](/doc/backend_error/project/40003.md) |
| The caller has no privilege grant for the project | 400 | [40002 ProjectNoPrivilege](/doc/backend_error/project/40002.md) |
| The calling user's account row no longer exists (privilege check) | 400 | [20001 UserIdNotExist](/doc/backend_error/user/20001.md) |

A missing or invalid JWT is answered with `401 Unauthorized` (empty body) before the handler runs.

## Example

Request: N/A (no body)

Success response:

```json
{
  "code": 200,
  "timestamp": 1752700000000,
  "data": {}
}
```
