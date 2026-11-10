# DELETE /v1/access_key/{accessKeyId}

> Controller: AccessKeyController

Delete an access key.

## Permission

[`ACCESS_KEY_WRITE`](/doc/permission/ACCESS_KEY_WRITE.md) — held by OWNER and ADMIN.

## Request

### Path Parameters

| Name | Type | Description |
| --- | --- | --- |
| `accessKeyId` | Long | Id of the [access key](/doc/tech_noun/TN0204_access_key.md) row to delete (**not** the Aliyun AccessKey ID string). |

### Query Parameters

N/A

### Body

N/A

## Response

DTO: `EmptyResponse`, returned inside the `PagerResponse` envelope as `data` — see the
[API index](/doc/api/README.md#response-envelope).

| Field | Type | Description |
| --- | --- | --- |
| — | — | `data` is an empty object. |

## Errors

| Case | HTTP Status | Error Code |
| --- | --- | --- |
| The caller's role does not hold `ACCESS_KEY_WRITE` (DEVELOPER, CUSTOMER) | 400 | [20002 UserNotPermitted](/doc/backend_error/user/20002.md) |
| No access key exists for `{accessKeyId}` | 400 | [30001 AccessKeyIdNotExist](/doc/backend_error/access_key/30001.md) |
| The access key belongs to another [organization](/doc/tech_noun/TN0201_organization.md) | 400 | [30002 AccessKeyNotBelongToOrganization](/doc/backend_error/access_key/30002.md) |
| The access key is still used by at least one [project](/doc/tech_noun/TN0301_project.md) | 400 | [30003 AccessKeyUsedInProject](/doc/backend_error/access_key/30003.md) |

A missing or invalid JWT is answered with `401 Unauthorized` (empty body) before the handler runs.

## Example

Request: N/A (no body)

Success response:

```json
{
  "code": 200,
  "timestamp": 1752700000000,
  "data": {}
}
```
