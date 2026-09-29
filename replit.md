# Neon Arena

Neon Arena is a mobile-first esports tournament platform for discovering matches, joining competitive rooms, and managing player wallets.

## Run & Operate

- `pnpm --filter @workspace/api-server run dev` — run the API server (port 5000)
- `pnpm run typecheck` — full typecheck across all packages
- `pnpm run build` — typecheck + build all packages
- `pnpm --filter @workspace/api-spec run codegen` — regenerate API hooks and Zod schemas from the OpenAPI spec
- `pnpm --filter @workspace/db run push` — push DB schema changes (dev only)
- `pnpm --filter @workspace/db run seed` — seed the development database with demo games, tournaments, a player wallet, and ledger activity
- Required env: `DATABASE_URL` — Postgres connection string

## Stack

- pnpm workspaces, Node.js 24, TypeScript 5.9
- API: Express 5
- DB: PostgreSQL + Drizzle ORM
- Validation: Zod (`zod/v4`), `drizzle-zod`
- API codegen: Orval (from OpenAPI spec)
- Build: esbuild (CJS bundle)

## Where things live

- `lib/api-spec/openapi.yaml` — source of truth for the API contract
- `lib/api-client-react/src/generated/` — generated React Query client and types
- `lib/api-zod/src/generated/` — generated server validation schemas
- `lib/db/src/schema/` — Drizzle schema for users, games, tournaments, entries, results, wallets, ledger transactions, deposits, withdrawals, and referrals
- `artifacts/api-server/src/routes/neon.ts` — current player, tournament, wallet, leaderboard, profile, and admin overview routes
- `artifacts/neon-arena/src/` — responsive web app and shared cyberpunk arena theme

## Architecture decisions

- API contracts are defined in OpenAPI first, then generated into the React client and Zod server validators.
- Money is stored as integer paise in Postgres and converted to rupees only at API boundaries.
- Wallet balances and an immutable wallet transaction ledger are separate models so financial history remains auditable.
- Tournament room credentials are stored on the tournament but only returned after membership and reveal-time checks.
- The first build uses a seeded demo player while authentication is added through the platform-managed auth flow.

## Product

- Player discovery dashboard with featured tournament cards and match stats
- Tournament detail, join, and room-access states
- Wallet summary, transaction history, manual UPI deposit, and withdrawal request flows
- Leaderboard, editable player profile, referral summary, admin overview, and OTP-style login entry screen

## User preferences

- The user requested a high-end commercial esports product rather than a basic AI-generated template.
- The user requested dark gaming mode, neon cyan with purple/red accents, glass surfaces, subtle glow, smooth motion, and mobile-first navigation.

## Gotchas

- Run `pnpm --filter @workspace/api-spec run codegen` after every OpenAPI change.
- Rebuild shared declarations with `pnpm run typecheck:libs` before checking leaf packages after changes in `lib/*`.

## Pointers

- See the `pnpm-workspace` skill for workspace structure, TypeScript setup, and package details
