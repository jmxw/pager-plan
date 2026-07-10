# Spec Template

**The specification doc must include all of the following items.** If an item is not applicable, enter `N/A`.

## How to Write Each Item

### 1. Screen Name

- Enter the **name** of the screen (e.g. `公司配置`)

### 2. Ref

- Attach the reference link (plan doc, design etc.) here

### 3. Statuses

- List **all possible states** that can occur on this screen, including modal dialogs

Example:

| Name | Description |
| --- | --- |
| Default | This is the default status of this screen |
| Search confirmation modal is open | The Search confirmation modal appears after clicking the Search button |
| Delete confirmation modal is open | The delete confirmation modal appears after clicking the Delete button |
| Specification tab active | The Specification tab is currently active |

### 4. Component Parameters

If this doc is for a component and a value is passed as a parameter, specify it here

Example:

| Name | Type | Description |
| --- | --- | --- |
| targetItemName | String | The name of the target item to be edited |

### 5. Design

- See [Design](./design.md) for how to write this section (wireframe + annotations)
- Each component is described in a table: `No | Name | Description | No Permission Display`

### 6. Validation

- See [Validation](./validation.md)

### 7. Permission

- Define the required permissions to access this screen
- Meaning of the Required column: ✅ = without this permission, this screen is not accessible / ❌ = this permission is not required, just used to control some actions

Example:

| Permission | Required | Description |
| --- | --- | --- |
| `XXX_MANAGE` | ✅ | Without this permission, this screen is not accessible |
| `YYY_WRITE` | ❌ | This permission is not required, just used to control some actions |

### 8. Endpoint

- If the target page has an endpoint (screen URL), specify it here (e.g. `/xxx/:id`)

### 9. Apis

- If the target page uses APIs, list them here with a link to the API detail document
- Example: `api 1: GET /v1/xxx`

### 10. Functions

- See [Functions](./functions.md)

### 11. Topics

- Document any additional topics not covered in the sections above