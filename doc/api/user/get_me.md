# GET /v1/user/me

> Controller: UserController

Get user information of the current user. The profile of the [user](/doc/tech_noun/TN0202_user.md)
identified by the JWT is returned, including the display name, avatar, assigned roles, and the
effective [permissions](/doc/permission/README.md) granted by the user's role.

## Permission

[`USER_READ`](/doc/permission/USER_READ.md)

## Request

### Path Parameters

N/A

### Query Parameters

N/A

### Body

N/A

## Response

DTO: `UserGetInfoResponse` — wrapped as `data` inside the standard `PagerResponse` envelope
(`{ code, timestamp, data }`).

| Field | Type | Description |
| --- | --- | --- |
| `username` | String | Display name of the current user. |
| `avatar` | String | Avatar URL of the current user. |
| `roles` | Set\<UserRole\> | The roles assigned to the user (currently the single role wrapped in a set). |
| `permissions` | Set\<UserPermission\> | The effective permissions granted by the user's role. |

## Errors

| Case | HTTP Status | Error Code |
| --- | --- | --- |
| Caller's role lacks the required permission | 400 Bad Request | [20002 UserNotPermitted](/doc/backend_error/user/20002.md) |
| The authenticated user does not exist | 400 Bad Request | [20001 UserIdNotExist](/doc/backend_error/user/20001.md) |

## Example

Request: `N/A` (no body).

Success response (the `data` payload):

```json
{
  "username": "李管理",
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
```
