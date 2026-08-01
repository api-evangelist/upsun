---
name: Onboard with an Upsun API token
description: Create an Upsun API token, exchange it for a bearer access token, and make your first authenticated API calls.
api: openapi/upsun-openapi-original.json
operations:
  - list-api-tokens
  - create-api-token
  - get-api-token
  - delete-api-token
---

# Onboard with an Upsun API token

## Steps

1. Sign up / sign in at `https://auth.upsun.com/register` and open the
   Console (`https://console.upsun.com/`). Create an API token there, or
   programmatically with `create-api-token` —
   `POST /users/{user_id}/api-tokens` (requires an existing session;
   `409` if the token name already exists).
2. Exchange the API token for an access token:

   ```bash
   curl -u platform-api-user: \
     -d 'grant_type=api_token&api_token=YOUR_API_TOKEN' \
     https://auth.upsun.com/oauth2/token
   ```

   The bearer token expires in **900 seconds**; re-exchange on `401`.
3. Call the API:

   ```bash
   curl -H "Authorization: Bearer YOUR_ACCESS_TOKEN" \
     https://api.upsun.com/projects
   ```

4. Manage tokens with `list-api-tokens`, `get-api-token`, and revoke
   with `delete-api-token`.

## Rules

- Never store the raw API token in code; scope one token per automation
  (same guidance as `https://developer.upsun.com/cli/api-tokens`).
- Errors follow RFC 9457 `application/problem+json`
  (`errors/upsun-problem-types.yml`).
