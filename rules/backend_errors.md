## Structure Rules

- Every BE error document belongs to a **category**
- Each error doc is created as a **child page** of its category doc 
- **For md-file usage**: a category is expressed as a subfolder

```
doc/backend_error/
├── README.md              # category index
├── <category-a>/
│   ├── README.md          # error list inside the category
│   ├── <error-code-1>.md  # 1 error code = 1 file
│   └── <error-code-2>.md
└── <category-b>/
    └── ...
```

## Templates

- **For md-file usage**: place equivalent md templates under `_format/` and copy them (never edit templates in `_format/` directly). At minimum the following items are included:

```markdown
# <ERROR_CODE> <EnumConstant>

> Category: <category> | HTTP Status: <4xx/5xx>

## Error Message

`<error message text with {placeholder}>`

## Placeholders

| Placeholder | Description |
| --- | --- |
| `{placeholder}` | <what it is replaced with> |

## When Returned

<condition(s) under which this error is returned>
```

## Content Rules

1. Each BE error doc contains the **error code and error message**, which must be **the same as the backend implementation**
2. When the error message contains a **placeholder**, the placeholder must be explained

## Update Rule (Important)

- Once the backend implementation is updated, the BE error docs **must be updated at the same time**
- Reason: BE error docs are **used in the Swagger API doc**, so any gap from the implementation directly becomes a wrong API spec
- For md-file usage, this sync is guaranteed by including the implementation change and the doc update **in the same PR**
