# Validation Rules

## Rules

- Validation is documented in the order: **target field → condition → error message → trigger timing**
- The terminology follows the [terminology page](https://ebara-d3.atlassian.net/wiki/x/AgBKC)
- Error messages (text shown on screen) are wrapped in code spans

## Example

The target field name is written in **bold**; conditions and triggers are bullet items:

```markdown
**Project name input area**

- not empty
  - error message: `请输入项目名称`
  - trigger: blur, change, submit
- max length: 255
  - error message: `项目名称不能超过255个字`
  - trigger: blur, change
```
