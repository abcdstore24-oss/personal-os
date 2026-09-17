# Architecture Decisions

Append-only log. Add a new numbered entry whenever a meaningful decision is
made. Never delete past entries — if a decision is reversed, add a new
entry that supersedes it and note that.

---

## 001 — Framework
**Decision**: React Native + Expo (TypeScript)
**Reason**: Offline-first, cross-platform, iOS becomes a later build target
instead of a separate rewrite. Large ecosystem for local DB, notifications,
biometrics.
**Date**: 2026-09-17

---

## 002 — Local database
**Decision**: SQLite, encrypted at rest.
**Reason**: Fully offline, no server dependency, mature RN support.
**Date**: 2026-09-17

---

## 003 — Vault encryption model
**Decision**: Zero-knowledge — AES-256 with key derived from the user's
PIN/biometric unlock. Encryption key never stored in plaintext.
**Reason**: Vault holds passwords, documents, personal info — must remain
private even if sync is added later.
**Date**: 2026-09-17

---

## 004 — Styling
**Decision**: NativeWind (Tailwind for React Native).
**Reason**: Fast to build a polished, consistent, themeable UI. Do not
introduce a second styling system.
**Date**: 2026-09-17

---

## 005 — AI approach
**Decision**: BYOK (bring your own key) — no shared hosted AI backend.
**Reason**: Avoids shared rate limits, keeps the app privacy-first, keeps
AI fully optional.
**Date**: 2026-09-17

---

## 006 — Build order
**Decision**: Phase 1 Foundation → Phase 2 Finance → Phase 3 Vault/Notes/
Documents → Phase 4 Goals/Tasks/Birthdays/Reminders → Phase 5 Recipes/
Beauty/Health/Wishlist → Phase 6 AI → Phase 7 iOS.
**Reason**: Build core infra and low-sensitivity modules first, prove
patterns before touching sensitive Vault data; AI and iOS are additive,
not core-path.
**Date**: 2026-09-17

---

## 007 — Stack change: Flutter + Supabase + PowerSync (supersedes 001, 002, 004)
**Decision**: Switch from React Native/Expo to Flutter; add Supabase
(Postgres + Auth + Storage + Realtime + RLS) as optional backend; add
PowerSync as the offline-sync engine. Local DB moves from plain
expo-sqlite to Drift + SQLCipher. State management moves from Zustand to
Riverpod. Styling moves from NativeWind to Flutter's native theming.
**Reason**: User decision — wants real multi-device sync, not just
local-only. Flutter chosen for single-codebase Android/iOS/Web. Supabase +
PowerSync is the standard, well-supported pairing for offline-first apps
needing SQLite↔Postgres sync with conflict resolution.
**Date**: 2026-09-17

---

## 008 — Two-layer auth model
**Decision**: Account login/logout (Supabase Auth) is separate from and
independent of the local app lock (biometric/PIN/pattern). Logging into
Supabase does not bypass the local lock.
**Reason**: Account layer identifies which cloud account syncs to this
device; local lock is the everyday privacy gate. Conflating them would
either weaken local security or make offline-only use impossible.
**Date**: 2026-09-17

---

## 009 — Logout does not wipe local data
**Decision**: Logging out of the Supabase account pauses/stops sync only.
Local data remains cached and fully usable offline. Re-login resumes sync.
**Reason**: Single-owner personal device — no need to force re-download of
data on logout, and no other user will ever use this device.
**Date**: 2026-09-17

---

## 010 — AI moves server-side (supersedes 005)
**Decision**: AI features (Phase 6) call a Supabase Edge Function, which
holds the API key server-side, instead of a client-side BYOK model.
**Reason**: Keeps API keys off-device entirely (more secure than
device-stored keys), fits naturally now that a backend exists anyway.
**Date**: 2026-09-17

---

## 011 — Design system locked
**Decision**: Fraunces (headers) + Inter/General Sans (body) via
google_fonts; deep charcoal (dark) / warm off-white (light) base with a
single warm amber/terracotta accent; 12–16px rounded corners; Phosphor or
Lucide icons app-wide. Full detail in CLAUDE.md Section 6.
**Reason**: Prevent UI drift across AI sessions — "amazing UI" needs a
concrete, reusable spec, not case-by-case improvisation.
**Date**: 2026-09-17
