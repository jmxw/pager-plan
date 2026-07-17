# DELETE /v1/user/{userId}

> Controller: UserController

Delete user.

**Stub endpoint** — the handler performs no work: nothing is deleted and no service is called
(`UserService.delete` exists but is empty and is not wired to the controller). This doc records
the declared surface; it is extended together with the implementation (same-PR sync rule).

## Permission

[`USER_WRITE`](/doc/permission/USER_WRITE.md) — held by OWNER and ADMIN.

## Request

### Path Parameters

| Name | Type | Description |
| --- | --- | --- |
| `userId` | Long | Id of the [user](/doc/tech_noun/TN0202_user.md) to delete. |

### Query Parameters

N/A

### Body

N/A

## Response

DTO: `EmptyResponse`, returned inside the `PagerResponse` envelope as `data` — see the
[API index](/doc/api/README.md#response-envelope).

| Field | Type | Description |
| --- | --- | --- |
| — | — | `data` is an empty object. |

## Errors

| Case | HTTP Status | Error Code |
| --- | --- | --- |
| The caller's role does not hold `USER_WRITE` (DEVELOPER, CUSTOMER) | 400 | [20002 UserNotPermitted](/doc/backend_error/user/20002.md) |

A missing or invalid JWT is answered with `401 Unauthorized` (empty body) before the handler runs.

## Example

Request: N/A (no body)

Success response:

```json
{
  "code": 200,
  "timestamp": 1752700000000,
  "data": {}
}
```
