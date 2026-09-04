# TAE v8 Planning Handoff

Date: 2026-09-04
Repository: `1940alex/sheldrake-field-project`
Branch: `claude/v39-three-phone`

## Purpose
Start the next conversation by reviewing and agreeing on the TAE v8 plan before implementation.

## Production baseline
- TAE v7 is built, tested, deployed, committed, tagged `quietcheck-v7`, and publicly available at https://futureofinquiry.org/field-meter/maglab/
- Release commit: `956bc4e3`; v7 implementation: `0db04979`; production handoff: `4617ea21`.
- Current release values: TAE/QuietCheck 7, analyzer 31, MagLab 59, console 22, command protocol 3.
- Known validation item: the v7 settle-based baseline needs real field data before its behavior can be judged.

## V8 planning artifacts
- `magno-sensor/docs/TAE_WORK_ORDER_V05.md`
- `magno-sensor/docs/TAE_TESTBED_CONTRACT.md`
- `magno-sensor/docs/tae_testbed_v1.csv`
- These are committed and pushed in `802dfcae` without implementation changes.
- The work order is explicitly a draft for joint review; do not begin implementation until the plan is agreed.

## Proposed v8 core
- One standalone single-phone block: 180 s settle, 420 s leg, 60 s gap, 420 s leg; acquisition ends at 18:00.
- Settle is never a baseline. Attention-first uses the previous completed control on that handset, or zero when unavailable.
- Frozen response-axis scoring remains `phone_a_full_vector_v1`.
- Chance model uses pooled residual SD, `sigma_nt = 1.68 × pooled residual SD`, and `z = separation / sigma_nt`.
- Final feedback is `clamp(floor(z), 0, 5)` bars; success is `z >= 2`.
- Device scores locally and the server recomputes; differences above ±0.5 nT are a loud defect.
- Acceptance replays all 15 benchmark rows through the app's actual pure scoring functions and matches the expected CSV.

## Decisions for the next conversation
1. Keep the 1.68 calibration from 28 Cherylee Phone A null pairs for all phones, or delay verdicts until a handset has its own calibration evidence?
2. Keep whole bars using `floor(z)`, or show proportional fill in the last bar?
3. Show the numerical z-score after each block, or only bars, separation, sigma, and verdict?
4. Show results immediately while upload finishes, or wait up to the 120 s closeout before showing them?

## Guardrails
- Never modify recorded run data.
- Do not touch `analyzer/db/build.py` or `analyzer/db/install_map.csv` for this work order.
- Keep command protocol 3 unless a protocol change is explicitly reviewed first.
- Before release, use `magno-sensor/tools/release.ps1` and verify the live APK and analyzer.
