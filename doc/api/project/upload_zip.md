# POST /v1/project/{projectId}/upload_zip

> Controller: ProjectController

Upload a zip file of template files for a project.

## Permission

[`PROJECT_WRITE`](/doc/permission/PROJECT_WRITE.md) — held by OWNER and ADMIN.

## Request

### Path Parameters

| Name | Type | Description |
| --- | --- | --- |
| `projectId` | Long | Id of the [project](/doc/tech_noun/TN0301_project.md) the zip is uploaded for. The project must still be in the `INIT` state. |

### Query Parameters

N/A

### Body

`multipart/form-data` upload (no JSON DTO):

| Part | Type | Required | Description |
| --- | --- | --- | --- |
| `zip` | file | ✅ | A zip archive of the site's initial [template](/doc/tech_noun/TN0401_template.md) tree. |

## Response

DTO: `EmptyResponse`, returned inside the `PagerResponse` envelope as `data` — see the
[API index](/doc/api/README.md#response-envelope). The upload is processed **asynchronously**:
the zip is stored on the metadata OSS bucket, the project state moves to `ZIP_UPLOADING`, and a
RabbitMQ message triggers the consumer, which unpacks the archive into the template tree
(`ZIP_ANALYSING` → `TEMPLATE_READY`). Progress is observed by polling
[get_project.md](get_project.md) for the `state` field.

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
| The project is not in the `INIT` state (a template tree already exists — reset first) | 400 | [40010 ProjectZipUploadReject](/doc/backend_error/project/40010.md) |
| The uploaded file cannot be written to the local cache | 400 | [40007 ProjectZipSavedError](/doc/backend_error/project/40007.md) |

A missing or invalid JWT is answered with `401 Unauthorized` (empty body) before the handler runs.

## Example

Request: N/A (`multipart/form-data` with a single `zip` file part)

Success response:

```json
{
  "code": 200,
  "timestamp": 1752700000000,
  "data": {}
}
```
