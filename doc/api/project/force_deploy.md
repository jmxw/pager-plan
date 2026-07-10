# PATCH /v1/project/{projectId}/force_deploy/{phase}

> Controller: ProjectController

Force-deploys a [project](/doc/tech_noun/TN0301_project.md) to the given
[phase](/doc/tech_noun/TN0701_deploy_task.md). Any in-flight deploy tasks are marked as interrupted,
all objects under the phase's [bucket](/doc/tech_noun/TN0702_bucket.md) are removed, the phase's
[revision](/doc/tech_noun/TN0102_revision.md) counter is reset to zero, and a fresh deploy is
submitted. The project must belong to the caller's
[organization](/doc/tech_noun/TN0201_organization.md) and the caller must hold a
[privilege](/doc/tech_noun/TN0203_privilege.md) on it. Regardless of the underlying result, the
controller always returns zero counts and an empty error-template list.

## Permission

`N/A (authenticated)` — login is required, but the controller method carries no `@Permitted`
annotation.

## Request

### Path Parameters

| Name | Type | Description |
| --- | --- | --- |
| `projectId` | Long | Physical id of the project to force-deploy. |
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
| `totalErrorCount` | Int | Always `0` for this endpoint. |
| `errorTemplateCount` | Int | Always `0` for this endpoint. |
| `errorTemplates` | List\<ErrorTemplateEntity\> | Always an empty list for this endpoint. |

## Errors

| Case | HTTP Status | Error Code |
| --- | --- | --- |
| No project exists with the given id | 400 Bad Request | [40001 ProjectIdNotExist](/doc/backend_error/project/40001.md) |
| The project belongs to a different organization than the caller's | 400 Bad Request | [40003 ProjectNotBelongToOrganization](/doc/backend_error/project/40003.md) |
| The caller holds no privilege on the project | 400 Bad Request | [40002 ProjectNoPrivilege](/doc/backend_error/project/40002.md) |
| Caller's user does not exist (deployer / privilege lookup) | 400 Bad Request | [20001 UserIdNotExist](/doc/backend_error/user/20001.md) |

## Example

Request:

N/A

Success response (the `data` payload):

```json
{
  "totalErrorCount": 0,
  "errorTemplateCount": 0,
  "errorTemplates": []
}
```
