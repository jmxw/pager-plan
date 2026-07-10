# GET /v1/access_key

> Controller: AccessKeyController

Returns all [access keys](/doc/tech_noun/TN0204_access_key.md) of the caller's
[organization](/doc/tech_noun/TN0201_organization.md), ordered by creation time.

## Permission

[`ACCESS_KEY_READ`](/doc/permission/ACCESS_KEY_READ.md)

## Request

### Path Parameters

N/A

### Query Parameters

N/A

### Body

N/A

## Response

DTO: `AccessKeyListResponse` — wrapped as `data` inside the standard `PagerResponse` envelope
(`{ code, timestamp, data }`).

| Field | Type | Description |
| --- | --- | --- |
| `accessKeys` | List\<AccessKeyEntity\> | The organization's access keys, ordered by creation time. |

Each `AccessKeyEntity` element carries the following fields:

| Field | Type | Description |
| --- | --- | --- |
| `id` | Long | Physical id of the access key. |
| `createAt` | Long | Creation timestamp in epoch milliseconds. |
| `name` | String | Display name of the access key. |
| `accessKeyId` | String | The decrypted access key id. |
| `accessKeySecret` | String | The decrypted access key secret, masked so that only the first four characters are kept and every remaining character is replaced with `*`. |

## Errors

| Case | HTTP Status | Error Code |
| --- | --- | --- |
| Caller's role lacks the required permission | 400 Bad Request | [20002 UserNotPermitted](/doc/backend_error/user/20002.md) |
| Caller's organization does not exist | 400 Bad Request | [10001 OrganizationIdNotExist](/doc/backend_error/organization/10001.md) |

## Example

Request: `N/A` (no body).

Success response (the `data` payload):

```json
{
  "accessKeys": [
    {
      "id": 1,
      "createAt": 1710000000000,
      "name": "生产环境密钥",
      "accessKeyId": "LTAI5tAbCdEfGhIjKlMn",
      "accessKeySecret": "abcd************************"
    }
  ]
}
```
