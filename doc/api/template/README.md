# Template apis (TemplateController)

Apis to manage project template files.

## Endpoints

| Method | Path | Summary | Doc |
| --- | --- | --- | --- |
| DELETE | `/v1/template/{templateId}` | Move a template to recycle bin. | [move_template_to_recycle_bin.md](/doc/api/template/move_template_to_recycle_bin.md) |
| POST | `/v1/template/{templateId}/add` | Add a new template to a parent template. | [add_template.md](/doc/api/template/add_template.md) |
| POST | `/v1/template/{templateId}/upload` | Upload a new template file to a parent template. | [upload_template.md](/doc/api/template/upload_template.md) |
| GET | `/v1/template/{templateId}/directory` | Get project, parent templates, child templates and the template it self. | [read_directory.md](/doc/api/template/read_directory.md) |
| GET | `/v1/template/{templateId}/content` | Get the text content of a text template file. | [read_file_content.md](/doc/api/template/read_file_content.md) |
| PATCH | `/v1/template/{templateId}/content` | Update the text content of a text template file. | [write_file_content.md](/doc/api/template/write_file_content.md) |
| GET | `/v1/template/{templateId}/binary` | Get url of a binary template file. | [get_binary_template_presigned_url.md](/doc/api/template/get_binary_template_presigned_url.md) |
| PATCH | `/v1/template/{templateId}/rename` | Rename a template. | [rename_template.md](/doc/api/template/rename_template.md) |
| PATCH | `/v1/template/{templateId}/copy_to/{targetTemplateId}` | Copy a template and its children as a child of the target template. | [copy_template.md](/doc/api/template/copy_template.md) |
| PATCH | `/v1/template/{templateId}/move_to/{targetTemplateId}` | Move a template and its children as a child of the target template. | [move_template.md](/doc/api/template/move_template.md) |
