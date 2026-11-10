# GET /v1/project/language_types

> Controller: ProjectController

Get all available language types for a project.

## Permission

[`PROJECT_READ`](/doc/permission/PROJECT_READ.md) — held by every role (OWNER, ADMIN, DEVELOPER,
CUSTOMER), so the permission check cannot fail for a valid login.

## Request

### Path Parameters

N/A

### Query Parameters

N/A

### Body

N/A

## Response

DTO: `ProjectLanguageTypesResponse`, returned inside the `PagerResponse` envelope as `data` —
see the [API index](/doc/api/README.md#response-envelope). All values of the `LanguageType` enum
are returned — the [languages](/doc/tech_noun/TN0302_language.md) a
[project](/doc/tech_noun/TN0301_project.md) can enable: `SIMPLIFIED_CHINESE`,
`TRADITIONAL_CHINESE`, `ENGLISH`, `FRENCH`, `GERMAN`, `ITALIAN`, `JAPANESE`, `KOREAN`. This is
the value set of every `languageType` request field and path parameter in the API. (Note the
enum's display names are partly mislabeled in the backend — e.g. `GERMAN` carries the label
`日语` (Japanese); the labels are returned as stored.)

| Field | Type | Description |
| --- | --- | --- |
| `languageTypes[].value` | String | The `LanguageType` enum constant name (e.g. `ENGLISH`). |
| `languageTypes[].languageName` | String | Display name of the language as stored in the enum (Chinese label, e.g. `英语`). |

## Errors

N/A — no business error is raised by this endpoint.

A missing or invalid JWT is answered with `401 Unauthorized` (empty body) before the handler runs.

## Example

Request: N/A (no body)

Success response (truncated to two of the 8 language types):

```json
{
  "code": 200,
  "timestamp": 1752700000000,
  "data": {
    "languageTypes": [
      { "value": "SIMPLIFIED_CHINESE", "languageName": "简体中文" },
      { "value": "ENGLISH", "languageName": "英语" }
    ]
  }
}
```
