# API Endpoints

One doc per API endpoint, grouped by controller. Method/path/summary are copied verbatim from
the controller; errors and tech-nouns are referenced by link, never copied. Kept in sync with
`source/pager-backend` in the same PR. See [rules/api_docs.md](../../rules/api_docs.md).

## Index

The tables below list every endpoint of the CMS API — **9 controllers, 67 endpoints** — extracted
from the controllers under
[`app/cms/.../com/xwkj/cms/controller/`](/source/pager-backend/app/cms/src/main/kotlin/com/xwkj/cms/controller).
Every endpoint has its own doc, linked in the **Doc** column (folder = controller class name
without the `Controller` suffix in snake_case, file = controller method name in snake_case, per
[rules/api_docs.md](../../rules/api_docs.md)); each controller folder carries its own `README.md`
endpoint list. New docs are started by copying the template in [`_format/api.md`](_format/api.md).

Reading the tables:

- **Summary** is the endpoint's `@Operation(summary = ...)` text, copied verbatim — including
  existing typos, which are honored as-is. The endpoints that had no summary were given one in
  the controller in the same PR that wrote their docs (same-PR sync rule).
- **Permission** is the exact `@Permitted(UserPermission.X)` value. `N/A (public)` = the path is
  whitelisted in
  [`SecurityConfiguration.kt`](/source/pager-backend/app/cms/src/main/kotlin/com/xwkj/cms/configuration/SecurityConfiguration.kt)
  (no login required); `N/A (authenticated)` = login required but the method carries no
  `@Permitted`.
- **†** marks an endpoint whose controller method is a stub (declared, but no service call is
  implemented yet — or, for `clear_project_recycle_bin` and `copy_template`, the called service
  method is a `TODO()`). Its doc records the declared surface and is extended together with the
  implementation.

## Response envelope

Every controller return value is wrapped by
[`PagerResponseBodyAdvice`](/source/pager-backend/app/cms/src/main/kotlin/com/xwkj/cms/configuration/PagerResponseBodyAdvice.kt)
into a `PagerResponse` before serialization, so a success body always looks like:

```json
{
  "code": 200,
  "timestamp": 1752700000000,
  "data": { }
}
```

`code` is the HTTP status (`200`), `timestamp` is the server time in epoch milliseconds, and
`data` is the endpoint's response DTO (an empty object for `EmptyResponse`). The field tables in
the endpoint docs describe `data` only.

Business errors are raised as `PagerExternalException` and answered by
[`CmsExceptionHandler`](/source/pager-backend/app/cms/src/main/kotlin/com/xwkj/cms/exception/CmsExceptionHandler.kt)
with HTTP `400` and the body `{ "errorCode": ..., "errorMsg": "...", "timestamp": ..., "code": 400 }`;
internal or unhandled exceptions become HTTP `500` with `errorCode: -9999`. See
[the backend error index](/doc/backend_error/README.md) for the full mechanics. On authenticated
endpoints, a missing or invalid JWT is answered with `401 Unauthorized` and an **empty** body
(no envelope) before any handler runs.

### `AccessKeyController` → `access_key/`

Source: [`AccessKeyController.kt`](/source/pager-backend/app/cms/src/main/kotlin/com/xwkj/cms/controller/AccessKeyController.kt)

| Doc | Method | Path | Permission | Summary |
| --- | --- | --- | --- | --- |
| [access_key/get_access_keys.md](access_key/get_access_keys.md) | GET | `/v1/access_key` | `ACCESS_KEY_READ` | Get all access keys of an organization. |
| [access_key/add_access_key.md](access_key/add_access_key.md) | POST | `/v1/access_key` | `ACCESS_KEY_WRITE` | Add a new access key into an organization. |
| [access_key/edit_access_key.md](access_key/edit_access_key.md) | PUT | `/v1/access_key/{accessKeyId}` | N/A (authenticated) | Edit an access key. |
| [access_key/delete_access_key.md](access_key/delete_access_key.md) | DELETE | `/v1/access_key/{accessKeyId}` | `ACCESS_KEY_WRITE` | Delete an access key. |

### `ArticleController` → `article/`

Source: [`ArticleController.kt`](/source/pager-backend/app/cms/src/main/kotlin/com/xwkj/cms/controller/ArticleController.kt)

| Doc | Method | Path | Permission | Summary |
| --- | --- | --- | --- | --- |
| [article/search_articles.md](article/search_articles.md) | POST | `/v1/article/search` | `ARTICLE_READ` | Search articles in a project. |
| [article/add_article.md](article/add_article.md) | POST | `/v1/article` | `ARTICLE_WRITE` | Add a new article into a project. |
| [article/get_article.md](article/get_article.md) | GET | `/v1/article/{articleId}` | `ARTICLE_READ` | Get detail information of an article. |
| [article/edit_article.md](article/edit_article.md) | PUT | `/v1/article/{articleId}` | `ARTICLE_WRITE` | Edit detail information of an article. |
| [article/delete_article.md](article/delete_article.md) | DELETE | `/v1/article/{articleId}` | `ARTICLE_WRITE` | Delete an article. |
| [article/upload_article_cover.md](article/upload_article_cover.md) | POST | `/v1/article/{articleId}/cover` | `ARTICLE_WRITE` | Upload cover image for an article. |

### `ArticleListController` → `article_list/`

Source: [`ArticleListController.kt`](/source/pager-backend/app/cms/src/main/kotlin/com/xwkj/cms/controller/ArticleListController.kt)

| Doc | Method | Path | Permission | Summary |
| --- | --- | --- | --- | --- |
| [article_list/search_article_lists.md](article_list/search_article_lists.md) | POST | `/v1/article_list/search` | `ARTICLE_READ` | Search article lists in a project. |
| [article_list/add_article_list.md](article_list/add_article_list.md) | POST | `/v1/article_list` | `ARTICLE_LIST_WRITE` | Add a new article list into a project. |
| [article_list/edit_article_list.md](article_list/edit_article_list.md) | PUT | `/v1/article_list/{articleListId}` | `ARTICLE_LIST_WRITE` | Edit an article list. |
| [article_list/delete_article_list.md](article_list/delete_article_list.md) | DELETE | `/v1/article_list/{articleListId}` | `ARTICLE_LIST_WRITE` | Delete an article list. |
| [article_list/get_article_list.md](article_list/get_article_list.md) | GET | `/v1/article_list/{articleListId}` | `ARTICLE_LIST_READ` | Get detail information of an article list. |
| [article_list/get_article_list_items.md](article_list/get_article_list_items.md) | POST | `/v1/article_list/{articleListId}/items` | `ARTICLE_LIST_READ` | Get all items of an article list. |
| [article_list/add_article_to_list.md](article_list/add_article_to_list.md) | PATCH | `/v1/article_list/{articleListId}/article/{articleId}` | `ARTICLE_LIST_WRITE` | Add an existing article into an article list. |
| [article_list/delete_article_from_list.md](article_list/delete_article_from_list.md) | DELETE | `/v1/article_list/{articleListId}/article/{articleId}` | `ARTICLE_LIST_WRITE` | Delete and article from an article list. |

### `LabelController` → `label/`

Source: [`LabelController.kt`](/source/pager-backend/app/cms/src/main/kotlin/com/xwkj/cms/controller/LabelController.kt)

All six endpoints are stubs — the controller has no service injected.

| Doc | Method | Path | Permission | Summary |
| --- | --- | --- | --- | --- |
| [label/add_label.md](label/add_label.md) † | POST | `/v1/label` | `LABEL_WRITE` | Add a new label into a project. |
| [label/get_label.md](label/get_label.md) † | GET | `/v1/label/{labelId}` | `LABEL_READ` | Get detail information of a label. |
| [label/edit_label.md](label/edit_label.md) † | PUT | `/v1/label/{labelId}` | `LABEL_WRITE` | Edit a label. |
| [label/delete_label.md](label/delete_label.md) † | DELETE | `/v1/label/{labelId}` | `LABEL_WRITE` | Delete a label. |
| [label/add_or_update_label_content.md](label/add_or_update_label_content.md) † | POST | `/v1/label/{labelId}/content/{languageType}` | `LABEL_WRITE` | Add or update the content of a label for a language. |
| [label/delete_label_content.md](label/delete_label_content.md) † | DELETE | `/v1/label/{labelId}/content/{languageType}` | `LABEL_WRITE` | Delete the content of a label for a language. |

### `NavigationController` → `navigation/`

Source: [`NavigationController.kt`](/source/pager-backend/app/cms/src/main/kotlin/com/xwkj/cms/controller/NavigationController.kt)

| Doc | Method | Path | Permission | Summary |
| --- | --- | --- | --- | --- |
| [navigation/get_navigation.md](navigation/get_navigation.md) † | GET | `/v1/navigation/{navigationId}` | `PROJECT_DEV` | Get detail information of a navigation menu. |
| [navigation/add_navigation_list.md](navigation/add_navigation_list.md) | POST | `/v1/navigation/{navigationId}/list` | `PROJECT_DEV` | Add a new list type navigation menu to a parent navigation menu. |
| [navigation/add_navigation_article.md](navigation/add_navigation_article.md) | POST | `/v1/navigation/{navigationId}/article` | `PROJECT_DEV` | Add a new article type navigation menu to a parent navigation menu. |
| [navigation/get_navigation_children.md](navigation/get_navigation_children.md) | GET | `/v1/navigation/{navigationId}/children` | `PROJECT_DEV` | Get project, parent navigations, children navigations and the navigation itself. |
| [navigation/move_navigation.md](navigation/move_navigation.md) † | PATCH | `/v1/navigation/{navigationId}/move_to/{targetNavigationId}` | `PROJECT_DEV` | Move a navigation menu as a child of an other parent navigation menu. |
| [navigation/move_up.md](navigation/move_up.md) | PATCH | `/v1/navigation/{navigationId}/up` | `PROJECT_DEV` | Move navigation menu up |
| [navigation/move_down.md](navigation/move_down.md) | PATCH | `/v1/navigation/{navigationId}/down` | `PROJECT_DEV` | Move navigation menu down |
| [navigation/delete_navigation.md](navigation/delete_navigation.md) | DELETE | `/v1/navigation/{navigationId}` | `PROJECT_DEV` | Delete a navigation menu. |

### `OrganizationController` → `organization/`

Source: [`OrganizationController.kt`](/source/pager-backend/app/cms/src/main/kotlin/com/xwkj/cms/controller/OrganizationController.kt)

| Doc | Method | Path | Permission | Summary |
| --- | --- | --- | --- | --- |
| [organization/register.md](organization/register.md) | POST | `/v1/organization/register` | N/A (public) | Register a new organization with its owner user. |

### `ProjectController` → `project/`

Source: [`ProjectController.kt`](/source/pager-backend/app/cms/src/main/kotlin/com/xwkj/cms/controller/ProjectController.kt)

| Doc | Method | Path | Permission | Summary |
| --- | --- | --- | --- | --- |
| [project/get_projects.md](project/get_projects.md) | GET | `/v1/project` | `PROJECT_READ` | Get all projects of an organization. |
| [project/add_project.md](project/add_project.md) | POST | `/v1/project` | `PROJECT_WRITE` | Add a new project into an organization. |
| [project/get_project_available_regions.md](project/get_project_available_regions.md) | GET | `/v1/project/regions` | `PROJECT_READ` | Get all available OSS regions for a project. |
| [project/get_project_available_language_types.md](project/get_project_available_language_types.md) | GET | `/v1/project/language_types` | `PROJECT_READ` | Get all available language types for a project. |
| [project/get_project.md](project/get_project.md) | GET | `/v1/project/{projectId}` | `PROJECT_READ` | Get detail information of a project. |
| [project/edit_project.md](project/edit_project.md) | PUT | `/v1/project/{projectId}` | `PROJECT_WRITE` | Edit detail information of a project. |
| [project/delete_project.md](project/delete_project.md) | DELETE | `/v1/project/{projectId}` | `PROJECT_WRITE` | Delete a project. |
| [project/get_project_languages.md](project/get_project_languages.md) | GET | `/v1/project/{projectId}/language` | `PROJECT_READ` | Get all languages of a project. |
| [project/add_project_language.md](project/add_project_language.md) | POST | `/v1/project/{projectId}/language` | `PROJECT_WRITE` | Add a new language into a project. |
| [project/upload_zip.md](project/upload_zip.md) | POST | `/v1/project/{projectId}/upload_zip` | `PROJECT_WRITE` | Upload a zip file of template files for a project. |
| [project/reset.md](project/reset.md) | PATCH | `/v1/project/{projectId}/reset` | `PROJECT_WRITE` | Reset all templates of a project. |
| [project/clear_project_recycle_bin.md](project/clear_project_recycle_bin.md) † | PATCH | `/v1/project/{projectId}/clear_recycle_bin` | `PROJECT_DEV` | Clear the recycle bin of a project. |
| [project/deploy.md](project/deploy.md) | PATCH | `/v1/project/{projectId}/deploy/{phase}` | `PROJECT_DEPLOY` | Deploy a project to a phase. |
| [project/force_deploy.md](project/force_deploy.md) | PATCH | `/v1/project/{projectId}/force_deploy/{phase}` | N/A (authenticated) | Force deploy a project to a phase. |

### `TemplateController` → `template/`

Source: [`TemplateController.kt`](/source/pager-backend/app/cms/src/main/kotlin/com/xwkj/cms/controller/TemplateController.kt)

| Doc | Method | Path | Permission | Summary |
| --- | --- | --- | --- | --- |
| [template/move_template_to_recycle_bin.md](template/move_template_to_recycle_bin.md) | DELETE | `/v1/template/{templateId}` | `PROJECT_DEV` | Move a template to recycle bin. |
| [template/add_template.md](template/add_template.md) | POST | `/v1/template/{templateId}/add` | `PROJECT_DEV` | Add a new template to a parent template. |
| [template/upload_template.md](template/upload_template.md) | POST | `/v1/template/{templateId}/upload` | `PROJECT_DEV` | Upload a new template file to a parent template. |
| [template/read_directory.md](template/read_directory.md) | GET | `/v1/template/{templateId}/directory` | `PROJECT_DEV` | Get project, parent templates, child templates and the template it self. |
| [template/read_file_content.md](template/read_file_content.md) | GET | `/v1/template/{templateId}/content` | `PROJECT_DEV` | Get the text content of a text template file. |
| [template/write_file_content.md](template/write_file_content.md) | PATCH | `/v1/template/{templateId}/content` | `PROJECT_DEV` | Update the text content of a text template file. |
| [template/get_binary_template_presigned_url.md](template/get_binary_template_presigned_url.md) | GET | `/v1/template/{templateId}/binary` | `PROJECT_DEV` | Get url of a binary template file. |
| [template/rename_template.md](template/rename_template.md) | PATCH | `/v1/template/{templateId}/rename` | `PROJECT_DEV` | Rename a template. |
| [template/copy_template.md](template/copy_template.md) † | PATCH | `/v1/template/{templateId}/copy_to/{targetTemplateId}` | `PROJECT_DEV` | Copy a template and its children as a child of the target template. |
| [template/move_template.md](template/move_template.md) | PATCH | `/v1/template/{templateId}/move_to/{targetTemplateId}` | `PROJECT_DEV` | Move a template and its children as a child of the target template. |

### `UserController` → `user/`

Source: [`UserController.kt`](/source/pager-backend/app/cms/src/main/kotlin/com/xwkj/cms/controller/UserController.kt)

| Doc | Method | Path | Permission | Summary |
| --- | --- | --- | --- | --- |
| [user/login.md](user/login.md) | POST | `/v1/user/login` | N/A (public) | User login |
| [user/logout.md](user/logout.md) | GET | `/v1/user/logout` | N/A (authenticated) | User logout |
| [user/get_users.md](user/get_users.md) | GET | `/v1/user` | `USER_READ` | Get all visible users of an organization. |
| [user/get_available_roles.md](user/get_available_roles.md) | GET | `/v1/user/roles` | `USER_READ` | Get all available user roles. |
| [user/add_admin.md](user/add_admin.md) | POST | `/v1/user/admin` | `ADMIN_WRITE` | Add an admin user. |
| [user/add_developer.md](user/add_developer.md) | POST | `/v1/user/developer` | `USER_WRITE` | Add a developer user. |
| [user/add_customer.md](user/add_customer.md) | POST | `/v1/user/customer` | `USER_WRITE` | Add a customer user. |
| [user/get_me.md](user/get_me.md) | GET | `/v1/user/me` | `USER_READ` | Get user information of the current user. |
| [user/edit_user.md](user/edit_user.md) † | PATCH | `/v1/user/{userId}` | `USER_WRITE` | Get user information by userId. |
| [user/delete_user.md](user/delete_user.md) † | DELETE | `/v1/user/{userId}` | `USER_WRITE` | Delete user. |

Note: the `edit_user` summary is copied verbatim from the source, where it reads like a
copy-paste leftover ("Get user information by userId."); correcting it belongs to the PR that
implements the endpoint, not to this index.

## Known frontend drift (for doc authors)

The frontend calls these endpoints through hand-written axios modules under
[`src/api/`](/source/pager-frontend/src/api) (`user.js`, `project.js`, `article.js`,
`articleList.js`, `navigation.js`, `template.js`, `accessKey.js`). One drift was found while
building this index: `user.js` posts to `/v1/user/register`, which does not exist on the
backend — the actual endpoint is `POST /v1/organization/register`. The index lists only the
endpoints that exist in the backend controllers.
