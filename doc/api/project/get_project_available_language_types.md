# GET /v1/project/language_types

> Controller: ProjectController

Lists the [language types](/doc/tech_noun/TN0302_language.md) that a project
[language](/doc/tech_noun/TN0302_language.md) may use. The list is derived from the fixed set of
supported language types and is not organization-specific.

## Permission

[`PROJECT_READ`](/doc/permission/PROJECT_READ.md)

## Request

### Path Parameters

N/A

### Query Parameters

N/A

### Body

N/A

## Response

DTO: `ProjectLanguageTypesResponse` — wrapped as `data` inside the standard `PagerResponse` envelope
(`{ code, timestamp, data }`).

| Field | Type | Description |
| --- | --- | --- |
| `languageTypes` | List\<LanguageTypeEntity\> | All language types available for a project language. |

Each `LanguageTypeEntity` has the following fields:

| Field | Type | Description |
| --- | --- | --- |
| `value` | String | The language type enum value (e.g. `SIMPLIFIED_CHINESE`); the value passed as `languageType` when adding a language. |
| `languageName` | String | Human-readable language name. |

## Errors

| Case | HTTP Status | Error Code |
| --- | --- | --- |
| Caller's role lacks the required permission | 400 Bad Request | [20002 UserNotPermitted](/doc/backend_error/user/20002.md) |

## Example

Request:

N/A

Success response (the `data` payload):

```json
{
  "languageTypes": [
    {
      "value": "SIMPLIFIED_CHINESE",
      "languageName": "简体中文"
    },
    {
      "value": "ENGLISH",
      "languageName": "英语"
    }
  ]
}
```
