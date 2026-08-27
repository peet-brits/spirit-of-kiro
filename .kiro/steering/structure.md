# Project Structure

This is a monorepo with three independently deployable services and shared scripts.

```
spirit-of-kiro/
├── client/               # Vue 3 game frontend
│   └── src/
│       ├── assets/       # Static images and CSS
│       ├── components/   # Vue SFCs — all game UI elements (GameItem, Workbench, HUD, etc.)
│       ├── composables/  # Reusable Vue composition functions (e.g. useEscapeKeyHandler)
│       ├── router/       # Vue Router route definitions
│       ├── stores/       # Pinia stores (game.ts is the single central store)
│       ├── systems/      # Game logic classes (physics, socket, items, inventory, etc.)
│       ├── utils/        # Pure utility functions (physics math, item helpers, world init)
│       └── views/        # Top-level routed pages (HomeView, GameView, SignIn, SignUp)
│
├── server/               # Bun WebSocket game server
│   ├── handlers/         # One file per WebSocket message type (e.g. pull-item.ts)
│   │   └── __tests__/    # Handler unit tests
│   ├── llm/              # Bedrock LLM integration (model.ts, prompts.ts, word-lists/)
│   ├── state/            # DynamoDB data access (item-store.ts, user-store.ts)
│   ├── utils/            # Shared server utilities (message formatting, password hashing)
│   ├── iac/              # CloudFormation templates (cognito, dynamodb, fargate)
│   ├── mocks/            # Static JSON mocks used in tests
│   ├── types.ts          # All shared TypeScript types (WebSocket message interfaces)
│   ├── config.ts         # Environment variable configuration
│   └── server.ts         # Entry point — WebSocket switch/dispatch
│
├── item-images/          # Standalone HTTP service for AI image generation
│   ├── handlers/         # HTTP route handlers
│   ├── lib/              # Image generation and vector matching logic
│   ├── state/            # MemoryDB / S3 state access
│   └── server.ts         # Entry point
│
├── docker/               # Docker volumes (e.g. local DynamoDB data)
├── docs/                 # Project documentation (architecture, setup, roadmap, etc.)
├── scripts/              # Shell and JS utility scripts (deploy, bootstrap, cleanup)
├── docker-compose.yml    # Local dev stack definition
└── .kiro/steering/       # AI assistant steering rules (this directory)
```

## Key Architectural Patterns

### Client: System + Store Pattern
The Pinia `game` store (`client/src/stores/game.ts`) is the single source of truth. It owns all reactive state and instantiates all system classes, exposing their methods directly. Components never instantiate systems — they only call store methods or read store state.

System classes (`client/src/systems/`) are plain TypeScript classes that encapsulate a domain of game logic. They communicate with each other through the `SocketSystem` event bus (publish/subscribe with wildcard support via `addEventListener` / `emitEvent`).

### Client: Component Conventions
- All UI is built with Vue Single File Components (`.vue`) using the **Composition API** (`<script setup>` or `setup()` function).
- Components live in `src/components/`. Views (routed pages) live in `src/views/`.
- Game object components (Workbench, Chest, Dispenser, etc.) interact with the store for socket calls and item state.

### Server: Message Handler Pattern
Each WebSocket message type has a dedicated handler file in `server/handlers/`. The main `server.ts` dispatches to the correct handler via a `switch` on `data.type`. Handlers receive `ConnectionState` (which carries the authenticated `userId`) and return `{ type, body }`.

### Server: Message Format
All WebSocket messages use the shared `formatMessage(type, body?)` utility from `server/utils/message.ts`. Incoming messages conform to typed interfaces in `server/types.ts`.

### Naming Conventions
- Files and directories: `kebab-case` everywhere
- Vue components: `PascalCase.vue`
- TypeScript types/interfaces: `PascalCase`
- WebSocket message types: `kebab-case` strings (e.g. `pull-item`, `list-inventory`)
- Server handler files mirror the message type name (e.g. `pull-item.ts` handles `pull-item`)
