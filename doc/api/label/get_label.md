# GET /v1/label/{labelId}

> Controller: LabelController

> Status: not yet implemented (controller method is a stub).

A single [label](/doc/tech_noun/TN0303_label.md) is retrieved by its id.

## Permission

[`LABEL_READ`](/doc/permission/LABEL_READ.md) — the exact `@Permitted(UserPermission.LABEL_READ)` on the method.

## Request

### Path Parameters

| Name | Type | Description |
| --- | --- | --- |
| `labelId` | Long | Identifies the label to retrieve. |

### Query Parameters

N/A

### Body

N/A

## Response

N/A — the controller method returns `Unit` (no return DTO defined yet).

## Errors

N/A — no service call yet; only the `@Permitted` gate is applied.

## Example

Request:

N/A

Success response (the `data` payload):

N/A
