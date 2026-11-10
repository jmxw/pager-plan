# USER_READ

> Area: User
> Roles: OWNER, ADMIN

Read the organization's user list, the assignable role list, and the caller's own profile.

## Description

Grants read access to the organization's users: listing accounts, retrieving the roles that may be
assigned when creating a user, and reading the caller's own account (`/v1/user/me`). Held by OWNER
and ADMIN, the roles that administer accounts.

## Gated Endpoints

| Method | Path | Endpoint Doc |
| --- | --- | --- |
| GET | `/v1/user` | [get_users.md](/doc/api/user/get_users.md) |
| GET | `/v1/user/roles` | [get_available_roles.md](/doc/api/user/get_available_roles.md) |
| GET | `/v1/user/me` | [get_me.md](/doc/api/user/get_me.md) |
