# POST /v1/user/login

> Controller: UserController

User login. The [user](/doc/tech_noun/TN0202_user.md) matching the supplied credentials is
authenticated, and a JWT token is issued for use on subsequent authenticated requests. This path is
whitelisted in `SecurityConfiguration`, so no authentication is required.

## Permission

`N/A (public)`

## Request

### Path Parameters

N/A

### Query Parameters

N/A

### Body

DTO: `UserLoginRequest`

| Field | Type | Required | Description |
| --- | --- | --- | --- |
| `userIdentifier` | String | Yes | The unique [identifier](/doc/tech_noun/TN0101_identifier.md) of the user to authenticate. |
| `password` | String | Yes | The plaintext password; it is matched against the stored encoded credential. |

## Response

DTO: `UserLoginResponse` — wrapped as `data` inside the standard `PagerResponse` envelope
(`{ code, timestamp, data }`).

| Field | Type | Description |
| --- | --- | --- |
| `identifier` | String | The identifier of the authenticated user, echoed from the request. |
| `token` | String | The issued JWT token, to be sent as the bearer credential on subsequent requests. |

## Errors

| Case | HTTP Status | Error Code |
| --- | --- | --- |
| No user exists with the given `userIdentifier` | 400 Bad Request | [20004 UserIdentifierNotExist](/doc/backend_error/user/20004.md) |
| The password does not match the stored credential | 400 Bad Request | [20005 UserPasswordWrong](/doc/backend_error/user/20005.md) |

## Example

Request:

```json
{
  "userIdentifier": "zhangsan",
  "password": "P@ssw0rd!"
}
```

Success response (the `data` payload):

```json
{
  "identifier": "zhangsan",
  "token": "eyJhbGciOiJIUzI1NiJ9.eyJ1c2VySWQiOjF9.3q2Lf0m8s9d..."
}
```
