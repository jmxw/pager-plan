# PATCH /v1/template/{templateId}/content

> Controller: TemplateController

Update the text content of a text template file.

## Permission

[`PROJECT_DEV`](/doc/permission/PROJECT_DEV.md) — held by OWNER, ADMIN, and DEVELOPER.

## Request

### Path Parameters

| Name | Type | Description |
| --- | --- | --- |
| `templateId` | Long | Id of the text-file [template](/doc/tech_noun/TN0401_template.md) node to write (`HTML`, `HTM`, `CSS`, `JS`, or `XML`). |

### Query Parameters

N/A

### Body

DTO: `TemplateContentRequest`

| Field | Type | Required | Description |
| --- | --- | --- | --- |
| `content` | String | ✅ | The new text content. A missing [template file](/doc/tech_noun/TN0402_template_file.md) row is created. |

## Response

DTO: `TemplateWriteFileContentResponse`, returned inside the `PagerResponse` envelope as `data` —
see the [API index](/doc/api/README.md#response-envelope). After saving, the content is analysed
for [Pager tags](/doc/tech_noun/TN0403_pager_tag.md); the found
[precompiled errors](/doc/tech_noun/TN0704_precompiled_error.md) are stored on the node
(`errorCount`), block deployment (see [deploy.md](/doc/api/project/deploy.md)), and are returned.
The node's [revision](/doc/tech_noun/TN0102_revision.md) is incremented. The `PagerErrorEntity`
fields below are the canonical shape reused wherever other endpoint docs reference a
`PagerErrorEntity`.

| Field | Type | Description |
| --- | --- | --- |
| `pagerErrors[].pagerErrorId` | Long | Id of the stored error row. |
| `pagerErrors[].createAt` | Long | Timestamp the error was recorded (epoch milliseconds). |
| `pagerErrors[].errorCode` | PagerErrorCode | The precompile error kind: `LABEL_NOT_PAIRED`, `TOO_MANY_LABELS`, `START_LABEL_ERROR`, `END_LABEL_ERROR`, `PAIRED_LABELS_IN_SINGLE_LINE`, `ARTICLE_LIST_ID_REQUIRED`, `ARTICLE_LIST_NOT_FOUND`, `INT_VALUE_REQUIRED`, `INCLUDE_FILE_REQUIRED`, `INCLUDE_FILE_EMPTY`, `BOOLEAN_VALUE_REQUIRED`, or `UNKNOWN`. |
| `pagerErrors[].tagType` | TagType | The tag the error was found in: `LIST`, `PAGE_BAR`, `PAGE_LINK`, `INCLUDE`, `NAVIGATION_MENU`, or `NAVIGATION_LIST`. |
| `pagerErrors[].lineNumber` | Int | 1-based line of the error in the written content. |

## Errors

| Case | HTTP Status | Error Code |
| --- | --- | --- |
| The caller's role does not hold `PROJECT_DEV` (CUSTOMER) | 400 | [20002 UserNotPermitted](/doc/backend_error/user/20002.md) |
| No template exists for `{templateId}` | 400 | [60001 TemplateIdNotExist](/doc/backend_error/template/60001.md) |
| The caller (DEVELOPER role) has no [privilege](/doc/tech_noun/TN0203_privilege.md) grant for the [project](/doc/tech_noun/TN0301_project.md) | 400 | [40002 ProjectNoPrivilege](/doc/backend_error/project/40002.md) |
| The calling user's account row no longer exists (privilege check) | 400 | [20001 UserIdNotExist](/doc/backend_error/user/20001.md) |
| The node is not a text file (`HTML` / `HTM` / `CSS` / `JS` / `XML`) | 400 | [60004 TemplateNotTextFile](/doc/backend_error/template/60004.md) |

A missing or invalid JWT is answered with `401 Unauthorized` (empty body) before the handler runs.

## Example

Request:

```json
{
  "content": "<html>{pager:list id=\"news\" size=\"10\"}<li>[list:title]</li>{/pager:list}</html>"
}
```

Success response (one precompile error found):

```json
{
  "code": 200,
  "timestamp": 1752700000000,
  "data": {
    "pagerErrors": [
      {
        "pagerErrorId": 9,
        "createAt": 1752700000000,
        "errorCode": "ARTICLE_LIST_NOT_FOUND",
        "tagType": "LIST",
        "lineNumber": 1
      }
    ]
  }
}
```
