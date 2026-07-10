# Access Key (AccessKeyController)

Apis to manage access keys of an organization.

## Endpoints

| Method | Path | Summary | Doc |
| --- | --- | --- | --- |
| GET | `/v1/access_key` | Returns all access keys of the caller's organization. | [get_access_keys.md](/doc/api/access_key/get_access_keys.md) |
| POST | `/v1/access_key` | Adds a new access key to the caller's organization. | [add_access_key.md](/doc/api/access_key/add_access_key.md) |
| PUT | `/v1/access_key/{accessKeyId}` | Updates the credentials of an existing access key. | [edit_access_key.md](/doc/api/access_key/edit_access_key.md) |
| DELETE | `/v1/access_key/{accessKeyId}` | Deletes an access key of the caller's organization. | [delete_access_key.md](/doc/api/access_key/delete_access_key.md) |
