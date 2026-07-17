# PROJECT_DEPLOY

> Area: Project
> Roles: OWNER, ADMIN, DEVELOPER, CUSTOMER

Publish a project to a deployment phase (staging / production).

## Description

Grants publishing of a project to a deployment phase — the action that enqueues the RabbitMQ
deployment event the consumer processes (unpack ZIP, precompile navigation/article/label content,
upload to OSS). Held by all four roles, so a customer can publish their own site. The unauthenticated
force-deploy endpoint (`PATCH /v1/project/{projectId}/force_deploy/{phase}`) declares no `@Permitted`
and is therefore not gated by this permission.

## Gated Endpoints

| Method | Path | Endpoint Doc |
| --- | --- | --- |
| PATCH | `/v1/project/{projectId}/deploy/{phase}` | [deploy.md](/doc/api/project/deploy.md) |
