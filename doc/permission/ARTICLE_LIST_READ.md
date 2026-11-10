# ARTICLE_LIST_READ

> Area: Article List
> Roles: OWNER, ADMIN, DEVELOPER, CUSTOMER

Read an article list and its member articles.

## Description

Grants read access to an article list — the predefined, id-addressable collection a
`{pager:list}` template tag renders (see the [template tag reference](/plan/common/template-tags.md)):
reading a single list and paging through its member articles. Held by all four roles.

## Gated Endpoints

| Method | Path | Endpoint Doc |
| --- | --- | --- |
| GET | `/v1/article_list/{articleListId}` | [get_article_list.md](/doc/api/article_list/get_article_list.md) |
| POST | `/v1/article_list/{articleListId}/items` | [get_article_list_items.md](/doc/api/article_list/get_article_list_items.md) |
