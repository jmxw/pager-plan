# GET /v1/template/{templateId}/directory

> Controller: TemplateController

Get project, parent templates, child templates and the template it self.

## Permission

[`PROJECT_DEV`](/doc/permission/PROJECT_DEV.md) — held by OWNER, ADMIN, and DEVELOPER.

## Request

### Path Parameters

| Name | Type | Description |
| --- | --- | --- |
| `templateId` | Long | Id of the directory [template](/doc/tech_noun/TN0401_template.md) node to list. Must be a `ROOT` or `DIRECTORY` node. |

### Query Parameters

N/A

### Body

N/A

## Response

DTO: `TemplateChildrenResponse`, returned inside the `PagerResponse` envelope as `data` — see
the [API index](/doc/api/README.md#response-envelope). `parents` is the chain from the root down
to the direct parent; `children` lists directories first, then files, each group sorted by name.
The `TemplateEntity` fields below (as in `self`) are the canonical shape reused wherever other
endpoint docs reference a `TemplateEntity`.

| Field | Type | Description |
| --- | --- | --- |
| `project` | ProjectEntity | The owning [project](/doc/tech_noun/TN0301_project.md) — fields as in [get_project.md](/doc/api/project/get_project.md). |
| `errorCount` | Int | Sum of the children's [precompiled error](/doc/tech_noun/TN0704_precompiled_error.md) counts. |
| `self.templateId` | Long | Id of the template node. |
| `self.createAt` | Long | Creation timestamp (epoch milliseconds). |
| `self.updateAt` | Long | Last-update timestamp (epoch milliseconds). |
| `self.type` | TemplateType | `ROOT`, `RECYCLE_BIN`, `DIRECTORY`, `HTML`, `HTM`, `JS`, `CSS`, `XML`, `JPG`, `JPEG`, `PNG`, `GIF`, `MP3`, `MP4`, or `OTHER`. |
| `self.name` | String | Name of the node (filename for files). |
| `self.layer` | Int | Depth of the node in the tree (`0` for the root). |
| `self.errorCount` | Int | Number of precompiled errors of this node. |
| `parents[]` | TemplateEntity | Parent chain of the node (same fields as `self`). |
| `children[]` | TemplateEntity | Direct children of the node (same fields as `self`). |

## Errors

| Case | HTTP Status | Error Code |
| --- | --- | --- |
| The caller's role does not hold `PROJECT_DEV` (CUSTOMER) | 400 | [20002 UserNotPermitted](/doc/backend_error/user/20002.md) |
| No template exists for `{templateId}` | 400 | [60001 TemplateIdNotExist](/doc/backend_error/template/60001.md) |
| The caller (DEVELOPER role) has no [privilege](/doc/tech_noun/TN0203_privilege.md) grant for the project | 400 | [40002 ProjectNoPrivilege](/doc/backend_error/project/40002.md) |
| The calling user's account row no longer exists (privilege check) | 400 | [20001 UserIdNotExist](/doc/backend_error/user/20001.md) |
| The node is not a directory (`ROOT` / `DIRECTORY`) | 400 | [60007 TemplateDirectoryRequired](/doc/backend_error/template/60007.md) |

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
    },
    "errorCount": 0,
    "self": {
      "templateId": 100,
      "createAt": 1752500000000,
      "updateAt": 1752650000000,
      "type": "ROOT",
      "name": "corp-site",
      "layer": 0,
      "errorCount": 0
    },
    "parents": [],
    "children": [
      {
        "templateId": 110,
        "createAt": 1752600000000,
        "updateAt": 1752650000000,
        "type": "DIRECTORY",
        "name": "css",
        "layer": 1,
        "errorCount": 0
      },
      {
        "templateId": 120,
        "createAt": 1752600000000,
        "updateAt": 1752650000000,
        "type": "HTML",
        "name": "index.html",
        "layer": 1,
        "errorCount": 0
      }
    ]
  }
}
```
