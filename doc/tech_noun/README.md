# Tech Nouns

Domain-term definitions — the source of terminology for all docs. Never invent similar terms;
always link to the existing definition here. See [rules/tech_noun.md](../../rules/tech_noun.md).

## How this index works

- One noun = one file in this folder, named `TNxxxx_<name>.md`.
- TN IDs are 4 digits: the first 2 digits are the group number, the last 2 are the noun's
  sequence inside the group. Numbers are stable once assigned; gaps between groups are never
  compacted (see the [numbering rules](../../rules/tech_noun.md#id-numbering)).
- Backing code names are copied verbatim from `pager-backend`. JPA entities live in
  `/source/pager-backend/domain/src/main/kotlin/com/xwkj/pager/domain/model/database/`,
  enums in `/source/pager-backend/domain/src/main/kotlin/com/xwkj/pager/domain/model/enum/`.
- When a model or an important field is created / updated / deleted, the corresponding noun doc
  must be created / updated / deleted in the same PR.

The following are deliberately **not** tech nouns — they are owned by other doc folders and are
referenced by link instead of being redefined here:

- `UserPermission` values and the role hierarchy → [/doc/permission/](../permission/README.md)
- Backend error codes → [/doc/backend_error/](../backend_error/README.md)
- Template-tag **syntax** → [template tag reference](../../plan/common/template-tags.md)
  (the tag *concept* is [TN0403](TN0403_pager_tag.md))

## Group 01 — Cross-cutting concepts

| ID | Noun | File | Backing code | Definition |
|---|---|---|---|---|
| TN0101 | Identifier | [TN0101_identifier.md](TN0101_identifier.md) | `identifier` fields on `Organization`, `User`, `Project`, `Navigation`, `ArticleList`, `LinkList`, `Label` | The stable machine-readable key of a model, distinct from the display `name`: bucket/host naming for a project, URL path segment for a navigation, and the `id` referenced by template tags for article lists, link lists, and labels. |
| TN0102 | Revision | [TN0102_revision.md](TN0102_revision.md) | `revision` fields on `Template`, `Article`, `ArticleList`, `Link`, `LinkList`, `Navigation`; `Project.stagingRevision` / `productionRevision`; `DeployTask.revision` | The change counter kept per model and compared against a project's staging / production revision to decide what a deployment must re-render. |

## Group 02 — Organization & account

| ID | Noun | File | Backing code | Definition |
|---|---|---|---|---|
| TN0201 | Organization | [TN0201_organization.md](TN0201_organization.md) | `Organization` (`pager_organization`) | The tenant unit that owns users and access keys; every user and access key belongs to exactly one organization. |
| TN0202 | User | [TN0202_user.md](TN0202_user.md) | `User` (`pager_user`), `UserRole` | A CMS login account belonging to an organization; the `role` (OWNER > ADMIN > DEVELOPER > CUSTOMER) determines the granted permission set (see [/doc/permission/](../permission/README.md)). |
| TN0203 | Privilege | [TN0203_privilege.md](TN0203_privilege.md) | `Privilege` (`pager_privilege`) | The grant that gives a user access to a specific project. |
| TN0204 | Access Key | [TN0204_access_key.md](TN0204_access_key.md) | `AccessKey` (`pager_access_key`) | An Aliyun access-key pair registered per organization; each project deploys through one access key. |

## Group 03 — Project & site-wide content

| ID | Noun | File | Backing code | Definition |
|---|---|---|---|---|
| TN0301 | Project | [TN0301_project.md](TN0301_project.md) | `Project` (`pager_project`), `ProjectState`, `OssRegion` | A website being built: identifier, OSS region, lifecycle state (`INIT` → `ZIP_UPLOADING` → `ZIP_ANALYSING` → `TEMPLATE_READY` → `WAIT_FOR_DEPLOY` → `DEPLOYING` → `DEPLOYED`), index / error documents, staging / production revisions, and references to the root template, recycle-bin template, and root navigation. |
| TN0302 | Language | [TN0302_language.md](TN0302_language.md) | `Language` (`pager_language`), `LanguageType` | A language enabled for a project, exactly one being `defaulted`; articles, label contents, and navigation titles are stored per language. |
| TN0303 | Label | [TN0303_label.md](TN0303_label.md) | `Label`, `LabelContent` (`pager_label`, `pager_label_content`) | A named, identifier-addressed content slot of a project whose per-language content is substituted into templates at deploy time. |

## Group 04 — Template

| ID | Noun | File | Backing code | Definition |
|---|---|---|---|---|
| TN0401 | Template | [TN0401_template.md](TN0401_template.md) | `Template` (`pager_template`), `TemplateType`, `DeployType` | A node of a project's site file tree — directory or file (HTML / CSS / JS / asset) — with parent chain, layer, revision, hidden flag, and error count; `ROOT` and `RECYCLE_BIN` are special nodes. |
| TN0402 | Template File | [TN0402_template_file.md](TN0402_template_file.md) | `TemplateFile` (`pager_template_file`) | The stored text content of a template node, edited in the online template editor. |
| TN0403 | Pager Tag | [TN0403_pager_tag.md](TN0403_pager_tag.md) | `TagType`, `PagerTag`, `PagerTagAnalyseResult` | The `{pager:*}` block directives (`list`, `pagebar`, `pagelink`, `include`, `navmenu`, `navlist`) and the `[list:*]` / `[page:*]` / `{content:*}` placeholders substituted at deploy time; syntax is defined in the [template tag reference](../../plan/common/template-tags.md). |

## Group 05 — Article & link content

| ID | Noun | File | Backing code | Definition |
|---|---|---|---|---|
| TN0501 | Article | [TN0501_article.md](TN0501_article.md) | `Article`, `ArticleContent` (`pager_article`, `pager_article_content`) | An authored content piece (title, description, cover, revision) written in one language with created / last authors; the body is stored separately in `pager_article_content`. |
| TN0502 | Article List | [TN0502_article_list.md](TN0502_article_list.md) | `ArticleList`, `ArticleListItem` (`pager_article_list`, `pager_article_list_item`) | A named, identifier-addressed list bound to a rendering template; articles enter via `pager_article_list_item` with a `publishAt` timestamp, and pages reference the list with `{pager:list id="…"}`. |
| TN0503 | Link | [TN0503_link.md](TN0503_link.md) | `Link` (`pager_link`) | An external hyperlink entry (href, title, cover) with language and created / last authors, belonging to a link list. |
| TN0504 | Link List | [TN0504_link_list.md](TN0504_link_list.md) | `LinkList` (`pager_link_list`) | A named, identifier-addressed list of links of a project. |

## Group 06 — Navigation

| ID | Noun | File | Backing code | Definition |
|---|---|---|---|---|
| TN0601 | Navigation | [TN0601_navigation.md](TN0601_navigation.md) | `Navigation` (`pager_navigation`), `NavigationType` | A node of the site's menu / URL tree (`ROOT` / `LIST` / `ARTICLE`) with sequence and layer; the identifier chain of its parents forms the page path (e.g. `aaa/bbb/index.html`). |
| TN0602 | Navigation Title | [TN0602_navigation_title.md](TN0602_navigation_title.md) | `NavigationTitle` (`pager_navigation_title`) | The per-language display title of a navigation node. |
| TN0603 | Navigation Article | [TN0603_navigation_article.md](TN0603_navigation_article.md) | `NavigationArticle` (`pager_navigation_article`) | The binding that renders one article on an `ARTICLE` navigation node with a given template, per language. |
| TN0604 | Navigation List | [TN0604_navigation_list.md](TN0604_navigation_list.md) | `NavigationList` (`pager_navigation_list`) | The binding that renders an article list on a `LIST` navigation node with a given template. |

## Group 07 — Deployment

| ID | Noun | File | Backing code | Definition |
|---|---|---|---|---|
| TN0701 | Deploy Task | [TN0701_deploy_task.md](TN0701_deploy_task.md) | `DeployTask` (`pager_deploy_task`), `DeployPhase`, `DeployState`, `ProjectDeployMessage` | One deployment run of a project to a phase (`STAGING` / `PRODUCTION`) at a revision, processed asynchronously by the consumer via RabbitMQ; states run `SUBMIT` → `SCANNING` → `FINISH` / `INTERRUPTED_BY_USER` / `INTERRUPTED_BY_ERRORS` / `NOTHING_TO_DEPLOY`. |
| TN0702 | Bucket | [TN0702_bucket.md](TN0702_bucket.md) | `BucketType`, `OssDeployer` (`lib/oss`) | The per-project public-read OSS buckets — `<identifier>-staging` and `<identifier>-production` — that the generated static site is deployed to and served from. |
| TN0703 | Precompiled Tag | [TN0703_precompiled_tag.md](TN0703_precompiled_tag.md) | `PrecompiledTag`; `PrecompiledArticleList`, `PrecompiledLinkList`, `PrecompiledNavigationList`, `PrecompiledPageBar`, `PrecompiledInclude` | A pager-tag occurrence extracted from a template during precompile, storing the original HTML and the reusable render template used at page-generation time. |
| TN0704 | Precompiled Error | [TN0704_precompiled_error.md](TN0704_precompiled_error.md) | `PrecompiledError`, `PagerErrorCode` | A tag-syntax error found during precompile, recorded per template and line number and surfaced as the template's `errorCount`. |
