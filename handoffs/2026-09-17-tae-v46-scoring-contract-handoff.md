# Sheldrake Field Project — TAE v46 scoring-contract handoff

Updated: 2026-09-17T17:54Z. Purpose: let a fresh Cursor, Codex, Claude, or ChatGPT conversation implement and release the behavior-preserving scoring-contract refactor.

## Start here
- Repository: `C:\Users\atsak\Downloads\ad-worktree\foi-projects-roots\SheldrakeFieldProjectRoot`; branch `cursor/tae-v9`.
- Pull first, read root `AGENTS.md` and `magno-sensor/MAGLAB.md`, and confirm `QuietRecordingService.kt` exists.
- Clean starting commit: `19a0ac60bb676ae05cf981c03ce508a523cf14eb`.
- Extensive implementation handoff: https://raw.githubusercontent.com/1940alex/sheldrake-field-project/19a0ac60bb676ae05cf981c03ce508a523cf14eb/magno-sensor/docs/TAE_V46_SCORING_CONTRACT_IMPLEMENTATION_HANDOFF.md
- Authoritative work order: https://raw.githubusercontent.com/1940alex/sheldrake-field-project/19a0ac60bb676ae05cf981c03ce508a523cf14eb/magno-sensor/docs/TAE_SCORING_CONTRACT_WORK_ORDER.md

## Authorized release target
- TAE 45 → 46; Analyzer 55 → 56; Results Console (`pairBrowser`) 22 → 23.
- MagLab app 59, command/session console 22, command protocol 3, and Room schema 10 remain unchanged.
- `release.ps1 -Console` currently bumps the wrong console. Add explicit Pair Browser/Results Console release support, then release through the script; do not use `-Console 23`.

## Decisions that must survive implementation
- Three sensors may have multiple outputs. Compass effect and Compass recovery remain separate formulas and scores.
- Do not invent Accelerometer or Gyroscope recovery recipes in v46.
- No current formula, window, gate, Fisher behavior, verdict, or historical fallback changes.
- Kotlin and Python may be implemented differently; named contract inputs and final results must agree within declared tolerance.
- Analyzer/database derived scores are the current public truth; phone scores remain immutable evidence of what was displayed.
- Never modify, delete, or re-upload raw runs.
- The separate Compass baseline-migration and capable-phone analyses do not authorize a v46 correction or qualification rule.

## Required result
- Introduce `tae-published-scores-v1` and descriptive recipe IDs, retaining v28/v29 aliases for history.
- Standardize explicit quiet/effect/post phase inputs and route every live/final phone output through a named recipe.
- Record structured score provenance in new phone runs while retaining legacy fields.
- Store versioned, rebuildable current scores and provenance in SQLite; browser export consumes that layer.
- Results Console header shows Results Console 23, Analyzer 56, TAE 46, contract ID, and all four current recipe IDs without cluttering run rows.
- Preserve v45 reference scores and verdicts within tolerance; unknown recipes fail closed.

## Finish
- Run focused and full TAE/analyzer/browser/database tests, version checks, generated-JavaScript validation, and the behavior-preservation comparison.
- Release TAE 46, Analyzer 56, and Pair Browser 23 with explicit targets; deploy and live-verify the results page, install page, APK bytes/SHA-256, refresh return code 0, and Signal Beyond untouched.
- Commit/tag/push and leave `cursor/tae-v9` clean. Report the live URLs, commit/tags, tests, APK identity, scoring manifest hash, and any real blocker.
