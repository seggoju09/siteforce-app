# SiteForce — Phase 1 MVP

This is the real starter codebase for SiteForce, matching the "Phase 1 — Website" tech
stack from the workflow doc: Next.js on the frontend, Supabase for auth/database. It
replaces the earlier HTML prototype with an app that actually reads and writes data.

## What's real here

- `/labourers` — a signup form that inserts a real row into a Supabase `labourers`
  table when submitted.
- `/companies` — a browse page that reads live from that same table. Publish a
  profile, then open this page: it's really there, not mocked.
- `supabase/schema.sql` — the actual database schema (labourers, companies,
  hire_requests tables) with row-level security policies.

## What's intentionally stubbed, and why

- **No login yet.** The database policies currently let anyone insert a labourer
  profile with no auth, so the demo works immediately. Before a real launch, add
  [Supabase Auth](https://supabase.com/docs/guides/auth) and tighten the RLS
  policies in `schema.sql` so a labourer can only edit their own row — the file has
  comments marking exactly where.
- **No payments.** Stripe Connect (for the "pay only when you hire" model) isn't
  wired in yet — that's the next real milestone once signups are flowing.
- **No "shortlist and hire" UI yet** — the `hire_requests` table exists in the
  schema so it's ready to build against, but there's no page for it yet.

## Setup (takes about 10 minutes)

You'll need two free accounts that only you can create — a Supabase login and a
Vercel login are personal credentials, not something that can be set up on your
behalf.

1. **Install dependencies**
   ```bash
   npm install
   ```

2. **Create a Supabase project** at [supabase.com](https://supabase.com) (free tier
   is enough to start). In the project's SQL editor, paste and run the contents of
   `supabase/schema.sql`.

3. **Copy your API keys.** In Supabase: Settings → API. Copy the Project URL and the
   `anon public` key.

4. **Set up your environment**
   ```bash
   cp .env.example .env.local
   ```
   Paste your Project URL and anon key into `.env.local`.

5. **Run it locally**
   ```bash
   npm run dev
   ```
   Open http://localhost:3000 — publish a test profile at `/labourers`, then check
   it shows up at `/companies`.

## Deploying it for real

The easiest path is [Vercel](https://vercel.com) (made by the Next.js team, free
tier is enough to start):

```bash
npm install -g vercel
vercel login          # your own account — this step is yours to do
vercel link
vercel env add NEXT_PUBLIC_SUPABASE_URL
vercel env add NEXT_PUBLIC_SUPABASE_ANON_KEY
vercel --prod
```

That gives you a real, public URL running the actual product — not a prototype —
which is the thing worth pointing the first companies at.

## Next milestones, in order

1. Supabase Auth (labourer + company accounts)
2. Stripe Connect (pay-on-hire)
3. The shortlist/hire request flow using the `hire_requests` table
4. Point the domain you register at this deployment
