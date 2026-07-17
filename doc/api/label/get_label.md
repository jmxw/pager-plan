# GET /v1/label/{labelId}

> Controller: LabelController

Get detail information of a label.

**Stub endpoint** — the handler body is empty: nothing is looked up and nothing is returned
(the method has no return type, so `data` is an empty object). This doc records the declared
surface; it is extended together with the implementation (same-PR sync rule).

## Permission

[`LABEL_READ`](/doc/permission/LABEL_READ.md) — currently granted to **no role**, so every call
is rejected with [20002 UserNotPermitted](/doc/backend_error/user/20002.md) today.

## Request

### Path Parameters

| Name | Type | Description |
| --- | --- | --- |
| `labelId` | Long | Id of the [label](/doc/tech_noun/TN0303_label.md) to read. |

### Query Parameters

N/A

### Body

N/A

## Response

No response DTO is declared yet; the empty handler result is returned inside the `PagerResponse`
envelope as `data` — see the [API index](/doc/api/README.md#response-envelope).

| Field | Type | Description |
| --- | --- | --- |
| — | — | `data` is an empty object. |

## Errors

| Case | HTTP Status | Error Code |
| --- | --- | --- |
| The caller's role does not hold `LABEL_READ` (currently every role) | 400 | [20002 UserNotPermitted](/doc/backend_error/user/20002.md) |

A missing or invalid JWT is answered with `401 Unauthorized` (empty body) before the handler runs.

## Example

Request: N/A (no body)

Success response (unreachable until a role grants the permission):

```json
{
  "code": 200,
  "timestamp": 1752700000000,
  "data": {}
}
```
