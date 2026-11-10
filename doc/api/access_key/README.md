# AccessKeyController

API docs for the endpoints of
[`AccessKeyController.kt`](/source/pager-backend/app/cms/src/main/kotlin/com/xwkj/cms/controller/AccessKeyController.kt).
One file per endpoint; see the parent [API index](../README.md) and
[rules/api_docs.md](../../../rules/api_docs.md).

| Doc | Method | Path | Permission | Summary |
| --- | --- | --- | --- | --- |
| [get_access_keys.md](get_access_keys.md) | GET | `/v1/access_key` | `ACCESS_KEY_READ` | Get all access keys of an organization. |
| [add_access_key.md](add_access_key.md) | POST | `/v1/access_key` | `ACCESS_KEY_WRITE` | Add a new access key into an organization. |
| [edit_access_key.md](edit_access_key.md) | PUT | `/v1/access_key/{accessKeyId}` | N/A (authenticated) | Edit an access key. |
| [delete_access_key.md](delete_access_key.md) | DELETE | `/v1/access_key/{accessKeyId}` | `ACCESS_KEY_WRITE` | Delete an access key. |
