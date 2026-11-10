# GET /v1/template/{templateId}/binary

> Controller: TemplateController

Get url of a binary template file.

## Permission

[`PROJECT_DEV`](/doc/permission/PROJECT_DEV.md) — held by OWNER, ADMIN, and DEVELOPER.

## Request

### Path Parameters

| Name | Type | Description |
| --- | --- | --- |
| `templateId` | Long | Id of the binary-file [template](/doc/tech_noun/TN0401_template.md) node (any file type that is not `HTML` / `HTM` / `CSS` / `JS` / `XML`, e.g. images, audio, video, `OTHER`). |

### Query Parameters

N/A

### Body

N/A

## Response

DTO: `TemplateUrlResponse`, returned inside the `PagerResponse` envelope as `data` — see the
[API index](/doc/api/README.md#response-envelope). The URL is a time-limited presigned link to
the file stored on the metadata OSS bucket.

| Field | Type | Description |
| --- | --- | --- |
| `name` | String | Display name, formatted `<project identifier>: <template name>`. |
| `url` | String | Presigned OSS URL of the binary file. |

## Errors

| Case | HTTP Status | Error Code |
| --- | --- | --- |
| The caller's role does not hold `PROJECT_DEV` (CUSTOMER) | 400 | [20002 UserNotPermitted](/doc/backend_error/user/20002.md) |
| No template exists for `{templateId}` | 400 | [60001 TemplateIdNotExist](/doc/backend_error/template/60001.md) |
| The caller (DEVELOPER role) has no [privilege](/doc/tech_noun/TN0203_privilege.md) grant for the [project](/doc/tech_noun/TN0301_project.md) | 400 | [40002 ProjectNoPrivilege](/doc/backend_error/project/40002.md) |
| The calling user's account row no longer exists (privilege check) | 400 | [20001 UserIdNotExist](/doc/backend_error/user/20001.md) |
| The node is not a binary file | 400 | [60005 TemplateNotBinaryFile](/doc/backend_error/template/60005.md) |
| The node has no [template file](/doc/tech_noun/TN0402_template_file.md) row (data inconsistency) | 400 | [60009 TemplateFileNotExist](/doc/backend_error/template/60009.md) |
| The stored object is missing on the metadata OSS bucket, so no URL can be generated | 400 | [60010 TemplateBinaryMetadataMissed](/doc/backend_error/template/60010.md) |

A missing or invalid JWT is answered with `401 Unauthorized` (empty body) before the handler runs.

## Example

Request: N/A (no body)

Success response:

```json
{
  "code": 200,
  "timestamp": 1752700000000,
  "data": {
    "name": "corp-site: logo.png",
    "url": "https://pager-metadata.oss-cn-hangzhou.aliyuncs.com/project/3/binary/uuid?Expires=..."
  }
}
```
