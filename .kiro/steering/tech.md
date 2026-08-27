# Tech Stack

## Runtime & Package Manager

- **Bun** — used as the JavaScript runtime and package manager across all sub-projects. Do not use Node/npm for running or installing; use `bun` and `bun install`.

## Client (`client/`)

| Concern | Library |
|---|---|
| UI framework | Vue 3 (Composition API) |
| State management | Pinia |
| Routing | Vue Router 4 |
| Build tool | Vite 6 |
| Type checking | TypeScript 5.7 + `vue-tsc` |
| Unit testing | Vitest + `@vue/test-utils` + jsdom |
| Linting | ESLint 9 (`eslint-plugin-vue`, `@vue/eslint-config-typescript`) |
| Formatting | Prettier |

## Server (`server/`)

| Concern | Library |
|---|---|
| WebSocket server | Bun native WebSocket (`ws` package also present) |
| AWS persistence | `@aws-sdk/client-dynamodb` + `@aws-sdk/lib-dynamodb` |
| Auth | `@aws-sdk/client-cognito-identity-provider` |
| AI / LLM | `@aws-sdk/client-bedrock-runtime` (Claude Sonnet 3.7/4, Nova Pro) |
| Image storage | `@aws-sdk/client-s3` |
| Image processing | `sharp` |
| Testing | Bun built-in test runner |

## Item Images Service (`item-images/`)

| Concern | Library |
|---|---|
| HTTP server | Bun native HTTP |
| Image generation | Amazon Nova Canvas (via Bedrock) |
| Vector embeddings | Amazon Titan Text Embeddings v2 (via Bedrock) |
| Vector store | Amazon MemoryDB (Redis-compatible, `redis` client) |
| Image storage | S3 + CloudFront |

## AWS Services

- **DynamoDB** — game state (inventories, item metadata)
- **Amazon Cognito** — user authentication
- **Amazon Bedrock** — LLM inference (Claude Sonnet 3.7/4, Nova Pro, Nova Canvas, Titan Text Embeddings v2)
- **S3 + CloudFront** — item image hosting
- **Amazon MemoryDB** — vector database for image deduplication
- **AWS Fargate** — production container hosting

## Local Development

The full stack runs via Docker Compose (or Podman Compose). The item-images service does not need to run locally; a remote version is provided.

```sh
# Verify dependencies
./scripts/check-dependencies.sh

# Deploy Cognito to AWS (one-time)
./scripts/deploy-cognito.sh game-auth

# Build and start the stack with live reload
podman compose build && podman compose up --watch --remove-orphans --timeout 0 --force-recreate

# Bootstrap DynamoDB tables (first run only)
podman exec server mkdir -p /app/server/iac && \
  podman cp scripts/bootstrap-local-dynamodb.js server:/app/ && \
  podman cp server/iac/dynamodb.yml server:/app/server/iac/ && \
  podman exec server bun run /app/bootstrap-local-dynamodb.js
```

Substitute `docker` for `podman` if using Docker Desktop.

## Running Without Containers

```sh
# Install dependencies
bun install              # root
cd client && bun install
cd server && bun install

# Run client dev server
cd client && bun run dev

# Run server with hot reload
cd server && bun --watch server.ts
```

## Common Commands

| Task | Command | Location |
|---|---|---|
| Client dev server | `bun run dev` | `client/` |
| Client type check | `bun run type-check` | `client/` |
| Client build | `bun run build` | `client/` |
| Client lint | `bun run lint` | `client/` |
| Client format | `bun run format` | `client/` |
| Client unit tests | `bun run test:unit` | `client/` |
| Server tests | `bun test` | `server/` |
| Bootstrap DynamoDB | `bun run bootstrap-dynamodb` | `server/` |
