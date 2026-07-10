# <ERROR_CODE> <EnumConstant>

> Category: <category> | HTTP Status: <400 | 500 (returned as `-9999 UnknownError`) | N/A (consumer, no HTTP response)>

<One-line summary of what went wrong, from the API caller's point of view.>

## Error Message

`<error message text with {placeholder}, copied verbatim from PagerErrorMessage.kt — including typos>`

## Placeholders

| Placeholder | Description |
| --- | --- |
| `{placeholder}` | <what it is replaced with; placeholders are positional, describe them in order> |

Write `N/A` when the message contains no placeholder.

## When Returned

<Condition(s) under which this error is returned. Start from the class(es) in the index's
"Thrown from" column, linking each to its source through the `/source/pager-backend/...`
symlink. Name the triggering endpoint(s) by their verbatim `METHOD /path` (link the API doc
`/doc/api/<controller>/<handler>.md` once it exists — the API doc is the forward reference,
this doc is its target), and link domain terms to their Tech Nouns (`/doc/tech_noun/...`) on
first mention — never copy their content.>
