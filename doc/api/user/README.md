# UserController

API docs for the endpoints of
[`UserController.kt`](/source/pager-backend/app/cms/src/main/kotlin/com/xwkj/cms/controller/UserController.kt).
One file per endpoint; see the parent [API index](../README.md) and
[rules/api_docs.md](../../../rules/api_docs.md).

**†** marks a stub endpoint (declared, but the handler performs no work); its doc is extended
together with the implementation (same-PR sync rule).

| Doc | Method | Path | Permission | Summary |
| --- | --- | --- | --- | --- |
| [login.md](login.md) | POST | `/v1/user/login` | N/A (public) | User login |
| [logout.md](logout.md) | GET | `/v1/user/logout` | N/A (authenticated) | User logout |
| [get_users.md](get_users.md) | GET | `/v1/user` | `USER_READ` | Get all visible users of an organization. |
| [get_available_roles.md](get_available_roles.md) | GET | `/v1/user/roles` | `USER_READ` | Get all available user roles. |
| [add_admin.md](add_admin.md) | POST | `/v1/user/admin` | `ADMIN_WRITE` | Add an admin user. |
| [add_developer.md](add_developer.md) | POST | `/v1/user/developer` | `USER_WRITE` | Add a developer user. |
| [add_customer.md](add_customer.md) | POST | `/v1/user/customer` | `USER_WRITE` | Add a customer user. |
| [get_me.md](get_me.md) | GET | `/v1/user/me` | `USER_READ` | Get user information of the current user. |
| [edit_user.md](edit_user.md) † | PATCH | `/v1/user/{userId}` | `USER_WRITE` | Get user information by userId. |
| [delete_user.md](delete_user.md) † | DELETE | `/v1/user/{userId}` | `USER_WRITE` | Delete user. |

Note: the `edit_user` summary is copied verbatim from the source, where it reads like a
copy-paste leftover ("Get user information by userId."); correcting it belongs to the PR that
implements the endpoint.
