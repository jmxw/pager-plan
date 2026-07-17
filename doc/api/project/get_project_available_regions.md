# GET /v1/project/regions

> Controller: ProjectController

Get all available OSS regions for a project.

## Permission

[`PROJECT_READ`](/doc/permission/PROJECT_READ.md) — held by every role (OWNER, ADMIN, DEVELOPER,
CUSTOMER), so the permission check cannot fail for a valid login.

## Request

### Path Parameters

N/A

### Query Parameters

N/A

### Body

N/A

## Response

DTO: `ProjectRegionsResponse`, returned inside the `PagerResponse` envelope as `data` — see the
[API index](/doc/api/README.md#response-envelope). All values of the `OssRegion` enum
(`lib/oss`) are returned — the regions a [project](/doc/tech_noun/TN0301_project.md)'s
[buckets](/doc/tech_noun/TN0702_bucket.md) can be created in.

| Field | Type | Description |
| --- | --- | --- |
| `regions[].value` | String | The `OssRegion` enum constant name (e.g. `CN_HANGZHOU`) — the value sent as `region` in [add_project.md](add_project.md). |
| `regions[].regionName` | String | Display name of the region (Aliyun's Chinese region label, e.g. `华东1（杭州）`). |
| `regions[].regionId` | String | The Aliyun region id (e.g. `oss-cn-hangzhou`). |

## Errors

N/A — no business error is raised by this endpoint.

A missing or invalid JWT is answered with `401 Unauthorized` (empty body) before the handler runs.

## Example

Request: N/A (no body)

Success response (truncated to two of the 25 regions):

```json
{
  "code": 200,
  "timestamp": 1752700000000,
  "data": {
    "regions": [
      { "value": "CN_HANGZHOU", "regionName": "华东1（杭州）", "regionId": "oss-cn-hangzhou" },
      { "value": "AP_NORTHEAST_1", "regionName": "日本（东京）", "regionId": "oss-ap-northeast-1" }
    ]
  }
}
```
