# POST /v1/template/{templateId}/upload

> Controller: TemplateController

Upload a new template file to a parent template.

The uploaded file is saved as a new child [template](/doc/tech_noun/TN0401_template.md) under the
parent template identified by the path, together with its
[template file](/doc/tech_noun/TN0402_template_file.md) record. The
[template type](/doc/tech_noun/TN0401_template.md) is derived from the uploaded file's extension.
Text files keep their decoded text as content; binary files are stored in the project's OSS
[bucket](/doc/tech_noun/TN0702_bucket.md) and referenced by a generated identifier.

## Permission

[`PROJECT_DEV`](/doc/permission/PROJECT_DEV.md)

## Request

### Path Parameters

| Name | Type | Description |
| --- | --- | --- |
| `templateId` | Long | Physical id of the parent template the uploaded file is added to. |

### Query Parameters

N/A

### Body

The request body is a `multipart/form-data` form (not JSON).

| Field | Type | Required | Description |
| --- | --- | --- | --- |
| `template` | File | Yes | The template file to upload. Its file name and extension determine the child template's name and type. |

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
| The parent template does not exist | 400 Bad Request | [60001 TemplateIdNotExist](/doc/backend_error/template/60001.md) |
| The caller resolved from the JWT does not exist | 400 Bad Request | [20001 UserIdNotExist](/doc/backend_error/user/20001.md) |
| The caller has no privilege on the template's project | 400 Bad Request | [40002 ProjectNoPrivilege](/doc/backend_error/project/40002.md) |
| The uploaded file could not be saved | 400 Bad Request | [60008 TemplateUploadFileSaveError](/doc/backend_error/template/60008.md) |
| A child with the same name already exists under the parent | 400 Bad Request | [60006 TemplateNameDuplicated](/doc/backend_error/template/60006.md) |

## Example

Request: `N/A` (multipart form upload; the file is sent in the `template` form field).

Success response (the `data` payload):

```json
{}
```
