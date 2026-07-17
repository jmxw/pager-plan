# GET /v1/user/me

> Controller: UserController

Get user information of the current user.

## Permission

[`USER_READ`](/doc/permission/USER_READ.md) — held by OWNER and ADMIN. Note that a DEVELOPER or
CUSTOMER caller is therefore rejected by this endpoint even though it only reads the caller's
own profile — relaxing that would be a backend change owned by a future plan.

## Request

### Path Parameters

N/A

### Query Parameters

N/A

### Body

N/A

## Response

DTO: `UserGetInfoResponse`, returned inside the `PagerResponse` envelope as `data` — see the
[API index](/doc/api/README.md#response-envelope).

| Field | Type | Description |
| --- | --- | --- |
| `username` | String | Display name of the calling [user](/doc/tech_noun/TN0202_user.md). |
| `avatar` | String | Avatar URL. Currently a hard-coded placeholder image for every user. |
| `roles` | Set&lt;UserRole&gt; | The caller's role (always a single-element set): `OWNER`, `ADMIN`, `DEVELOPER`, or `CUSTOMER`. |
| `permissions` | Set&lt;UserPermission&gt; | The permission set granted by the role — see the [permission index](/doc/permission/README.md). |

## Errors

| Case | HTTP Status | Error Code |
| --- | --- | --- |
| The caller's role does not hold `USER_READ` (DEVELOPER, CUSTOMER) | 400 | [20002 UserNotPermitted](/doc/backend_error/user/20002.md) |
| The calling user's account row no longer exists | 400 | [20001 UserIdNotExist](/doc/backend_error/user/20001.md) |

A missing or invalid JWT is answered with `401 Unauthorized` (empty body) before the handler runs.

## Example

Request: N/A (no body)

Success response:

```json
{
  "code": 200,
  "timestamp": 1752700000000,
  "data": {
    "username": "Alice",
    "avatar": "https://avatars.githubusercontent.com/u/9463655",
    "roles": ["ADMIN"],
    "permissions": [
      "USER_READ",
      "USER_WRITE",
      "ACCESS_KEY_READ",
      "ACCESS_KEY_WRITE",
      "PROJECT_READ",
      "PROJECT_WRITE",
      "PROJECT_DEV",
      "PROJECT_DEPLOY",
      "ARTICLE_LIST_READ",
      "ARTICLE_LIST_WRITE",
      "ARTICLE_READ",
      "ARTICLE_WRITE"
    ]
  }
}
```
