# LabelController

API docs for the endpoints of
[`LabelController.kt`](/source/pager-backend/app/cms/src/main/kotlin/com/xwkj/cms/controller/LabelController.kt).
One file per endpoint; see the parent [API index](../README.md) and
[rules/api_docs.md](../../../rules/api_docs.md).

All six endpoints are **stubs**: the controller has no service injected, the handler bodies are
empty, and no request DTO exists yet. In addition, no role currently grants
[`LABEL_READ`](/doc/permission/LABEL_READ.md) / [`LABEL_WRITE`](/doc/permission/LABEL_WRITE.md),
so every call is rejected by the permission check today. Each doc records the declared surface
and is extended together with the implementation (same-PR sync rule).

| Doc | Method | Path | Permission | Summary |
| --- | --- | --- | --- | --- |
| [add_label.md](add_label.md) | POST | `/v1/label` | `LABEL_WRITE` | Add a new label into a project. |
| [get_label.md](get_label.md) | GET | `/v1/label/{labelId}` | `LABEL_READ` | Get detail information of a label. |
| [edit_label.md](edit_label.md) | PUT | `/v1/label/{labelId}` | `LABEL_WRITE` | Edit a label. |
| [delete_label.md](delete_label.md) | DELETE | `/v1/label/{labelId}` | `LABEL_WRITE` | Delete a label. |
| [add_or_update_label_content.md](add_or_update_label_content.md) | POST | `/v1/label/{labelId}/content/{languageType}` | `LABEL_WRITE` | Add or update the content of a label for a language. |
| [delete_label_content.md](delete_label_content.md) | DELETE | `/v1/label/{labelId}/content/{languageType}` | `LABEL_WRITE` | Delete the content of a label for a language. |
