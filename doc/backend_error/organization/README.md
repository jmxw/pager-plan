# Organization Errors

Backend error codes in the Organization category (code range `10000`). One file per error code; see the parent [backend error index](../README.md) and [rules/backend_errors.md](../../../rules/backend_errors.md).

| Code | Enum Constant | HTTP | Message |
| --- | --- | --- | --- |
| [10001.md](10001.md) | `OrganizationIdNotExist` | 400 | `Organization id {} not exist.` |
| [10002.md](10002.md) | `OrganizationIdentifierExist` | 400 | `Organization identifier {} exist.` |
| `10003.md` † | `OrganizationIdentifierNotExist` | 400 | `Organization identifier {} not exist.` |

† Declared in `PagerErrorMessage` but never thrown yet — the doc is written together with the code that first throws it (same-PR sync rule).
