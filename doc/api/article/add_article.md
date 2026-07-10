# POST /v1/article

> Controller: ArticleController

Add a new [article](/doc/tech_noun/TN0501_article.md) into a [project](/doc/tech_noun/TN0301_project.md).

## Permission

[`ARTICLE_WRITE`](/doc/permission/ARTICLE_WRITE.md)

## Request

### Path Parameters

N/A

### Query Parameters

N/A

### Body

DTO: `ArticleAddRequest`

| Field | Type | Required | Description |
| --- | --- | --- | --- |
| `projectId` | Long | Yes | Physical id of the project the article is created in. |
| `title` | String | Yes | Article title. |
| `description` | String | Yes | Short article description. |
| `articleListIds` | Set\<Long\> | Yes | Ids of the [article lists](/doc/tech_noun/TN0502_article_list.md) the new article is attached to; may be empty. |
| `languageType` | LanguageType | No | [Language type](/doc/tech_noun/TN0302_language.md) of the article; when omitted the project's default language is used. |

The article is created with empty content and no cover; its revision is initialized from the
project's current maximum revision, and every referenced article list is touched (its update time
is refreshed).

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
| Caller does not exist | 400 Bad Request | [20001 UserIdNotExist](/doc/backend_error/user/20001.md) |
| Project does not exist | 400 Bad Request | [40001 ProjectIdNotExist](/doc/backend_error/project/40001.md) |
| Project belongs to another organization | 400 Bad Request | [40003 ProjectNotBelongToOrganization](/doc/backend_error/project/40003.md) |
| Caller has no privilege on the project | 400 Bad Request | [40002 ProjectNoPrivilege](/doc/backend_error/project/40002.md) |
| The requested language type does not exist in the project | 400 Bad Request | [50002 LanguageNotExist](/doc/backend_error/language/50002.md) |
| One or more referenced article lists do not belong to the project | 400 Bad Request | [70006 ArticleListProjectMismatching](/doc/backend_error/article_list/70006.md) |

## Example

Request:

```json
{
  "projectId": 12,
  "title": "公司新闻",
  "description": "最新的公司动态与新闻发布。",
  "articleListIds": [3, 5],
  "languageType": "SIMPLIFIED_CHINESE"
}
```

Success response (the `data` payload):

```json
{}
```
