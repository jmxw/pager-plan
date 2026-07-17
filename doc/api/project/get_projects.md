# GET /v1/project

> Controller: ProjectController

Get all projects of an organization.

## Permission

[`PROJECT_READ`](/doc/permission/PROJECT_READ.md) — held by every role (OWNER, ADMIN, DEVELOPER,
CUSTOMER), so the permission check cannot fail for a valid login. Note that the service currently
returns projects only for OWNER and ADMIN callers; a DEVELOPER or CUSTOMER receives an **empty
list** (per-[privilege](/doc/tech_noun/TN0203_privilege.md) filtering is a `TODO` in
`ProjectService.getByPage`).

## Request

### Path Parameters

N/A

### Query Parameters

N/A

### Body

N/A

## Response

DTO: `ProjectListResponse`, returned inside the `PagerResponse` envelope as `data` — see the
[API index](/doc/api/README.md#response-envelope). All
[projects](/doc/tech_noun/TN0301_project.md) of the caller's
[organization](/doc/tech_noun/TN0201_organization.md) are returned, ordered by creation time.

| Field | Type | Description |
| --- | --- | --- |
| `projects[]` | ProjectEntity | The organization's projects — fields as in [get_project.md](get_project.md). |

## Errors

| Case | HTTP Status | Error Code |
| --- | --- | --- |
| The organization carried by the JWT no longer exists | 400 | [10001 OrganizationIdNotExist](/doc/backend_error/organization/10001.md) |

A missing or invalid JWT is answered with `401 Unauthorized` (empty body) before the handler runs.

## Example

Request: N/A (no body)

Success response:

```json
{
  "code": 200,
  "timestamp": 1752700000000,
  "data": {
    "projects": [
      {
        "projectId": 3,
        "createAt": 1752500000000,
        "updateAt": 1752650000000,
        "name": "Corporate site",
        "identifier": "corp-site",
        "region": { "value": "CN_HANGZHOU", "regionName": "华东1（杭州）", "regionId": "oss-cn-hangzhou" },
        "state": "TEMPLATE_READY",
        "stagingRevision": 4,
        "productionRevision": 2,
        "rootTemplateId": 100,
        "rootNavigationId": 200,
        "indexDocument": "index.html",
        "errorDocument": "error.html"
      }
    ]
  }
}
```
