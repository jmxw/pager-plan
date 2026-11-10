# POST /v1/user/admin

> Controller: UserController

Add an admin user.

## Permission

[`ADMIN_WRITE`](/doc/permission/ADMIN_WRITE.md) — held by OWNER only.

## Request

### Path Parameters

N/A

### Query Parameters

N/A

### Body

DTO: `UserAddRequest`

| Field | Type | Required | Description |
| --- | --- | --- | --- |
| `organizationId` | Long | ✅ | Present in the DTO but **ignored** — the new [user](/doc/tech_noun/TN0202_user.md) is always created in the caller's own [organization](/doc/tech_noun/TN0201_organization.md). |
| `identifier` | String | ✅ | Login identifier of the new user; globally unique. |
| `name` | String | ✅ | Display name of the new user. |
| `password` | String | ✅ | Password of the new user; stored as a hash. |
| `mail` | String | ✅ | Mail address of the new user; must not be used by another user. |

## Response

DTO: `EmptyResponse`, returned inside the `PagerResponse` envelope as `data` — see the
[API index](/doc/api/README.md#response-envelope). The user is created with the ADMIN role and
`enabled = true`.

| Field | Type | Description |
| --- | --- | --- |
| — | — | `data` is an empty object. |

## Errors

| Case | HTTP Status | Error Code |
| --- | --- | --- |
| The caller's role does not hold `ADMIN_WRITE` (ADMIN, DEVELOPER, CUSTOMER) | 400 | [20002 UserNotPermitted](/doc/backend_error/user/20002.md) |
| `identifier` is already used by another user | 400 | [20003 UserIdentifierExist](/doc/backend_error/user/20003.md) |
| `mail` is already used by another user | 400 | [20006 UserMailExist](/doc/backend_error/user/20006.md) |
| The organization carried by the JWT no longer exists | 400 | [10001 OrganizationIdNotExist](/doc/backend_error/organization/10001.md) |

A missing or invalid JWT is answered with `401 Unauthorized` (empty body) before the handler runs.

## Example

Request:

```json
{
  "organizationId": 1,
  "identifier": "alice",
  "name": "Alice",
  "password": "s3cret-Passw0rd",
  "mail": "alice@example.com"
}
```

Success response:

```json
{
  "code": 200,
  "timestamp": 1752700000000,
  "data": {}
}
```
