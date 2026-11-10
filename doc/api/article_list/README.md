# ArticleListController

API docs for the endpoints of
[`ArticleListController.kt`](/source/pager-backend/app/cms/src/main/kotlin/com/xwkj/cms/controller/ArticleListController.kt).
One file per endpoint; see the parent [API index](../README.md) and
[rules/api_docs.md](../../../rules/api_docs.md).

| Doc | Method | Path | Permission | Summary |
| --- | --- | --- | --- | --- |
| [search_article_lists.md](search_article_lists.md) | POST | `/v1/article_list/search` | `ARTICLE_READ` | Search article lists in a project. |
| [add_article_list.md](add_article_list.md) | POST | `/v1/article_list` | `ARTICLE_LIST_WRITE` | Add a new article list into a project. |
| [edit_article_list.md](edit_article_list.md) | PUT | `/v1/article_list/{articleListId}` | `ARTICLE_LIST_WRITE` | Edit an article list. |
| [delete_article_list.md](delete_article_list.md) | DELETE | `/v1/article_list/{articleListId}` | `ARTICLE_LIST_WRITE` | Delete an article list. |
| [get_article_list.md](get_article_list.md) | GET | `/v1/article_list/{articleListId}` | `ARTICLE_LIST_READ` | Get detail information of an article list. |
| [get_article_list_items.md](get_article_list_items.md) | POST | `/v1/article_list/{articleListId}/items` | `ARTICLE_LIST_READ` | Get all items of an article list. |
| [add_article_to_list.md](add_article_to_list.md) | PATCH | `/v1/article_list/{articleListId}/article/{articleId}` | `ARTICLE_LIST_WRITE` | Add an existing article into an article list. |
| [delete_article_from_list.md](delete_article_from_list.md) | DELETE | `/v1/article_list/{articleListId}/article/{articleId}` | `ARTICLE_LIST_WRITE` | Delete and article from an article list. |
