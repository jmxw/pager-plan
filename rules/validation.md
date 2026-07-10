# Validation Rules

## Rules

- Validation is documented in the order: **target field → condition → error message → trigger timing**
- Error messages (text shown on screen) are wrapped in spec doc

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
