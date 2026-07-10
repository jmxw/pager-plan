# Article List (ArticleListController)

Apis to manage article lists of a project

## Endpoints

| Method | Path | Summary | Doc |
| --- | --- | --- | --- |
| POST | `/v1/article_list/search` | Search article lists in a project. | [search_article_lists.md](/doc/api/article_list/search_article_lists.md) |
| POST | `/v1/article_list` | Add a new article list into a project. | [add_article_list.md](/doc/api/article_list/add_article_list.md) |
| PUT | `/v1/article_list/{articleListId}` | Edit an article list. | [edit_article_list.md](/doc/api/article_list/edit_article_list.md) |
| DELETE | `/v1/article_list/{articleListId}` | Delete an article list. | [delete_article_list.md](/doc/api/article_list/delete_article_list.md) |
| GET | `/v1/article_list/{articleListId}` | Get detail information of an article list. | [get_article_list.md](/doc/api/article_list/get_article_list.md) |
| POST | `/v1/article_list/{articleListId}/items` | Get all items of an article list. | [get_article_list_items.md](/doc/api/article_list/get_article_list_items.md) |
| PATCH | `/v1/article_list/{articleListId}/article/{articleId}` | Add an existing article into an article list. | [add_article_to_list.md](/doc/api/article_list/add_article_to_list.md) |
| DELETE | `/v1/article_list/{articleListId}/article/{articleId}` | Delete and article from an article list. | [delete_article_from_list.md](/doc/api/article_list/delete_article_from_list.md) |
