# Project (ProjectController)

Apis to manage project

## Endpoints

| Method | Path | Summary | Doc |
| --- | --- | --- | --- |
| GET | `/v1/project` | Get all projects of an organization. | [get_projects.md](/doc/api/project/get_projects.md) |
| POST | `/v1/project` | Creates a project, its OSS bucket, and its initial templates, navigation, and default language. | [add_project.md](/doc/api/project/add_project.md) |
| GET | `/v1/project/regions` | Lists the OSS regions a project may be created in. | [get_project_available_regions.md](/doc/api/project/get_project_available_regions.md) |
| GET | `/v1/project/language_types` | Lists the language types a project language may use. | [get_project_available_language_types.md](/doc/api/project/get_project_available_language_types.md) |
| GET | `/v1/project/{projectId}` | Returns a single project by id. | [get_project.md](/doc/api/project/get_project.md) |
| PUT | `/v1/project/{projectId}` | Updates a project's name and its bucket index/error documents. | [edit_project.md](/doc/api/project/edit_project.md) |
| DELETE | `/v1/project/{projectId}` | Deletes a project and its OSS bucket. | [delete_project.md](/doc/api/project/delete_project.md) |
| GET | `/v1/project/{projectId}/language` | Lists the languages configured on a project. | [get_project_languages.md](/doc/api/project/get_project_languages.md) |
| POST | `/v1/project/{projectId}/language` | Adds a language to a project. | [add_project_language.md](/doc/api/project/add_project_language.md) |
| POST | `/v1/project/{projectId}/upload_zip` | Uploads a template ZIP artifact for asynchronous analysis. | [upload_zip.md](/doc/api/project/upload_zip.md) |
| PATCH | `/v1/project/{projectId}/reset` | Resets a project back to its initial state. | [reset.md](/doc/api/project/reset.md) |
| PATCH | `/v1/project/{projectId}/clear_recycle_bin` | Clears a project's recycle bin. | [clear_project_recycle_bin.md](/doc/api/project/clear_project_recycle_bin.md) |
| PATCH | `/v1/project/{projectId}/deploy/{phase}` | Deploys a project to the given phase. | [deploy.md](/doc/api/project/deploy.md) |
| PATCH | `/v1/project/{projectId}/force_deploy/{phase}` | Force-deploys a project, discarding in-flight tasks and existing files. | [force_deploy.md](/doc/api/project/force_deploy.md) |
