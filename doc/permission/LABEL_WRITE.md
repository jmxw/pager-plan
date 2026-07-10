# LABEL_WRITE

> Area: Label
> Roles: None (no role grants this)

Create, edit, and delete labels and their per-language content.

## Description

Grants management of labels: creating, editing, and deleting a label, and adding, updating, or
deleting a label's content for a given `languageType`. A label is a reusable, language-keyed content
snippet substituted into templates at build time. **No `UserRole` currently includes `LABEL_WRITE`
in its permission set**, so the gated endpoints below are unreachable in practice — any
authenticated caller is rejected with `UserNotPermitted`. This is recorded as-is; granting it would
require adding `LABEL_WRITE` to a role's `UserRole.permissions` set.

## Gated Endpoints

| Method | Path | Endpoint Doc |
| --- | --- | --- |
| POST | `/v1/label` | ⬜ [add_label.md](/doc/api/label/add_label.md) |
| PUT | `/v1/label/{labelId}` | ⬜ [edit_label.md](/doc/api/label/edit_label.md) |
| DELETE | `/v1/label/{labelId}` | ⬜ [delete_label.md](/doc/api/label/delete_label.md) |
| POST | `/v1/label/{labelId}/content/{languageType}` | ⬜ [add_or_update_label_content.md](/doc/api/label/add_or_update_label_content.md) |
| DELETE | `/v1/label/{labelId}/content/{languageType}` | ⬜ [delete_label_content.md](/doc/api/label/delete_label_content.md) |
