# PUT /v1/project/{projectId}

> Controller: ProjectController

Updates a [project](/doc/tech_noun/TN0301_project.md)'s display name and its OSS
[bucket](/doc/tech_noun/TN0702_bucket.md) index and error documents. When either document changes,
the bucket's static-website configuration is updated accordingly. The project must belong to the
caller's [organization](/doc/tech_noun/TN0201_organization.md) and the caller must hold a
[privilege](/doc/tech_noun/TN0203_privilege.md) on it.

## Permission

[`PROJECT_WRITE`](/doc/permission/PROJECT_WRITE.md)

## Request

### Path Parameters

| Name | Type | Description |
| --- | --- | --- |
| `projectId` | Long | Physical id of the project to update. |

### Query Parameters

N/A

### Body

DTO: `ProjectEditRequest`

| Field | Type | Required | Description |
| --- | --- | --- | --- |
| `name` | String | Yes | New display name of the project. |
| `indexDocument` | String | Yes | Bucket index document (home page path). |
| `errorDocument` | String | Yes | Bucket error document (error page path). |

## Response

DTO: `EmptyResponse` — wrapped as `data` inside the standard `PagerResponse` envelope
(`{ code, timestamp, data }`). The `data` body is empty (`{}`).

| Field | Type | Description |
| --- | --- | --- |
| — | — | The response body is empty (`{}`). |

## Errors

| Case | HTTP Status | Error Code |
| --- | --- | --- |
| Caller's role lacks the required permission | 400 Bad Request | [20002 UserNotPermitted](/doc/backend_error/user/20002.md) |
| No project exists with the given id | 400 Bad Request | [40001 ProjectIdNotExist](/doc/backend_error/project/40001.md) |
| The project belongs to a different organization than the caller's | 400 Bad Request | [40003 ProjectNotBelongToOrganization](/doc/backend_error/project/40003.md) |
| The caller holds no privilege on the project | 400 Bad Request | [40002 ProjectNoPrivilege](/doc/backend_error/project/40002.md) |
| Caller's user does not exist (privilege lookup) | 400 Bad Request | [20001 UserIdNotExist](/doc/backend_error/user/20001.md) |

## Example

Request:

```json
{
  "name": "公司官网",
  "indexDocument": "index.html",
  "errorDocument": "error.html"
}
```

Success response (the `data` payload):

```json
{}
```
