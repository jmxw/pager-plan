# PATCH /v1/project/{projectId}/reset

> Controller: ProjectController

Resets a [project](/doc/tech_noun/TN0301_project.md) back to its `INIT` state: its
[template](/doc/tech_noun/TN0401_template.md) files are removed from OSS, all templates and their
precompiled data are cleared, the staging and production [revisions](/doc/tech_noun/TN0102_revision.md)
are reset to zero, and fresh root and recycle-bin templates are recreated. The reset is rejected
while an artifact is being uploaded or analysed. The project must belong to the caller's
[organization](/doc/tech_noun/TN0201_organization.md) and the caller must hold a
[privilege](/doc/tech_noun/TN0203_privilege.md) on it.

## Permission

[`PROJECT_WRITE`](/doc/permission/PROJECT_WRITE.md)

## Request

### Path Parameters

| Name | Type | Description |
| --- | --- | --- |
| `projectId` | Long | Physical id of the project to reset. |

### Query Parameters

N/A

### Body

N/A

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
| The project is being uploaded or analysed (`ZIP_UPLOADING` / `ZIP_ANALYSING`), so a reset is rejected | 400 Bad Request | [40011 ProjectResetReject](/doc/backend_error/project/40011.md) |

## Example

Request:

N/A

Success response (the `data` payload):

```json
{}
```
