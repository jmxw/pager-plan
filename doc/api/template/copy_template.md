# PATCH /v1/template/{templateId}/copy_to/{targetTemplateId}

> Controller: TemplateController

Copy a template and its children as a child of the target template.

**Not implemented** — the controller calls `TemplateService.copy`, whose body is a Kotlin
`TODO()`. Until it is implemented, every permitted call fails with `-9999 UnknownError` (see
Errors). This doc records the declared surface; it is extended together with the implementation
(same-PR sync rule).

## Permission

[`PROJECT_DEV`](/doc/permission/PROJECT_DEV.md) — held by OWNER, ADMIN, and DEVELOPER.

## Request

### Path Parameters

| Name | Type | Description |
| --- | --- | --- |
| `templateId` | Long | Id of the [template](/doc/tech_noun/TN0401_template.md) node to copy (with its subtree). |
| `targetTemplateId` | Long | Id of the template node that receives the copy as a child. |

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
| The caller's role does not hold `PROJECT_DEV` (CUSTOMER) | 400 | [20002 UserNotPermitted](/doc/backend_error/user/20002.md) |
| The service method is not implemented (`TODO()`) — currently every permitted call fails this way | 500 | [-9999 UnknownError](/doc/backend_error/unknown/-9999.md) |

A missing or invalid JWT is answered with `401 Unauthorized` (empty body) before the handler runs.

## Example

Request: N/A (no body)

Success response (unreachable until the service is implemented):

```json
{
  "code": 200,
  "timestamp": 1752700000000,
  "data": {}
}
```
