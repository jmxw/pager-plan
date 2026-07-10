# LABEL_READ

> Area: Label
> Roles: None (no role grants this)

Read a label.

## Description

Grants read access to a label — a reusable, language-keyed content snippet substituted into
templates at build time. **No `UserRole` currently includes `LABEL_READ` in its permission set**,
so the gated endpoint below is unreachable in practice: any authenticated caller is rejected with
`UserNotPermitted`. This is recorded as-is; granting it would require adding `LABEL_READ` to a
role's `UserRole.permissions` set.

## Gated Endpoints

| Method | Path | Endpoint Doc |
| --- | --- | --- |
| GET | `/v1/label/{labelId}` | ⬜ [get_label.md](/doc/api/label/get_label.md) |
