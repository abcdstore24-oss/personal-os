# Current Status

## Phase 1 — Foundation (not started)
- [ ] Project scaffold (Flutter + Riverpod)
- [ ] Theme system (light/dark tokens, Fraunces + Inter typography)
- [ ] Local app lock screen (biometric + PIN/pattern fallback)
- [ ] Local encrypted DB setup (Drift + SQLCipher)
- [ ] Supabase project setup (Auth, Postgres, RLS policies scaffolded)
- [ ] Login/logout screens (Supabase Auth — optional, app works without it)
- [ ] PowerSync connection wired up (can be stubbed/disabled initially)
- [ ] Navigation shell
- [ ] Dashboard shell (empty states for all widgets)

## Phase 2 — Finance (not started)
- [ ] Transactions (income/expense) CRUD
- [ ] Categories
- [ ] Monthly summary + graphs
- [ ] Budget tracking
- [ ] Investment tracking

## Phase 3 — Vault, Notes, Documents (not started)
- [ ] Vault encryption implementation + tests
- [ ] Credentials storage
- [ ] Document references storage
- [ ] Personal info storage
- [ ] Notes system (folders, tags, search)

## Phase 4 — Goals, Tasks, Birthdays, Reminders (not started)
- [ ] Goal tracking (short/long term, deadlines, progress)
- [ ] Task management (to-do, recurring, priority)
- [ ] Birthday & event manager + calendar view
- [ ] Local notifications for reminders/birthdays/tasks
- [ ] Alarm clock feature

## Phase 5 — Recipes, Beauty, Health, Wishlist (not started)
- [ ] Recipe manager
- [ ] Beauty care section
- [ ] Healthy lifestyle section + habit tracking
- [ ] Purchase planner / wishlist + savings-progress linking

## Phase 6 — AI (not started, optional)
- [ ] BYOK settings screen
- [ ] Smart expense categorization
- [ ] Smart reminders
- [ ] Personalized recommendations

## Phase 7 — iOS (not started, optional, later)
- [ ] iOS build target
- [ ] iOS-specific QA (biometrics, notifications)

---

## Current task
_Nothing in progress yet — project is in planning/setup stage._

## Changelog
- 2026-09-17: Project planning started. Stack, architecture, and phase
  order decided (see DECISIONS.md).
- 2026-09-17: Stack switched to Flutter + Supabase + PowerSync. Two-layer
  auth model (account login/logout + local lock) added. Design system
  locked. See DECISIONS.md 007–011.
