# Dev Skills

[![skills.sh](https://skills.sh/b/jorgeAgoiz/dev-skills)](https://skills.sh/jorgeAgoiz/dev-skills)

A collection of agent skills for software development. Each skill provides specialized instructions and workflows for specific tasks.

This repository is organized as a skills collection. Each skill lives under `skills/<skill-name>/` with its own `SKILL.md` and any supporting reference files it needs.

## Available Skills

| Skill | Purpose |
| --- | --- |
| [`api-to-bruno`](skills/api-to-bruno/SKILL.md) | Generate Bruno collections from APIs, OpenAPI/Swagger, or WSDL contracts. |

## Install

Install the collection with the `skills` CLI:

```bash
npx skills add jorgeAgoiz/dev-skills
```

Install only a specific skill:

```bash
npx skills add https://github.com/jorgeAgoiz/dev-skills --skill api-to-bruno
```

Browse the repository on skills.sh:

```text
https://skills.sh/jorgeAgoiz/dev-skills
```

## Skills

### api-to-bruno

`api-to-bruno` turns API surface area into usable [Bruno](https://www.usebruno.com/docs/introduction) requests without guessing, leaking secrets, or running untrusted tooling silently.

What it does:

- Finds OpenAPI, Swagger, or WSDL contracts before reading application code.
- Falls back to source-code route inventory when no formal contract exists.
- Generates classic Bruno `.bru` request files, `bruno.json`, and a local environment with `baseUrl`.
- Maps path params, query params, headers, auth hints, and request bodies when there is evidence in the contract or code.
- Verifies the generated collection structure without sending API traffic by default.

Example prompts:

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

#### api-to-bruno Discovery

Supported sources:

- Git repository URLs.
- Local API project paths.
- Direct OpenAPI or Swagger URLs.
- Direct WSDL URLs.
- Repositories with route definitions but no contract.

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

#### api-to-bruno Output

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

#### Safety Model

The skills in this repository should favor static analysis, explicit user approval, and safe placeholders over silent execution or speculation.

For `api-to-bruno` specifically:

- It does not send API requests unless the user explicitly asks.
- It does not install dependencies or start servers unless static analysis is insufficient and the user approves.
- It does not execute `npx`, `npm exec`, `pnpm dlx`, `yarn dlx`, package installs, or Docker images automatically.
- If Bruno CLI is unavailable, the agent must ask before running any remote package or container.
- Approval requests must include the exact pinned command, version or image tag, risk note, working directory, and reason.
- Secrets from `.env`, config files, tokens, API keys, or cookies must never be copied into generated requests.

When remote tooling is not approved, `api-to-bruno` generates `.bru` files manually from the contract or route inventory.

#### Security Model for External Sources

To reduce prompt-injection and supply-chain risk when the API source is a Git repository URL or a remote OpenAPI/WSDL URL:

- Only Git hosts `github.com`, `gitlab.com`, and `bitbucket.org` are accepted without explicit confirmation.
- The cloned ref is pinned and resolved to a commit SHA, which is reported in the final summary.
- A single file larger than 256 KB is not read into context; multi-file searches cap at 50 candidates.
- The user is shown the resolved inventory (source, ref, mode, output path, route count, unresolved items) and must approve it before any `.bru` file is written.
- Generated files are always written outside the cloned repo, and the clone is cleaned up after generation.
- Raw external content is read only by a subagent that returns a structured summary; the main agent does not ingest the raw file bodies.
- Local API paths are preferred when both a local path and a remote URL are available.

## Repository Structure

```text
.
+-- README.md
+-- LICENSE
`-- skills/
    `-- api-to-bruno/
        +-- SKILL.md
        `-- BRUNO.md
```

## Adding Skills

Future skills should be added under `skills/<skill-name>/`:

```text
skills/<skill-name>/
  SKILL.md
  reference-or-supporting-files.md
```

Keep skill-specific reference files next to the skill that uses them, and update the Available Skills table when adding a new skill.

## License

MIT. See [`LICENSE`](LICENSE).
