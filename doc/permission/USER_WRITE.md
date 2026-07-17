# USER_WRITE

> Area: User
> Roles: OWNER, ADMIN

Create developer and customer accounts and edit or delete users.

## Description

Grants management of non-admin users: creating `DEVELOPER` and `CUSTOMER` accounts, editing a
user, and deleting a user. Held by OWNER and ADMIN. Creating an `ADMIN` account is governed
separately by `ADMIN_WRITE` (OWNER only).

## Gated Endpoints

| Method | Path | Endpoint Doc |
| --- | --- | --- |
| POST | `/v1/user/developer` | [add_developer.md](/doc/api/user/add_developer.md) |
| POST | `/v1/user/customer` | [add_customer.md](/doc/api/user/add_customer.md) |
| PATCH | `/v1/user/{userId}` | [edit_user.md](/doc/api/user/edit_user.md) |
| DELETE | `/v1/user/{userId}` | [delete_user.md](/doc/api/user/delete_user.md) |
