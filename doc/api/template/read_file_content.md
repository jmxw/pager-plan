# GET /v1/template/{templateId}/content

> Controller: TemplateController

Get the text content of a text template file.

## Permission

[`PROJECT_DEV`](/doc/permission/PROJECT_DEV.md) — held by OWNER, ADMIN, and DEVELOPER.

## Request

### Path Parameters

| Name | Type | Description |
| --- | --- | --- |
| `templateId` | Long | Id of the text-file [template](/doc/tech_noun/TN0401_template.md) node to read (`HTML`, `HTM`, `CSS`, `JS`, or `XML`). |

### Query Parameters

N/A

### Body

N/A

## Response

DTO: `TemplateContentResponse`, returned inside the `PagerResponse` envelope as `data` — see the
[API index](/doc/api/README.md#response-envelope). When the node has no
[template file](/doc/tech_noun/TN0402_template_file.md) row yet (a file created via
[add_template.md](add_template.md) but never written), `content` is the empty string.

| Field | Type | Description |
| --- | --- | --- |
| `project` | ProjectEntity | The owning [project](/doc/tech_noun/TN0301_project.md) — fields as in [get_project.md](/doc/api/project/get_project.md). |
| `parents[]` | TemplateEntity | Parent chain of the node — fields as in [read_directory.md](read_directory.md). |
| `self` | TemplateEntity | The read node itself. |
| `content` | String | The stored text content. |

## Errors

| Case | HTTP Status | Error Code |
| --- | --- | --- |
| The caller's role does not hold `PROJECT_DEV` (CUSTOMER) | 400 | [20002 UserNotPermitted](/doc/backend_error/user/20002.md) |
| No template exists for `{templateId}` | 400 | [60001 TemplateIdNotExist](/doc/backend_error/template/60001.md) |
| The caller (DEVELOPER role) has no [privilege](/doc/tech_noun/TN0203_privilege.md) grant for the project | 400 | [40002 ProjectNoPrivilege](/doc/backend_error/project/40002.md) |
| The calling user's account row no longer exists (privilege check) | 400 | [20001 UserIdNotExist](/doc/backend_error/user/20001.md) |
| The node is not a text file (`HTML` / `HTM` / `CSS` / `JS` / `XML`) | 400 | [60004 TemplateNotTextFile](/doc/backend_error/template/60004.md) |

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
    "parents": [
      {
        "templateId": 100,
        "createAt": 1752500000000,
        "updateAt": 1752650000000,
        "type": "ROOT",
        "name": "corp-site",
        "layer": 0,
        "errorCount": 0
      }
    ],
    "self": {
      "templateId": 120,
      "createAt": 1752600000000,
      "updateAt": 1752650000000,
      "type": "HTML",
      "name": "index.html",
      "layer": 1,
      "errorCount": 0
    },
    "content": "<html>...</html>"
  }
}
```
