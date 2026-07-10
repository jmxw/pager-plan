## Positioning

- Tech Noun defines the basic concepts of the Dev Navi system
- Tech Noun is **the source of terminology for all specification documents**. The names and concepts must be used by every other spec doc
- **Prohibited**: DO NOT use self-created nouns which have similar meaning with the existing nouns in the Tech Noun doc

## Content

Tech Noun only defines basic concepts and describes the data structure of the project. Specific specifications (UI/UX, etc.) must not be included.

### Included

- Original names, to help members build the connection between dev and UI
- Explanation of a data model and its related data models
- Explanation of important fields of a data model, especially fields defined by enum classes
- Relationships among multiple data models

### Not Included

- Text for i18n
- Anything related to specific UI/UX specifications

### Optional

- Underlying logic and relationships among multiple objects that are difficult to describe from the PRD
- Any other topics to be discussed

## Update

- Once an important model is created / updated / deleted, or a field of a model is created / updated / deleted, the corresponding documentation must be created / updated / deleted accordingly

## Notes for md-file Usage

- Existing Tech Noun definitions live in Confluence, so md specs link to them with links.
- If Tech Noun itself is managed as md files, use "1 noun = 1 file" named `TNxxx_<name>.md`, grouped in a folder with a `README.md` index

## ID Numbering

- TN IDs are 4 digits, allocated **by group**: the **first 2 digits are the group number**,
  the last 2 digits are the noun's sequence inside the group (e.g. `TN0203` = group `02`,
  noun `03`), so new nouns can always be added inside their group later.
- A new noun takes the next free number **within its group**. IDs are intentionally not
  globally continuous — do not compact the gaps between groups.
- Numbers are stable once assigned: never renumber an existing noun and never reuse a deleted
  number.

## Relationship Diagrams

- Draw the relationships among data models as a **Mermaid `classDiagram`** (UML style), not an
  `erDiagram` — the class diagram shows plain field names without forcing a type/description on
  every line.
- Inside each class box, list **only the very important fields and the reference (FK) fields** —
  not every column. Keep the identity/discriminator fields (e.g. `name`, `type`) and the fields
  that point to other models; drop descriptive-only extras (the full field list already lives in
  the noun's **Important fields** table).
- Keep each relationship as its own labeled edge (do not merge two references into one line),
  and annotate cardinality on the edge (e.g. `"1"`, `"*"`, `"0..1"`).
