# User (UserController)

Apis to manage user and login

## Endpoints

| Method | Path | Summary | Doc |
| --- | --- | --- | --- |
| POST | `/v1/user/login` | User login | [login.md](/doc/api/user/login.md) |
| GET | `/v1/user/logout` | User logout | [logout.md](/doc/api/user/logout.md) |
| GET | `/v1/user` | Get all visible users of an organization. | [get_users.md](/doc/api/user/get_users.md) |
| GET | `/v1/user/roles` | Get all available user roles. | [get_available_roles.md](/doc/api/user/get_available_roles.md) |
| POST | `/v1/user/admin` | Add an admin user. | [add_admin.md](/doc/api/user/add_admin.md) |
| POST | `/v1/user/developer` | Add a developer user. | [add_developer.md](/doc/api/user/add_developer.md) |
| POST | `/v1/user/customer` | Add a customer user. | [add_customer.md](/doc/api/user/add_customer.md) |
| GET | `/v1/user/me` | Get user information of the current user. | [get_me.md](/doc/api/user/get_me.md) |
| PATCH | `/v1/user/{userId}` | Get user information by userId. | [edit_user.md](/doc/api/user/edit_user.md) |
| DELETE | `/v1/user/{userId}` | Delete user. | [delete_user.md](/doc/api/user/delete_user.md) |
