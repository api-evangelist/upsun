---
name: Manage Upsun preview environments
description: List, branch, merge, pause, resume, and redeploy Upsun environments for a project through the Upsun REST API.
api: openapi/upsun-openapi-original.json
operations:
  - list-projects-environments
  - get-environment
  - branch-environment
  - merge-environment
  - pause-environment
  - resume-environment
  - redeploy-environment
  - synchronize-environment
---

# Manage Upsun preview environments

Upsun maps git branches to environments. Every write below returns an
activity you can poll.

## Auth

Exchange an API token for a bearer token (valid 900 s):
`POST https://auth.upsun.com/oauth2/token` with
`grant_type=api_token&api_token=...` (basic user `platform-api-user`).
Send `Authorization: Bearer <token>` to `https://api.upsun.com`.
Re-exchange on `401`.

## Steps

1. `list-projects-environments` — `GET /projects/{projectId}/environments`
   to find the environment ids and their parent relationships.
2. `get-environment` — inspect state (`active`, `paused`, `inactive`).
3. `branch-environment` — `POST .../environments/{environmentId}/branch`
   to create a child preview environment from a parent.
4. Iterate: `redeploy-environment` after config changes;
   `synchronize-environment` to pull data/code from the parent.
5. `merge-environment` — merge the child back into its parent when ready.
6. `pause-environment` / `resume-environment` — stop and restart idle
   preview environments to save resources.

## Rules

- Errors follow RFC 9457 `application/problem+json`; check `403` (no
  access), `404` (bad id) — see `errors/upsun-problem-types.yml`.
- List endpoints paginate with `page[before]` / `page[after]` /
  `page[size]` and support `sort` and `filter[...]` params.
- There is no idempotency-key contract; do not blind-retry writes —
  re-list state first (`conventions/upsun-conventions.yml`).
