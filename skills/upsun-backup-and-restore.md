---
name: Back up and restore an Upsun environment
description: Create a backup of an Upsun environment, list available backups, and restore one through the Upsun REST API.
api: openapi/upsun-openapi-original.json
operations:
  - backup-environment
  - list-projects-environments-backups
  - get-projects-environments-backups
  - restore-backup
  - delete-projects-environments-backups
---

# Back up and restore an Upsun environment

## Auth

Bearer token from the API-token exchange
(`https://auth.upsun.com/oauth2/token`, `grant_type=api_token`), sent to
`https://api.upsun.com`. Tokens last 900 s.

## Steps

1. `backup-environment` —
   `POST /projects/{projectId}/environments/{environmentId}/backup`
   starts a backup; the response is an activity to poll.
2. `list-projects-environments-backups` —
   `GET .../environments/{environmentId}/backups` to enumerate backups
   (cursor pagination: `page[size]`, `page[after]`).
3. `get-projects-environments-backups` — check a backup's status before
   restoring.
4. `restore-backup` —
   `POST .../backups/{backupId}/restore` to restore into the
   environment.
5. `delete-projects-environments-backups` — prune old backups.

## Rules

- Backups and restores are asynchronous activities; poll
  `list-projects-environments-activities` until complete before further
  writes.
- Errors are RFC 9457 `application/problem+json`
  (`errors/upsun-problem-types.yml`); no idempotency keys — never
  blind-retry a restore.
