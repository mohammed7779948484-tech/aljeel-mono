# 01 — Repo and Submodule State

**Date:** 2026-06-12  
**Task:** Port the legacy frontend homepage into the new Payload platform.

---

## Root monorepo (`aljeel-mono`)

- **Default branch:** `main`
- **Working branch (this task):** `feature/port-legacy-homepage` (created from `main`)
- **Top-level layout:**
  - `frontend/` — legacy Next.js frontend (visual reference, source of truth for the port)
  - `aau_university/` — legacy Frappe backend (read-only reference, will NOT be ported)
  - `aau-payload-platform/` — Git submodule for the new Payload+Next.js platform
  - `docs/` — migration, research, planning docs (we add `docs/homepage-port/` here)

`git submodule status` before checkout:
```
 fc59bdd431d8a39b7d84e44dbfcd2dbec4d4a2cb aau-payload-platform (remotes/origin/locale)
```

That was the `locale` tip. We move the submodule onto the stabilization branch
that previous work produced, then branch off it for this task.

---

## Submodule (`aau-payload-platform`)

- **Base branch:** `fix/phase-1-stabilization` (origin) — verified to contain
  the 7 stabilization commits from the previous phase:
  ```
  2435a91 docs: add stabilization validation report
  39828ca refactor: add puck config renderer and section blocks
  51b9dbc fix: secure public form submissions
  6a557c0 fix: harden rbac and coordinator scope
  b707205 fix: stabilize payload localization strategy (hybrid plan)
  1db349c fix: replace route locale architecture with same-page language switch
  af07852 docs: audit phase 1 stabilization issues
  ```
- **Working branch (this task):** `feature/port-legacy-homepage` (created from
  `fix/phase-1-stabilization`)
- **Working tree:** clean before this task starts.

### Stabilization artefacts present (all verified)

| File | Status |
|---|---|
| `docs/stabilization/01-current-state-audit.md` | ✅ |
| `docs/stabilization/02-best-practices-research.md` | ✅ |
| `docs/stabilization/03-localization-decision.md` | ✅ |
| `docs/stabilization/04-stabilization-plan.md` | ✅ |
| `docs/stabilization/05-validation-report.md` | ✅ |
| `src/i18n/LanguageProvider.tsx` | ✅ |
| `src/i18n/dictionary.ts` | ✅ |
| `src/i18n/locale.ts` | ✅ |
| `src/puck/config.ts` | ✅ |
| `src/puck/renderer/PuckRenderer.tsx` | ✅ |
| `src/puck/blocks/` (Hero, RichText, Stats, CTA) | ✅ |
| `src/app/(frontend)/page.tsx` | ✅ (Puck-aware with `HomeBody` fallback) |
| `src/app/(frontend)/layout.tsx` | ✅ (cookie-based locale, single root) |
| `src/app/api/public/contact/route.ts` | ✅ |
| `src/app/api/public/join-request/route.ts` | ✅ |

No required file is missing.

---

## Branches created for this task

| Location | Branch | Based on |
|---|---|---|
| Root monorepo (`aljeel-mono`) | `feature/port-legacy-homepage` | `main` |
| Submodule (`aau-payload-platform`) | `feature/port-legacy-homepage` | `fix/phase-1-stabilization` |

Constraints honoured:
- Not modifying `main` directly.
- Not modifying `locale` directly.
- Not modifying `fix/phase-1-stabilization` directly (we branch off it).
- No `--force` pushes, no `--hard` resets, no destruction of untracked files.

---

## Next step

Step 2 — deep audit of the legacy frontend homepage. Output:
`docs/homepage-port/02-legacy-homepage-audit.md`.
