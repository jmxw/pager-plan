# POST /v1/project

> Controller: ProjectController

Add a new project into an organization.

## Permission

[`PROJECT_WRITE`](/doc/permission/PROJECT_WRITE.md) — held by OWNER and ADMIN.

## Request

### Path Parameters

N/A

### Query Parameters

N/A

### Body

DTO: `ProjectAddRequest`

| Field | Type | Required | Description |
| --- | --- | --- | --- |
| `identifier` | String | ✅ | The [identifier](/doc/tech_noun/TN0101_identifier.md) of the new [project](/doc/tech_noun/TN0301_project.md); globally unique — it names the project's OSS [buckets](/doc/tech_noun/TN0702_bucket.md) and hosts. |
| `region` | OssRegion | ✅ | The Aliyun OSS region the project's buckets are created in (enum values: see [get_project_available_regions.md](get_project_available_regions.md), field `value`). |
| `name` | String | ✅ | Display name of the project. |
| `accessKeyId` | Long | ✅ | Id of the [access key](/doc/tech_noun/TN0204_access_key.md) row the project deploys through (**not** the Aliyun AccessKey ID string). |
| `defaultLanguageType` | LanguageType | ✅ | The default [language](/doc/tech_noun/TN0302_language.md) of the project (enum values: see [get_project_available_language_types.md](get_project_available_language_types.md), field `value`). |

## Response

DTO: `ProjectAddResponse`, returned inside the `PagerResponse` envelope as `data` — see the
[API index](/doc/api/README.md#response-envelope). Besides the project row, the OSS bucket, the
`ROOT` and `RECYCLE_BIN` [templates](/doc/tech_noun/TN0401_template.md), the root
[navigation](/doc/tech_noun/TN0601_navigation.md), and the default language are created.

| Field | Type | Description |
| --- | --- | --- |
| `projectId` | Long | Id of the new project. |

## Errors

| Case | HTTP Status | Error Code |
| --- | --- | --- |
| The caller's role does not hold `PROJECT_WRITE` (DEVELOPER, CUSTOMER) | 400 | [20002 UserNotPermitted](/doc/backend_error/user/20002.md) |
| `identifier` is already used by another project | 400 | [40004 ProjectIdentifierExist](/doc/backend_error/project/40004.md) |
| No access key exists for `accessKeyId` | 400 | [30001 AccessKeyIdNotExist](/doc/backend_error/access_key/30001.md) |
| The access key belongs to another organization | 400 | [30002 AccessKeyNotBelongToOrganization](/doc/backend_error/access_key/30002.md) |
| An OSS bucket named after `identifier` already exists | 400 | [40006 ProjectBucketExist](/doc/backend_error/project/40006.md) |
| The organization carried by the JWT no longer exists | 400 | [10001 OrganizationIdNotExist](/doc/backend_error/organization/10001.md) |

A missing or invalid JWT is answered with `401 Unauthorized` (empty body) before the handler runs.

## Example

Request:

```json
{
  "identifier": "corp-site",
  "region": "CN_HANGZHOU",
  "name": "Corporate site",
  "accessKeyId": 1,
  "defaultLanguageType": "SIMPLIFIED_CHINESE"
}
```

Success response:

```json
{
  "code": 200,
  "timestamp": 1752700000000,
  "data": {
    "projectId": 3
  }
}
```
