# Database

Local: SQLite via Drift, encrypted with SQLCipher.
Cloud (optional, for sync): Postgres via Supabase, mirrored via PowerSync.
Schema should stay structurally identical between local Drift tables and
Supabase Postgres tables so PowerSync sync rules stay simple.

Status: **not yet designed in detail — fill in as Phase 1–5 tables are built.**

## Conventions
- Every table: `id` (uuid), `created_at`, `updated_at`, `user_id` (for
  Supabase RLS scoping — even if unused locally when not synced)
- Vault-related tables store encrypted blobs only, never plaintext columns
  for sensitive fields (passwords, document data, credential values,
  personal info)
- Soft-delete (`deleted_at`) rather than hard delete
- Supabase Row-Level Security: every table policy restricts rows to
  `auth.uid() = user_id` — no cross-user access, ever

## Auth-related (Supabase-managed, not custom tables)
- `auth.users` — handled by Supabase Auth directly
- App only needs a `profiles` table (id references auth.users, display
  name, birthdate, avatar) for the personalized dashboard

## Planned tables (draft — refine during Phase 1–5 implementation)

### finance
- `transactions` (id, user_id, type[income/expense], amount, category_id, note, date)
- `categories` (id, user_id, name, type)
- `budgets` (id, user_id, category_id, monthly_limit)
- `investments` (id, user_id, name, amount_invested, current_value, notes, updated_at)

### vault (encrypted fields marked *enc* — ciphertext only, even in Postgres)
- `credentials` (id, user_id, title, username *enc*, password *enc*, url, notes *enc*)
- `documents` (id, user_id, title, type, encrypted_blob *enc*, notes *enc*)
- `personal_info` (id, user_id, label, value *enc*)
- `vault_notes` (id, user_id, title *enc*, body *enc*)

### notes
- `notes` (id, user_id, title, body, folder_id, tags, pinned)
- `folders` (id, user_id, name)

### goals
- `goals` (id, user_id, title, type[short/long], deadline, progress, status)

### birthdays_events
- `people` (id, user_id, name, birthdate, notes)
- `events` (id, user_id, title, date, recurring, reminder_offset)

### tasks
- `tasks` (id, user_id, title, due_date, priority, recurring_rule, status)

### recipes
- `recipes` (id, user_id, title, ingredients, instructions, tags, category)

### beauty
- `beauty_routines` (id, user_id, type[skin/hair], frequency, steps, notes)

### health
- `habits` (id, user_id, name, target_frequency, streak)
- `habit_logs` (id, user_id, habit_id, date, completed)
- `health_notes` (id, user_id, title, body, category)

### wishlist
- `wishlist_items` (id, user_id, title, category[short/long], est_price,
  priority, notes, link, target_date, status[planned/saved/purchased],
  linked_savings_amount)

## Relationships
- `wishlist_items.linked_savings_amount` relates to `finance.budgets` /
  `finance.transactions` for savings-progress tracking
- `events` and `people` both feed the dashboard's "upcoming" widget
- `habit_logs` feeds the dashboard's habit-tracking widget
- `profiles.birthdate` feeds the dashboard's personalized-details widget
