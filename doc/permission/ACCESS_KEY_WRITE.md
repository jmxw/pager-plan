# ACCESS_KEY_WRITE

> Area: Access Key
> Roles: OWNER, ADMIN

Create and delete the organization's access keys.

## Description

Grants creation and deletion of access keys. Held by OWNER and ADMIN. Note that editing an access
key (`PUT /v1/access_key/{accessKeyId}`) declares no `@Permitted` annotation and is therefore not
gated by this permission.

## Gated Endpoints

| Method | Path | Endpoint Doc |
| --- | --- | --- |
| POST | `/v1/access_key` | [add_access_key.md](/doc/api/access_key/add_access_key.md) |
| DELETE | `/v1/access_key/{accessKeyId}` | [delete_access_key.md](/doc/api/access_key/delete_access_key.md) |
