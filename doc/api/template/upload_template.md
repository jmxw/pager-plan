# POST /v1/template/{templateId}/upload

> Controller: TemplateController

Upload a new template file to a parent template.

## Permission

[`PROJECT_DEV`](/doc/permission/PROJECT_DEV.md) — held by OWNER, ADMIN, and DEVELOPER.

## Request

### Path Parameters

| Name | Type | Description |
| --- | --- | --- |
| `templateId` | Long | Id of the parent [template](/doc/tech_noun/TN0401_template.md) node the uploaded file is created under. The parent is **not** validated to be a directory. |

### Query Parameters

N/A

### Body

`multipart/form-data` upload (no JSON DTO):

| Part | Type | Required | Description |
| --- | --- | --- | --- |
| `template` | file | ✅ | The file to upload. Its original filename becomes the node name (unique among the parent's children) and its extension determines the `TemplateType`. |

## Response

DTO: `EmptyResponse`, returned inside the `PagerResponse` envelope as `data` — see the
[API index](/doc/api/README.md#response-envelope). A text file (`html`, `htm`, `css`, `js`,
`xml`) is stored as a [template file](/doc/tech_noun/TN0402_template_file.md) row in the
database; a binary file is uploaded to the metadata OSS bucket and only its UUID is stored.

| Field | Type | Description |
| --- | --- | --- |
| — | — | `data` is an empty object. |

## Errors

| Case | HTTP Status | Error Code |
| --- | --- | --- |
| The caller's role does not hold `PROJECT_DEV` (CUSTOMER) | 400 | [20002 UserNotPermitted](/doc/backend_error/user/20002.md) |
| No template exists for `{templateId}` | 400 | [60001 TemplateIdNotExist](/doc/backend_error/template/60001.md) |
| The caller (DEVELOPER role) has no [privilege](/doc/tech_noun/TN0203_privilege.md) grant for the [project](/doc/tech_noun/TN0301_project.md) | 400 | [40002 ProjectNoPrivilege](/doc/backend_error/project/40002.md) |
| The calling user's account row no longer exists (privilege check) | 400 | [20001 UserIdNotExist](/doc/backend_error/user/20001.md) |
| The uploaded file cannot be written to the local cache | 400 | [60008 TemplateUploadFileSaveError](/doc/backend_error/template/60008.md) |
| The filename is already used by another child of the parent node | 400 | [60006 TemplateNameDuplicated](/doc/backend_error/template/60006.md) |

A missing or invalid JWT is answered with `401 Unauthorized` (empty body) before the handler runs.

## Example

Request: N/A (`multipart/form-data` with a single `template` file part)

Success response:

```json
{
  "code": 200,
  "timestamp": 1752700000000,
  "data": {}
}
```
