# POST /v1/template/{templateId}/add

> Controller: TemplateController

Add a new template to a parent template.

A new child [template](/doc/tech_noun/TN0401_template.md) is created under the parent template
identified by the path. When `isDirectory` is `true` the new node is a directory; otherwise its
[template type](/doc/tech_noun/TN0401_template.md) is derived from the file-name extension of
`name`. The new node carries no content of its own — text content is written later through the
update-content endpoint, and binary files are created through the upload endpoint.

## Permission

[`PROJECT_DEV`](/doc/permission/PROJECT_DEV.md)

## Request

### Path Parameters

| Name | Type | Description |
| --- | --- | --- |
| `templateId` | Long | Physical id of the parent template the new child is added to. |

### Query Parameters

N/A

### Body

DTO: `TemplateAddRequest`

| Field | Type | Required | Description |
| --- | --- | --- | --- |
| `name` | String | Yes | Name of the new template. For a file, its extension determines the template type. |
| `isDirectory` | Boolean | Yes | Whether the new template is a directory (`true`) or a file (`false`). |

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
| A child with the same name already exists under the parent | 400 Bad Request | [60006 TemplateNameDuplicated](/doc/backend_error/template/60006.md) |

## Example

Request:

```json
{
  "name": "about.html",
  "isDirectory": false
}
```

Success response (the `data` payload):

```json
{}
```
