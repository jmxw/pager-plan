# Language Errors

Backend error codes in the Language category (code range `50000`). One file per error code; see the parent [backend error index](../README.md) and [rules/backend_errors.md](../../../rules/backend_errors.md).

| Code | Enum Constant | HTTP | Message |
| --- | --- | --- | --- |
| [50001.md](50001.md) | `LanguageExist` | 400 | `Language {} exist in project({}).` |
| [50002.md](50002.md) | `LanguageNotExist` | 400 | `Language {} not exist in project({}).` |
| [50003.md](50003.md) | `LanguageDefaultNotFound` | 500 → `-9999` | `Default language not found in project({}).` |
