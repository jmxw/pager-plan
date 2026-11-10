# POST /v1/user/login

> Controller: UserController

User login

## Permission

N/A (public) — the path is whitelisted in
[`SecurityConfiguration.kt`](/source/pager-backend/app/cms/src/main/kotlin/com/xwkj/cms/configuration/SecurityConfiguration.kt);
no login is required.

## Request

### Path Parameters

N/A

### Query Parameters

N/A

### Body

DTO: `UserLoginRequest`

| Field | Type | Required | Description |
| --- | --- | --- | --- |
| `userIdentifier` | String | ✅ | Login identifier of the [user](/doc/tech_noun/TN0202_user.md). |
| `password` | String | ✅ | The user's password. |

## Response

DTO: `UserLoginResponse`, returned inside the `PagerResponse` envelope as `data` — see the
[API index](/doc/api/README.md#response-envelope).

| Field | Type | Description |
| --- | --- | --- |
| `identifier` | String | The login identifier, echoed back. |
| `token` | String | The JWT to send as the `Authorization: Bearer <token>` header on every authenticated call. |

## Errors

| Case | HTTP Status | Error Code |
| --- | --- | --- |
| No user exists for `userIdentifier` | 400 | [20004 UserIdentifierNotExist](/doc/backend_error/user/20004.md) |
| The password does not match | 400 | [20005 UserPasswordWrong](/doc/backend_error/user/20005.md) |

## Example

Request:

```json
{
  "userIdentifier": "acme-owner",
  "password": "s3cret-Passw0rd"
}
```

Success response:

```json
{
  "code": 200,
  "timestamp": 1752700000000,
  "data": {
    "identifier": "acme-owner",
    "token": "eyJhbGciOiJIUzUxMiJ9..."
  }
}
```
