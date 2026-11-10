# POST /v1/access_key

> Controller: AccessKeyController

Add a new access key into an organization.

## Permission

[`ACCESS_KEY_WRITE`](/doc/permission/ACCESS_KEY_WRITE.md) — held by OWNER and ADMIN.

## Request

### Path Parameters

N/A

### Query Parameters

N/A

### Body

DTO: `AccessKeyUpsertRequest`

| Field | Type | Required | Description |
| --- | --- | --- | --- |
| `name` | String | ✅ | Display name of the [access key](/doc/tech_noun/TN0204_access_key.md). |
| `accessKeyId` | String | ✅ | The Aliyun AccessKey ID; stored encrypted. |
| `accessKeySecret` | String | ✅ | The Aliyun AccessKey secret; stored encrypted. |

## Response

DTO: `EmptyResponse`, returned inside the `PagerResponse` envelope as `data` — see the
[API index](/doc/api/README.md#response-envelope). The key is registered to the caller's
[organization](/doc/tech_noun/TN0201_organization.md).

| Field | Type | Description |
| --- | --- | --- |
| — | — | `data` is an empty object. |

## Errors

| Case | HTTP Status | Error Code |
| --- | --- | --- |
| The caller's role does not hold `ACCESS_KEY_WRITE` (DEVELOPER, CUSTOMER) | 400 | [20002 UserNotPermitted](/doc/backend_error/user/20002.md) |
| The organization carried by the JWT no longer exists | 400 | [10001 OrganizationIdNotExist](/doc/backend_error/organization/10001.md) |

A missing or invalid JWT is answered with `401 Unauthorized` (empty body) before the handler runs.

## Example

Request:

```json
{
  "name": "aliyun main account",
  "accessKeyId": "LTAI5tExampleId",
  "accessKeySecret": "Ab3dExampleSecret"
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
