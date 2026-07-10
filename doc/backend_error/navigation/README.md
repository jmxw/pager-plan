# Navigation Errors

Backend error codes in the Navigation category (code range `90000`). One file per error code; see the parent [backend error index](../README.md) and [rules/backend_errors.md](../../../rules/backend_errors.md).

| Code | Enum Constant | HTTP | Message |
| --- | --- | --- | --- |
| [90001.md](90001.md) | `NavigationIdNotExist` | 400 | `Navigation id {} not exist` |
| [90002.md](90002.md) | `NavigationIdentifierDuplicated` | 400 | `Navigation identifier {} is duplicated in the parent navigation({}).` |
| [90003.md](90003.md) | `NavigationArticleMismatching` | 400 | `Navigation({}} and article({}) must belong to a same project` |
| [90004.md](90004.md) | `NavigationArticleListMismatching` | 400 | `Navigation({}} and article list({}) must belong to a same project` |
| [90005.md](90005.md) | `NavigationTemplateMismatching` | 400 | `Navigation({}} and template({}) must belong to a same project` |
| `90006.md` ‡ | `NavigationListLost` | N/A (consumer) | `Navigation list of navigation({}) lost.` |
| `90006.md` ‡ | `NavigationArticleLost` | N/A (consumer) | `Navigation article of navigation({}) lost.` |
| [90007.md](90007.md) | `NavigationDeleteRejectWithChildren` | 400 | `Navigation({}) with children navigations cannot be deleted.` |
| [90008.md](90008.md) | `NavigationRootDeleteReject` | 400 | `Root navigation({}) cannot be deleted.` |

‡ Two constants share this code, so "one code = one file" cannot be satisfied until the backend renumbers one of them; see the "Duplicated codes" section of the [index](../README.md).
