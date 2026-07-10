# Article List Errors

Backend error codes in the Article List category (code range `70000`). One file per error code; see the parent [backend error index](../README.md) and [rules/backend_errors.md](../../../rules/backend_errors.md).

| Code | Enum Constant | HTTP | Message |
| --- | --- | --- | --- |
| [70001.md](70001.md) | `ArticleListIdNotExist` | 400 | `Article list id {} not exist.` |
| `70002.md` † | `ArticleListNoPrivilege` | 400 | `No privilege to access article list({})` |
| [70003.md](70003.md) | `ArticleListIdentifierExist` | 400 | `Article list identifier {} exist in project({}).` |
| `70004.md` † | `ArticleListIdentifierNotExist` | 400 | `Article list identifier {} not exist in project({}).` |
| [70005.md](70005.md) | `ArticleListTemplateFormatError` | 400 | `Article template must be html.` |
| [70006.md](70006.md) | `ArticleListProjectMismatching` | 400 | `Article list and project is mismatching.` |
| [70007.md](70007.md) | `ArticleListArticleMismatching` | 400 | `Article list({}) and article({}) is mismatching, they must belong to a same project.` |
| [70008.md](70008.md) | `ArticleListArticleExist` | 400 | `Article list({}) contains article({}).` |
| [70009.md](70009.md) | `ArticleListArticleNotExist` | 400 | `Article list({}) does not contain article({}).` |
| [70010.md](70010.md) | `ArticleDeleteRejectedDueToPagerListExisting` | 400 | `Article list({}) cannot be deleted, because it was referenced by more that 1 templates.` |

† Declared in `PagerErrorMessage` but never thrown yet — the doc is written together with the code that first throws it (same-PR sync rule).
