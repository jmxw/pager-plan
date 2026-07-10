# Backend Errors

One doc per backend error code, grouped by category. These must always stay in sync with the
backend implementation (they are used in the Swagger API doc). See
[rules/backend_errors.md](../../rules/backend_errors.md).

## Index

The tables below list every error defined in
[`PagerErrorMessage.kt`](/source/pager-backend/domain/src/main/kotlin/com/xwkj/pager/domain/common/exception/PagerErrorMessage.kt)
— **11 categories, 68 enum constants (66 distinct codes)** — plus one hard-coded code (`9998`)
that clients can observe but that has no enum constant. The categories follow the constant groups
of the enum itself (one blank-line-separated block per code prefix); per
[rules/backend_errors.md](../../rules/backend_errors.md), each category becomes a subfolder and
each error code becomes one file inside it, named after the code (`<category>/<code>.md`).

All error docs are **to be written**; none exists yet. The **Doc** column names the file each
error's doc will live in. When a doc lands, its **Doc** cell becomes a link and the category
folder gains its own `README.md` error list in the same PR. New docs are started by copying the
template in [`_format/backend_error.md`](_format/backend_error.md).

### How an error reaches the client

Errors are raised as one of two exception types (both carry a `PagerErrorMessage` constant) and
converted to a response by
[`CmsExceptionHandler`](/source/pager-backend/app/cms/src/main/kotlin/com/xwkj/cms/exception/CmsExceptionHandler.kt):

- **`PagerExternalException`** → HTTP **400** with the constant's code in `errorCode` and the
  formatted message in `errorMsg`.
- **`PagerInternalException`** → logged at the constant's log level, then answered as
  `UnknownError`: HTTP **500** with `errorCode: -9999`. The constant's own code never reaches
  the client.
- Any other unhandled exception → also answered as `UnknownError` (HTTP 500, `errorCode: -9999`).
- Exceptions handled by Spring MVC itself (malformed request, unsupported method, …) → the
  Spring HTTP status (4xx) with the hard-coded `errorCode: 9998`.
- Exceptions thrown in the `app/consumer` deployment worker produce **no HTTP response at all**
  (RabbitMQ context): they are logged, and `ProjectDeployInterruptedByPagerErrors` additionally
  flips the deploy task state to `INTERRUPTED_BY_ERRORS`.

The error response body shape is `{ errorCode, errorMsg, timestamp, code }` (`code` = HTTP
status).

### Reading the tables

- **Message** is the constant's message text, copied verbatim — including existing typos
  (`interuptted`, `more that 1 templates`, a trailing space, and the unbalanced braces in
  `Navigation({}}` / `Project(({})`), which are honored as-is. `{}` placeholders are positional
  (SLF4J `MessageFormatter`); each error doc must explain its placeholders.
- **HTTP** is what a CMS API client observes when the error is raised: `400` (external — the
  client receives the constant's own code), `500 as -9999` (internal, raised inside a CMS
  request — the client receives `UnknownError`), or `N/A (consumer)` (raised only in the
  deployment worker — no HTTP response exists).
- **Thrown from** lists the class(es) that raise the error today — the starting point for the
  doc's "When Returned" section. Classes living in the consumer app are prefixed `consumer:`;
  all others are in the `domain` module or the CMS app.
- **†** marks a constant that is declared but never thrown anywhere. Its doc should be written
  together with the code that first throws it (same-PR sync rule), so its **HTTP** and
  **Thrown from** cells read `—`.

### Unknown → `unknown/`

| Doc | Code | Enum Constant | HTTP | Thrown from | Message |
| --- | --- | --- | --- | --- | --- |
| [unknown/-9999.md](unknown/-9999.md) | -9999 | `UnknownError` | 500 | `CmsExceptionHandler` (fallback for every internal or unhandled exception) | `Unknown error: {}` |
| [unknown/9998.md](unknown/9998.md) | 9998 | — (hard-coded, no enum constant) | 4xx (Spring's status) | `CmsExceptionHandler.handleExceptionInternal` | — (the Spring exception's own message) |

### Organization (10000) → `organization/`

| Doc | Code | Enum Constant | HTTP | Thrown from | Message |
| --- | --- | --- | --- | --- | --- |
| [organization/10001.md](organization/10001.md) | 10001 | `OrganizationIdNotExist` | 400 | `OrganizationRepository` | `Organization id {} not exist.` |
| [organization/10002.md](organization/10002.md) | 10002 | `OrganizationIdentifierExist` | 400 | `OrganizationService` | `Organization identifier {} exist.` |
| `organization/10003.md` † | 10003 | `OrganizationIdentifierNotExist` | — | — | `Organization identifier {} not exist.` |

### User (20000) → `user/`

| Doc | Code | Enum Constant | HTTP | Thrown from | Message |
| --- | --- | --- | --- | --- | --- |
| [user/20001.md](user/20001.md) | 20001 | `UserIdNotExist` | 400 | `UserRepository`, `PrivilegeComponent` | `User id {} not exist.` |
| [user/20002.md](user/20002.md) | 20002 | `UserNotPermitted` | 400 | `UserPermissionInterceptor` | `User({}) with role {} does not have permission {}.` |
| [user/20003.md](user/20003.md) | 20003 | `UserIdentifierExist` | 400 | `UserService`, `OrganizationService` | `User identifier {} exist.` |
| [user/20004.md](user/20004.md) | 20004 | `UserIdentifierNotExist` | 400 | `UserService` | `User identifier {} not exist.` |
| [user/20005.md](user/20005.md) | 20005 | `UserPasswordWrong` | 400 | `UserService` | `Password is wrong for user {}` |
| [user/20006.md](user/20006.md) | 20006 | `UserMailExist` | 400 | `UserService` | `User mail {} exist in organization {}.` |

### Access Key (30000) → `access_key/`

| Doc | Code | Enum Constant | HTTP | Thrown from | Message |
| --- | --- | --- | --- | --- | --- |
| [access_key/30001.md](access_key/30001.md) | 30001 | `AccessKeyIdNotExist` | 400 | `AccessKeyRepository` | `Access key id {} not exist.` |
| [access_key/30002.md](access_key/30002.md) | 30002 | `AccessKeyNotBelongToOrganization` | 400 | `AccessKeyRepository` | `Access key({}) does not belong to organization({})` |
| [access_key/30003.md](access_key/30003.md) | 30003 | `AccessKeyUsedInProject` | 400 | `AccessKeyService` | `Access key({}) has been used in projects and cannot be deleted.` |

### Project (40000) → `project/`

| Doc | Code | Enum Constant | HTTP | Thrown from | Message |
| --- | --- | --- | --- | --- | --- |
| [project/40001.md](project/40001.md) | 40001 | `ProjectIdNotExist` | 400 | `ProjectRepository` | `Project id {} not exist.` |
| [project/40002.md](project/40002.md) | 40002 | `ProjectNoPrivilege` | 400 | `PrivilegeComponent` | `No privilege to access project({})` |
| [project/40003.md](project/40003.md) | 40003 | `ProjectNotBelongToOrganization` | 400 | `ProjectRepository` | `Project({}) does not belong to organization({})` |
| [project/40004.md](project/40004.md) | 40004 | `ProjectIdentifierExist` | 400 | `ProjectService` | `Project identifier {} exist.` |
| `project/40005.md` † | 40005 | `ProjectIdentifierNotExist` | — | — | `Project identifier {} not exist.` |
| [project/40006.md](project/40006.md) | 40006 | `ProjectBucketExist` | 400 | `ProjectService` | `Project bucket exist with identifier {}.` |
| [project/40007.md](project/40007.md) | 40007 | `ProjectZipSavedError` | 400 | `ProjectService` | `Error to save project zip file for project({}).` |
| [project/40008.md](project/40008.md) | 40008 | `ProjectZipDownloadFailed` | N/A (consumer) | consumer: `ProjectZipHandlerService` | `Error to download project zip for project({}) from {}.` |
| [project/40009.md](project/40009.md) | 40009 | `ProjectZipFileUnzipFailed` | N/A (consumer) | consumer: `ProjectZipHandlerService` | `Error to unzip file for project({})` |
| [project/40010.md](project/40010.md) | 40010 | `ProjectZipUploadReject` | 400 | `ProjectService` | `Reject to upload zip file for project({}), please reset templates!` |
| [project/40011.md](project/40011.md) | 40011 | `ProjectResetReject` | 400 | `ProjectService` | `Project({}) is processing, try to reset later.` |
| [project/40012.md](project/40012.md) | 40012 | `ProjectIsDeploying` | 400 | `DeployService` | `Project({}) is deploying, try to deploy later.` |
| `project/40013.md` ‡ | 40013 | `ProjectDeployInterruptedByPagerErrors` | N/A (consumer) | consumer: `ProjectDeployService` | `Project(({}) deploy is interuptted ` |
| `project/40013.md` ‡ | 40013 | `ProjectIsWaitForDeploy` | 400 | `DeployService` | `Project({}) is wait for deploy now.` |

### Language (50000) → `language/`

| Doc | Code | Enum Constant | HTTP | Thrown from | Message |
| --- | --- | --- | --- | --- | --- |
| [language/50001.md](language/50001.md) | 50001 | `LanguageExist` | 400 | `LanguageService` | `Language {} exist in project({}).` |
| [language/50002.md](language/50002.md) | 50002 | `LanguageNotExist` | 400 | `LanguageRepository` | `Language {} not exist in project({}).` |
| [language/50003.md](language/50003.md) | 50003 | `LanguageDefaultNotFound` | 500 as -9999 | `LanguageRepository` (reached from `NavigationListService` / `NavigationArticleService`) | `Default language not found in project({}).` |

### Template (60000) → `template/`

| Doc | Code | Enum Constant | HTTP | Thrown from | Message |
| --- | --- | --- | --- | --- | --- |
| [template/60001.md](template/60001.md) | 60001 | `TemplateIdNotExist` | 400 | `TemplateRepository` | `Template id {} not exist.` |
| `template/60002.md` † | 60002 | `TemplateNoPrivilege` | — | — | `No privilege to access template({})` |
| [template/60003.md](template/60003.md) | 60003 | `TemplateNotBelongToProject` | 400 | `ArticleListService` | `Template({}) does not belong to project({}).` |
| [template/60004.md](template/60004.md) | 60004 | `TemplateNotTextFile` | 400 | `TemplateService` | `Template({}) is not a text file.` |
| [template/60005.md](template/60005.md) | 60005 | `TemplateNotBinaryFile` | 400 | `TemplateService` | `Template({}) is not a binary file.` |
| [template/60006.md](template/60006.md) | 60006 | `TemplateNameDuplicated` | 400 | `TemplateService` | `Template name {} is duplicated in the directory template({}).` |
| [template/60007.md](template/60007.md) | 60007 | `TemplateDirectoryRequired` | 400 | `TemplateService` | `Template({}) is not a directory.` |
| [template/60008.md](template/60008.md) | 60008 | `TemplateUploadFileSaveError` | 400 | `TemplateService` | `Error to save uploaded file for template({}).` |
| [template/60009.md](template/60009.md) | 60009 | `TemplateFileNotExist` | 400 | `TemplateService` | `Template file not exist for template({}).` |
| [template/60010.md](template/60010.md) | 60010 | `TemplateBinaryMetadataMissed` | 400 | `TemplateService` | `Metadata file for binary template({}) is missed.` |
| [template/60011.md](template/60011.md) | 60011 | `TemplateRenameReject` | 400 | `TemplateService` | `Cannot rename ROOT or RECYCLE_BIN template({})` |
| [template/60012.md](template/60012.md) | 60012 | `TemplateSameProjectRequired` | 400 | `TemplateService` | `The project of template({}) and target template({}) must be same.` |

### Article List (70000) → `article_list/`

| Doc | Code | Enum Constant | HTTP | Thrown from | Message |
| --- | --- | --- | --- | --- | --- |
| [article_list/70001.md](article_list/70001.md) | 70001 | `ArticleListIdNotExist` | 400 | `ArticleListRepository` | `Article list id {} not exist.` |
| `article_list/70002.md` † | 70002 | `ArticleListNoPrivilege` | — | — | `No privilege to access article list({})` |
| [article_list/70003.md](article_list/70003.md) | 70003 | `ArticleListIdentifierExist` | 400 | `ArticleListService` | `Article list identifier {} exist in project({}).` |
| `article_list/70004.md` † | 70004 | `ArticleListIdentifierNotExist` | — | — | `Article list identifier {} not exist in project({}).` |
| [article_list/70005.md](article_list/70005.md) | 70005 | `ArticleListTemplateFormatError` | 400 | `ArticleListService` | `Article template must be html.` |
| [article_list/70006.md](article_list/70006.md) | 70006 | `ArticleListProjectMismatching` | 400 | `ArticleListRepository` | `Article list and project is mismatching.` |
| [article_list/70007.md](article_list/70007.md) | 70007 | `ArticleListArticleMismatching` | 400 | `ArticleListService` | `Article list({}) and article({}) is mismatching, they must belong to a same project.` |
| [article_list/70008.md](article_list/70008.md) | 70008 | `ArticleListArticleExist` | 400 | `ArticleListService` | `Article list({}) contains article({}).` |
| [article_list/70009.md](article_list/70009.md) | 70009 | `ArticleListArticleNotExist` | 400 | `ArticleListService` | `Article list({}) does not contain article({}).` |
| [article_list/70010.md](article_list/70010.md) | 70010 | `ArticleDeleteRejectedDueToPagerListExisting` | 400 | `ArticleListService` | `Article list({}) cannot be deleted, because it was referenced by more that 1 templates.` |

### Article (80000) → `article/`

| Doc | Code | Enum Constant | HTTP | Thrown from | Message |
| --- | --- | --- | --- | --- | --- |
| [article/80001.md](article/80001.md) | 80001 | `ArticleIdNotExist` | 400 | `ArticleRepository` | `Article id {} not exist.` |
| [article/80002.md](article/80002.md) | 80002 | `ArticleContentNotExist` | 400 | `ArticleService` | `Article content of article({}) not exist.` |
| [article/80003.md](article/80003.md) | 80003 | `ArticleCoverSaveError` | 400 | `ArticleService` | `Error to save cover file for article({}).` |

### Navigation (90000) → `navigation/`

| Doc | Code | Enum Constant | HTTP | Thrown from | Message |
| --- | --- | --- | --- | --- | --- |
| [navigation/90001.md](navigation/90001.md) | 90001 | `NavigationIdNotExist` | 400 | `NavigationRepository` | `Navigation id {} not exist` |
| [navigation/90002.md](navigation/90002.md) | 90002 | `NavigationIdentifierDuplicated` | 400 | `NavigationListService`, `NavigationArticleService` | `Navigation identifier {} is duplicated in the parent navigation({}).` |
| [navigation/90003.md](navigation/90003.md) | 90003 | `NavigationArticleMismatching` | 400 | `NavigationArticleService` | `Navigation({}} and article({}) must belong to a same project` |
| [navigation/90004.md](navigation/90004.md) | 90004 | `NavigationArticleListMismatching` | 400 | `NavigationListService` | `Navigation({}} and article list({}) must belong to a same project` |
| [navigation/90005.md](navigation/90005.md) | 90005 | `NavigationTemplateMismatching` | 400 | `NavigationListService`, `NavigationArticleService` | `Navigation({}} and template({}) must belong to a same project` |
| `navigation/90006.md` ‡ | 90006 | `NavigationListLost` | N/A (consumer) | consumer: `ProjectDeployNavigationExtension` | `Navigation list of navigation({}) lost.` |
| `navigation/90006.md` ‡ | 90006 | `NavigationArticleLost` | N/A (consumer) | consumer: `ProjectDeployNavigationExtension` | `Navigation article of navigation({}) lost.` |
| [navigation/90007.md](navigation/90007.md) | 90007 | `NavigationDeleteRejectWithChildren` | 400 | `NavigationService` | `Navigation({}) with children navigations cannot be deleted.` |
| [navigation/90008.md](navigation/90008.md) | 90008 | `NavigationRootDeleteReject` | 400 | `NavigationService` | `Root navigation({}) cannot be deleted.` |

### Deploy Task (11000) → `deploy_task/`

| Doc | Code | Enum Constant | HTTP | Thrown from | Message |
| --- | --- | --- | --- | --- | --- |
| [deploy_task/11001.md](deploy_task/11001.md) | 11001 | `DeployTaskIdNotExist` | N/A (consumer) | `DeployTaskRepository` (only reached from the consumer) | `Deploy task id {} not exist.` |
| [deploy_task/11002.md](deploy_task/11002.md) | 11002 | `DeployTaskSkipDueToStarted` | N/A (consumer) | consumer: `ProjectDeployService` | `Deploy task({}) skipped, because it was started, current state is {}, cannot be ran again.` |
| [deploy_task/11003.md](deploy_task/11003.md) | 11003 | `DeployTaskSkipDueToNoTemplate` | N/A (consumer) | consumer: `ProjectDeployService` | `Deploy task({}) skipped, because the template of the project({}) is not ready.` |
| [deploy_task/11004.md](deploy_task/11004.md) | 11004 | `DeployDirectoryError` | N/A (consumer) | consumer: `ProjectDeployService` | `Error to find or create a deploy directory for project({})` |

## Duplicated codes (‡) — backend fix required before those docs land

Two pairs of constants share one code, so the "one code = one file" rule cannot be satisfied for
them under the current numbering:

- **40013** — `ProjectDeployInterruptedByPagerErrors` (internal, consumer) and
  `ProjectIsWaitForDeploy` (external, `DeployService`).
- **90006** — `NavigationListLost` and `NavigationArticleLost` (both internal, consumer).

Renumbering one constant of each pair is a backend change: it must be owned by a plan in the
current release and shipped in the same PR as the two error docs it unblocks (same-PR sync rule).
Until then, both rows of each pair are listed verbatim and neither doc can be written. Note that
the 40013 collision is already user-visible on the frontend (see drift below).

Two smaller quirks are honored verbatim as well: the Deploy Task group uses 5-digit codes
(11001–11004) that break the 10000-per-group pattern of every other category and sort between the
Organization and User ranges, and the 40013 message ends with a trailing space.

## Known frontend drift (for doc authors)

The frontend turns `errorCode` into a toast via
[`errorMessage.config.js`](/source/pager-frontend/src/config/errorMessage.config.js), called from
the axios response interceptor
[`request.js`](/source/pager-frontend/src/utils/request.js); codes missing from the map fall back
to the toast `未知错误` (unknown error). Drift found while building this index:

- The whole Navigation **90000** group is absent from the map, so every navigation error shows
  the fallback toast.
- The map's **40013** text (`项目部署失败，请先检查预编译错误` — "deploy failed, check the
  precompile errors first") describes `ProjectDeployInterruptedByPagerErrors`, but the only
  40013 a client can actually receive is `ProjectIsWaitForDeploy` — the toast is wrong for it.
- The map carries entries for codes that can never reach the client with their own code
  (40008, 40009, 50003, 11001–11004 — all internal or consumer-only): dead entries today.
- `-9999` and `9998` have no entries; both fall back to `未知错误`, which for `-9999` is the
  intended wording anyway.

Out of scope for this index: the per-line **precompile errors** shown in the deploy dialog
(`Deploy.vue`) are a separate mechanism — see
[TN0704 precompiled error](../tech_noun/TN0704_precompiled_error.md) — and are not
`PagerErrorMessage` codes.
