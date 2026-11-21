# STAT ATTACK — public website starter
Includes responsive website, Supabase schema, Edge Function and a blank CSV question template.

## Content integrity
No BBL stats have been fabricated or bundled. The site becomes playable after at least **7 verified active questions** are inserted. Every question must include an exact numeric answer, clear season/scope, source name and public source URL. Super Over uses six random active questions.

## Setup
1. Create a free Supabase project: https://supabase.com
2. In `index.html`, replace `YOUR_SUPABASE_URL` and `YOUR_SUPABASE_ANON_KEY` in the `CONFIG` object.
3. In Supabase SQL Editor, run `supabase/migrations/001_stat_attack.sql`.
4. Install Supabase CLI, then from this folder run:
   `supabase login`
   `supabase link --project-ref YOUR_PROJECT_REF`
   `supabase functions deploy game-api`
   Supabase Edge Functions provide `SUPABASE_URL` and `SUPABASE_SERVICE_ROLE_KEY` automatically; if needed set them as function secrets. Never place service-role key in the website.
5. Add verified question rows in SQL Editor. Example (replace all placeholders with real verified values):
   `insert into public.stat_questions(prompt,context,answer,unit,season,source_name,source_url,scope,active) values ('Question text','Exact season and stat definition',123,'runs','BBL|XX','Official scorecard','https://REAL-SOURCE-URL','BBL',true);`
   Use `content/questions-template.csv` as your data layout. The answer field must be numeric. Set active=true only after checking the source.
6. Publish free on Cloudflare Pages: push this folder to GitHub; Cloudflare Dashboard → Workers & Pages → Create → Pages → Connect to Git. Framework preset None, build command blank, output directory `/`. Use the free `*.pages.dev` address.

## Behaviour
- Nickname-only player access (saved locally in browser).
- Daily Seven is stored server-side per date and shared by everyone; resets at midnight Australia/Brisbane.
- Six-question random Super Over.
- Server computes scores: accuracy is 90% of points; speed contributes at most 10%.
- Share button uses native phone share where supported, otherwise copies a share message.
- Global daily leaderboard.

## Production hardening before a public launch
This is a deployable starter, not a security-audited production service. Browser-reported elapsed time can be manipulated; add server-issued question timing/session tokens and rate limits for stronger anti-cheat. Also add one-entry-per-player controls, nickname moderation/profanity filtering, abuse reporting, privacy/terms pages and backups. Nicknames are not authenticated accounts, so a player can change identity by changing browsers/devices.
