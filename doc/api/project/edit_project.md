# PUT /v1/project/{projectId}

> Controller: ProjectController

Edit detail information of a project.

## Permission

[`PROJECT_WRITE`](/doc/permission/PROJECT_WRITE.md) — held by OWNER and ADMIN.

## Request

### Path Parameters

| Name | Type | Description |
| --- | --- | --- |
| `projectId` | Long | Id of the [project](/doc/tech_noun/TN0301_project.md) to edit. |

### Query Parameters

N/A

### Body

DTO: `ProjectEditRequest`

| Field | Type | Required | Description |
| --- | --- | --- | --- |
| `name` | String | ✅ | New display name of the project. |
| `indexDocument` | String | ✅ | Index document of the deployed site (e.g. `index.html`). |
| `errorDocument` | String | ✅ | Error document of the deployed site (e.g. `error.html`). |

## Response

DTO: `EmptyResponse`, returned inside the `PagerResponse` envelope as `data` — see the
[API index](/doc/api/README.md#response-envelope). When `indexDocument` or `errorDocument`
changes, the website configuration of the project's OSS
[bucket](/doc/tech_noun/TN0702_bucket.md) is updated as well.

| Field | Type | Description |
| --- | --- | --- |
| — | — | `data` is an empty object. |

## Errors

| Case | HTTP Status | Error Code |
| --- | --- | --- |
| The caller's role does not hold `PROJECT_WRITE` (DEVELOPER, CUSTOMER) | 400 | [20002 UserNotPermitted](/doc/backend_error/user/20002.md) |
| No project exists for `{projectId}` | 400 | [40001 ProjectIdNotExist](/doc/backend_error/project/40001.md) |
| The project belongs to another organization | 400 | [40003 ProjectNotBelongToOrganization](/doc/backend_error/project/40003.md) |
| The caller has no [privilege](/doc/tech_noun/TN0203_privilege.md) grant for the project | 400 | [40002 ProjectNoPrivilege](/doc/backend_error/project/40002.md) |
| The calling user's account row no longer exists (privilege check) | 400 | [20001 UserIdNotExist](/doc/backend_error/user/20001.md) |

A missing or invalid JWT is answered with `401 Unauthorized` (empty body) before the handler runs.

## Example

Request:

```json
{
  "name": "Corporate site",
  "indexDocument": "index.html",
  "errorDocument": "404.html"
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
