# PATCH /v1/user/{userId}

> Controller: UserController

> Status: not yet implemented (controller method is a stub).

Get user information by userId. The [user](/doc/tech_noun/TN0202_user.md) identified by the `userId`
path variable is the target of this endpoint. The controller method currently has an empty body and
performs no action.

## Permission

[`USER_WRITE`](/doc/permission/USER_WRITE.md)

## Request

### Path Parameters

| Name | Type | Description |
| --- | --- | --- |
| `userId` | Long | Identifies the user targeted by the request. |

### Query Parameters

N/A

### Body

N/A — no request DTO exists yet; the method takes only the path variable.

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
{}
```
