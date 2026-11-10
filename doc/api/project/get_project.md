# GET /v1/project/{projectId}

> Controller: ProjectController

Get detail information of a project.

## Permission

[`PROJECT_READ`](/doc/permission/PROJECT_READ.md) — held by every role (OWNER, ADMIN, DEVELOPER,
CUSTOMER), so the permission check cannot fail for a valid login.

## Request

### Path Parameters

| Name | Type | Description |
| --- | --- | --- |
| `projectId` | Long | Id of the [project](/doc/tech_noun/TN0301_project.md) to read. |

### Query Parameters

N/A

### Body

N/A

## Response

DTO: `ProjectGetResponse`, returned inside the `PagerResponse` envelope as `data` — see the
[API index](/doc/api/README.md#response-envelope). The `ProjectEntity` fields below are the
canonical shape reused wherever other endpoint docs reference a `ProjectEntity`.

| Field | Type | Description |
| --- | --- | --- |
| `project.projectId` | Long | Id of the project. |
| `project.createAt` | Long | Creation timestamp (epoch milliseconds). |
| `project.updateAt` | Long | Last-update timestamp (epoch milliseconds). |
| `project.name` | String | Display name of the project. |
| `project.identifier` | String | The [identifier](/doc/tech_noun/TN0101_identifier.md) — names the project's OSS [buckets](/doc/tech_noun/TN0702_bucket.md) and hosts. |
| `project.region` | RegionEntity | The OSS region of the project — fields as in [get_project_available_regions.md](get_project_available_regions.md). |
| `project.state` | ProjectState | Lifecycle state: `INIT`, `ZIP_UPLOADING`, `ZIP_ANALYSING`, `TEMPLATE_READY`, `WAIT_FOR_DEPLOY`, `DEPLOYING`, or `DEPLOYED`. |
| `project.stagingRevision` | Long | The [revision](/doc/tech_noun/TN0102_revision.md) most recently deployed to the staging phase. |
| `project.productionRevision` | Long | The revision most recently deployed to the production phase. |
| `project.rootTemplateId` | Long (nullable) | Id of the `ROOT` node of the project's [template](/doc/tech_noun/TN0401_template.md) tree. |
| `project.rootNavigationId` | Long (nullable) | Id of the `ROOT` node of the project's [navigation](/doc/tech_noun/TN0601_navigation.md) tree. |
| `project.indexDocument` | String | Index document of the deployed site (default `index.html`). |
| `project.errorDocument` | String | Error document of the deployed site (default `error.html`). |

## Errors

| Case | HTTP Status | Error Code |
| --- | --- | --- |
| No project exists for `{projectId}` | 400 | [40001 ProjectIdNotExist](/doc/backend_error/project/40001.md) |
| The project belongs to another organization | 400 | [40003 ProjectNotBelongToOrganization](/doc/backend_error/project/40003.md) |
| The caller (DEVELOPER / CUSTOMER role) has no [privilege](/doc/tech_noun/TN0203_privilege.md) grant for the project | 400 | [40002 ProjectNoPrivilege](/doc/backend_error/project/40002.md) |
| The calling user's account row no longer exists (privilege check) | 400 | [20001 UserIdNotExist](/doc/backend_error/user/20001.md) |

A missing or invalid JWT is answered with `401 Unauthorized` (empty body) before the handler runs.

## Example

Request: N/A (no body)

Success response:

```json
{
  "code": 200,
  "timestamp": 1752700000000,
  "data": {
    "project": {
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
  }
}
```
