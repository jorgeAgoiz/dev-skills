---
name: api-to-bruno
description: Bruno collection / coleccion Bruno generation from an API repository URL or local path; use when the user asks to generate Bruno requests, mentions usebruno/.bru/bru import, or provides an API source to derive requests from its contract or routes.
---

# API to Bruno

Turn an API source into a classic Bruno `.bru` collection. The leading word is **inventory**: build a route inventory first, then generate files from that inventory.

## Inputs

Require one API source: a Git URL, an OpenAPI/WSDL URL, or a local path relative to the current working directory. If no source is provided, ask for it.

If the user gives an output path, use it. Otherwise use `<api-root>/bruno/<collection-slug>` for a local source and `<cwd>/bruno/<collection-slug>` for a remote source. If that directory already exists and is non-empty, ask before overwriting or merging.

## Process

1. Resolve the source. For a local path, resolve it against the current working directory and verify it exists. For a Git URL, clone a shallow copy into the session temp area and keep generated output outside the clone unless the user asked otherwise. For a direct spec URL, keep it as the source. Completion: exactly one source and one safe output directory are known.

2. Search for a contract before reading application code. Look for OpenAPI or Swagger files (`openapi.*`, `swagger.*`, `api-docs.*`) and WSDL files in docs, spec, api, public, resources, generated, and root folders. Confirm candidates by reading them for `openapi`, `swagger`, or WSDL markers. If several plausible contracts conflict, ask which to use. Completion: choose `contract` mode with one contract, or `source-inventory` mode because no contract was found.

3. In `contract` mode, read [`BRUNO.md`](BRUNO.md), then import with Bruno CLI using classic `--collection-format=bru`. Prefer an already installed `bru` binary. If `bru` is missing, do not run `npx`, install packages, or execute Docker automatically; ask for explicit approval with the exact pinned command, risk note, working directory, and reason it is needed, or generate `.bru` files manually if approval is denied. Completion: the output contains `bruno.json` and every contract operation is represented by a request file, or any importer gap is listed explicitly.

4. In `source-inventory` mode, inventory every route-defining file before generating. Search framework route cues: Express/Koa/Fastify routers, NestJS decorators, FastAPI/Flask decorators, Django URL patterns and DRF routers, Spring mappings, ASP.NET route attributes and minimal APIs, Laravel routes, Rails `routes.rb`, Go Gin/Echo/chi/http handlers, and GraphQL schemas or resolvers. Completion: the inventory includes every discovered method, path, source file, handler name, path params, query params, body shape, auth hint, and tags/folder grouping; unknown fields are marked `unknown`, not omitted.

5. Generate from the inventory. Read [`BRUNO.md`](BRUNO.md) before writing Bruno files. Create a stable folder layout by resource or controller, `bruno.json`, a local environment with `baseUrl`, and one `.bru` request per inventory row. Convert path params to Bruno `:param` syntax, preserve query params, add headers and bodies only when evidenced by contract, validators, DTOs, schemas, or handler code. Use placeholders such as `{{token}}` and `{{apiKey}}`; never copy real secrets. Completion: every inventory row has exactly one `.bru` file and every known parameter/body/auth hint is represented or called out as unresolved.

6. Verify without sending API traffic unless the user asked to run requests. Check that `bruno.json` is valid JSON, every request file has `meta` plus exactly one method block, sequence numbers are stable within each folder, and the generated request count matches the contract operations or route inventory count. Completion: final response reports source, mode, output path, request count, verification performed, and unresolved assumptions.

## Defaults

- Collection name comes from `info.title`, package/project metadata, repository name, or directory name, in that order.
- `baseUrl` comes from OpenAPI `servers`, app config, README, Docker/compose ports, `.env.example`, or `http://localhost:3000`, in that order.
- Request names use `<METHOD> <path>` when no operation summary or handler name exists.
- Do not install project dependencies or start servers unless the static inventory is insufficient and the user approves.
- Do not execute remote package managers (`npx`, `npm exec`, `pnpm dlx`, `yarn dlx`) or Docker images without explicit user approval for the exact command and version/tag.
