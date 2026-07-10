# PATCH /v1/template/{templateId}/rename

> Controller: TemplateController

Rename a template.

The [template](/doc/tech_noun/TN0401_template.md) identified by the path is renamed to the supplied
name. For a file, the [template type](/doc/tech_noun/TN0401_template.md) is re-derived from the new
name's extension. The root template and the recycle-bin template cannot be renamed.

## Permission

[`PROJECT_DEV`](/doc/permission/PROJECT_DEV.md)

## Request

### Path Parameters

| Name | Type | Description |
| --- | --- | --- |
| `templateId` | Long | Physical id of the template to rename. |

### Query Parameters

N/A

### Body

DTO: `TemplateRenameRequest`

| Field | Type | Required | Description |
| --- | --- | --- | --- |
| `name` | String | Yes | The new name of the template. |

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
| The template does not exist | 400 Bad Request | [60001 TemplateIdNotExist](/doc/backend_error/template/60001.md) |
| The caller resolved from the JWT does not exist | 400 Bad Request | [20001 UserIdNotExist](/doc/backend_error/user/20001.md) |
| The caller has no privilege on the template's project | 400 Bad Request | [40002 ProjectNoPrivilege](/doc/backend_error/project/40002.md) |
| The template is the root or the recycle-bin template and cannot be renamed | 400 Bad Request | [60011 TemplateRenameReject](/doc/backend_error/template/60011.md) |

## Example

Request:

```json
{
  "name": "contact.html"
}
```

Success response (the `data` payload):

```json
{}
```
