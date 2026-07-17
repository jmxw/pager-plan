# GET /v1/project/{projectId}/language

> Controller: ProjectController

Get all languages of a project.

## Permission

[`PROJECT_READ`](/doc/permission/PROJECT_READ.md) — held by every role (OWNER, ADMIN, DEVELOPER,
CUSTOMER), so the permission check cannot fail for a valid login.

## Request

### Path Parameters

| Name | Type | Description |
| --- | --- | --- |
| `projectId` | Long | Id of the [project](/doc/tech_noun/TN0301_project.md) whose [languages](/doc/tech_noun/TN0302_language.md) are read. |

### Query Parameters

N/A

### Body

N/A

## Response

DTO: `ProjectLanguagesResponse`, returned inside the `PagerResponse` envelope as `data` — see
the [API index](/doc/api/README.md#response-envelope). The enabled languages are returned,
ordered by creation time; exactly one is the default.

| Field | Type | Description |
| --- | --- | --- |
| `languages[].createAt` | Long | Timestamp the language was enabled (epoch milliseconds). |
| `languages[].languageType` | LanguageTypeEntity | The language — fields as in [get_project_available_language_types.md](get_project_available_language_types.md). |
| `languages[].defaulted` | Boolean | `true` for the project's default language. |

## Errors

| Case | HTTP Status | Error Code |
| --- | --- | --- |
| No project exists for `{projectId}` | 400 | [40001 ProjectIdNotExist](/doc/backend_error/project/40001.md) |
| The project belongs to another organization | 400 | [40003 ProjectNotBelongToOrganization](/doc/backend_error/project/40003.md) |
| The caller (DEVELOPER / CUSTOMER role) has no [privilege](/doc/tech_noun/TN0203_privilege.md) grant for the project | 400 | [40002 ProjectNoPrivilege](/doc/backend_error/project/40002.md) |
| The calling user's account row no longer exists (privilege check) | 400 | [20001 UserIdNotExist](/doc/backend_error/user/20001.md) |

A missing or invalid JWT is answered with `401 Unauthorized` (empty body) before the handler runs.

## Example

Request: N/A (no body)

Success response:

```json
{
  "code": 200,
  "timestamp": 1752700000000,
  "data": {
    "languages": [
      {
        "createAt": 1752500000000,
        "languageType": { "value": "SIMPLIFIED_CHINESE", "languageName": "简体中文" },
        "defaulted": true
      },
      {
        "createAt": 1752600000000,
        "languageType": { "value": "ENGLISH", "languageName": "英语" },
        "defaulted": false
      }
    ]
  }
}
```
