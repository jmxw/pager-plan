# PUT /v1/access_key/{accessKeyId}

> Controller: AccessKeyController

Updates the credentials of an existing [access key](/doc/tech_noun/TN0204_access_key.md) that
belongs to the caller's [organization](/doc/tech_noun/TN0201_organization.md). The submitted access
key id and secret are re-encrypted before being persisted.

## Permission

`N/A (authenticated)` — login is required, but the controller method carries no `@Permitted`
annotation.

## Request

### Path Parameters

| Name | Type | Description |
| --- | --- | --- |
| `accessKeyId` | Long | Physical id of the access key to update. |

### Query Parameters

N/A

### Body

DTO: `AccessKeyUpsertRequest`

| Field | Type | Required | Description |
| --- | --- | --- | --- |
| `name` | String | Yes | Present in the request DTO but ignored on edit; the stored name is left unchanged. |
| `accessKeyId` | String | Yes | The new access key id to store (encrypted at rest). |
| `accessKeySecret` | String | Yes | The new access key secret to store (encrypted at rest). |

## Response

DTO: `EmptyResponse` — wrapped as `data` inside the standard `PagerResponse` envelope
(`{ code, timestamp, data }`). The `data` body is empty (`{}`).

| Field | Type | Description |
| --- | --- | --- |
| — | — | The response body is empty (`{}`). |

## Errors

| Case | HTTP Status | Error Code |
| --- | --- | --- |
| No access key exists with the given id | 400 Bad Request | [30001 AccessKeyIdNotExist](/doc/backend_error/access_key/30001.md) |
| The access key belongs to a different organization than the caller's | 400 Bad Request | [30002 AccessKeyNotBelongToOrganization](/doc/backend_error/access_key/30002.md) |

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
