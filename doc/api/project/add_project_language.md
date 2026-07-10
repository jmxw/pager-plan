# POST /v1/project/{projectId}/language

> Controller: ProjectController

Adds a [language](/doc/tech_noun/TN0302_language.md) to a [project](/doc/tech_noun/TN0301_project.md).
The first language added to a project becomes its default language. The project must belong to the
caller's [organization](/doc/tech_noun/TN0201_organization.md) and the caller must hold a
[privilege](/doc/tech_noun/TN0203_privilege.md) on it.

## Permission

[`PROJECT_WRITE`](/doc/permission/PROJECT_WRITE.md)

## Request

### Path Parameters

| Name | Type | Description |
| --- | --- | --- |
| `projectId` | Long | Physical id of the project to add the language to. |

### Query Parameters

N/A

### Body

DTO: `ProjectAddLanguageRequest`

| Field | Type | Required | Description |
| --- | --- | --- | --- |
| `languageType` | LanguageType | Yes | The language type to add (enum value, e.g. `ENGLISH`). See the available language types endpoint. |

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
| The caller holds no privilege on the project | 400 Bad Request | [40002 ProjectNoPrivilege](/doc/backend_error/project/40002.md) |
| Caller's user does not exist (privilege lookup) | 400 Bad Request | [20001 UserIdNotExist](/doc/backend_error/user/20001.md) |
| The language type already exists on the project | 400 Bad Request | [50001 LanguageExist](/doc/backend_error/language/50001.md) |

## Example

Request:

```json
{
  "languageType": "ENGLISH"
}
```

Success response (the `data` payload):

```json
{}
```
