# PATCH /v1/template/{templateId}/rename

> Controller: TemplateController

Rename a template.

## Permission

[`PROJECT_DEV`](/doc/permission/PROJECT_DEV.md) — held by OWNER, ADMIN, and DEVELOPER.

## Request

### Path Parameters

| Name | Type | Description |
| --- | --- | --- |
| `templateId` | Long | Id of the [template](/doc/tech_noun/TN0401_template.md) node to rename. `ROOT` and `RECYCLE_BIN` nodes cannot be renamed. |

### Query Parameters

N/A

### Body

DTO: `TemplateRenameRequest`

| Field | Type | Required | Description |
| --- | --- | --- | --- |
| `name` | String | ✅ | The new name. For a file, the new extension re-derives the `TemplateType`. The name is **not** checked for duplicates among the siblings (unlike [add_template.md](add_template.md)). |

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
| No template exists for `{templateId}` | 400 | [60001 TemplateIdNotExist](/doc/backend_error/template/60001.md) |
| The caller (DEVELOPER role) has no [privilege](/doc/tech_noun/TN0203_privilege.md) grant for the [project](/doc/tech_noun/TN0301_project.md) | 400 | [40002 ProjectNoPrivilege](/doc/backend_error/project/40002.md) |
| The calling user's account row no longer exists (privilege check) | 400 | [20001 UserIdNotExist](/doc/backend_error/user/20001.md) |
| The node is a `ROOT` or `RECYCLE_BIN` node | 400 | [60011 TemplateRenameReject](/doc/backend_error/template/60011.md) |

A missing or invalid JWT is answered with `401 Unauthorized` (empty body) before the handler runs.

## Example

Request:

```json
{
  "name": "news-v2.html"
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
