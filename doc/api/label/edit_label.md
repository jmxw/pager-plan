# PUT /v1/label/{labelId}

> Controller: LabelController

> Status: not yet implemented (controller method is a stub).

An existing [label](/doc/tech_noun/TN0303_label.md) is updated by its id.

## Permission

[`LABEL_WRITE`](/doc/permission/LABEL_WRITE.md) — the exact `@Permitted(UserPermission.LABEL_WRITE)` on the method.

## Request

### Path Parameters

| Name | Type | Description |
| --- | --- | --- |
| `labelId` | Long | Identifies the label to update. |

### Query Parameters

N/A

### Body

N/A — no request DTO exists yet; the method takes only the path variable and the authenticated principal.

## Response

DTO: `EmptyResponse` — wrapped as `data` inside the standard `PagerResponse` envelope
(`{ code, timestamp, data }`). The `data` payload is an empty object `{}`.

## Errors

N/A — no service call yet; only the `@Permitted` gate is applied.

## Example

Request:

N/A

Success response (the `data` payload):

```json
{ }
```
