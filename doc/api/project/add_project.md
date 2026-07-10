# POST /v1/project

> Controller: ProjectController

Creates a new [project](/doc/tech_noun/TN0301_project.md) under the caller's
[organization](/doc/tech_noun/TN0201_organization.md). A new OSS [bucket](/doc/tech_noun/TN0702_bucket.md)
named after the project identifier is provisioned in the chosen region using the referenced
[access key](/doc/tech_noun/TN0204_access_key.md), the bucket is configured for static website
hosting, and the project's root template, recycle-bin template, root navigation, and default
[language](/doc/tech_noun/TN0302_language.md) are created in the same transaction.

## Permission

[`PROJECT_WRITE`](/doc/permission/PROJECT_WRITE.md)

## Request

### Path Parameters

N/A

### Query Parameters

N/A

### Body

DTO: `ProjectAddRequest`

| Field | Type | Required | Description |
| --- | --- | --- | --- |
| `identifier` | String | Yes | Unique [identifier](/doc/tech_noun/TN0101_identifier.md) of the project; also used as the OSS bucket name. |
| `name` | String | Yes | Display name of the project. |
| `accessKeyId` | Long | Yes | Physical id of the access key used to provision the bucket; must belong to the caller's organization. |
| `region` | OssRegion | Yes | OSS region the bucket is created in (enum value, e.g. `CN_HANGZHOU`). See the available regions endpoint. |
| `defaultLanguageType` | LanguageType | Yes | The default language type for the project (enum value, e.g. `SIMPLIFIED_CHINESE`). |

## Response

DTO: `ProjectAddResponse` — wrapped as `data` inside the standard `PagerResponse` envelope
(`{ code, timestamp, data }`).

| Field | Type | Description |
| --- | --- | --- |
| `projectId` | Long | Physical id of the newly created project. |

## Errors

| Case | HTTP Status | Error Code |
| --- | --- | --- |
| Caller's role lacks the required permission | 400 Bad Request | [20002 UserNotPermitted](/doc/backend_error/user/20002.md) |
| A project with the same identifier already exists | 400 Bad Request | [40004 ProjectIdentifierExist](/doc/backend_error/project/40004.md) |
| No access key exists with the given id | 400 Bad Request | [30001 AccessKeyIdNotExist](/doc/backend_error/access_key/30001.md) |
| The access key belongs to a different organization than the caller's | 400 Bad Request | [30002 AccessKeyNotBelongToOrganization](/doc/backend_error/access_key/30002.md) |
| A bucket with the same identifier already exists (creation rejected) | 400 Bad Request | [40006 ProjectBucketExist](/doc/backend_error/project/40006.md) |
| Caller's organization does not exist | 400 Bad Request | [10001 OrganizationIdNotExist](/doc/backend_error/organization/10001.md) |

## Example

Request:

```json
{
  "identifier": "acme-site",
  "name": "公司官网",
  "accessKeyId": 7,
  "region": "CN_HANGZHOU",
  "defaultLanguageType": "SIMPLIFIED_CHINESE"
}
```

Success response (the `data` payload):

```json
{
  "projectId": 12
}
```
