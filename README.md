# XAAJI NEXUS AI

Next.js 15 + TypeScript + Tailwind + Supabase (Postgres, Auth, Row Level Security). AI runs only on the server.

## 1. Services you must configure
| Service | Why | What to do |
|---|---|---|
| Supabase (free tier ok) | database, auth, RLS | Create project, run `supabase/schema.sql` in SQL Editor, copy URL + anon key |
| Anthropic API | AI chat and Creator Studio text tools | Create API key at console.anthropic.com |
| WhatsApp number (optional) | support button | Put official number (digits, with country code) in `NEXT_PUBLIC_WHATSAPP_NUMBER` |

Supabase > Authentication > URL Configuration: set Site URL to your Vercel URL and add `http://localhost:3000` to Redirect URLs.

## 2. Run locally
```bash
npm install
cp .env.example .env.local   # fill in values
npm run dev                  # http://localhost:3000
npm run typecheck
```

## 3. Create the first admin
Register in the app, then in Supabase SQL Editor:
```sql
update public.profiles set role = 'admin' where id = (select id from auth.users where email = 'YOUR_EMAIL');
```
Admin permissions are enforced by Postgres RLS (`is_admin()`) and a server-side guard in `src/app/admin/layout.tsx`, not by hidden buttons.

## 4. Deploy: GitHub + Vercel
```bash
git init && git add . && git commit -m "XAAJI NEXUS AI"
git branch -M main
git remote add origin https://github.com/YOUR_USER/xaaji-nexus-ai.git
git push -u origin main
```
Vercel > Add New Project > import the repo > add the 5 environment variables from `.env.example` > Deploy. Never commit `.env.local`.

## 5. Honest notes
- Leaderboard scores are sent by the browser. The database enforces login, ownership and score ranges, but a determined cheater can still submit a valid-range score. Treat it as casual, not competitive.
- Support form has validation and a honeypot, no CAPTCHA or per-IP limit. Add one (e.g. Cloudflare Turnstile) if you get spam.
- Creator Studio: the prompt generator is a template (no AI). Ideas, captions and posts are AI text. None of them create video.
- Tic-Tac-Toe score is per session only.
- Videos: add a valid `https://` YouTube or .mp4/.webm URL on a lesson in the admin panel.
