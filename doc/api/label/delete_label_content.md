# DELETE /v1/label/{labelId}/content/{languageType}

> Controller: LabelController

> Status: not yet implemented (controller method is a stub).

The content of a [label](/doc/tech_noun/TN0303_label.md) for a given
[language type](/doc/tech_noun/TN0302_language.md) is deleted.

## Permission

[`LABEL_WRITE`](/doc/permission/LABEL_WRITE.md) — the exact `@Permitted(UserPermission.LABEL_WRITE)` on the method.

## Request

### Path Parameters

| Name | Type | Description |
| --- | --- | --- |
| `labelId` | Long | Identifies the label whose content is removed. |
| `languageType` | LanguageType | The language type of the content to remove (enum value bound from the path). |

### Query Parameters

N/A

### Body

N/A

## Response

DTO: `EmptyResponse` — wrapped as `data` inside the standard `PagerResponse` envelope
(`{ code, timestamp, data }`). The `data` payload is an empty object `{}`.

## Errors

N/A — no service call yet; only the `@Permitted` gate is applied.

## Example

Request:

N/A

Success response (the `data` payload):

```json
{ }
```
