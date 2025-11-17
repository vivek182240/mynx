# mynx-co-replica

Goal
- Build a full-stack web application that replicates the look-and-feel and core functionality of https://mynx.co.

Notes
- We will not copy proprietary text, images, or code from the target site. Use original content and placeholder assets while recreating the UX/UI and behavior.
- Frontend: modern React/Next.js stack with TypeScript and Tailwind CSS (can adjust later).
- Backend: API routes and services to support features we discover (auth, content, forms, etc.).

Getting Started
- Project was initialized as a git repository.
- Further scaffolding will follow project rules you configure next.

## Running the full stack locally

Prerequisites:
- Docker + docker-compose
- Node.js 18+ and npm

### 1. Start infrastructure (Postgres + Redis)

From the repo root:

```bash
docker-compose up -d
```

This will start Postgres on `localhost:5432` and Redis on `localhost:6379` using the credentials defined in `docker-compose.yml`.

### 2. Configure and run the API service

From `services/api`:

```bash
cp .env.example .env
# edit .env if needed (DATABASE_URL, REDIS_URL, PORT, etc.)
npm install
npx prisma migrate dev
npm run prisma:seed   # optional: load sample users/creators + demo wallet balance
npm run dev
```

The API will listen on the port specified in `.env` (default `3001`). It exposes endpoints used by the web app, such as:
- `POST /auth/register`, `POST /auth/login`
- `GET /creators`, `GET /creators/by-username/:username`
- `POST /creators/:id/bookings`
- `POST /media/creators/:creatorId/upload`

### 3. Configure and run the web app

From `web`:

```bash
npm install
# point the frontend at the API
set NEXT_PUBLIC_API_URL=http://localhost:3001
npm run dev
```

Then open http://localhost:3000 in your browser. The web app will:
- Fetch creators and model data from the API
- Let users register/login and persist auth in `localStorage`
- Allow creators to upload media and fans to book calls (via the API)

### 4. Quality checks

From `web`:

```bash
npm run build   # Next.js production build
npm run lint    # ESLint (Next.js + React rules)
npm run test:run  # Vitest unit tests
```

From `services/api`:

```bash
npm run build   # TypeScript build for NestJS API
npm run test    # Jest integration tests (requires DATABASE_URL and running Postgres)
```
