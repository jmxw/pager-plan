# PUT /v1/article_list/{articleListId}

> Controller: ArticleListController

Edit an article list.

Updates the name and/or bound [template](/doc/tech_noun/TN0401_template.md) of an existing
[article list](/doc/tech_noun/TN0502_article_list.md). Both body fields are optional; a `null` field
leaves the current value unchanged. The article list revision is bumped so the change is picked up
on the next deployment. When a new `templateId` is supplied it is resolved by id only — it is not
re-validated for HTML type or project membership on edit.

## Permission

[`ARTICLE_LIST_WRITE`](/doc/permission/ARTICLE_LIST_WRITE.md)

## Request

### Path Parameters

| Name | Type | Description |
| --- | --- | --- |
| `articleListId` | Long | Physical id of the article list to edit. |

### Query Parameters

N/A

### Body

DTO: `ArticleListEditRequest`

| Field | Type | Required | Description |
| --- | --- | --- | --- |
| `templateId` | Long? | No | New template id to bind. When `null`, the current template is kept. |
| `name` | String? | No | New display name. When `null`, the current name is kept. |

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
| No article list exists with the given id | 400 Bad Request | [70001 ArticleListIdNotExist](/doc/backend_error/article_list/70001.md) |
| Caller has no privilege on the article list's project | 400 Bad Request | [40002 ProjectNoPrivilege](/doc/backend_error/project/40002.md) |
| Caller's user record does not exist | 400 Bad Request | [20001 UserIdNotExist](/doc/backend_error/user/20001.md) |
| A new `templateId` was supplied but no such template exists | 400 Bad Request | [60001 TemplateIdNotExist](/doc/backend_error/template/60001.md) |

## Example

Request:

```json
{
  "templateId": 101,
  "name": "最新新闻"
}
```

Success response (the `data` payload):

```json
{}
```
