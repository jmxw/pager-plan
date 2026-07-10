# Access Key Errors

Backend error codes in the Access Key category (code range `30000`). One file per error code; see the parent [backend error index](../README.md) and [rules/backend_errors.md](../../../rules/backend_errors.md).

| Code | Enum Constant | HTTP | Message |
| --- | --- | --- | --- |
| [30001.md](30001.md) | `AccessKeyIdNotExist` | 400 | `Access key id {} not exist.` |
| [30002.md](30002.md) | `AccessKeyNotBelongToOrganization` | 400 | `Access key({}) does not belong to organization({})` |
| [30003.md](30003.md) | `AccessKeyUsedInProject` | 400 | `Access key({}) has been used in projects and cannot be deleted.` |
