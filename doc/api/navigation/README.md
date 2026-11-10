# NavigationController

API docs for the endpoints of
[`NavigationController.kt`](/source/pager-backend/app/cms/src/main/kotlin/com/xwkj/cms/controller/NavigationController.kt).
One file per endpoint; see the parent [API index](../README.md) and
[rules/api_docs.md](../../../rules/api_docs.md).

**†** marks a stub endpoint (declared, but the handler body is empty); its doc is extended
together with the implementation (same-PR sync rule).

| Doc | Method | Path | Permission | Summary |
| --- | --- | --- | --- | --- |
| [get_navigation.md](get_navigation.md) † | GET | `/v1/navigation/{navigationId}` | `PROJECT_DEV` | Get detail information of a navigation menu. |
| [add_navigation_list.md](add_navigation_list.md) | POST | `/v1/navigation/{navigationId}/list` | `PROJECT_DEV` | Add a new list type navigation menu to a parent navigation menu. |
| [add_navigation_article.md](add_navigation_article.md) | POST | `/v1/navigation/{navigationId}/article` | `PROJECT_DEV` | Add a new article type navigation menu to a parent navigation menu. |
| [get_navigation_children.md](get_navigation_children.md) | GET | `/v1/navigation/{navigationId}/children` | `PROJECT_DEV` | Get project, parent navigations, children navigations and the navigation itself. |
| [move_navigation.md](move_navigation.md) † | PATCH | `/v1/navigation/{navigationId}/move_to/{targetNavigationId}` | `PROJECT_DEV` | Move a navigation menu as a child of an other parent navigation menu. |
| [move_up.md](move_up.md) | PATCH | `/v1/navigation/{navigationId}/up` | `PROJECT_DEV` | Move navigation menu up |
| [move_down.md](move_down.md) | PATCH | `/v1/navigation/{navigationId}/down` | `PROJECT_DEV` | Move navigation menu down |
| [delete_navigation.md](delete_navigation.md) | DELETE | `/v1/navigation/{navigationId}` | `PROJECT_DEV` | Delete a navigation menu. |
