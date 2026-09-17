# PersonalOS (working name)

## 1. What this app is
An offline-first personal life management app. Single user (the owner).
All data lives locally first; an optional cloud layer (Supabase +
PowerSync) enables multi-device sync. A locked "vault" section holds
sensitive data (passwords, documents, personal info) with zero-knowledge
encryption — the server must never be able to read vault plaintext, synced
or not.

## 2. Platforms
Flutter — single codebase targets Android, iOS, and Web. iOS build/release
only happens once Apple Developer account is set up (later phase); Android
and Web are the initial targets.

## 3. Tech stack
| Layer | Choice |
|---|---|
| Client framework | Flutter |
| Local database | Drift (SQLite wrapper) |
| Local encryption | SQLCipher (encrypted local DB) + flutter_secure_storage (Android Keystore / iOS Keychain for keys) |
| State management | Riverpod |
| Charts/graphs | fl_chart |
| Backend | Supabase (Postgres + Auth + Storage + Realtime + Row-Level Security) |
| Offline sync engine | PowerSync (SQLite ↔ Postgres delta sync, conflict resolution) |
| Push notifications | Firebase Cloud Messaging |
| AI features (Phase 6, optional) | Supabase Edge Function → Claude/OpenAI API (server-side only, key never on-device) |

**Cost at personal scale**: $0/month running cost (free tiers). One-time
$25 Google Play fee at Android launch; $99/year Apple Developer fee only
if/when iOS ships.

## 4. Auth model — two independent lock layers
1. **Account login/logout** (Supabase Auth — email/password or magic
   link): identifies which cloud account this device syncs to. Required
   only for sync; app should still be usable fully offline/local-only
   without ever logging in, if the user chooses not to sync.
2. **Local app lock** (biometric / PIN / pattern): the everyday unlock
   gate, every time the app is opened. Independent of account login —
   being logged into Supabase does not skip the local lock.

**Logout behavior (decided)**: logout pauses/stops sync only. It does
**not** wipe local data — this is a single-owner personal device, so local
data stays cached and usable offline after logout. Re-login resumes sync.

**Vault + sync**: Vault data is encrypted client-side (SQLCipher +
per-item AES via secure-storage-derived key) *before* anything is written
to a syncable table. Supabase RLS scopes rows to the owner's account, but
the server only ever stores ciphertext it cannot read — this holds true
whether syncing or fully offline.

## 5. Folder structure (Flutter conventions)
```
lib/
  main.dart
  app/                     -- app shell, routing, theming bootstrap
  features/
    dashboard/
    finance/
    vault/
    notes/
    goals/
    birthdays/
    tasks/
    recipes/
    beauty/
    health/
    wishlist/
    settings/
    auth/                  -- login/logout, session state
  core/
    db/                    -- Drift schema, migrations, DAOs
    encryption/            -- SQLCipher + key derivation, vault crypto
    sync/                  -- PowerSync setup and config
    notifications/         -- local + FCM notification handling
    theme/                 -- design tokens, light/dark themes, typography
    widgets/               -- shared/reusable UI components
    api/                   -- calls to Supabase Edge Functions (AI, Phase 6)
AI/
  CLAUDE.md
  DATABASE.md
  API.md
  DECISIONS.md
  TODO.md
  HANDOFF.md
```

## 6. Design system (decided — not left open-ended)
- **Mood**: calm, private, premium. This app holds money, passwords, and
  documents — it should feel trustworthy and personal, not like a generic
  CRUD utility.
- **Typography**: display/header font — Fraunces (serif, warm,
  distinctive). Body font — Inter or General Sans (clean grotesk). Load
  via `google_fonts`. Never mix in a third font family.
- **Color**: dark mode = deep charcoal/near-black base; light mode = warm
  off-white base. Single warm accent color (amber/terracotta) used
  sparingly for CTAs, active states, and highlights — not a multi-color
  dashboard. Define all colors as theme tokens, never hardcoded hex in
  widgets.
- **Shape/elevation**: rounded corners (12–16px radius) throughout. Soft
  shadows in light mode only; flat/borderless cards in dark mode.
  Generous whitespace — avoid dense data-grid layouts, even in Finance.
- **Icons**: one consistent icon set app-wide (Phosphor or Lucide). Never
  mix icon styles across modules.
- **Personal touch**: dashboard uses the owner's stored birthdate for
  personalized details (age, days to next birthday) — a signature
  feature, not generic.
- **Theme toggle**: light/dark required from Phase 1, driven entirely by
  theme tokens so it works everywhere by default.

## 7. Rules AI must follow when working on this project
1. Do not rewrite or restructure existing working code unless explicitly
   asked to.
2. Make only the requested change for the current task — do not "improve"
   unrelated files.
3. Do not introduce a new package when one already in the stack table
   above covers the need.
4. Do not create duplicate widgets, DAOs, or providers — check existing
   implementation first.
5. Follow the design system in Section 6 exactly — do not invent new
   colors, fonts, or shapes on the fly.
6. Vault/encryption code (`core/encryption/`, `core/sync/`) is sensitive —
   do not modify casually; explicitly flag any proposed change to it
   before making it.
7. Follow the folder structure above. Don't invent new top-level folders
   without asking.
8. Before writing code: identify which files are affected, briefly
   explain the plan, then implement.
9. After finishing a feature: update `AI/TODO.md` status and add an entry
   to `AI/DECISIONS.md` if any non-trivial decision was made.

## 8. Current status
See `AI/TODO.md` for live status. Planning phase — implementation not yet
started.
