# Doc Rules

This folder summarizes the the rules that work with markdown files.

## Quick Reference — All Rules at a Glance

1. **Common**: link to related docs instead of copying text; relative paths inside the repo, full URLs outside. Write in passive voice; do not use personal pronouns as subjects
2. **Tech Noun is the source of all terminology**: never invent similar terms; always link to the existing Tech Noun definition
3. **Spec doc is written in English** and must contain every item of the [Template](./template.md). Not applicable = `N/A`, undecided = `TBD`
4. **On-screen text goes in code spans / code blocks**; parameters use the `{parameter}` format
5. **Validation** is documented in the order: target field → condition → error message → trigger timing
6. **Design** should be generated with SVG, only wireframe is generated.
7. **Functions** must answer 3 questions: UI/UX, business logic, error handling. Explanations longer than 3 lines go to the Functions section
8. **Backend Errors** must always stay in sync with the backend implementation (they are used in the Swagger API doc)
9. **API Docs**: one file per endpoint, grouped by controller; method/path/summary copied verbatim from the controller, errors and Tech Nouns referenced by link (never copied), kept in sync in the same PR
10. **Permissions**: one file per `UserPermission` value, grouped by area; the enum constant copied verbatim, gated endpoints referenced by link (the reverse index of each API doc's permission), kept in sync in the same PR
