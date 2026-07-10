# ADMIN_WRITE

> Area: User
> Roles: OWNER

Create administrator accounts within the organization.

## Description

Grants creation of `ADMIN`-role users under the organization. Held only by the OWNER, so the
organization owner is the sole role that can promote a new administrator; ADMIN and lower roles
manage developers and customers through `USER_WRITE` but cannot mint further admins.

## Gated Endpoints

| Method | Path | Endpoint Doc |
| --- | --- | --- |
| POST | `/v1/user/admin` | ⬜ [add_admin.md](/doc/api/user/add_admin.md) |
