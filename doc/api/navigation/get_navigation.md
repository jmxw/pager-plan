# GET /v1/navigation/{navigationId}

> Controller: NavigationController

> Status: not yet implemented (controller method is a stub).

Get detail information of a [navigation](/doc/tech_noun/TN0601_navigation.md) menu.

## Permission

[`PROJECT_DEV`](/doc/permission/PROJECT_DEV.md)

## Request

### Path Parameters

| Name | Type | Description |
| --- | --- | --- |
| `navigationId` | Long | Physical id of the navigation menu whose detail is requested. |

### Query Parameters

N/A

### Body

N/A

## Response

N/A — the controller method has an empty body and returns `Unit`, so no payload is produced yet.

## Errors

N/A — no service call yet; only the `@Permitted` gate is applied.

## Example

Request:

N/A

Success response (the `data` payload):

N/A
