# PATCH /v1/template/{templateId}/copy_to/{targetTemplateId}

> Controller: TemplateController

> Status: not yet implemented (controller method delegates to a service method that is a `TODO()` stub).

Copy a template and its children as a child of the target template.

The [template](/doc/tech_noun/TN0401_template.md) identified by `templateId`, together with its
descendant templates, is intended to be duplicated as a child of the target template identified by
`targetTemplateId` within the same [project](/doc/tech_noun/TN0301_project.md). The service method
backing this endpoint is currently a stub, so the operation is not yet functional.

## Permission

[`PROJECT_DEV`](/doc/permission/PROJECT_DEV.md)

## Request

### Path Parameters

| Name | Type | Description |
| --- | --- | --- |
| `templateId` | Long | Physical id of the template to copy. |
| `targetTemplateId` | Long | Physical id of the target directory template the copy is placed under. |

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

N/A — the service method is a stub (`TODO()`); beyond the `@Permitted` gate no business validation
is performed yet. Once implemented it is expected to raise the same errors as
[move_template](/doc/api/template/move_template.md) plus a duplicate-name error
([60006 TemplateNameDuplicated](/doc/backend_error/template/60006.md)).

## Example

Request: `N/A` (no body).

Success response (the `data` payload):

```json
{}
```
