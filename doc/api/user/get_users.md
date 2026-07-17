# GET /v1/user

> Controller: UserController

Get all visible users of an organization.

## Permission

[`USER_READ`](/doc/permission/USER_READ.md) — held by OWNER and ADMIN.

## Request

### Path Parameters

N/A

### Query Parameters

N/A

### Body

N/A

## Response

DTO: `UserListResponse`, returned inside the `PagerResponse` envelope as `data` — see the
[API index](/doc/api/README.md#response-envelope). Visibility is by role: an OWNER caller sees
ADMIN, DEVELOPER, and CUSTOMER [users](/doc/tech_noun/TN0202_user.md); an ADMIN caller sees
DEVELOPER and CUSTOMER users; the caller itself and other OWNERs are never listed. Each role
group is ordered by creation time. Note: the underlying query is currently **not** filtered by
[organization](/doc/tech_noun/TN0201_organization.md) — users of every organization with the
visible roles are returned (the summary describes the intent).

The `UserEntity` fields below are the canonical shape reused wherever other endpoint docs
reference a `UserEntity`.

| Field | Type | Description |
| --- | --- | --- |
| `users[].userId` | Long | Id of the user. |
| `users[].identifier` | String | Login identifier of the user. |
| `users[].name` | String | Display name of the user. |
| `users[].mail` | String | Mail address of the user. |
| `users[].enabled` | Boolean | Whether the account is enabled. |

## Errors

| Case | HTTP Status | Error Code |
| --- | --- | --- |
| The caller's role does not hold `USER_READ` (DEVELOPER, CUSTOMER) | 400 | [20002 UserNotPermitted](/doc/backend_error/user/20002.md) |

A missing or invalid JWT is answered with `401 Unauthorized` (empty body) before the handler runs.

## Example

Request: N/A (no body)

Success response:

```json
{
  "code": 200,
  "timestamp": 1752700000000,
  "data": {
    "users": [
      {
        "userId": 2,
        "identifier": "alice",
        "name": "Alice",
        "mail": "alice@example.com",
        "enabled": true
      }
    ]
  }
}
```
