# TemplateController

API docs for the endpoints of
[`TemplateController.kt`](/source/pager-backend/app/cms/src/main/kotlin/com/xwkj/cms/controller/TemplateController.kt).
One file per endpoint; see the parent [API index](../README.md) and
[rules/api_docs.md](../../../rules/api_docs.md).

| Doc | Method | Path | Permission | Summary |
| --- | --- | --- | --- | --- |
| [move_template_to_recycle_bin.md](move_template_to_recycle_bin.md) | DELETE | `/v1/template/{templateId}` | `PROJECT_DEV` | Move a template to recycle bin. |
| [add_template.md](add_template.md) | POST | `/v1/template/{templateId}/add` | `PROJECT_DEV` | Add a new template to a parent template. |
| [upload_template.md](upload_template.md) | POST | `/v1/template/{templateId}/upload` | `PROJECT_DEV` | Upload a new template file to a parent template. |
| [read_directory.md](read_directory.md) | GET | `/v1/template/{templateId}/directory` | `PROJECT_DEV` | Get project, parent templates, child templates and the template it self. |
| [read_file_content.md](read_file_content.md) | GET | `/v1/template/{templateId}/content` | `PROJECT_DEV` | Get the text content of a text template file. |
| [write_file_content.md](write_file_content.md) | PATCH | `/v1/template/{templateId}/content` | `PROJECT_DEV` | Update the text content of a text template file. |
| [get_binary_template_presigned_url.md](get_binary_template_presigned_url.md) | GET | `/v1/template/{templateId}/binary` | `PROJECT_DEV` | Get url of a binary template file. |
| [rename_template.md](rename_template.md) | PATCH | `/v1/template/{templateId}/rename` | `PROJECT_DEV` | Rename a template. |
| [copy_template.md](copy_template.md) | PATCH | `/v1/template/{templateId}/copy_to/{targetTemplateId}` | `PROJECT_DEV` | Copy a template and its children as a child of the target template. |
| [move_template.md](move_template.md) | PATCH | `/v1/template/{templateId}/move_to/{targetTemplateId}` | `PROJECT_DEV` | Move a template and its children as a child of the target template. |
