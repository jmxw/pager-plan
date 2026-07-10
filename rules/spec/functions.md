# Function

## When to Use the Functions Section

- If the content cannot be fully described within the [Design](./design.md) description field (as a guideline, **when it exceeds 3 lines**), it is documented in the Functions section
- **Only functions that are directly related are included under a single feature. Multiple unrelated functions must not be grouped into one**

## Referencing Design Components

- If a function is related to a component described in the [Design](./design.md) section, the component number **must be referenced from the function with an anchor link**
- **Replacement for md**: put an HTML anchor in the component table cell and reference it from the function.
  - Components are numbered **continuously across the whole file** (1 … N), so both the anchor id and the displayed `No` are unique without a per-status prefix. The id is `component-<n>` and the `No` cell shows just the number `<n>`; the same number is the on-image badge (see [Design](./design.md#how-to-assign-numbers-to-images))
  - Example: the component's first appearance defines the anchor — in the `No` cell write `<a id="component-8"></a>8`. A status that **reuses** it does not redefine the anchor: its `No` cell is a plain linked number `[8](#component-8)` and the description says "Same as No.8". In prose elsewhere, reference a component as `[No.8](#component-8)`

## The 3 Questions a Function Must Answer

Functions are the most important part of a PRD doc. Each function must answer:

1. What is the UI/UX for this function?
2. What is the business logic for this function?
3. How is the error handled?

If the function is related to any of these topics, that part must be described in the Functions section.

## UI/UX

When the user triggers an action on the page (e.g. clicks "Save", "Delete", or "Create"), specify the following behaviors:

### Trigger Conditions and States

- Trigger: button click / form submit / key press / drag-and-drop
- Prevent repeated operations: disable the button or show a loading spinner
- Data validation: perform basic front-end checks (required fields, format, length, range)
- UI feedback:
  - **In progress:** show a loading animation or disable inputs
  - **Success:** show a success message, refresh or redirect the page
  - **Failure:** display an error message based on the returned error code
- etc.

## Business Logic

The steps to collect, handle, and save data must be described.

### Request Sending

- The API used to send the request

### Request Parsing and Validation

Normally, validation is checked on the frontend side. To prevent attacks, the same validation is also implemented on the backend side — this does not need to be described again in the function. However, **validations that can only be checked on the backend side must be described** in the business logic part:

- Verify authentication and permissions
- Validate parameters (required fields, type, range)
- Check idempotency: if the same request key exists, return the previous result
- etc.

### Business Logic Execution

- Perform database operations (create, update, delete, query)
- Enforce business rules (e.g. no duplicate names, valid status transitions)
- If asynchronous tasks are needed (e.g. generate report, send email), enqueue them in a message queue
- etc.

### Response

- Only the **success data** is described here
- Error data has a unified data format in the DevNavi system; the error handling part contains the error codes for this function

## Error Handling

- Error cases are described separately when error handling is necessary, in a table with 3 columns:

| Column | Content |
| --- | --- |
| **Case** | What the error case is. Must not duplicate other cases of the same function, **even when the error codes are the same** |
| **Error Code** | The backend error code defined in Backend Errors|
| **Action** | The next action after receiving this error code |

- **Describe only the possible error cases.** Even if more error codes are defined for the API used by this function, an error case is not needed for a code that can never be returned due to UI/UX restrictions
