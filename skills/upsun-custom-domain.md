---
name: Add a custom domain to an Upsun project
description: Attach a custom domain and SSL certificate to an Upsun project and its environments through the Upsun REST API.
api: openapi/upsun-openapi-original.json
operations:
  - list-projects-domains
  - create-projects-domains
  - get-projects-domains
  - create-projects-environments-domains
  - list-projects-certificates
  - create-projects-certificates
---

# Add a custom domain to an Upsun project

## Auth

Bearer token from the API-token exchange
(`https://auth.upsun.com/oauth2/token`, `grant_type=api_token`), sent to
`https://api.upsun.com`.

## Steps

1. `list-projects-domains` — `GET /projects/{projectId}/domains` to see
   what is already attached.
2. `create-projects-domains` — `POST /projects/{projectId}/domains` to
   attach the production domain to the project.
3. For a non-production environment domain, use
   `create-projects-environments-domains` —
   `POST .../environments/{environmentId}/domains`.
4. Certificates: Upsun provisions Let's Encrypt automatically; to bring
   your own, `create-projects-certificates` —
   `POST /projects/{projectId}/certificates` — and verify with
   `list-projects-certificates`.
5. Confirm with `get-projects-domains` and point DNS at the target shown
   in the domain resource.

## Rules

- `409 Conflict` responses (RFC 9457 problem+json) mean the domain or
  certificate already exists — reconcile with the list operations
  instead of retrying.
- Domain changes trigger deployment activities; wait for them to finish
  before validating HTTPS.
