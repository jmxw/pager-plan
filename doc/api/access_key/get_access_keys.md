# GET /v1/access_key

> Controller: AccessKeyController

Get all access keys of an organization.

## Permission

[`ACCESS_KEY_READ`](/doc/permission/ACCESS_KEY_READ.md) — held by OWNER and ADMIN.

## Request

### Path Parameters

N/A

### Query Parameters

N/A

### Body

N/A

## Response

DTO: `AccessKeyListResponse`, returned inside the `PagerResponse` envelope as `data` — see the
[API index](/doc/api/README.md#response-envelope). All [access keys](/doc/tech_noun/TN0204_access_key.md)
of the caller's [organization](/doc/tech_noun/TN0201_organization.md) are returned, ordered by
creation time.

| Field | Type | Description |
| --- | --- | --- |
| `accessKeys[].id` | Long | Id of the access key row (used as `{accessKeyId}` in the edit/delete paths and as `accessKeyId` when adding a project). |
| `accessKeys[].createAt` | Long | Creation timestamp (epoch milliseconds). |
| `accessKeys[].name` | String | Display name of the access key. |
| `accessKeys[].accessKeyId` | String | The Aliyun AccessKey ID, decrypted for display. |
| `accessKeys[].accessKeySecret` | String | The Aliyun AccessKey secret, masked: the first 4 characters are kept, every later character is replaced with `*`. |

## Errors

| Case | HTTP Status | Error Code |
| --- | --- | --- |
| The caller's role does not hold `ACCESS_KEY_READ` (DEVELOPER, CUSTOMER) | 400 | [20002 UserNotPermitted](/doc/backend_error/user/20002.md) |
| The organization carried by the JWT no longer exists | 400 | [10001 OrganizationIdNotExist](/doc/backend_error/organization/10001.md) |

A missing or invalid JWT is answered with `401 Unauthorized` (empty body) before the handler runs.

## Example

Request: N/A (no body)

Success response:

```json
{
  "code": 200,
  "timestamp": 1752700000000,
  "data": {
    "accessKeys": [
      {
        "id": 1,
        "createAt": 1752600000000,
        "name": "aliyun main account",
        "accessKeyId": "LTAI5tExampleId",
        "accessKeySecret": "Ab3d**************"
      }
    ]
  }
}
```
