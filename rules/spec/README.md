# Spec Rules

## Content

- All items defined in the template.md **must be included** in the PRD doc
- Content not related to the template items is not recommended to be appended directly; when necessary, it is described in the **Topics** section

### Common Rules for the Main Content

1. **Heading levels** (Confluence rule: Heading 1 / Heading 3. Replaced for md as follows)
   - Major sections: `#` (heading level 1)
   - Sub-sections: `###` (heading level 3)
   - Body content: normal text
2. By default, all content is created **in English**
3. **A link is inserted for each screen** defined as a separate PRD page (relative path links in md)
4. When a predefined term (a domain model / tech noun) is used, the **first mention** in the doc is linked inline to its [Tech Noun](../tech_noun.md) entry; later mentions in the same doc are plain text (same convention as the API docs). Terms must not be redefined based on personal judgment — the Tech Noun entry is the single source of truth
5. For **non-applicable items**, write `N/A`. For **undecided items**, write `TBD`
6. If unrelated topics are mixed in the same file, **separate them into different files or subfolders** (← follow this thoroughly)
   - Clearly distinguish between parent (folder + index) and child (file) when documenting. Carefully consider the hierarchy before adding or organizing content
7. **Capitalization rules**
   - Titles: capitalize the first letter of each word (Title Case)
   - All other text: capitalize only the first letter of the first word (Sentence case)
8. **Review feedback**: in Confluence, reviewers must use the comment feature. For md usage, reviewers use **GitHub PR review comments (line comments / suggestions)** to indicate suggested revisions
9. **Page (screen) numbering** can only be determined by the specification owner. If the numbering is to be assigned later, the placeholder format `SPXX_XX` is used
10. **Text shown directly on the screen goes in a code span / code block**
    - If the text contains a parameter, the parameter is wrapped in `{}`, like `text {parameter}`

### File & Folder Layout

**One spec = one folder.** Because a spec owns its wireframes (one per status), keep each spec
self-contained in its own folder rather than a flat `.md` beside a shared image pile:

```
<spec-name>/
├── README.md                # the spec itself (the 11 template items)
└── svg/
    ├── <status>.drawio.svg  # editable draw.io SVG; renders in the doc AND opens in draw.io
    └── <status>.drawio.svg
```

- The spec is the folder's `README.md`, so a link to the folder resolves to the spec.
- Each status has a `svg/<status>.drawio.svg` — draw.io's editable-SVG format, which is both the
  image the doc embeds and the file you open in draw.io to edit (no separate `.drawio` source). The
  doc embeds it with a **relative** path `![...](./svg/<status>.drawio.svg)` — never a shared
  cross-spec image folder. This keeps a spec movable, avoids filename collisions between screens,
  and keeps the wireframes next to the doc that references them. See the
  [Example Spec](./example/login/README.md).

## Update PRD

- When a new feature, screen, or definition is added, modified, or removed, the corresponding documentation must also be created, updated, or deleted accordingly

## Detailed Rules per Part

The detailed rules for each part are described separately:

- [PRD Template](./template.md) — required items
- [PRD Validation](./validation.md) — how to write validations
- [PRD Design](./design.md) — how to draw the wireframe and annotate components
- [PRD Functions](./functions.md) — how to write function specs
- [Example Spec](./example/login/README.md) — a filled-in spec (the login screen) that satisfies all 11 template items
