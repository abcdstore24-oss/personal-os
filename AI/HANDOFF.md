# Handoff Prompt

Paste this at the start of any new AI chat/session/account when working on
this project. Attach or paste the contents of the 5 files it references.

---

You are joining an existing project called **PersonalOS**, an offline-first
personal life management app (Flutter + Riverpod + Drift/SQLCipher +
Supabase + PowerSync).

Before making any changes, read these project files (attached below):
1. `CLAUDE.md` — project overview, architecture, design system, rules
2. `DATABASE.md` — schema
3. `API.md` — AI/BYOK integration approach
4. `DECISIONS.md` — locked-in decisions, do not re-litigate these
5. `TODO.md` — current status and what's next

These files are the project's source of truth. Do not redesign the
architecture, introduce new libraries, or restructure folders unless I
explicitly ask you to — follow the Rules section in `CLAUDE.md`.

For the current task, I will tell you which specific source files (if any
already exist) are relevant. Only inspect and modify those, plus anything
that logically must change with them. Do not touch unrelated files.

Before writing code:
- Briefly confirm your understanding of the task
- Identify which files will be affected
- Then implement only what was asked

When the task is done, tell me what (if anything) should be updated in
`TODO.md` or `DECISIONS.md` — I will update them myself before switching
sessions.

My current task is:
[DESCRIBE THE SPECIFIC FEATURE/BUG/TASK HERE]
