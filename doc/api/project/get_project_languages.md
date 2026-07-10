# GET /v1/project/{projectId}/language

> Controller: ProjectController

Lists the [languages](/doc/tech_noun/TN0302_language.md) configured on a
[project](/doc/tech_noun/TN0301_project.md), ordered by creation time. The project must belong to the
caller's [organization](/doc/tech_noun/TN0201_organization.md) and the caller must hold a
[privilege](/doc/tech_noun/TN0203_privilege.md) on it.

## Permission

[`PROJECT_READ`](/doc/permission/PROJECT_READ.md)

## Request

### Path Parameters

| Name | Type | Description |
| --- | --- | --- |
| `projectId` | Long | Physical id of the project whose languages are listed. |

### Query Parameters

N/A

### Body

N/A

## Response

DTO: `ProjectLanguagesResponse` — wrapped as `data` inside the standard `PagerResponse` envelope
(`{ code, timestamp, data }`).

| Field | Type | Description |
| --- | --- | --- |
| `languages` | List\<LanguageEntity\> | The project's languages, ordered by creation time. |

Each `LanguageEntity` has the following fields:

| Field | Type | Description |
| --- | --- | --- |
| `createAt` | Long | Creation timestamp (epoch milliseconds). |
| `languageType` | LanguageTypeEntity | The language type (`value`, `languageName`). |
| `defaulted` | Boolean | Whether this is the project's default language. |

## Errors

| Case | HTTP Status | Error Code |
| --- | --- | --- |
| Caller's role lacks the required permission | 400 Bad Request | [20002 UserNotPermitted](/doc/backend_error/user/20002.md) |
| No project exists with the given id | 400 Bad Request | [40001 ProjectIdNotExist](/doc/backend_error/project/40001.md) |
| The project belongs to a different organization than the caller's | 400 Bad Request | [40003 ProjectNotBelongToOrganization](/doc/backend_error/project/40003.md) |
| The caller holds no privilege on the project | 400 Bad Request | [40002 ProjectNoPrivilege](/doc/backend_error/project/40002.md) |
| Caller's user does not exist (privilege lookup) | 400 Bad Request | [20001 UserIdNotExist](/doc/backend_error/user/20001.md) |

## Example

Request:

N/A

Success response (the `data` payload):

```json
{
  "languages": [
    {
      "createAt": 1704067200000,
      "languageType": {
        "value": "SIMPLIFIED_CHINESE",
        "languageName": "简体中文"
      },
      "defaulted": true
    },
    {
      "createAt": 1704153600000,
      "languageType": {
        "value": "ENGLISH",
        "languageName": "英语"
      },
      "defaulted": false
    }
  ]
}
```
