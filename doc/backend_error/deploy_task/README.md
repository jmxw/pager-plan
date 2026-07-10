# Deploy Task Errors

Backend error codes in the Deploy Task category (code range `11000`). One file per error code; see the parent [backend error index](../README.md) and [rules/backend_errors.md](../../../rules/backend_errors.md).

| Code | Enum Constant | HTTP | Message |
| --- | --- | --- | --- |
| [11001.md](11001.md) | `DeployTaskIdNotExist` | N/A (consumer) | `Deploy task id {} not exist.` |
| [11002.md](11002.md) | `DeployTaskSkipDueToStarted` | N/A (consumer) | `Deploy task({}) skipped, because it was started, current state is {}, cannot be ran again.` |
| [11003.md](11003.md) | `DeployTaskSkipDueToNoTemplate` | N/A (consumer) | `Deploy task({}) skipped, because the template of the project({}) is not ready.` |
| [11004.md](11004.md) | `DeployDirectoryError` | N/A (consumer) | `Error to find or create a deploy directory for project({})` |
