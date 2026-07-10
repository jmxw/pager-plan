# GET /v1/template/{templateId}/content

> Controller: TemplateController

Get the text content of a text template file.

Returns the text [template file](/doc/tech_noun/TN0402_template_file.md) content of the
[template](/doc/tech_noun/TN0401_template.md) identified by the path, along with its owning
[project](/doc/tech_noun/TN0301_project.md) and ancestor templates. The endpoint requires the
target template to be a text file (`HTML`, `HTM`, `CSS`, `JS`, or `XML`). When no content has been
written yet, `content` is an empty string.

## Permission

[`PROJECT_DEV`](/doc/permission/PROJECT_DEV.md)

## Request

### Path Parameters

| Name | Type | Description |
| --- | --- | --- |
| `templateId` | Long | Physical id of the text template file to read. |

### Query Parameters

N/A

### Body

N/A

## Response

DTO: `TemplateContentResponse` — wrapped as `data` inside the standard `PagerResponse` envelope
(`{ code, timestamp, data }`).

| Field | Type | Description |
| --- | --- | --- |
| `project` | ProjectEntity | The project the template belongs to. See [read_directory](/doc/api/template/read_directory.md) for the `ProjectEntity` field breakdown. |
| `parents` | List\<TemplateEntity\> | Ancestor templates from the root down to the immediate parent. |
| `self` | TemplateEntity | The requested text template file. |
| `content` | String | The text content of the template file, or an empty string when no content has been written yet. |

Each `TemplateEntity` carries the following fields:

| Field | Type | Description |
| --- | --- | --- |
| `templateId` | Long | Physical id of the template. |
| `createAt` | Long | Creation timestamp in epoch milliseconds. |
| `updateAt` | Long | Last-update timestamp in epoch milliseconds. |
| `type` | TemplateType | Template type (e.g. `HTML`, `CSS`, `JS`, `XML`). |
| `name` | String | Template name. |
| `layer` | Int | Depth of the template in the tree. |
| `errorCount` | Int | Number of precompile errors detected in this template. |

## Errors

| Case | HTTP Status | Error Code |
| --- | --- | --- |
| Caller's role lacks the required permission | 400 Bad Request | [20002 UserNotPermitted](/doc/backend_error/user/20002.md) |
| The template does not exist | 400 Bad Request | [60001 TemplateIdNotExist](/doc/backend_error/template/60001.md) |
| The caller resolved from the JWT does not exist | 400 Bad Request | [20001 UserIdNotExist](/doc/backend_error/user/20001.md) |
| The caller has no privilege on the template's project | 400 Bad Request | [40002 ProjectNoPrivilege](/doc/backend_error/project/40002.md) |
| The template is not a text file | 400 Bad Request | [60004 TemplateNotTextFile](/doc/backend_error/template/60004.md) |

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
  "parents": [
    {
      "templateId": 100,
      "createAt": 1710000000000,
      "updateAt": 1710000000000,
      "type": "DIRECTORY",
      "name": "pages",
      "layer": 1,
      "errorCount": 0
    }
  ],
  "self": {
    "templateId": 102,
    "createAt": 1710000000000,
    "updateAt": 1710000100000,
    "type": "HTML",
    "name": "about.html",
    "layer": 2,
    "errorCount": 0
  },
  "content": "<html>\n  <body>关于我们</body>\n</html>"
}
```
