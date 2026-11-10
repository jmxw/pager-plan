# PATCH /v1/project/{projectId}/reset

> Controller: ProjectController

Reset all templates of a project.

## Permission

[`PROJECT_WRITE`](/doc/permission/PROJECT_WRITE.md) — held by OWNER and ADMIN.

## Request

### Path Parameters

| Name | Type | Description |
| --- | --- | --- |
| `projectId` | Long | Id of the [project](/doc/tech_noun/TN0301_project.md) to reset. |

### Query Parameters

N/A

### Body

N/A

## Response

DTO: `EmptyResponse`, returned inside the `PagerResponse` envelope as `data` — see the
[API index](/doc/api/README.md#response-envelope). The whole
[template](/doc/tech_noun/TN0401_template.md) tree (including
[template files](/doc/tech_noun/TN0402_template_file.md), binary files on the metadata OSS
bucket, and precompiled tag rows) is deleted; fresh `ROOT` and `RECYCLE_BIN` nodes are created,
the state returns to `INIT`, and both
[revisions](/doc/tech_noun/TN0102_revision.md) return to `0`. Articles, article lists,
navigations, labels, and languages are kept.

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
| The project is in the `ZIP_UPLOADING` or `ZIP_ANALYSING` state (zip processing in progress) | 400 | [40011 ProjectResetReject](/doc/backend_error/project/40011.md) |

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
