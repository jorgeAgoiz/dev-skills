# API to Bruno

[![skills.sh](https://skills.sh/b/jorgeAgoiz/api-to-bruno)](https://skills.sh/jorgeAgoiz/api-to-bruno)

Generate a classic [Bruno](https://www.usebruno.com/) `.bru` collection from an API repository, local API project, OpenAPI/Swagger document, or WSDL contract.

This skill is built for agents that need to turn API surface area into usable Bruno requests without guessing, leaking secrets, or running untrusted tooling silently.

## What It Does

- Finds OpenAPI, Swagger, or WSDL contracts before reading application code.
- Falls back to source-code route inventory when no formal contract exists.
- Generates classic Bruno `.bru` request files, `bruno.json`, and a local environment with `baseUrl`.
- Maps path params, query params, headers, auth hints, and request bodies when there is evidence in the contract or code.
- Verifies the generated collection structure without sending API traffic by default.

## Why Use It

API projects often have usable routes spread across contracts, controllers, decorators, route files, DTOs, validators, framework conventions, and docs. This skill gives the agent a deterministic process:

1. Resolve the API source.
2. Prefer the formal API contract.
3. Inventory routes when no contract exists.
4. Generate Bruno files from the inventory.
5. Verify the output without calling the API.

The leading behavior is **inventory first**: collect what exists, mark unknowns explicitly, then generate files from that evidence.

## Install

Install with the `skills` CLI:

```bash
npx skills add jorgeAgoiz/api-to-bruno
```

Or browse it on skills.sh:

```text
https://skills.sh/jorgeAgoiz/api-to-bruno
```

## Example Prompts

```text
Generate a Bruno collection from https://github.com/acme/payments-api
```

```text
Use api-to-bruno for ./apps/api and write the collection to ./bruno/payments
```

```text
Create Bruno requests from https://example.com/openapi.yaml
```

```text
Generate a Bruno collection from this NestJS API. Do not run the server.
```

## Supported Sources

- Git repository URLs.
- Local API project paths.
- Direct OpenAPI or Swagger URLs.
- Direct WSDL URLs.
- Repositories with route definitions but no contract.

## Supported API Discovery

Contract-first discovery:

- OpenAPI files such as `openapi.yaml`, `openapi.json`, and similar names.
- Swagger files such as `swagger.yaml`, `swagger.json`, and similar names.
- WSDL service contracts.

Source inventory discovery:

- Express, Koa, Fastify.
- NestJS.
- FastAPI, Flask, Django, Django REST Framework.
- Spring.
- ASP.NET route attributes and minimal APIs.
- Laravel.
- Rails.
- Go routers such as Gin, Echo, chi, and `net/http`.
- GraphQL schemas and resolvers.

## Output

The generated collection uses Bruno's classic `.bru` format:

```text
bruno/<collection>/
  bruno.json
  environments/
    Local.bru
  Users/
    Get User.bru
    Create User.bru
```

Each request includes a `meta` block and one HTTP method block. Path parameters use Bruno `:param` syntax, query parameters go into `params:query`, and request bodies are included only when the contract or source code supports them.

## Safety Model

This skill is intentionally conservative:

- It does not send API requests unless the user explicitly asks.
- It does not install dependencies or start servers unless static analysis is insufficient and the user approves.
- It does not execute `npx`, `npm exec`, `pnpm dlx`, `yarn dlx`, package installs, or Docker images automatically.
- If Bruno CLI is unavailable, the agent must ask before running any remote package or container.
- Approval requests must include the exact pinned command, version or image tag, risk note, working directory, and reason.
- Secrets from `.env`, config files, tokens, API keys, or cookies must never be copied into generated requests.

When remote tooling is not approved, the skill generates `.bru` files manually from the contract or route inventory.

## Repository Contents

- [`SKILL.md`](SKILL.md): agent-facing workflow and invocation metadata.
- [`BRUNO.md`](BRUNO.md): Bruno format reference, import commands, mapping rules, and safety rules.

## Status

This skill is designed for practical API-to-Bruno generation and favors correctness over speculation. Unknown route details are reported as unresolved assumptions instead of being invented.
