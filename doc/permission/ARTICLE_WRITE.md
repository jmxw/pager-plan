# ARTICLE_WRITE

> Area: Article
> Roles: OWNER, ADMIN, DEVELOPER, CUSTOMER

Create, edit, and delete articles and upload article covers.

## Description

Grants management of articles: creating, editing, and deleting an article, and uploading an
article's cover image. Held by all four roles, so a customer can author the content of their own
site.

## Gated Endpoints

| Method | Path | Endpoint Doc |
| --- | --- | --- |
| POST | `/v1/article` | [add_article.md](/doc/api/article/add_article.md) |
| PUT | `/v1/article/{articleId}` | [edit_article.md](/doc/api/article/edit_article.md) |
| DELETE | `/v1/article/{articleId}` | [delete_article.md](/doc/api/article/delete_article.md) |
| POST | `/v1/article/{articleId}/cover` | [upload_article_cover.md](/doc/api/article/upload_article_cover.md) |
