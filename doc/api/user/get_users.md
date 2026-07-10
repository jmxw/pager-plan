# GET /v1/user

> Controller: UserController

Get all visible users of an [organization](/doc/tech_noun/TN0201_organization.md). The
[users](/doc/tech_noun/TN0202_user.md) visible to the caller are returned, ordered by creation time.
Visibility depends on the caller's role: an `OWNER` sees `ADMIN`, `DEVELOPER`, and `CUSTOMER` users;
an `ADMIN` sees `DEVELOPER` and `CUSTOMER` users; a `DEVELOPER` or `CUSTOMER` sees no users.

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

DTO: `UserListResponse` — wrapped as `data` inside the standard `PagerResponse` envelope
(`{ code, timestamp, data }`).

| Field | Type | Description |
| --- | --- | --- |
| `users` | List\<UserEntity\> | The users visible to the caller, ordered by creation time. |

Each `UserEntity` element carries the following fields:

| Field | Type | Description |
| --- | --- | --- |
| `userId` | Long | Physical id of the user. |
| `identifier` | String | The user's unique identifier. |
| `name` | String | Display name of the user. |
| `mail` | String | The user's email address. |
| `enabled` | Boolean | Whether the user account is enabled. |

## Errors

| Case | HTTP Status | Error Code |
| --- | --- | --- |
| Caller's role lacks the required permission | 400 Bad Request | [20002 UserNotPermitted](/doc/backend_error/user/20002.md) |

## Example

Request: `N/A` (no body).

Success response (the `data` payload):

```json
{
  "users": [
    {
      "userId": 2,
      "identifier": "li-admin",
      "name": "李管理",
      "mail": "li-admin@monstar-lab.com",
      "enabled": true
    },
    {
      "userId": 3,
      "identifier": "dev-wang",
      "name": "王开发",
      "mail": "dev-wang@monstar-lab.com",
      "enabled": true
    }
  ]
}
```
