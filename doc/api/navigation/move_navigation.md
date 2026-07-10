# PATCH /v1/navigation/{navigationId}/move_to/{targetNavigationId}

> Controller: NavigationController

> Status: not yet implemented (controller method is a stub).

Move a [navigation](/doc/tech_noun/TN0601_navigation.md) menu as a child of an other parent navigation menu.

## Permission

[`PROJECT_DEV`](/doc/permission/PROJECT_DEV.md)

## Request

### Path Parameters

| Name | Type | Description |
| --- | --- | --- |
| `navigationId` | Long | Physical id of the navigation menu to be moved. |
| `targetNavigationId` | Long | Physical id of the navigation menu that becomes the new parent. |

### Query Parameters

N/A

### Body

N/A

## Response

DTO: `EmptyResponse` — wrapped as `data` inside the standard `PagerResponse` envelope
(`{ code, timestamp, data }`). The `data` payload is an empty object `{}`.

| Field | Type | Description |
| --- | --- | --- |
| — | — | The response body is empty (`{}`). |

## Errors

N/A — no service call yet; only the `@Permitted` gate is applied.

## Example

Request:

N/A

Success response (the `data` payload):

```json
{}
```
