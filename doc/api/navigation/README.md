# Navigation (NavigationController)

Apis to manager virtual navigation menus of a project.

## Endpoints

| Method | Path | Summary | Doc |
| --- | --- | --- | --- |
| GET | `/v1/navigation/{navigationId}` | Get detail information of a navigation menu. | [get_navigation.md](/doc/api/navigation/get_navigation.md) |
| POST | `/v1/navigation/{navigationId}/list` | Add a new list type navigation menu to a parent navigation menu. | [add_navigation_list.md](/doc/api/navigation/add_navigation_list.md) |
| POST | `/v1/navigation/{navigationId}/article` | Add a new article type navigation menu to a parent navigation menu. | [add_navigation_article.md](/doc/api/navigation/add_navigation_article.md) |
| GET | `/v1/navigation/{navigationId}/children` | Get project, parent navigations, children navigations and the navigation itself. | [get_navigation_children.md](/doc/api/navigation/get_navigation_children.md) |
| PATCH | `/v1/navigation/{navigationId}/move_to/{targetNavigationId}` | Move a navigation menu as a child of an other parent navigation menu. | [move_navigation.md](/doc/api/navigation/move_navigation.md) |
| PATCH | `/v1/navigation/{navigationId}/up` | Move navigation menu up | [move_up.md](/doc/api/navigation/move_up.md) |
| PATCH | `/v1/navigation/{navigationId}/down` | Move navigation menu down | [move_down.md](/doc/api/navigation/move_down.md) |
| DELETE | `/v1/navigation/{navigationId}` | Delete a navigation menu. | [delete_navigation.md](/doc/api/navigation/delete_navigation.md) |
