# GET /v1/project

> Controller: ProjectController

Get all projects of an organization.

The projects returned are those owned by the caller's [organization](/doc/tech_noun/TN0201_organization.md),
ordered by creation time. The full list is returned only for callers whose
[role](/doc/tech_noun/TN0202_user.md) is `OWNER` or `ADMIN`; for other roles an empty list is
returned (per-user [project](/doc/tech_noun/TN0301_project.md) scoping is not yet implemented).

## Permission

[`PROJECT_READ`](/doc/permission/PROJECT_READ.md)

## Request

### Path Parameters

N/A

### Query Parameters

N/A

### Body

N/A

## Response

DTO: `ProjectListResponse` — wrapped as `data` inside the standard `PagerResponse` envelope
(`{ code, timestamp, data }`).

| Field | Type | Description |
| --- | --- | --- |
| `projects` | List\<ProjectEntity\> | The projects of the caller's organization, ordered by creation time. |

Each `ProjectEntity` has the following fields:

| Field | Type | Description |
| --- | --- | --- |
| `projectId` | Long | Physical id of the project. |
| `createAt` | Long | Creation timestamp (epoch milliseconds). |
| `updateAt` | Long | Last-update timestamp (epoch milliseconds). |
| `name` | String | Display name of the project. |
| `identifier` | String | Unique [identifier](/doc/tech_noun/TN0101_identifier.md) of the project (also the OSS bucket name). |
| `region` | RegionEntity | The OSS [region](/doc/tech_noun/TN0301_project.md) the project is hosted in (`value`, `regionName`, `regionId`). |
| `state` | ProjectState | Current lifecycle state of the project. |
| `stagingRevision` | Long | Latest staging [revision](/doc/tech_noun/TN0102_revision.md) number. |
| `productionRevision` | Long | Latest production revision number. |
| `rootTemplateId` | Long? | Id of the project's root [template](/doc/tech_noun/TN0401_template.md), or `null`. |
| `rootNavigationId` | Long? | Id of the project's root [navigation](/doc/tech_noun/TN0601_navigation.md), or `null`. |
| `indexDocument` | String | Bucket index document (home page path). |
| `errorDocument` | String | Bucket error document (error page path). |

## Errors

| Case | HTTP Status | Error Code |
| --- | --- | --- |
| Caller's role lacks the required permission | 400 Bad Request | [20002 UserNotPermitted](/doc/backend_error/user/20002.md) |
| Caller's organization does not exist | 400 Bad Request | [10001 OrganizationIdNotExist](/doc/backend_error/organization/10001.md) |

## Example

Request:

N/A

Success response (the `data` payload):

```json
{
  "projects": [
    {
      "projectId": 12,
      "createAt": 1704067200000,
      "updateAt": 1704153600000,
      "name": "公司官网",
      "identifier": "acme-site",
      "region": {
        "value": "CN_HANGZHOU",
        "regionName": "华东1（杭州）",
        "regionId": "oss-cn-hangzhou"
      },
      "state": "TEMPLATE_READY",
      "stagingRevision": 3,
      "productionRevision": 1,
      "rootTemplateId": 45,
      "rootNavigationId": 78,
      "indexDocument": "index.html",
      "errorDocument": "error.html"
    }
  ]
}
```
