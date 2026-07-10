# GET /v1/project/{projectId}

> Controller: ProjectController

Returns a single [project](/doc/tech_noun/TN0301_project.md) by id. The project must belong to the
caller's [organization](/doc/tech_noun/TN0201_organization.md), and the caller must hold a
[privilege](/doc/tech_noun/TN0203_privilege.md) on it (owners and admins are granted access
implicitly).

## Permission

[`PROJECT_READ`](/doc/permission/PROJECT_READ.md)

## Request

### Path Parameters

| Name | Type | Description |
| --- | --- | --- |
| `projectId` | Long | Physical id of the project to fetch. |

### Query Parameters

N/A

### Body

N/A

## Response

DTO: `ProjectGetResponse` — wrapped as `data` inside the standard `PagerResponse` envelope
(`{ code, timestamp, data }`).

| Field | Type | Description |
| --- | --- | --- |
| `project` | ProjectEntity | The requested project. |

The `ProjectEntity` has the following fields:

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
| No project exists with the given id | 400 Bad Request | [40001 ProjectIdNotExist](/doc/backend_error/project/40001.md) |
| The project belongs to a different organization than the caller's | 400 Bad Request | [40003 ProjectNotBelongToOrganization](/doc/backend_error/project/40003.md) |
| The caller holds no privilege on the project | 400 Bad Request | [40002 ProjectNoPrivilege](/doc/backend_error/project/40002.md) |
| Caller's user does not exist (privilege lookup) | 400 Bad Request | [20001 UserIdNotExist](/doc/backend_error/user/20001.md) |

## Example

Request:

N/A

Success response (the `data` payload):

```json
{
  "project": {
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
}
```
