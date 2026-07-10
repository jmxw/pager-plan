# User Errors

Backend error codes in the User category (code range `20000`). One file per error code; see the parent [backend error index](../README.md) and [rules/backend_errors.md](../../../rules/backend_errors.md).

| Code | Enum Constant | HTTP | Message |
| --- | --- | --- | --- |
| [20001.md](20001.md) | `UserIdNotExist` | 400 | `User id {} not exist.` |
| [20002.md](20002.md) | `UserNotPermitted` | 400 | `User({}) with role {} does not have permission {}.` |
| [20003.md](20003.md) | `UserIdentifierExist` | 400 | `User identifier {} exist.` |
| [20004.md](20004.md) | `UserIdentifierNotExist` | 400 | `User identifier {} not exist.` |
| [20005.md](20005.md) | `UserPasswordWrong` | 400 | `Password is wrong for user {}` |
| [20006.md](20006.md) | `UserMailExist` | 400 | `User mail {} exist in organization {}.` |
