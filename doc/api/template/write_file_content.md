# PATCH /v1/template/{templateId}/content

> Controller: TemplateController

Update the text content of a text template file.

The text [template file](/doc/tech_noun/TN0402_template_file.md) content of the
[template](/doc/tech_noun/TN0401_template.md) identified by the path is replaced with the supplied
content, the template's revision is bumped, and the content is recompiled. Precompilation scans
[pager tags](/doc/tech_noun/TN0403_pager_tag.md) (only for `HTML`/`HTM` templates) and returns the
resulting [precompile errors](/doc/tech_noun/TN0704_precompiled_error.md); the template's stored
`errorCount` is updated to match. The endpoint requires the target template to be a text file.

## Permission

[`PROJECT_DEV`](/doc/permission/PROJECT_DEV.md)

## Request

### Path Parameters

| Name | Type | Description |
| --- | --- | --- |
| `templateId` | Long | Physical id of the text template file to update. |

### Query Parameters

N/A

### Body

DTO: `TemplateContentRequest`

| Field | Type | Required | Description |
| --- | --- | --- | --- |
| `content` | String | Yes | The new full text content of the template file. |

## Response

DTO: `TemplateWriteFileContentResponse` — wrapped as `data` inside the standard `PagerResponse`
envelope (`{ code, timestamp, data }`).

| Field | Type | Description |
| --- | --- | --- |
| `pagerErrors` | List\<PagerErrorEntity\> | Precompile errors detected in the saved content. Empty when the content compiles cleanly or the template is not an HTML file. |

Each `PagerErrorEntity` carries the following fields:

| Field | Type | Description |
| --- | --- | --- |
| `pagerErrorId` | Long | Physical id of the precompile error. |
| `createAt` | Long | Creation timestamp in epoch milliseconds. |
| `errorCode` | PagerErrorCode | The precompile error kind (e.g. `LABEL_NOT_PAIRED`, `ARTICLE_LIST_NOT_FOUND`, `INCLUDE_FILE_REQUIRED`). |
| `tagType` | TagType | The pager tag the error was found in (e.g. `list`, `pagebar`, `include`, `navmenu`). |
| `lineNumber` | Int | Line number of the offending tag within the content. |

## Errors

| Case | HTTP Status | Error Code |
| --- | --- | --- |
| Caller's role lacks the required permission | 400 Bad Request | [20002 UserNotPermitted](/doc/backend_error/user/20002.md) |
| The template does not exist | 400 Bad Request | [60001 TemplateIdNotExist](/doc/backend_error/template/60001.md) |
| The caller resolved from the JWT does not exist | 400 Bad Request | [20001 UserIdNotExist](/doc/backend_error/user/20001.md) |
| The caller has no privilege on the template's project | 400 Bad Request | [40002 ProjectNoPrivilege](/doc/backend_error/project/40002.md) |
| The template is not a text file | 400 Bad Request | [60004 TemplateNotTextFile](/doc/backend_error/template/60004.md) |

## Example

Request:

```json
{
  "content": "<html>\n  <body>{pager:list id=\"1\"}[list:title]{/pager:list}</body>\n</html>"
}
```

Success response (the `data` payload):

```json
{
  "pagerErrors": [
    {
      "pagerErrorId": 501,
      "createAt": 1710000600000,
      "errorCode": "ARTICLE_LIST_NOT_FOUND",
      "tagType": "LIST",
      "lineNumber": 2
    }
  ]
}
```
