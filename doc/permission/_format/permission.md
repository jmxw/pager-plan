# <ENUM_CONSTANT>

> Area: <area>
> Roles: <comma-separated roles that hold it, or "None (no role grants this)">

<One-line summary of the access this permission grants.>

## Description

<What the permission controls, in business terms — the resources and operations it unlocks. Link a
domain term to its Tech Noun (`/doc/tech_noun/...`) on first mention, plain text after. Cross-doc
links use a root-absolute path from the repo root (`/doc/api/`, `/doc/tech_noun/`, `/plan/release/`).>

## Gated Endpoints

| Method | Path | Endpoint Doc |
| --- | --- | --- |
| POST | `/v1/<resource>` | [<handler>.md](/doc/api/<controller>/<handler>.md) |

Write `N/A` when no endpoint declares this permission. Link each row to its API doc
(`/doc/api/<controller>/<handler>.md`); mark `⬜` when that endpoint's API doc is not yet written.
