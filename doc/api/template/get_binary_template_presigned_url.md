# GET /v1/template/{templateId}/binary

> Controller: TemplateController

Get url of a binary template file.

Generates a presigned download URL for the binary [template file](/doc/tech_noun/TN0402_template_file.md)
of the [template](/doc/tech_noun/TN0401_template.md) identified by the path. The file is fetched from
the project's OSS [bucket](/doc/tech_noun/TN0702_bucket.md) and the returned `name` combines the
project identifier and the template name. The endpoint requires the target template to be a binary
file (any file that is not a text file, e.g. `PNG`, `JPG`, `MP3`, `MP4`).

## Permission

[`PROJECT_DEV`](/doc/permission/PROJECT_DEV.md)

## Request

### Path Parameters

| Name | Type | Description |
| --- | --- | --- |
| `templateId` | Long | Physical id of the binary template file. |

### Query Parameters

N/A

### Body

N/A

## Response

DTO: `TemplateUrlResponse` — wrapped as `data` inside the standard `PagerResponse` envelope
(`{ code, timestamp, data }`).

| Field | Type | Description |
| --- | --- | --- |
| `name` | String | Human-readable name, formed as `<project identifier>: <template name>`. |
| `url` | String | Presigned URL from which the binary file can be downloaded. |

## Errors

| Case | HTTP Status | Error Code |
| --- | --- | --- |
| Caller's role lacks the required permission | 400 Bad Request | [20002 UserNotPermitted](/doc/backend_error/user/20002.md) |
| The template does not exist | 400 Bad Request | [60001 TemplateIdNotExist](/doc/backend_error/template/60001.md) |
| The caller resolved from the JWT does not exist | 400 Bad Request | [20001 UserIdNotExist](/doc/backend_error/user/20001.md) |
| The caller has no privilege on the template's project | 400 Bad Request | [40002 ProjectNoPrivilege](/doc/backend_error/project/40002.md) |
| The template is not a binary file | 400 Bad Request | [60005 TemplateNotBinaryFile](/doc/backend_error/template/60005.md) |
| The template has no stored template file | 400 Bad Request | [60009 TemplateFileNotExist](/doc/backend_error/template/60009.md) |
| The binary object is missing from OSS so no presigned URL can be generated | 400 Bad Request | [60010 TemplateBinaryMetadataMissed](/doc/backend_error/template/60010.md) |

## Example

Request: `N/A` (no body).

Success response (the `data` payload):

```json
{
  "name": "official-site: logo.png",
  "url": "https://official-site.oss-cn-hangzhou.aliyuncs.com/binary/9f1c...png?Expires=1710004200&OSSAccessKeyId=LTAI...&Signature=abcd%3D"
}
```
