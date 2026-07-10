# POST /v1/label

> Controller: LabelController

> Status: not yet implemented (controller method is a stub).

A new [label](/doc/tech_noun/TN0303_label.md) is added to a project.

## Permission

[`LABEL_WRITE`](/doc/permission/LABEL_WRITE.md) — the exact `@Permitted(UserPermission.LABEL_WRITE)` on the method.

## Request

### Path Parameters

N/A

### Query Parameters

N/A

### Body

N/A — no request DTO exists yet; the method takes only the authenticated principal.

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
