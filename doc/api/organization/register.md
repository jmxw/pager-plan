# POST /v1/organization/register

> Controller: OrganizationController

Registers a new [organization](/doc/tech_noun/TN0201_organization.md) together with its initial
owner [user](/doc/tech_noun/TN0202_user.md), which is created with the `OWNER` role. This path is
whitelisted in `SecurityConfiguration`, so no authentication is required.

## Permission

`N/A (public)`

## Request

### Path Parameters

N/A

### Query Parameters

N/A

### Body

DTO: `OrganizationRegisterRequest`

| Field | Type | Required | Description |
| --- | --- | --- | --- |
| `organizationIdentifier` | String | Yes | The unique [identifier](/doc/tech_noun/TN0101_identifier.md) of the organization to create. |
| `userIdentifier` | String | Yes | The unique identifier of the owner user to create. |
| `password` | String | Yes | The plaintext password of the owner user; it is encoded before being stored. |
| `mail` | String | Yes | The email address of the owner user. |
| `name` | String | Yes | The display name; it is applied to both the created organization and the created owner user. |

## Response

DTO: `OrganizationRegisterResponse` — wrapped as `data` inside the standard `PagerResponse`
envelope (`{ code, timestamp, data }`).

| Field | Type | Description |
| --- | --- | --- |
| `organization` | OrganizationEntity | The newly created organization. |

The `organization` object carries the following fields:

| Field | Type | Description |
| --- | --- | --- |
| `createAt` | Long | Creation timestamp in epoch milliseconds. |
| `updateAt` | Long | Last-update timestamp in epoch milliseconds. |
| `identifier` | String | The organization's unique identifier. |
| `name` | String | Display name of the organization. |

## Errors

| Case | HTTP Status | Error Code |
| --- | --- | --- |
| An organization with the given `organizationIdentifier` already exists | 400 Bad Request | [10002 OrganizationIdentifierExist](/doc/backend_error/organization/10002.md) |
| A user with the given `userIdentifier` already exists | 400 Bad Request | [20003 UserIdentifierExist](/doc/backend_error/user/20003.md) |

## Example

Request:

```json
{
  "organizationIdentifier": "monstar-lab",
  "userIdentifier": "admin",
  "password": "P@ssw0rd!",
  "mail": "admin@monstar-lab.com",
  "name": "梦星实验室"
}
```

Success response (the `data` payload):

```json
{
  "organization": {
    "createAt": 1710000000000,
    "updateAt": 1710000000000,
    "identifier": "monstar-lab",
    "name": "梦星实验室"
  }
}
```
