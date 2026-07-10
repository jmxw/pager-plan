# GET /v1/user/roles

> Controller: UserController

Get all available user roles. Every [user](/doc/tech_noun/TN0202_user.md) role defined by the
`UserRole` enumeration is returned, each with its enum value and localized display name.

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

DTO: `UserRolesResponse` — wrapped as `data` inside the standard `PagerResponse` envelope
(`{ code, timestamp, data }`).

| Field | Type | Description |
| --- | --- | --- |
| `userRoles` | List\<UserRoleEntity\> | All available user roles. |

Each `UserRoleEntity` element carries the following fields:

| Field | Type | Description |
| --- | --- | --- |
| `value` | String | The role's enum value (`OWNER`, `ADMIN`, `DEVELOPER`, `CUSTOMER`). |
| `roleName` | String | The role's localized display name. |

## Errors

| Case | HTTP Status | Error Code |
| --- | --- | --- |
| Caller's role lacks the required permission | 400 Bad Request | [20002 UserNotPermitted](/doc/backend_error/user/20002.md) |

## Example

Request: `N/A` (no body).

Success response (the `data` payload):

```json
{
  "userRoles": [
    { "value": "OWNER", "roleName": "所有者" },
    { "value": "ADMIN", "roleName": "管理员" },
    { "value": "DEVELOPER", "roleName": "开发者" },
    { "value": "CUSTOMER", "roleName": "客户" }
  ]
}
```
