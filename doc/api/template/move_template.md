# PATCH /v1/template/{templateId}/move_to/{targetTemplateId}

> Controller: TemplateController

Move a template and its children as a child of the target template.

The [template](/doc/tech_noun/TN0401_template.md) identified by `templateId`, together with its
descendant templates, is re-parented under the target template identified by `targetTemplateId`; the
layer of the moved template and every descendant is recalculated. The target must be a directory and
must belong to the same [project](/doc/tech_noun/TN0301_project.md) as the moved template.

## Permission

[`PROJECT_DEV`](/doc/permission/PROJECT_DEV.md)

## Request

### Path Parameters

| Name | Type | Description |
| --- | --- | --- |
| `templateId` | Long | Physical id of the template to move. |
| `targetTemplateId` | Long | Physical id of the target directory template the moved template is placed under. |

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
| The template or the target template does not exist | 400 Bad Request | [60001 TemplateIdNotExist](/doc/backend_error/template/60001.md) |
| The caller resolved from the JWT does not exist | 400 Bad Request | [20001 UserIdNotExist](/doc/backend_error/user/20001.md) |
| The caller has no privilege on the template's project | 400 Bad Request | [40002 ProjectNoPrivilege](/doc/backend_error/project/40002.md) |
| The target template is not a directory | 400 Bad Request | [60007 TemplateDirectoryRequired](/doc/backend_error/template/60007.md) |
| The template and the target template belong to different projects | 400 Bad Request | [60012 TemplateSameProjectRequired](/doc/backend_error/template/60012.md) |

## Example

Request: `N/A` (no body).

Success response (the `data` payload):

```json
{}
```
