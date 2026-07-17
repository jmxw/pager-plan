# POST /v1/project/{projectId}/language

> Controller: ProjectController

Add a new language into a project.

## Permission

[`PROJECT_WRITE`](/doc/permission/PROJECT_WRITE.md) — held by OWNER and ADMIN.

## Request

### Path Parameters

| Name | Type | Description |
| --- | --- | --- |
| `projectId` | Long | Id of the [project](/doc/tech_noun/TN0301_project.md) the [language](/doc/tech_noun/TN0302_language.md) is enabled for. |

### Query Parameters

N/A

### Body

DTO: `ProjectAddLanguageRequest`

| Field | Type | Required | Description |
| --- | --- | --- | --- |
| `languageType` | LanguageType | ✅ | The language to enable (enum values: see [get_project_available_language_types.md](get_project_available_language_types.md), field `value`). |

## Response

DTO: `EmptyResponse`, returned inside the `PagerResponse` envelope as `data` — see the
[API index](/doc/api/README.md#response-envelope). The language becomes the default only when
the project had no language at all (normally the default is created with the project).

| Field | Type | Description |
| --- | --- | --- |
| — | — | `data` is an empty object. |

## Errors

| Case | HTTP Status | Error Code |
| --- | --- | --- |
| The caller's role does not hold `PROJECT_WRITE` (DEVELOPER, CUSTOMER) | 400 | [20002 UserNotPermitted](/doc/backend_error/user/20002.md) |
| No project exists for `{projectId}` | 400 | [40001 ProjectIdNotExist](/doc/backend_error/project/40001.md) |
| The project belongs to another organization | 400 | [40003 ProjectNotBelongToOrganization](/doc/backend_error/project/40003.md) |
| The caller has no [privilege](/doc/tech_noun/TN0203_privilege.md) grant for the project | 400 | [40002 ProjectNoPrivilege](/doc/backend_error/project/40002.md) |
| The calling user's account row no longer exists (privilege check) | 400 | [20001 UserIdNotExist](/doc/backend_error/user/20001.md) |
| The language is already enabled for the project | 400 | [50001 LanguageExist](/doc/backend_error/language/50001.md) |

A missing or invalid JWT is answered with `401 Unauthorized` (empty body) before the handler runs.

## Example

Request:

```json
{
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
