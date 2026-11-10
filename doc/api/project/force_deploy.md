# PATCH /v1/project/{projectId}/force_deploy/{phase}

> Controller: ProjectController

Force deploy a project to a phase.

## Permission

N/A (authenticated) — the method declares no `@Permitted`, so any logged-in role may call it. It
is listed among the [ungated endpoints](/doc/permission/README.md#ungated-endpoints); aligning it
with [`PROJECT_DEPLOY`](/doc/permission/PROJECT_DEPLOY.md) would be a backend change owned by a
future plan.

## Request

### Path Parameters

| Name | Type | Description |
| --- | --- | --- |
| `projectId` | Long | Id of the [project](/doc/tech_noun/TN0301_project.md) to force-deploy. |
| `phase` | DeployPhase | The target phase: `STAGING` or `PRODUCTION`. |

### Query Parameters

N/A

### Body

N/A

## Response

DTO: `ProjectDeployResponse`, returned inside the `PagerResponse` envelope as `data` — see the
[API index](/doc/api/README.md#response-envelope). Unlike [deploy.md](deploy.md), the running
[deploy tasks](/doc/tech_noun/TN0701_deploy_task.md) of the project are first marked
`INTERRUPTED_BY_ERRORS`, the phase [bucket](/doc/tech_noun/TN0702_bucket.md) is emptied, and the
phase [revision](/doc/tech_noun/TN0102_revision.md) is reset to `0` so the next run re-renders
everything; the `WAIT_FOR_DEPLOY` / `DEPLOYING` state checks are skipped. The response is
**always** zeroed — even when [precompiled errors](/doc/tech_noun/TN0704_precompiled_error.md)
block the deployment from being submitted, the controller discards the service result
(`errorTemplates` from the service never reaches the client; the actual outcome is observed via
[get_project.md](get_project.md) / [read_directory.md](/doc/api/template/read_directory.md)).

| Field | Type | Description |
| --- | --- | --- |
| `totalErrorCount` | Int | Always `0` (hard-coded in the controller). |
| `errorTemplateCount` | Int | Always `0` (hard-coded in the controller). |
| `errorTemplates` | List | Always empty (hard-coded in the controller). |

## Errors

| Case | HTTP Status | Error Code |
| --- | --- | --- |
| No project exists for `{projectId}` | 400 | [40001 ProjectIdNotExist](/doc/backend_error/project/40001.md) |
| The project belongs to another organization | 400 | [40003 ProjectNotBelongToOrganization](/doc/backend_error/project/40003.md) |
| The caller (DEVELOPER / CUSTOMER role) has no [privilege](/doc/tech_noun/TN0203_privilege.md) grant for the project | 400 | [40002 ProjectNoPrivilege](/doc/backend_error/project/40002.md) |
| The calling user's account row no longer exists | 400 | [20001 UserIdNotExist](/doc/backend_error/user/20001.md) |

A missing or invalid JWT is answered with `401 Unauthorized` (empty body) before the handler runs.

## Example

Request: N/A (no body)

Success response:

```json
{
  "code": 200,
  "timestamp": 1752700000000,
  "data": {
    "totalErrorCount": 0,
    "errorTemplateCount": 0,
    "errorTemplates": []
  }
}
```
