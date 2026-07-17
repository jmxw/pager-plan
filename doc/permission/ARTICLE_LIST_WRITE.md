# ARTICLE_LIST_WRITE

> Area: Article List
> Roles: OWNER, ADMIN, DEVELOPER, CUSTOMER

Create, edit, and delete article lists and manage their membership.

## Description

Grants management of article lists: creating, editing, and deleting a list, and adding or removing
an article from a list. An article list is the predefined collection a `{pager:list}` tag renders
(see the [template tag reference](/plan/common/template-tags.md)). Held by all four roles.

## Gated Endpoints

| Method | Path | Endpoint Doc |
| --- | --- | --- |
| POST | `/v1/article_list` | [add_article_list.md](/doc/api/article_list/add_article_list.md) |
| PUT | `/v1/article_list/{articleListId}` | [edit_article_list.md](/doc/api/article_list/edit_article_list.md) |
| DELETE | `/v1/article_list/{articleListId}` | [delete_article_list.md](/doc/api/article_list/delete_article_list.md) |
| PATCH | `/v1/article_list/{articleListId}/article/{articleId}` | [add_article_to_list.md](/doc/api/article_list/add_article_to_list.md) |
| DELETE | `/v1/article_list/{articleListId}/article/{articleId}` | [delete_article_from_list.md](/doc/api/article_list/delete_article_from_list.md) |
