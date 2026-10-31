# Repository instructions

## Challenge workflow

- Complete the required `README.md` TODOs before optional bonuses. Work in the candidate's private copy; never push or open PRs against the upstream challenge repository.
- Never edit, delete, or disable `.opencode/plugin/tracking.ts`, or manually edit `.opencode/logs/session.jsonl`. Final delivery requires the generated session log and a completed `REFERENCES.md`.
- Commit subjects follow the history: English `type(scope): summary`, with scope optional. UI and quest error messages are in pt-BR.

## Commands and setup

- Use Bun **1.4.0** (`.bun-version`); install dependencies once at the workspace root with `bun install`. Nx targets come from workspace `package.json` scripts; shared packages export TypeScript source directly.
- First-time setup is in `README.md#how-to-run-the-project`: create root, `apps/rest-server`, and `apps/web` `.env` files from their samples; start Docker with `docker compose up -d --wait`; apply both `bun run migrate` and `bun run migrate:test`.
- From the root, `bun run dev` starts API `:3000` and web `:4000`. Start one app with `bunx nx dev @repo/rest-server` or `bunx nx dev @repo/web`.
- Full verification: `bun run typecheck`, then `bun run test`. The root `pretest` already runs `bun run lint` (oxlint + oxfmt); Nx runs test targets serially. Use `bun run test`, not root-level `bun test`.
- Focused test commands below bypass root `pretest`; run `bun run lint` separately when needed. After dependency changes, also run `bun run packages:check`.

| Scope / working directory | Command |
| --- | --- |
| API suite / root | `bunx nx test @repo/rest-server` |
| API file / root | `bunx nx test @repo/rest-server src/api/quests/quests.post.test.ts` |
| API name filter / root | `bunx nx test @repo/rest-server -t "should create quest"` |
| Web suite / root | `bunx nx test @repo/web` |
| Web file / `apps/web` | `bun run test src/components/quests/create-quest-modal.component.test.tsx` |

## Tests, environment, and database

- Backend/core tests use `bun:test`; web tests use Vitest + jsdom (`apps/web/vitest.config.ts`). In `apps/web`, use `bun run test`, not `bun test`.
- Every rest-server test run preloads `test/setup.test.ts` via `bunfig.toml`, opening a real API on `:3001` and a PostgreSQL connection, including for domain unit tests. Start and migrate the test database first.
- Docker maps dev to `localhost:5442/quest_board_dev` and test to `localhost:5443/quest_board_test`. API tests use versioned root/app `test.env` files and truncate tables with `clearDatabase()`; do not run API test processes concurrently against that shared database.
- API tests use `RequestMaker`, `QuestsSeed` from `@repo/db/test`, and checkers under `apps/rest-server/test/checkers/`. Imports of `@repo/*/test` or `#test/*` are forbidden in production code by oxlint.
- `@repo/env` loads the root env file, then the same filename relative to the package working directory; file values overwrite existing environment values. Keep package-specific commands in their package directory or run them through Nx.
- `migrate:test` explicitly sets `MIGRATE_ENV_FILE=test.env`; keep this override because Nx may already have loaded the dev `DATABASE_URL`. See `packages/db/drizzle.config.ts` for precedence.
- Drizzle ORM and Kit are pinned to **1.0.0-rc.4**; follow local v1 table/query patterns. Schema changes go in `packages/db/src/schema/`, then `bun run migrate:generate`; include the generated migration and apply it to both databases. New tables must be exported in `schema/index.ts` and registered in `schema/relations.ts`.

## Wiring and contracts

- Backend entry: `apps/rest-server/src/index.ts` → `server.config.ts` → `api/rest.config.ts`. Register controllers in `api/controllers.ts`. All routes have `/api/v1`; routes ending in `/` are rejected at startup.
- Backend request flow is `api/` (Zod payloads/controllers) → `domain/` (use cases) → `data/` (Drizzle datasources). Use cases currently import datasources directly; `api/` and `data/` must not import each other. Backend imports use `#api`, `#domain`, `#data`, etc., with `.js` suffixes despite TypeScript source files.
- Web `App.tsx` renders the board directly; there is no router. Keep requests in `data/`, loading/error state in `domain/quests/` hooks, and screen components in `components/quests/`. `core/http/http-client.ts` already appends `/api/v1`; `VITE_API_URL` is only the server origin.
- Web aliases are `@assets`, `@atomic`, `@components`, `@core`, `@data`, and `@domain`, configured in both `tsconfig.app.json` and `vite.config.ts`. There is no `@app` or `@utils` alias despite the latter appearing in `FRONTEND.md`.
- Backend and web quest models/schemas are separate: update both for API contract changes. Backend `createdAt` is a `Date`; web receives a JSON string. `xpReward` is derived in backend `domain/quests/quests.utils.ts`, not stored in the quest table.
- `apps/web/src/atomic/` and `.storybook/` are vendored template code. Lint/format ignore most of them but re-include specific atomic files: consult `.oxlintrc.json` and `.oxfmtrc.json` rather than assuming the entire design system is excluded or reformatting it wholesale.

## Challenge rules

- Never edit `.opencode/plugin/` or `.opencode/logs/`; `session.jsonl` only goes in the final commit.
- Don't edit `apps/web/src/atomic/` (vendored design system).
- Don't add dependencies, migrations or broad refactors: the columns the TODOs need already exist.
- Backend layers: `api/` → `domain/` ← `data/`; `api/` and `data/` never import each other; XP and level rules live in `domain/`.
- Drizzle 1.0 RC, Zod 4, Elysia 1.4, Vitest 4: check the docs via context7 before using an API that doesn't already appear in the code.
- Backend tests need Docker running; to run one file: `bunx nx test @repo/rest-server <file>`.
- Behavior changes: test first (write or adjust it, run it, show it failing), then implement until it passes.
- Before finishing a task: `bun run typecheck` and `bun run test`.
- Code, test names and commits in English; user-facing text in pt-BR.
- Stage files explicitly (`git add <files>`), never `git add .`.