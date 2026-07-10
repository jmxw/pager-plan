# Common Rules

Common rules when creating or updating specification documents:

1. **Reference related documents with links, not copied text**
   - Links are used so that readers can navigate to the related docs; content is not copy-pasted
   - Writing the same content in multiple files causes update leaks, so each definition lives in one place and is referenced by link
2. **Write links so they do not break** 
   - For documents inside the repo: use **relative path links** `[link text](../path/file.md)`
   - For external documents (Confluence / JIRA / Figma): use full URLs (for Confluence, the `wiki/x/` tiny link format is recommended)
3. **Passive voice is strongly recommended**
   - Personal pronouns (I / we / you, etc.) should not be used as subjects
   - Example: ~~You should click the button~~ → The button is clicked to ...