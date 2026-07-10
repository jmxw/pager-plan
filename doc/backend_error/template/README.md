# Template Errors

Backend error codes in the Template category (code range `60000`). One file per error code; see the parent [backend error index](../README.md) and [rules/backend_errors.md](../../../rules/backend_errors.md).

| Code | Enum Constant | HTTP | Message |
| --- | --- | --- | --- |
| [60001.md](60001.md) | `TemplateIdNotExist` | 400 | `Template id {} not exist.` |
| `60002.md` † | `TemplateNoPrivilege` | 400 | `No privilege to access template({})` |
| [60003.md](60003.md) | `TemplateNotBelongToProject` | 400 | `Template({}) does not belong to project({}).` |
| [60004.md](60004.md) | `TemplateNotTextFile` | 400 | `Template({}) is not a text file.` |
| [60005.md](60005.md) | `TemplateNotBinaryFile` | 400 | `Template({}) is not a binary file.` |
| [60006.md](60006.md) | `TemplateNameDuplicated` | 400 | `Template name {} is duplicated in the directory template({}).` |
| [60007.md](60007.md) | `TemplateDirectoryRequired` | 400 | `Template({}) is not a directory.` |
| [60008.md](60008.md) | `TemplateUploadFileSaveError` | 400 | `Error to save uploaded file for template({}).` |
| [60009.md](60009.md) | `TemplateFileNotExist` | 400 | `Template file not exist for template({}).` |
| [60010.md](60010.md) | `TemplateBinaryMetadataMissed` | 400 | `Metadata file for binary template({}) is missed.` |
| [60011.md](60011.md) | `TemplateRenameReject` | 400 | `Cannot rename ROOT or RECYCLE_BIN template({})` |
| [60012.md](60012.md) | `TemplateSameProjectRequired` | 400 | `The project of template({}) and target template({}) must be same.` |

† Declared in `PagerErrorMessage` but never thrown yet — the doc is written together with the code that first throws it (same-PR sync rule).
