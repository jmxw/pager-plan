# GET /v1/project/regions

> Controller: ProjectController

Lists the OSS [regions](/doc/tech_noun/TN0301_project.md) a [project](/doc/tech_noun/TN0301_project.md)
may be created in. The list is derived from the fixed set of supported Aliyun OSS regions and is not
organization-specific.

## Permission

[`PROJECT_READ`](/doc/permission/PROJECT_READ.md)

## Request

### Path Parameters

N/A

### Query Parameters

N/A

### Body

N/A

## Response

DTO: `ProjectRegionsResponse` — wrapped as `data` inside the standard `PagerResponse` envelope
(`{ code, timestamp, data }`).

| Field | Type | Description |
| --- | --- | --- |
| `regions` | List\<RegionEntity\> | All OSS regions available for project creation. |

Each `RegionEntity` has the following fields:

| Field | Type | Description |
| --- | --- | --- |
| `value` | String | The region enum value (e.g. `CN_HANGZHOU`); the value passed as `region` when creating a project. |
| `regionName` | String | Human-readable region name. |
| `regionId` | String | The Aliyun OSS region id (e.g. `oss-cn-hangzhou`). |

## Errors

| Case | HTTP Status | Error Code |
| --- | --- | --- |
| Caller's role lacks the required permission | 400 Bad Request | [20002 UserNotPermitted](/doc/backend_error/user/20002.md) |

## Example

Request:

N/A

Success response (the `data` payload):

```json
{
  "regions": [
    {
      "value": "CN_HANGZHOU",
      "regionName": "华东1（杭州）",
      "regionId": "oss-cn-hangzhou"
    },
    {
      "value": "US_WEST_1",
      "regionName": "美国（硅谷）",
      "regionId": "oss-us-west-1"
    }
  ]
}
```
