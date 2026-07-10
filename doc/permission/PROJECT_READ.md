# PROJECT_READ

> Area: Project
> Roles: OWNER, ADMIN, DEVELOPER, CUSTOMER

Read projects and their configuration options.

## Description

Grants read access to projects (a project is one website): listing projects, reading a single
project, reading a project's configured languages, and reading the option catalogs used when
creating or editing a project (available regions and language types). Held by all four roles, so
any authenticated user can view the projects they belong to.

## Gated Endpoints

| Method | Path | Endpoint Doc |
| --- | --- | --- |
| GET | `/v1/project` | ⬜ [get_projects.md](/doc/api/project/get_projects.md) |
| GET | `/v1/project/regions` | ⬜ [get_project_available_regions.md](/doc/api/project/get_project_available_regions.md) |
| GET | `/v1/project/language_types` | ⬜ [get_project_available_language_types.md](/doc/api/project/get_project_available_language_types.md) |
| GET | `/v1/project/{projectId}` | ⬜ [get_project.md](/doc/api/project/get_project.md) |
| GET | `/v1/project/{projectId}/language` | ⬜ [get_project_languages.md](/doc/api/project/get_project_languages.md) |
