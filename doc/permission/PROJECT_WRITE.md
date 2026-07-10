# PROJECT_WRITE

> Area: Project
> Roles: OWNER, ADMIN

Create, edit, and delete projects and manage their languages and source archive.

## Description

Grants project lifecycle management: creating a project, editing and deleting a project, adding a
project language, uploading the project's source ZIP archive, and resetting the project. Held by
OWNER and ADMIN — the administrative roles that own the site inventory. Developer-level work inside
a project's templates and navigation is governed separately by `PROJECT_DEV`, and publishing by
`PROJECT_DEPLOY`.

## Gated Endpoints

| Method | Path | Endpoint Doc |
| --- | --- | --- |
| POST | `/v1/project` | ⬜ [add_project.md](/doc/api/project/add_project.md) |
| PUT | `/v1/project/{projectId}` | ⬜ [edit_project.md](/doc/api/project/edit_project.md) |
| DELETE | `/v1/project/{projectId}` | ⬜ [delete_project.md](/doc/api/project/delete_project.md) |
| POST | `/v1/project/{projectId}/language` | ⬜ [add_project_language.md](/doc/api/project/add_project_language.md) |
| POST | `/v1/project/{projectId}/upload_zip` | ⬜ [upload_zip.md](/doc/api/project/upload_zip.md) |
| PATCH | `/v1/project/{projectId}/reset` | ⬜ [reset_project.md](/doc/api/project/reset_project.md) |
