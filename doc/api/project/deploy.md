# PATCH /v1/project/{projectId}/deploy/{phase}

> Controller: ProjectController

Deploy a project to a phase.

## Permission

[`PROJECT_DEPLOY`](/doc/permission/PROJECT_DEPLOY.md) — held by every role (OWNER, ADMIN,
DEVELOPER, CUSTOMER), so the permission check cannot fail for a valid login.

## Request

### Path Parameters

| Name | Type | Description |
| --- | --- | --- |
| `projectId` | Long | Id of the [project](/doc/tech_noun/TN0301_project.md) to deploy. |
| `phase` | DeployPhase | The target phase: `STAGING` or `PRODUCTION`. |

### Query Parameters

N/A

### Body

N/A

## Response

DTO: `ProjectDeployResponse`, returned inside the `PagerResponse` envelope as `data` — see the
[API index](/doc/api/README.md#response-envelope). Two success shapes exist:

- **Templates carry [precompiled errors](/doc/tech_noun/TN0704_precompiled_error.md)** — no
  deployment is started; the response lists the failing
  [templates](/doc/tech_noun/TN0401_template.md) and their errors.
- **No template errors** — a [deploy task](/doc/tech_noun/TN0701_deploy_task.md) is created,
  the project state moves to `WAIT_FOR_DEPLOY`, and a RabbitMQ message triggers the consumer,
  which renders and uploads the site **asynchronously**. All counts are `0` and `errorTemplates`
  is empty.

| Field | Type | Description |
| --- | --- | --- |
| `totalErrorCount` | Int | Sum of the precompiled errors across all failing templates; `0` when the deployment was submitted. |
| `errorTemplateCount` | Int | Number of failing templates; `0` when the deployment was submitted. |
| `errorTemplates[].template` | TemplateEntity | The failing template — fields as in [read_directory.md](/doc/api/template/read_directory.md). |
| `errorTemplates[].errorCount` | Int | Number of errors in this template. |
| `errorTemplates[].errors[]` | PagerErrorEntity | The template's precompiled errors — fields as in [write_file_content.md](/doc/api/template/write_file_content.md). |

## Errors

| Case | HTTP Status | Error Code |
| --- | --- | --- |
| No project exists for `{projectId}` | 400 | [40001 ProjectIdNotExist](/doc/backend_error/project/40001.md) |
| The project belongs to another organization | 400 | [40003 ProjectNotBelongToOrganization](/doc/backend_error/project/40003.md) |
| The caller (DEVELOPER / CUSTOMER role) has no [privilege](/doc/tech_noun/TN0203_privilege.md) grant for the project | 400 | [40002 ProjectNoPrivilege](/doc/backend_error/project/40002.md) |
| The calling user's account row no longer exists | 400 | [20001 UserIdNotExist](/doc/backend_error/user/20001.md) |
| The project is already in the `WAIT_FOR_DEPLOY` state | 400 | [40013 ProjectIsWaitForDeploy](/doc/backend_error/project/README.md) |
| The project is already in the `DEPLOYING` state | 400 | [40012 ProjectIsDeploying](/doc/backend_error/project/40012.md) |

The 40013 link points at the [Project error category](/doc/backend_error/project/README.md)
because `40013.md` cannot be written yet — two enum constants share that code (see the
"Duplicated codes" section of the [backend error index](/doc/backend_error/README.md)).

A missing or invalid JWT is answered with `401 Unauthorized` (empty body) before the handler runs.

## Example

Request: N/A (no body)

Success response (deployment blocked by one precompiled error):

```json
{
  "code": 200,
  "timestamp": 1752700000000,
  "data": {
    "totalErrorCount": 1,
    "errorTemplateCount": 1,
    "errorTemplates": [
      {
        "template": {
          "templateId": 120,
          "createAt": 1752600000000,
          "updateAt": 1752650000000,
          "type": "HTML",
          "name": "news.html",
          "layer": 2,
          "errorCount": 1
        },
        "errorCount": 1,
        "errors": [
          {
            "pagerErrorId": 9,
            "createAt": 1752650000000,
            "errorCode": "ARTICLE_LIST_NOT_FOUND",
            "tagType": "LIST",
            "lineNumber": 42
          }
        ]
      }
    ]
  }
}
```
