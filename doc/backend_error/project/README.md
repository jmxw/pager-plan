# Project Errors

Backend error codes in the Project category (code range `40000`). One file per error code; see the parent [backend error index](../README.md) and [rules/backend_errors.md](../../../rules/backend_errors.md).

| Code | Enum Constant | HTTP | Message |
| --- | --- | --- | --- |
| [40001.md](40001.md) | `ProjectIdNotExist` | 400 | `Project id {} not exist.` |
| [40002.md](40002.md) | `ProjectNoPrivilege` | 400 | `No privilege to access project({})` |
| [40003.md](40003.md) | `ProjectNotBelongToOrganization` | 400 | `Project({}) does not belong to organization({})` |
| [40004.md](40004.md) | `ProjectIdentifierExist` | 400 | `Project identifier {} exist.` |
| `40005.md` † | `ProjectIdentifierNotExist` | 400 | `Project identifier {} not exist.` |
| [40006.md](40006.md) | `ProjectBucketExist` | 400 | `Project bucket exist with identifier {}.` |
| [40007.md](40007.md) | `ProjectZipSavedError` | 400 | `Error to save project zip file for project({}).` |
| [40008.md](40008.md) | `ProjectZipDownloadFailed` | N/A (consumer) | `Error to download project zip for project({}) from {}.` |
| [40009.md](40009.md) | `ProjectZipFileUnzipFailed` | N/A (consumer) | `Error to unzip file for project({})` |
| [40010.md](40010.md) | `ProjectZipUploadReject` | 400 | `Reject to upload zip file for project({}), please reset templates!` |
| [40011.md](40011.md) | `ProjectResetReject` | 400 | `Project({}) is processing, try to reset later.` |
| [40012.md](40012.md) | `ProjectIsDeploying` | 400 | `Project({}) is deploying, try to deploy later.` |
| `40013.md` ‡ | `ProjectDeployInterruptedByPagerErrors` | N/A (consumer) | `Project(({}) deploy is interuptted ` |
| `40013.md` ‡ | `ProjectIsWaitForDeploy` | 400 | `Project({}) is wait for deploy now.` |

† Declared in `PagerErrorMessage` but never thrown yet — the doc is written together with the code that first throws it (same-PR sync rule).
‡ Two constants share this code, so "one code = one file" cannot be satisfied until the backend renumbers one of them; see the "Duplicated codes" section of the [index](../README.md).
