# DELETE /v1/access_key/{accessKeyId}

> Controller: AccessKeyController

Deletes an [access key](/doc/tech_noun/TN0204_access_key.md) that belongs to the caller's
[organization](/doc/tech_noun/TN0201_organization.md). Deletion is rejected while the access key is
still referenced by any project.

## Permission

[`ACCESS_KEY_WRITE`](/doc/permission/ACCESS_KEY_WRITE.md)

## Request

### Path Parameters

| Name | Type | Description |
| --- | --- | --- |
| `accessKeyId` | Long | Physical id of the access key to delete. |

### Query Parameters

N/A

### Body

N/A

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
| No access key exists with the given id | 400 Bad Request | [30001 AccessKeyIdNotExist](/doc/backend_error/access_key/30001.md) |
| The access key belongs to a different organization than the caller's | 400 Bad Request | [30002 AccessKeyNotBelongToOrganization](/doc/backend_error/access_key/30002.md) |
| The access key is still used by at least one project | 400 Bad Request | [30003 AccessKeyUsedInProject](/doc/backend_error/access_key/30003.md) |

## Example

Request: `N/A` (no body).

Success response (the `data` payload):

```json
{}
```
