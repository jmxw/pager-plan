# Label (LabelController)

Apis to manage labels of a project.

All six endpoints are currently stubs (empty controller bodies with no service call);
each is marked `Status: not yet implemented` in its own doc. They are gated by
`LABEL_READ` / `LABEL_WRITE`, neither of which is granted to any role yet
(see [permission overview](/doc/permission/README.md)), so these endpoints are effectively
unreachable today.

## Endpoints

| Method | Path | Summary | Doc |
| --- | --- | --- | --- |
| POST | `/v1/label` | — | [add_label.md](/doc/api/label/add_label.md) |
| GET | `/v1/label/{labelId}` | — | [get_label.md](/doc/api/label/get_label.md) |
| PUT | `/v1/label/{labelId}` | — | [edit_label.md](/doc/api/label/edit_label.md) |
| DELETE | `/v1/label/{labelId}` | — | [delete_label.md](/doc/api/label/delete_label.md) |
| POST | `/v1/label/{labelId}/content/{languageType}` | — | [add_or_update_label_content.md](/doc/api/label/add_or_update_label_content.md) |
| DELETE | `/v1/label/{labelId}/content/{languageType}` | — | [delete_label_content.md](/doc/api/label/delete_label_content.md) |
