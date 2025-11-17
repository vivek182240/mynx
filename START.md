# Quick Start Guide

## Prerequisites Completed ✅
- Dependencies installed
- Environment files created
- Code fixed and builds successfully

## Database Setup (Choose ONE)

### Option A: Supabase (Easiest)
1. Create account at https://supabase.com
2. Create new project
3. Get connection details from Settings → Database → Connection string (URI mode)
4. Update `services/api/.env`:
   ```
   DATABASE_URL=postgresql://postgres:[password]@[host]:5432/postgres
   ```
5. Update `web/.env`:
   ```
   NEXT_PUBLIC_SUPABASE_URL=https://[project-ref].supabase.co
   NEXT_PUBLIC_SUPABASE_ANON_KEY=[anon-key]
   ```

### Option B: Local PostgreSQL
1. Download from https://www.postgresql.org/download/windows/
2. Install with password `app` (or update DATABASE_URL in services/api/.env)
3. Current `.env` settings should work

## Run Migrations (After DB is ready)

```powershell
cd services/api
npm run prisma:migrate
npm run prisma:seed  # Optional: adds sample data
```

## Start Development Servers

### Terminal 1 - API:
```powershell
cd services/api
npm run dev
```
API will run on http://localhost:3001

### Terminal 2 - Web:
```powershell
cd web
npm run dev
```
Web will run on http://localhost:3000

## Test the App
1. Open http://localhost:3000
2. Sign up for an account
3. Browse creators
4. Book a call

## What's Working
- ✅ Full authentication (register/login)
- ✅ Creator profiles and browsing
- ✅ Booking flow with payment QR
- ✅ Admin dashboard
- ✅ Creator dashboard
- ✅ Real-time chat (Socket.IO ready)
- ✅ Payment integration (Stripe)
- ✅ Wallet system

## Optional: Add External Services
- **Stripe**: Add keys to `services/api/.env` for real payments
- **Resend**: Add key to `web/.env` for contact form emails
- **Video calls**: Integrate Agora/Twilio SDK in `/call/[username]` page
