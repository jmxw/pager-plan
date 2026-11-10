# POST /v1/organization/register

> Controller: OrganizationController

Register a new organization with its owner user.

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

DTO: `OrganizationRegisterRequest`

| Field | Type | Required | Description |
| --- | --- | --- | --- |
| `organizationIdentifier` | String | ✅ | The [identifier](/doc/tech_noun/TN0101_identifier.md) of the new [organization](/doc/tech_noun/TN0201_organization.md); globally unique. |
| `userIdentifier` | String | ✅ | Login identifier of the first [user](/doc/tech_noun/TN0202_user.md), created with the OWNER role; globally unique. |
| `password` | String | ✅ | Password of the owner user; stored as a hash. |
| `mail` | String | ✅ | Mail address of the owner user. |
| `name` | String | ✅ | Display name used for **both** the organization and the owner user. |

## Response

DTO: `OrganizationRegisterResponse`, returned inside the `PagerResponse` envelope as `data` —
see the [API index](/doc/api/README.md#response-envelope).

| Field | Type | Description |
| --- | --- | --- |
| `organization.createAt` | Long | Creation timestamp (epoch milliseconds). |
| `organization.updateAt` | Long | Last-update timestamp (epoch milliseconds). |
| `organization.identifier` | String | The identifier of the new organization. |
| `organization.name` | String | Display name of the new organization. |

## Errors

| Case | HTTP Status | Error Code |
| --- | --- | --- |
| `organizationIdentifier` is already used by another organization | 400 | [10002 OrganizationIdentifierExist](/doc/backend_error/organization/10002.md) |
| `userIdentifier` is already used by another user | 400 | [20003 UserIdentifierExist](/doc/backend_error/user/20003.md) |

## Example

Request:

```json
{
  "organizationIdentifier": "acme",
  "userIdentifier": "acme-owner",
  "password": "s3cret-Passw0rd",
  "mail": "owner@acme.example.com",
  "name": "ACME Inc."
}
```

Success response:

```json
{
  "code": 200,
  "timestamp": 1752700000000,
  "data": {
    "organization": {
      "createAt": 1752700000000,
      "updateAt": 1752700000000,
      "identifier": "acme",
      "name": "ACME Inc."
    }
  }
}
```
