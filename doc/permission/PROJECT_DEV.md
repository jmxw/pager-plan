# PROJECT_DEV

> Area: Navigation / Template
> Roles: OWNER, ADMIN, DEVELOPER

Develop a project: edit its template files and navigation tree, and clear its recycle bin.

## Description

Grants the developer-facing authoring surface of a project: reading and writing template files and
directories (add, upload, read directory, read/write content, presigned binary URL, rename, copy,
move, and move-to-recycle-bin), managing the navigation tree (add list/article nodes, read
children, move/reorder/up/down, delete), and clearing the project recycle bin. Held by OWNER,
ADMIN, and DEVELOPER — the roles that build the site. `CUSTOMER` does not hold this permission, so
customers cannot edit templates or navigation. Project-level create/edit/delete stays with
`PROJECT_WRITE` and publishing with `PROJECT_DEPLOY`.

## Gated Endpoints

| Method | Path | Endpoint Doc |
| --- | --- | --- |
| PATCH | `/v1/project/{projectId}/clear_recycle_bin` | [clear_project_recycle_bin.md](/doc/api/project/clear_project_recycle_bin.md) |
| GET | `/v1/navigation/{navigationId}` | [get_navigation.md](/doc/api/navigation/get_navigation.md) |
| POST | `/v1/navigation/{navigationId}/list` | [add_navigation_list.md](/doc/api/navigation/add_navigation_list.md) |
| POST | `/v1/navigation/{navigationId}/article` | [add_navigation_article.md](/doc/api/navigation/add_navigation_article.md) |
| GET | `/v1/navigation/{navigationId}/children` | [get_navigation_children.md](/doc/api/navigation/get_navigation_children.md) |
| PATCH | `/v1/navigation/{navigationId}/move_to/{targetNavigationId}` | [move_navigation.md](/doc/api/navigation/move_navigation.md) |
| PATCH | `/v1/navigation/{navigationId}/up` | [move_up.md](/doc/api/navigation/move_up.md) |
| PATCH | `/v1/navigation/{navigationId}/down` | [move_down.md](/doc/api/navigation/move_down.md) |
| DELETE | `/v1/navigation/{navigationId}` | [delete_navigation.md](/doc/api/navigation/delete_navigation.md) |
| POST | `/v1/template/{templateId}/add` | [add_template.md](/doc/api/template/add_template.md) |
| POST | `/v1/template/{templateId}/upload` | [upload_template.md](/doc/api/template/upload_template.md) |
| GET | `/v1/template/{templateId}/directory` | [read_directory.md](/doc/api/template/read_directory.md) |
| GET | `/v1/template/{templateId}/content` | [read_file_content.md](/doc/api/template/read_file_content.md) |
| PATCH | `/v1/template/{templateId}/content` | [write_file_content.md](/doc/api/template/write_file_content.md) |
| GET | `/v1/template/{templateId}/binary` | [get_binary_template_presigned_url.md](/doc/api/template/get_binary_template_presigned_url.md) |
| PATCH | `/v1/template/{templateId}/rename` | [rename_template.md](/doc/api/template/rename_template.md) |
| PATCH | `/v1/template/{templateId}/copy_to/{targetTemplateId}` | [copy_template.md](/doc/api/template/copy_template.md) |
| PATCH | `/v1/template/{templateId}/move_to/{targetTemplateId}` | [move_template.md](/doc/api/template/move_template.md) |
| DELETE | `/v1/template/{templateId}` | [move_template_to_recycle_bin.md](/doc/api/template/move_template_to_recycle_bin.md) |
