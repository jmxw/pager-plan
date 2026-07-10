# Article apis (ArticleController)

Apis to manage articles of a project

## Endpoints

| Method | Path | Summary | Doc |
| --- | --- | --- | --- |
| POST | `/v1/article/search` | Search articles in a project. | [search_articles.md](/doc/api/article/search_articles.md) |
| POST | `/v1/article` | Add a new article into a project. | [add_article.md](/doc/api/article/add_article.md) |
| GET | `/v1/article/{articleId}` | Get detail information of an article. | [get_article.md](/doc/api/article/get_article.md) |
| PUT | `/v1/article/{articleId}` | Edit detail information of an article. | [edit_article.md](/doc/api/article/edit_article.md) |
| DELETE | `/v1/article/{articleId}` | Delete an article. | [delete_article.md](/doc/api/article/delete_article.md) |
| POST | `/v1/article/{articleId}/cover` | Upload cover image for an article. | [upload_article_cover.md](/doc/api/article/upload_article_cover.md) |
