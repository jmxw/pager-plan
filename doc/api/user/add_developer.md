# POST /v1/user/developer

> Controller: UserController

Add a developer user. A new [user](/doc/tech_noun/TN0202_user.md) is created with the `DEVELOPER`
role in the caller's [organization](/doc/tech_noun/TN0201_organization.md). The plaintext password is
encoded before being stored.

## Permission

[`USER_WRITE`](/doc/permission/USER_WRITE.md)

## Request

### Path Parameters

N/A

### Query Parameters

N/A

### Body

DTO: `UserAddRequest`

| Field | Type | Required | Description |
| --- | --- | --- | --- |
| `organizationId` | Long | Yes | The target organization id. Currently ignored by the controller; the new user is created in the caller's own organization, resolved from the JWT. |
| `identifier` | String | Yes | The unique [identifier](/doc/tech_noun/TN0101_identifier.md) for the new user; it must not already be in use. |
| `name` | String | Yes | Display name of the new user. |
| `password` | String | Yes | The plaintext password; it is encoded before being stored. |
| `mail` | String | Yes | The email address of the new user; it must not already be in use. |

The role is fixed to `DEVELOPER` by the controller and is not read from the request body.

## Response

DTO: `EmptyResponse` — wrapped as `data` inside the standard `PagerResponse` envelope
(`{ code, timestamp, data }`). The `data` body is empty (`{}`).

| Field | Type | Description |
| --- | --- | --- |
| — | — | The response body is empty (`{}`). |

## Errors

| Case | HTTP Status | Error Code |
| --- | --- | --- |
| Caller's role lacks the required permission | 400 Bad Request | [20002 UserNotPermitted](/doc/backend_error/user/20002.md) |
| A user with the given `identifier` already exists | 400 Bad Request | [20003 UserIdentifierExist](/doc/backend_error/user/20003.md) |
| A user with the given `mail` already exists | 400 Bad Request | [20006 UserMailExist](/doc/backend_error/user/20006.md) |
| The caller's organization does not exist | 400 Bad Request | [10001 OrganizationIdNotExist](/doc/backend_error/organization/10001.md) |

## Example

Request:

```json
{
  "organizationId": 1,
  "identifier": "dev-wang",
  "name": "王开发",
  "password": "P@ssw0rd!",
  "mail": "dev-wang@monstar-lab.com"
}
```

Success response (the `data` payload):

```json
{}
```
