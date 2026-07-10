# POST /v1/access_key

> Controller: AccessKeyController

Adds a new [access key](/doc/tech_noun/TN0204_access_key.md) to the caller's
[organization](/doc/tech_noun/TN0201_organization.md). The access key id and secret are encrypted
before being persisted.

## Permission

[`ACCESS_KEY_WRITE`](/doc/permission/ACCESS_KEY_WRITE.md)

## Request

### Path Parameters

N/A

### Query Parameters

N/A

### Body

DTO: `AccessKeyUpsertRequest`

| Field | Type | Required | Description |
| --- | --- | --- | --- |
| `name` | String | Yes | Display name of the access key. |
| `accessKeyId` | String | Yes | The access key id to store (encrypted at rest). |
| `accessKeySecret` | String | Yes | The access key secret to store (encrypted at rest). |

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
| Caller's organization does not exist | 400 Bad Request | [10001 OrganizationIdNotExist](/doc/backend_error/organization/10001.md) |

## Example

Request:

```json
{
  "name": "生产环境密钥",
  "accessKeyId": "LTAI5tAbCdEfGhIjKlMn",
  "accessKeySecret": "wJalrXUtnFEMIK7MDENGbPxRfiCYEXAMPLEKEY"
}
```

Success response (the `data` payload):

```json
{}
```
