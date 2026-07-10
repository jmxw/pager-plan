# POST /v1/project/{projectId}/upload_zip

> Controller: ProjectController

Uploads a template ZIP artifact for a [project](/doc/tech_noun/TN0301_project.md). The upload is
accepted only while the project is in its `INIT` state; the project is moved to `ZIP_UPLOADING`, the
file is stored to OSS, and a message is published so the artifact is analysed asynchronously. The
project must belong to the caller's [organization](/doc/tech_noun/TN0201_organization.md) and the
caller must hold a [privilege](/doc/tech_noun/TN0203_privilege.md) on it.

## Permission

[`PROJECT_WRITE`](/doc/permission/PROJECT_WRITE.md)

## Request

### Path Parameters

| Name | Type | Description |
| --- | --- | --- |
| `projectId` | Long | Physical id of the project the artifact is uploaded for. |

### Query Parameters

N/A

### Body

This endpoint consumes a `multipart/form-data` request (not JSON).

| Part | Type | Required | Description |
| --- | --- | --- | --- |
| `zip` | File | Yes | The template ZIP artifact to upload. |

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
| The project is not in its `INIT` state, so a ZIP upload is rejected | 400 Bad Request | [40010 ProjectZipUploadReject](/doc/backend_error/project/40010.md) |
| The uploaded ZIP could not be saved to the local cache | 400 Bad Request | [40007 ProjectZipSavedError](/doc/backend_error/project/40007.md) |

## Example

Request:

`multipart/form-data` with a single `zip` file part:

```
POST /v1/project/12/upload_zip
Content-Type: multipart/form-data; boundary=----boundary

------boundary
Content-Disposition: form-data; name="zip"; filename="template.zip"
Content-Type: application/zip

<binary ZIP bytes>
------boundary--
```

Success response (the `data` payload):

```json
{}
```
