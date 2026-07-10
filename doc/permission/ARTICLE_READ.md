# ARTICLE_READ

> Area: Article
> Roles: OWNER, ADMIN, DEVELOPER, CUSTOMER

Search and read articles, and search article lists.

## Description

Grants read access to articles: searching articles, reading a single article, and searching article
lists. Held by all four roles. Note that the article-list **search** endpoint declares
`ARTICLE_READ` (not `ARTICLE_LIST_READ`), so it is listed here; reading a specific list and its
items is governed by [`ARTICLE_LIST_READ`](ARTICLE_LIST_READ.md).

## Gated Endpoints

| Method | Path | Endpoint Doc |
| --- | --- | --- |
| POST | `/v1/article/search` | ⬜ [search_articles.md](/doc/api/article/search_articles.md) |
| GET | `/v1/article/{articleId}` | ⬜ [get_article.md](/doc/api/article/get_article.md) |
| POST | `/v1/article_list/search` | ⬜ [search_article_lists.md](/doc/api/article_list/search_article_lists.md) |
