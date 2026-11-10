# PATCH /v1/template/{templateId}/move_to/{targetTemplateId}

> Controller: TemplateController

Move a template and its children as a child of the target template.

## Permission

[`PROJECT_DEV`](/doc/permission/PROJECT_DEV.md) — held by OWNER, ADMIN, and DEVELOPER.

## Request

### Path Parameters

| Name | Type | Description |
| --- | --- | --- |
| `templateId` | Long | Id of the [template](/doc/tech_noun/TN0401_template.md) node to move (with its subtree). |
| `targetTemplateId` | Long | Id of the directory node that becomes the new parent. Must belong to the same [project](/doc/tech_noun/TN0301_project.md). |

### Query Parameters

N/A

### Body

N/A

## Response

DTO: `EmptyResponse`, returned inside the `PagerResponse` envelope as `data` — see the
[API index](/doc/api/README.md#response-envelope). The node is re-parented under the target and
the `layer` of the node and its whole subtree is recomputed.

| Field | Type | Description |
| --- | --- | --- |
| — | — | `data` is an empty object. |

## Errors

| Case | HTTP Status | Error Code |
| --- | --- | --- |
| The caller's role does not hold `PROJECT_DEV` (CUSTOMER) | 400 | [20002 UserNotPermitted](/doc/backend_error/user/20002.md) |
| No template exists for `{templateId}` or `{targetTemplateId}` | 400 | [60001 TemplateIdNotExist](/doc/backend_error/template/60001.md) |
| The caller (DEVELOPER role) has no [privilege](/doc/tech_noun/TN0203_privilege.md) grant for the project | 400 | [40002 ProjectNoPrivilege](/doc/backend_error/project/40002.md) |
| The calling user's account row no longer exists (privilege check) | 400 | [20001 UserIdNotExist](/doc/backend_error/user/20001.md) |
| The target node is a file (a directory is required) | 400 | [60007 TemplateDirectoryRequired](/doc/backend_error/template/60007.md) |
| The node and the target belong to different projects | 400 | [60012 TemplateSameProjectRequired](/doc/backend_error/template/60012.md) |

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
