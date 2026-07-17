# POST /v1/label/{labelId}/content/{languageType}

> Controller: LabelController

Add or update the content of a label for a language.

**Stub endpoint** — the handler body is empty: no [label](/doc/tech_noun/TN0303_label.md) content
is written, no request DTO is declared, and no service is called. This doc records the declared
surface; it is extended together with the implementation (same-PR sync rule).

## Permission

[`LABEL_WRITE`](/doc/permission/LABEL_WRITE.md) — currently granted to **no role**, so every call
is rejected with [20002 UserNotPermitted](/doc/backend_error/user/20002.md) today.

## Request

### Path Parameters

| Name | Type | Description |
| --- | --- | --- |
| `labelId` | Long | Id of the [label](/doc/tech_noun/TN0303_label.md) whose per-language content is written. |
| `languageType` | LanguageType | The [language](/doc/tech_noun/TN0302_language.md) of the content (enum values: see [get_project_available_language_types.md](/doc/api/project/get_project_available_language_types.md)). |

### Query Parameters

N/A

### Body

N/A — no request DTO is declared yet.

## Response

DTO: `EmptyResponse`, returned inside the `PagerResponse` envelope as `data` — see the
[API index](/doc/api/README.md#response-envelope).

| Field | Type | Description |
| --- | --- | --- |
| — | — | `data` is an empty object. |

## Errors

| Case | HTTP Status | Error Code |
| --- | --- | --- |
| The caller's role does not hold `LABEL_WRITE` (currently every role) | 400 | [20002 UserNotPermitted](/doc/backend_error/user/20002.md) |

A missing or invalid JWT is answered with `401 Unauthorized` (empty body) before the handler runs.

## Example

Request: N/A (no body is declared yet)

Success response (unreachable until a role grants the permission):

```json
{
  "code": 200,
  "timestamp": 1752700000000,
  "data": {}
}
```
