# New Football League site

Static site (one `index.html`) + Supabase for the inquiry form. Hosted on Vercel from GitHub.

## 1. Supabase (inquiry form)
Create a project, open **SQL Editor**, run:

```sql
create table public.inquiries (
  id bigint generated always as identity primary key,
  type text not null check (type in ('sponsor','career','general')),
  name text not null check (char_length(name) <= 120),
  email text not null check (char_length(email) <= 200),
  message text not null check (char_length(message) <= 4000),
  created_at timestamptz not null default now()
);
alter table public.inquiries enable row level security;
-- Visitors can only add rows. Nobody can read them with the public key.
create policy "anyone can submit" on public.inquiries for insert to anon with check (true);
```
Then **Project Settings → API**: copy the Project URL and the `anon` public key into `SUPABASE_URL` and `SUPABASE_ANON_KEY` at the top of the script in `index.html`. The anon key is meant to be public; the policy above is what protects your data.
Read inquiries in **Table Editor**. For email alerts, add a Database Webhook or Edge Function later.

## 2. GitHub
```bash
cd new-football-league
git init && git add . && git commit -m "Initial site"
git branch -M main
git remote add origin https://github.com/YOUR-USER/new-football-league.git
git push -u origin main
```

## 3. Vercel
**Add New → Project → import the repo.** Framework preset: *Other*. No build command, no output directory. Deploy.
Every push to `main` redeploys automatically.

## 4. Domain
Vercel project → **Settings → Domains** → add your domain (e.g. newfootballleague.com) and follow the DNS instructions.
Then, on chsserviceprovider.com, point the old League page to the new domain.

## Editing
- Teams and divisions: the `DIVISIONS` object in `index.html`.
- Rules and copy: plain HTML in the sections.
