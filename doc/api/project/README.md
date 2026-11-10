# ProjectController

API docs for the endpoints of
[`ProjectController.kt`](/source/pager-backend/app/cms/src/main/kotlin/com/xwkj/cms/controller/ProjectController.kt).
One file per endpoint; see the parent [API index](../README.md) and
[rules/api_docs.md](../../../rules/api_docs.md).

| Doc | Method | Path | Permission | Summary |
| --- | --- | --- | --- | --- |
| [get_projects.md](get_projects.md) | GET | `/v1/project` | `PROJECT_READ` | Get all projects of an organization. |
| [add_project.md](add_project.md) | POST | `/v1/project` | `PROJECT_WRITE` | Add a new project into an organization. |
| [get_project_available_regions.md](get_project_available_regions.md) | GET | `/v1/project/regions` | `PROJECT_READ` | Get all available OSS regions for a project. |
| [get_project_available_language_types.md](get_project_available_language_types.md) | GET | `/v1/project/language_types` | `PROJECT_READ` | Get all available language types for a project. |
| [get_project.md](get_project.md) | GET | `/v1/project/{projectId}` | `PROJECT_READ` | Get detail information of a project. |
| [edit_project.md](edit_project.md) | PUT | `/v1/project/{projectId}` | `PROJECT_WRITE` | Edit detail information of a project. |
| [delete_project.md](delete_project.md) | DELETE | `/v1/project/{projectId}` | `PROJECT_WRITE` | Delete a project. |
| [get_project_languages.md](get_project_languages.md) | GET | `/v1/project/{projectId}/language` | `PROJECT_READ` | Get all languages of a project. |
| [add_project_language.md](add_project_language.md) | POST | `/v1/project/{projectId}/language` | `PROJECT_WRITE` | Add a new language into a project. |
| [upload_zip.md](upload_zip.md) | POST | `/v1/project/{projectId}/upload_zip` | `PROJECT_WRITE` | Upload a zip file of template files for a project. |
| [reset.md](reset.md) | PATCH | `/v1/project/{projectId}/reset` | `PROJECT_WRITE` | Reset all templates of a project. |
| [clear_project_recycle_bin.md](clear_project_recycle_bin.md) | PATCH | `/v1/project/{projectId}/clear_recycle_bin` | `PROJECT_DEV` | Clear the recycle bin of a project. |
| [deploy.md](deploy.md) | PATCH | `/v1/project/{projectId}/deploy/{phase}` | `PROJECT_DEPLOY` | Deploy a project to a phase. |
| [force_deploy.md](force_deploy.md) | PATCH | `/v1/project/{projectId}/force_deploy/{phase}` | N/A (authenticated) | Force deploy a project to a phase. |
