# PUT /v1/access_key/{accessKeyId}

> Controller: AccessKeyController

Edit an access key.

## Permission

N/A (authenticated) — the method declares no `@Permitted`, so any logged-in role may call it. It
is listed among the [ungated endpoints](/doc/permission/README.md#ungated-endpoints); tightening
it to `ACCESS_KEY_WRITE` would be a backend change owned by a future plan.

## Request

### Path Parameters

| Name | Type | Description |
| --- | --- | --- |
| `accessKeyId` | Long | Id of the [access key](/doc/tech_noun/TN0204_access_key.md) row to edit (**not** the Aliyun AccessKey ID string). |

### Query Parameters

N/A

### Body

DTO: `AccessKeyUpsertRequest`

| Field | Type | Required | Description |
| --- | --- | --- | --- |
| `name` | String | ✅ | Present in the DTO but **ignored** by the edit path — the stored name is not changed. |
| `accessKeyId` | String | ✅ | The new Aliyun AccessKey ID; stored encrypted. |
| `accessKeySecret` | String | ✅ | The new Aliyun AccessKey secret; stored encrypted. |

## Response

DTO: `EmptyResponse`, returned inside the `PagerResponse` envelope as `data` — see the
[API index](/doc/api/README.md#response-envelope).

| Field | Type | Description |
| --- | --- | --- |
| — | — | `data` is an empty object. |

## Errors

| Case | HTTP Status | Error Code |
| --- | --- | --- |
| No access key exists for `{accessKeyId}` | 400 | [30001 AccessKeyIdNotExist](/doc/backend_error/access_key/30001.md) |
| The access key belongs to another [organization](/doc/tech_noun/TN0201_organization.md) | 400 | [30002 AccessKeyNotBelongToOrganization](/doc/backend_error/access_key/30002.md) |

A missing or invalid JWT is answered with `401 Unauthorized` (empty body) before the handler runs.

## Example

Request:

```json
{
  "name": "aliyun main account",
  "accessKeyId": "LTAI5tNewId",
  "accessKeySecret": "Ab3dNewSecret"
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
