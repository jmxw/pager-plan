# Permissions

One doc per [`UserPermission`](/source/pager-backend/domain/src/main/kotlin/com/xwkj/pager/domain/model/enum/UserPermission.kt)
value. The enum constant is copied verbatim into the file name and heading; gated endpoints are
referenced by link (the reverse index of each API doc's `Permission`). Authoring conventions live
in [rules/permissions.md](../../rules/permissions.md).

Permissions are granted per **role**, not per user: each
[`UserRole`](/source/pager-backend/domain/src/main/kotlin/com/xwkj/pager/domain/model/enum/UserRole.kt)
maps to a fixed permission set (`UserRole.permissions`), and `UserPermissionInterceptor` admits a
request when the caller's role holds the endpoint's `@Permitted` value. Role hierarchy:
OWNER > ADMIN > DEVELOPER > CUSTOMER.

| Permission | Area | Held by roles |
|---|---|---|
| [ADMIN_WRITE](ADMIN_WRITE.md) | User | OWNER |
| [USER_READ](USER_READ.md) | User | OWNER, ADMIN |
| [USER_WRITE](USER_WRITE.md) | User | OWNER, ADMIN |
| [ACCESS_KEY_READ](ACCESS_KEY_READ.md) | Access Key | OWNER, ADMIN |
| [ACCESS_KEY_WRITE](ACCESS_KEY_WRITE.md) | Access Key | OWNER, ADMIN |
| [PROJECT_READ](PROJECT_READ.md) | Project | OWNER, ADMIN, DEVELOPER, CUSTOMER |
| [PROJECT_WRITE](PROJECT_WRITE.md) | Project | OWNER, ADMIN |
| [PROJECT_DEV](PROJECT_DEV.md) | Navigation / Template | OWNER, ADMIN, DEVELOPER |
| [PROJECT_DEPLOY](PROJECT_DEPLOY.md) | Project | OWNER, ADMIN, DEVELOPER, CUSTOMER |
| [ARTICLE_LIST_READ](ARTICLE_LIST_READ.md) | Article List | OWNER, ADMIN, DEVELOPER, CUSTOMER |
| [ARTICLE_LIST_WRITE](ARTICLE_LIST_WRITE.md) | Article List | OWNER, ADMIN, DEVELOPER, CUSTOMER |
| [ARTICLE_READ](ARTICLE_READ.md) | Article | OWNER, ADMIN, DEVELOPER, CUSTOMER |
| [ARTICLE_WRITE](ARTICLE_WRITE.md) | Article | OWNER, ADMIN, DEVELOPER, CUSTOMER |
| [LABEL_READ](LABEL_READ.md) | Label | None (no role grants this) |
| [LABEL_WRITE](LABEL_WRITE.md) | Label | None (no role grants this) |

## Ungated endpoints

These endpoints declare **no** `@Permitted` annotation, so they belong to no permission doc — the
interceptor lets any request through the permission check. Listed here for completeness:

| Method | Path | Handler |
|---|---|---|
| POST | `/v1/organization/register` | `OrganizationController.register` |
| POST | `/v1/user/login` | `UserController.login` |
| GET | `/v1/user/logout` | `UserController.logout` |
| PUT | `/v1/access_key/{accessKeyId}` | `AccessKeyController.editAccessKey` |
| PATCH | `/v1/project/{projectId}/force_deploy/{phase}` | `ProjectController.forceDeploy` |
