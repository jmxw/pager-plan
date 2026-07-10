# GET /v1/template/{templateId}/directory

> Controller: TemplateController

Get project, parent templates, child templates and the template it self.

Returns the directory [template](/doc/tech_noun/TN0401_template.md) identified by the path together
with its owning [project](/doc/tech_noun/TN0301_project.md), its ancestor templates, and its direct
children. Children are ordered directories first, then files, each group sorted by name. The
endpoint requires the target template to be a directory.

## Permission

[`PROJECT_DEV`](/doc/permission/PROJECT_DEV.md)

## Request

### Path Parameters

| Name | Type | Description |
| --- | --- | --- |
| `templateId` | Long | Physical id of the directory template to read. |

### Query Parameters

N/A

### Body

N/A

## Response

DTO: `TemplateChildrenResponse` — wrapped as `data` inside the standard `PagerResponse` envelope
(`{ code, timestamp, data }`).

| Field | Type | Description |
| --- | --- | --- |
| `project` | ProjectEntity | The project the template belongs to. |
| `errorCount` | Int | Total number of precompile errors across the direct children (sum of each child's `errorCount`). |
| `self` | TemplateEntity | The requested directory template. |
| `parents` | List\<TemplateEntity\> | Ancestor templates from the root down to the immediate parent. |
| `children` | List\<TemplateEntity\> | Direct child templates, directories first then files, each group sorted by name. |

Each `TemplateEntity` carries the following fields:

| Field | Type | Description |
| --- | --- | --- |
| `templateId` | Long | Physical id of the template. |
| `createAt` | Long | Creation timestamp in epoch milliseconds. |
| `updateAt` | Long | Last-update timestamp in epoch milliseconds. |
| `type` | TemplateType | Template type (e.g. `ROOT`, `RECYCLE_BIN`, `DIRECTORY`, `HTML`, `CSS`, `PNG`). |
| `name` | String | Template name. |
| `layer` | Int | Depth of the template in the tree. |
| `errorCount` | Int | Number of precompile errors detected in this template. |

The `project` field is a `ProjectEntity`:

| Field | Type | Description |
| --- | --- | --- |
| `projectId` | Long | Physical id of the project. |
| `createAt` | Long | Creation timestamp in epoch milliseconds. |
| `updateAt` | Long | Last-update timestamp in epoch milliseconds. |
| `name` | String | Display name of the project. |
| `identifier` | String | Unique identifier of the project. |
| `region` | RegionEntity | OSS region the project is hosted in. |
| `state` | ProjectState | Current project state. |
| `stagingRevision` | Long | Revision currently deployed to staging. |
| `productionRevision` | Long | Revision currently deployed to production. |
| `rootTemplateId` | Long? | Id of the project's root template, or `null`. |
| `rootNavigationId` | Long? | Id of the project's root navigation, or `null`. |
| `indexDocument` | String | Index document path served for the site. |
| `errorDocument` | String | Error document path served for the site. |

## Errors

| Case | HTTP Status | Error Code |
| --- | --- | --- |
| Caller's role lacks the required permission | 400 Bad Request | [20002 UserNotPermitted](/doc/backend_error/user/20002.md) |
| The template does not exist | 400 Bad Request | [60001 TemplateIdNotExist](/doc/backend_error/template/60001.md) |
| The caller resolved from the JWT does not exist | 400 Bad Request | [20001 UserIdNotExist](/doc/backend_error/user/20001.md) |
| The caller has no privilege on the template's project | 400 Bad Request | [40002 ProjectNoPrivilege](/doc/backend_error/project/40002.md) |
| The template is not a directory | 400 Bad Request | [60007 TemplateDirectoryRequired](/doc/backend_error/template/60007.md) |

## Example

Request: `N/A` (no body).

Success response (the `data` payload):

```json
{
  "project": {
    "projectId": 1,
    "createAt": 1710000000000,
    "updateAt": 1710000500000,
    "name": "官网",
    "identifier": "official-site",
    "region": { "value": "HANGZHOU", "regionName": "华东1（杭州）", "regionId": "oss-cn-hangzhou" },
    "state": "NORMAL",
    "stagingRevision": 12,
    "productionRevision": 10,
    "rootTemplateId": 100,
    "rootNavigationId": 200,
    "indexDocument": "index.html",
    "errorDocument": "error.html"
  },
  "errorCount": 1,
  "self": {
    "templateId": 100,
    "createAt": 1710000000000,
    "updateAt": 1710000000000,
    "type": "DIRECTORY",
    "name": "pages",
    "layer": 1,
    "errorCount": 0
  },
  "parents": [
    {
      "templateId": 1,
      "createAt": 1710000000000,
      "updateAt": 1710000000000,
      "type": "ROOT",
      "name": "root",
      "layer": 0,
      "errorCount": 0
    }
  ],
  "children": [
    {
      "templateId": 101,
      "createAt": 1710000000000,
      "updateAt": 1710000000000,
      "type": "DIRECTORY",
      "name": "partials",
      "layer": 2,
      "errorCount": 0
    },
    {
      "templateId": 102,
      "createAt": 1710000000000,
      "updateAt": 1710000100000,
      "type": "HTML",
      "name": "about.html",
      "layer": 2,
      "errorCount": 1
    }
  ]
}
```
