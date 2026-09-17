# API

This app is offline-first by default. Supabase is the backend for optional
sync — most "API" surface is Supabase's auto-generated REST/Realtime API
plus PowerSync's sync protocol, not custom hand-written endpoints.

## Auth
- Supabase Auth handles login/logout (email/password or magic link).
- Session token managed by the Supabase Flutter SDK; app should work
  fully offline/local-only if the user never logs in.
- Logout stops sync but does not wipe local data (see DECISIONS.md 007).

## Sync
- PowerSync connects local Drift/SQLite to Supabase Postgres.
- Sync rules scope every table to the authenticated user's rows only
  (mirrors Postgres RLS policies).
- Vault tables sync as ciphertext only — PowerSync/Supabase never
  transmit or store plaintext vault data under any circumstance.

## Phase 6 — AI (server-side, via Supabase Edge Function)
No API keys live on-device. Flow:
1. App calls a Supabase Edge Function (e.g. `categorize-expense`,
   `smart-reminder-parse`, `recommendation`).
2. Edge Function holds the Anthropic/OpenAI API key as a server-side
   secret and makes the actual AI call.
3. Only the minimum necessary data for the specific task is sent (e.g.
   one transaction note) — never bulk personal data, and **never**
   anything from the Vault module, ever, regardless of feature.

### Planned Edge Functions
- `categorize-expense` — suggest a category from a transaction note
- `habit-insights` — simple summary from local habit data
- `smart-reminder` — natural language → structured reminder
- `recommend` — recipe/goal recommendations based on stored (non-vault) data

## Storage
- Supabase Storage may be used later for larger document files (if
  `documents` blobs grow beyond what's practical in Postgres columns) —
  client-side encrypted before upload, same zero-knowledge rule applies.
