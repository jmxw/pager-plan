# ArticleController

API docs for the endpoints of
[`ArticleController.kt`](/source/pager-backend/app/cms/src/main/kotlin/com/xwkj/cms/controller/ArticleController.kt).
One file per endpoint; see the parent [API index](../README.md) and
[rules/api_docs.md](../../../rules/api_docs.md).

| Doc | Method | Path | Permission | Summary |
| --- | --- | --- | --- | --- |
| [search_articles.md](search_articles.md) | POST | `/v1/article/search` | `ARTICLE_READ` | Search articles in a project. |
| [add_article.md](add_article.md) | POST | `/v1/article` | `ARTICLE_WRITE` | Add a new article into a project. |
| [get_article.md](get_article.md) | GET | `/v1/article/{articleId}` | `ARTICLE_READ` | Get detail information of an article. |
| [edit_article.md](edit_article.md) | PUT | `/v1/article/{articleId}` | `ARTICLE_WRITE` | Edit detail information of an article. |
| [delete_article.md](delete_article.md) | DELETE | `/v1/article/{articleId}` | `ARTICLE_WRITE` | Delete an article. |
| [upload_article_cover.md](upload_article_cover.md) | POST | `/v1/article/{articleId}/cover` | `ARTICLE_WRITE` | Upload cover image for an article. |
