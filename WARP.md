# WARP.md

This file provides guidance to WARP (warp.dev) when working with code in this repository.

Repository overview
- Monorepo with two apps:
  - web/ — Next.js (TypeScript, App Router, Tailwind v4, MDX). Unit tests via Vitest + Testing Library; E2E via Playwright.
  - services/api/ — NestJS (TypeScript) with modules for auth, chat (Socket.IO gateway), payments (Stripe), and wallet. Prisma for PostgreSQL.
- Goal (from README): replicate the UX/UI and core functionality of https://mynx.co using original content/assets.

Common commands

Frontend (web/)
- Install deps:
  ```pwsh
  cd web
  npm install
  ```
- Dev server (port 3000):
  ```pwsh
  npm run dev
  ```
- Build / start (prod):
  ```pwsh
  npm run build
  npm start
  ```
- Lint:
  ```pwsh
  npm run lint -- .
  ```

Unit tests (web)
- Watch:
  ```pwsh
  npm run test
  ```
- Run once:
  ```pwsh
  npm run test:run
  ```
- Single file:
  ```pwsh
  npm run test -- src/components/__tests__/Hello.test.tsx
  ```
- By name/pattern:
  ```pwsh
  npm run test -- -t "partial test name"
  ```

E2E tests (web)
- Run all / headed / UI / report:
  ```pwsh
  npm run e2e
  npm run e2e:headed
  npm run e2e:ui
  npm run e2e:report
  ```
- Single spec:
  ```pwsh
  npm run e2e -- tests/e2e/<file>.spec.ts
  ```
- Note: Playwright baseURL is http://localhost:3000 — start the web dev server first.

Backend (services/api/)
- Install deps:
  ```pwsh
  cd services/api
  npm install
  ```
- Prisma (requires DATABASE_URL):
  ```pwsh
  npm run prisma:generate
  npm run prisma:migrate
  ```
- Dev server (port 3001 default) / build / start:
  ```pwsh
  npm run dev
  npm run build
  npm start
  ```
- Lint:
  ```pwsh
  npm run lint
  ```

Environment
- web/.env.example
  - NEXT_PUBLIC_SITE_URL (default http://localhost:3000)
  - RESEND_API_KEY, CONTACT_TO, CONTACT_FROM (for contact email route)
- services/api/.env.example
  - DATABASE_URL (PostgreSQL), REDIS_URL, JWT_SECRET
  - STRIPE_SECRET, STRIPE_WEBHOOK_SECRET
  - COMMISSION_BPS, PORT (defaults to 3001 if unset)

Architecture (big picture)

web/
- App Router under src/app (grouped routes like (marketing)/). API routes under src/app/api/<segment>/route.ts — e.g., contact form (Resend).
- MDX enabled via @next/mdx (see next.config.ts). pageExtensions include ts/tsx/mdx.
- Components in src/components (feature folders like marketing/ and generic ui/). Global styles in src/app/globals.css. Tailwind v4 via @tailwindcss/postcss.
- Testing: Vitest configured with jsdom and setup at src/test/setup.ts; E2E via Playwright (tests/e2e, baseURL http://localhost:3000). TS path alias: "@/*" -> src/*.

services/api/
- NestJS modules: AuthModule (JWT), ChatModule (Socket.IO gateway at /chat; joins rooms per conversation), PaymentsModule (Stripe integration), WalletModule (ledger ops). PrismaService is app-wide.
- main.ts: CORS enabled, cookie parsing, global ValidationPipe (whitelist/transform). App listens on PORT or 3001.
- Prisma schema models: User, Creator, Conversation, Message (TEXT/IMAGE/AUDIO/VIDEO), WalletAccount, LedgerEntry.

Run the stack locally
- Terminal 1 (API):
  ```pwsh
  cd services/api
  copy .env.example .env
  npm install
  npm run prisma:generate
  npm run prisma:migrate
  npm run dev
  ```
- Terminal 2 (Web):
  ```pwsh
  cd web
  copy .env.example .env
  npm install
  npm run dev
  ```
- E2E tests assume the web app is running at http://localhost:3000.
