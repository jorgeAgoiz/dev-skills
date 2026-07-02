# Bruno Reference

Load this reference when importing, generating, or validating Bruno files.

## CLI Import

Use the Bruno CLI binary `bru` when it is already installed.

OpenAPI to classic `.bru` collection:

```bash
bru import openapi --source "<openapi.yaml>" --output "<output-dir>" --collection-name "<name>" --collection-format=bru
```

WSDL to collection:

```bash
bru import wsdl --source "<service.wsdl>" --output "<output-dir>" --collection-name "<name>"
```

If `bru` is unavailable, ask before executing any remote package or container. Present the exact command, pin the version or image tag, state that it downloads and executes third-party code, name the working directory, explain why the importer is needed, and wait for approval. Example with a pinned package version:

```bash
npx -y @usebruno/cli@<version> import openapi --source "<openapi.yaml>" --output "<output-dir>" --collection-name "<name>" --collection-format=bru
```

If approval is denied, generate the classic `.bru` files manually from the contract or route inventory instead of running the importer.

## Execution Safety

- Never execute `npx`, `npm exec`, `pnpm dlx`, `yarn dlx`, package installs, or Docker images automatically.
- Pin remote tools to an exact package version or image tag before asking for approval.
- Prefer a trusted local `bru` binary, a project-pinned dependency, or a prebuilt CI image controlled by the user.
- Do not run importers from directories that contain unnecessary secrets when a safer temp/input path is available.
- Never pass real tokens, API keys, cookies, or `.env` values to generated requests.

## Classic Layout

Root file:

```json
{
  "version": "1",
  "name": "Example API",
  "type": "collection"
}
```

Environment file at `environments/Local.bru`:

```bru
vars {
  baseUrl: http://localhost:3000
}
```

Request files end in `.bru` and may live in folders. Minimal GET:

```bru
meta {
  name: Get User
  type: http
  seq: 1
}

get {
  url: {{baseUrl}}/users/:id
  body: none
  auth: none
}

params:path {
  id: 123
}

params:query {
  include: profile
}
```

JSON body request:

```bru
meta {
  name: Create User
  type: http
  seq: 2
}

post {
  url: {{baseUrl}}/users
  body: json
  auth: bearer
}

headers {
  content-type: application/json
  Authorization: Bearer {{token}}
}

body {
  {
    "name": "Jane Doe",
    "email": "jane@example.com"
  }
}
```

## Mapping Rules

- Methods map to lowercase blocks: `get`, `post`, `put`, `patch`, `delete`, `options`, `head`, `trace`, `connect`.
- Convert OpenAPI `{id}`, Express `:id`, FastAPI `{id}`, Flask `<id>`, Spring `{id}`, and ASP.NET `{id}` to Bruno `:id` in URLs.
- Put route parameters in `params:path` with safe examples such as `123` or `example-id`.
- Put query parameters in `params:query`; use defaults from the contract/code or harmless placeholders.
- Add `headers` only when evidenced. Use `content-type: application/json` for JSON request bodies.
- Use `body: none` for methods without a request body. Use `body: json`, `body: text`, `body: xml`, `body: form-urlencoded`, `body: multipart-form`, or `body: graphql` when evidenced.
- Represent bearer/API-key auth with placeholders, never literals copied from `.env`, config, or docs.

## Stable Files

- Sanitize filenames by replacing `/`, `\\`, `:`, `*`, `?`, `"`, `<`, `>`, `|`, and control characters with `-`.
- Keep sequence numbers deterministic: sort by folder, then path, then method order `GET`, `POST`, `PUT`, `PATCH`, `DELETE`, `OPTIONS`, `HEAD`, `TRACE`, `CONNECT`.
- Prefer updating a matching existing request file over creating a duplicate when regenerating into an existing approved collection.
