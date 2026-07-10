# TN0101 Identifier

An **Identifier** is the stable, machine-readable key of a model, stored in a field named
`identifier` and kept distinct from the human-facing display `name` that sits next to it on the
same entity. The `name` is free display text; the `identifier` is what machines address: it names
a project's OSS [Bucket](TN0702_bucket.md)s, forms a navigation node's URL path segment, is the
`id` an author writes into a [Pager Tag](TN0403_pager_tag.md), and identifies the tenant and the
login account. Seven entities carry the field; the uniqueness scope and the addressing role
differ per model and are listed below.

## Code mapping

The concept is not a class of its own — it is the `identifier: String` column (declared
`@Column(nullable = false)`) on each of these entities:

| Model | Entity class (DB table) | Source |
|---|---|---|
| [Organization](TN0201_organization.md) | `Organization` (`pager_organization`) | [Organization.kt](/source/pager-backend/domain/src/main/kotlin/com/xwkj/pager/domain/model/database/Organization.kt) |
| [User](TN0202_user.md) | `User` (`pager_user`) | [User.kt](/source/pager-backend/domain/src/main/kotlin/com/xwkj/pager/domain/model/database/User.kt) |
| [Project](TN0301_project.md) | `Project` (`pager_project`) | [Project.kt](/source/pager-backend/domain/src/main/kotlin/com/xwkj/pager/domain/model/database/Project.kt) |
| [Navigation](TN0601_navigation.md) | `Navigation` (`pager_navigation`) | [Navigation.kt](/source/pager-backend/domain/src/main/kotlin/com/xwkj/pager/domain/model/database/Navigation.kt) |
| [Article List](TN0502_article_list.md) | `ArticleList` (`pager_article_list`) | [ArticleList.kt](/source/pager-backend/domain/src/main/kotlin/com/xwkj/pager/domain/model/database/ArticleList.kt) |
| [Link List](TN0504_link_list.md) | `LinkList` (`pager_link_list`) | [LinkList.kt](/source/pager-backend/domain/src/main/kotlin/com/xwkj/pager/domain/model/database/LinkList.kt) |
| [Label](TN0303_label.md) | `Label` (`pager_label`) | [Label.kt](/source/pager-backend/domain/src/main/kotlin/com/xwkj/pager/domain/model/database/Label.kt) |

Bucket naming from the project identifier lives in
[BucketType.kt](/source/pager-backend/lib/oss/src/main/kotlin/com/xwkj/pager/oss/BucketType.kt)
(`lib/oss`); tag-id resolution lives in
[PrecompileArticleListExtension.kt](/source/pager-backend/domain/src/main/kotlin/com/xwkj/pager/domain/component/PrecompileArticleListExtension.kt).

## Usage across models

| Model | Uniqueness scope | Duplicate guard (DAO / error) | What the identifier addresses |
|---|---|---|---|
| `Organization` | System-wide | `OrganizationDao.getByIdentifier` / `OrganizationIdentifierExist` | The tenant. |
| `User` | System-wide | `UserDao.getByIdentifier` / `UserIdentifierExist` | The login account; becomes the JWT subject. |
| `Project` | System-wide (and Aliyun-wide via bucket names) | `ProjectDao.getByIdentifier` / `ProjectIdentifierExist`; bucket collision raises `ProjectBucketExist` | The OSS buckets (and therefore the site hosts). |
| `Navigation` | Per parent node (among siblings) | `NavigationDao.getByParentAndIdentifier` / `NavigationIdentifierDuplicated` | One URL path segment of the generated site. |
| `ArticleList` | Per project | `ArticleListDao.getByProjectAndIdentifier` / `ArticleListIdentifierExist` | The `id` attribute of `{pager:list}` tags. |
| `LinkList` | Per project (by convention) | None found in the current code (no `LinkListDao` exists) | The list id mirrored as `PrecompiledLinkList.linkListIdentifier`. |
| `Label` | Per project (by convention) | None found in the current code | The identifier-addressed content slot substituted at deploy time. |

### Project — bucket naming

`Project.identifier` drives the naming of the two per-project OSS
[Bucket](TN0702_bucket.md)s. `BucketType.bucketName(projectIdentifier)` returns
`"$projectIdentifier-staging"` for `STAGING` and `"$projectIdentifier-production"` for
`PRODUCTION`. At project creation (`ProjectService.add`), both buckets are created from the
identifier via `Oss.createBucket(identifier, ossCredential)` — the add is rejected with
`ProjectBucketExist` when either bucket name is already taken — so the identifier is effectively
unique across Aliyun OSS bucket namespace, not just within Pager. The project identifier is also
reused as the `name` of the project's `ROOT` and `RECYCLE_BIN` [Template](TN0401_template.md)
nodes and as both `identifier` and `name` of its `ROOT` [Navigation](TN0601_navigation.md) node.

### Navigation — URL path segment

`Navigation.identifier` is one segment of the generated page URL. Three computed properties on
`Navigation` build the path from the identifiers of the ancestor chain (the `ROOT` node is
excluded by `parents.drop(1)`):

| Computed property | Value |
|---|---|
| `path` | Ancestor identifiers joined with `/`, with a trailing `/` (empty string for top-level nodes). |
| `pathIdentifier` | `path + identifier`. |
| `pathname` | `"$pathIdentifier/index.html"` — the source comment reads `like aaa/bbb/index.html`. |

Uniqueness is enforced only among siblings (`getByParentAndIdentifier`), which is exactly what a
path segment requires.

### Article list, link list, label — ids referenced by template tags

These identifiers are the ids that template authors write into a
[Pager Tag](TN0403_pager_tag.md) (syntax in the
[template tag reference](../../plan/common/template-tags.md)):

- `ArticleList.identifier` is the value of the `id` attribute in `{pager:list id="…"}`. During
  precompile, `PrecompileArticleListExtension` reads the attribute as `articleListIdentifier`
  and resolves it with `articleListDao.getByProjectAndIdentifier(...)`; an unknown id is a
  precompile error (see [Precompiled Error](TN0704_precompiled_error.md)).
- `LinkList.identifier` follows the same pattern; the resolved id is stored verbatim as
  `PrecompiledLinkList.linkListIdentifier`. In the current code, `Link` and `LinkList` are
  referenced only by `PrecompiledLinkList` — no DAO or service resolves the identifier yet.
- `Label.identifier` addresses the content slot whose per-language content is substituted into
  templates at deploy time (see [Label](TN0303_label.md)). `LabelService.add(jwtUser, name,
  identifier)` accepts the identifier but currently has an empty body — recorded verbatim from
  the source.

### Organization and user — tenant and login account

`Organization.identifier` identifies the tenant; `User.identifier` is the login account name.
Login is performed against the identifier (`UserService.login(userIdentifier, password)`,
looked up via `UserDao.getByIdentifier`), and the issued JWT carries it as the token subject
(`JwtComponent` calls `.subject(user.identifier)`).

Models **without** an identifier are addressed by other keys: a [Template](TN0401_template.md)
by its `name` / computed `pathname`, an [Article](TN0501_article.md) and a
[Link](TN0503_link.md) by primary key and list membership.

## Relationships

- **[Project](TN0301_project.md)** — one project identifier (`1`) names two (`2`, one per
  `BucketType`) [Bucket](TN0702_bucket.md)s; the identifier is copied into the project's `1`
  root / recycle-bin template names and its `1` root navigation identifier at creation.
- **[Navigation](TN0601_navigation.md)** — each node's identifier is unique among the children
  (`*`) of one parent (`1`); the chain of ancestor identifiers forms the page path.
- **[Article List](TN0502_article_list.md)**, **[Link List](TN0504_link_list.md)**,
  **[Label](TN0303_label.md)** — each identifier is scoped to one (`1`) owning project and may
  be referenced by many (`*`) [Pager Tag](TN0403_pager_tag.md) occurrences in templates.
- **[Organization](TN0201_organization.md)** / **[User](TN0202_user.md)** — system-wide unique;
  the user identifier is the JWT subject of every session token.
