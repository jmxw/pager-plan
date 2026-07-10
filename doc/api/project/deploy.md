# PATCH /v1/project/{projectId}/deploy/{phase}

> Controller: ProjectController

Deploys a [project](/doc/tech_noun/TN0301_project.md) to the given
[phase](/doc/tech_noun/TN0701_deploy_task.md). If any [template](/doc/tech_noun/TN0401_template.md)
still carries [precompile errors](/doc/tech_noun/TN0704_precompiled_error.md), the deploy is not
started and those error templates are returned so they can be fixed first. Otherwise a deploy task is
created, the project moves to its `WAIT_FOR_DEPLOY` state, a deployment message is published, and an
empty error list is returned. The project must belong to the caller's
[organization](/doc/tech_noun/TN0201_organization.md) and the caller must hold a
[privilege](/doc/tech_noun/TN0203_privilege.md) on it.

## Permission

[`PROJECT_DEPLOY`](/doc/permission/PROJECT_DEPLOY.md)

## Request

### Path Parameters

| Name | Type | Description |
| --- | --- | --- |
| `projectId` | Long | Physical id of the project to deploy. |
| `phase` | DeployPhase | The deploy phase, one of `STAGING` or `PRODUCTION`. |

### Query Parameters

N/A

### Body

N/A

## Response

DTO: `ProjectDeployResponse` — wrapped as `data` inside the standard `PagerResponse` envelope
(`{ code, timestamp, data }`).

| Field | Type | Description |
| --- | --- | --- |
| `totalErrorCount` | Int | Total number of precompile errors across all error templates (sum of each template's `errorCount`). Zero when the deploy was submitted. |
| `errorTemplateCount` | Int | Number of templates carrying at least one precompile error. Zero when the deploy was submitted. |
| `errorTemplates` | List\<ErrorTemplateEntity\> | The templates blocking the deploy; empty when the deploy was submitted. |

Each `ErrorTemplateEntity` has the following fields:

| Field | Type | Description |
| --- | --- | --- |
| `template` | TemplateEntity | The offending template (`templateId`, `createAt`, `updateAt`, `type`, `name`, `layer`, `errorCount`). |
| `errorCount` | Int | Number of precompile errors on this template. |
| `errors` | List\<PagerErrorEntity\> | The individual precompile errors on this template. |

Each `PagerErrorEntity` has the following fields:

| Field | Type | Description |
| --- | --- | --- |
| `pagerErrorId` | Long | Physical id of the precompile error. |
| `createAt` | Long | Creation timestamp (epoch milliseconds). |
| `errorCode` | PagerErrorCode | The precompile error code (enum value). |
| `tagType` | TagType | The pager tag type the error was found on (enum value). |
| `lineNumber` | Int | Line number of the offending tag in the template file. |

## Errors

| Case | HTTP Status | Error Code |
| --- | --- | --- |
| Caller's role lacks the required permission | 400 Bad Request | [20002 UserNotPermitted](/doc/backend_error/user/20002.md) |
| No project exists with the given id | 400 Bad Request | [40001 ProjectIdNotExist](/doc/backend_error/project/40001.md) |
| The project belongs to a different organization than the caller's | 400 Bad Request | [40003 ProjectNotBelongToOrganization](/doc/backend_error/project/40003.md) |
| The caller holds no privilege on the project | 400 Bad Request | [40002 ProjectNoPrivilege](/doc/backend_error/project/40002.md) |
| Caller's user does not exist (deployer / privilege lookup) | 400 Bad Request | [20001 UserIdNotExist](/doc/backend_error/user/20001.md) |
| The project is already waiting for a deploy (`WAIT_FOR_DEPLOY`) | 400 Bad Request | `40013 ProjectIsWaitForDeploy` (no doc — code collision, see backend_error index) |
| The project is currently deploying (`DEPLOYING`) | 400 Bad Request | [40012 ProjectIsDeploying](/doc/backend_error/project/40012.md) |

## Example

Request:

N/A

Success response (the `data` payload) when the deploy was submitted (no precompile errors):

```json
{
  "totalErrorCount": 0,
  "errorTemplateCount": 0,
  "errorTemplates": []
}
```

Success response (the `data` payload) when the deploy is blocked by precompile errors:

```json
{
  "totalErrorCount": 2,
  "errorTemplateCount": 1,
  "errorTemplates": [
    {
      "template": {
        "templateId": 45,
        "createAt": 1704067200000,
        "updateAt": 1704153600000,
        "type": "ROOT",
        "name": "acme-site",
        "layer": 0,
        "errorCount": 2
      },
      "errorCount": 2,
      "errors": [
        {
          "pagerErrorId": 901,
          "createAt": 1704153600000,
          "errorCode": "ARTICLE_LIST_NOT_FOUND",
          "tagType": "LIST",
          "lineNumber": 12
        },
        {
          "pagerErrorId": 902,
          "createAt": 1704153600000,
          "errorCode": "INCLUDE_FILE_REQUIRED",
          "tagType": "INCLUDE",
          "lineNumber": 30
        }
      ]
    }
  ]
}
```
