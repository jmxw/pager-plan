# Template Tag Reference

Pager renders website pages from templates by substituting **tags** at build time. This document
is the shared reference for the template-tag syntax; feature plans link here rather than
redefining tags. Two kinds of tags exist:

- **Block directives** — `{pager:...}` … `{/pager:...}`, which iterate or delimit a region.
- **Placeholders** — `[list:*]`, `[page:*]`, `{content:*}`, which are replaced by a single value.

## Article list — `{pager:list}`

Inserts an article list. The list `id` must be **predefined in the CMS**; a page cannot be
generated for an undefined list id.

```html
{pager:list id="listId"}
<a href="[list:href]">[list:title]</a>
{/pager:list}
```

Placeholders available **inside** a `{pager:list}` block:

| Placeholder | Description |
|---|---|
| `[list:href]` | Hyperlink to the article content page |
| `[list:title]` | Article title |
| `[list:cover]` | Article cover image; empty string when the article has no cover |
| `[list:author]` | Article's creating author |
| `[list:lastAuthor]` | Article's last editing author |
| `[list:publishAt]` | Article publish time |
| `[list:description]` | Article description |

## Pagination bar — `[page:bar]` / `{pager:pagebar}`

A pagination bar is only valid on an article-list page; it has no effect elsewhere.

**Built-in bar** — `[page:bar]` renders the complete built-in pagination bar:

```html
{pager:pagebar size="10"}
[page:bar]
{/pager:pagebar}
```

**Custom bar** — build a custom bar from these placeholders inside `{pager:pagelink}`:

| Placeholder | Description |
|---|---|
| `[page:total]` | Total number of pages |
| `[page:href]` | Page link |
| `[page:current]` | Current page number |
| `[page:currentClass]` | Current-page CSS class, marking whether a link is the current page |
| `[page:pre]` | Previous-page link |
| `[page:next]` | Next-page link |
| `[page:first]` | First-page link |
| `[page:last]` | Last-page link |
| `[page:index]` | Page number rendered inside `{pager:pagelink}` |

```html
<div>
    <a class="pager-page-item" href="[page:first]">首页</a>
    <a class="pager-page-item" href="[page:pre]">上一页</a>
    {pager:pagelink currentClass="pager-page-item-current"}
    <a class="pager-page-item [page:currentClass]" href="[page:href]">[page:index]</a>
    {/pager:pagelink}
    <a class="pager-page-item" href="[page:next]">下一页</a>
    <a class="pager-page-item" href="[page:last]">尾页</a>
</div>
```

## Article content — `{content:*}`

On an article content page, `{content:*}` placeholders are substituted into the content template:

| Placeholder | Description |
|---|---|
| `{content:title}` | Article title |
| `{content:author}` | Article author |
| `{content:createdAt}` | Creation time |
| `{content:updatedAt}` | Last-update time |
| `{content:content}` | Article body |

```html
<html>
  <head>
    <title>{content:title}</title>
  </head>
  <body>
    作者：{content:author}
    创建时间：{content:createdAt}
    更新时间: {content:updatedAt}
    正文内容：{content:content}
  </body>
</html>
```
