# GET /v1/user/roles

> Controller: UserController

Get all available user roles.

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

DTO: `UserRolesResponse`, returned inside the `PagerResponse` envelope as `data` — see the
[API index](/doc/api/README.md#response-envelope). All values of the `UserRole` enum are
returned — the role hierarchy of a [user](/doc/tech_noun/TN0202_user.md) is
OWNER > ADMIN > DEVELOPER > CUSTOMER (see the [permission index](/doc/permission/README.md)).

| Field | Type | Description |
| --- | --- | --- |
| `userRoles[].value` | String | The `UserRole` enum constant name (`OWNER`, `ADMIN`, `DEVELOPER`, `CUSTOMER`). |
| `userRoles[].roleName` | String | Display name of the role as stored in the enum (Chinese label, e.g. `管理员`). |

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
    "userRoles": [
      { "value": "OWNER", "roleName": "所有者" },
      { "value": "ADMIN", "roleName": "管理员" },
      { "value": "DEVELOPER", "roleName": "开发者" },
      { "value": "CUSTOMER", "roleName": "客户" }
    ]
  }
}
```
