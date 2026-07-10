# GET /v1/user/logout

> Controller: UserController

User logout. The endpoint accepts the caller's JWT and returns an empty payload. The controller
method performs no service call, so no server-side state is changed; token invalidation, if any, is
handled on the client side.

## Permission

`N/A (authenticated)`

## Request

### Path Parameters

N/A

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

N/A

## Example

Request: `N/A` (no body).

Success response (the `data` payload):

```json
{}
```
