# MagLab TracFone Noise Handoff — 2026-08-18

## Session objective
Identify the Motorola and Samsung TracFone runs, expose durable handset names in MagLab Analyzer, and determine whether either phone is an adequate instrument relative to Pixel 7.

## Durable device identity
- Motorola TracFone / moto g - 2025: install `60081111-cf52-4924-be09-405fd20a86f8`.
- Samsung TracFone / SM-S156V: install `a16db91b-ddef-4a92-bda9-be2dcc3175fa`.
- Recent comparison Pixel 7: install `a610eeb9-8327-4113-a684-f5b8f5bf2755`.
- Morning Motorola runs: 100267, 100272, 100274, 100276.
- Morning Samsung runs: 100273, 100275, 100277. Earlier Samsung engineering runs: 100260, 100262.

## Corrected technical conclusion
- Do not describe the TracFones as having worse raw magnetometer noise.
- Robust 50 Hz sample noise: Pixel ~572 nT, Motorola ~255 nT, Samsung ~218 nT.
- One-second residual variation: Pixel ~89 nT, Motorola ~105 nT, Samsung ~154 nT.
- Clean five-minute calibrated drift: recent Pixel -143 to +57 nT; Motorola +438 to +601 nT; Samsung +223 to +360 nT.
- All three deliver complete 50 Hz streams with zero recounted sampling gaps.
- TracFone raw precision is good; the concern is repeatable low-frequency handset drift.

## Likely mechanism
- Most likely: slowly changing handset-generated field or sensor offset under recording load, left imperfectly removed by Android's fixed per-run bias correction.
- Motorola raw field is ~224 µT and calibrates to ~57 µT; Samsung raw is ~241 µT and calibrates to ~46–48 µT, demonstrating large handset hard-iron corrections.
- The reported Android bias vector is constant in each clean run, so it cannot track an offset that changes after recording begins.
- Ambient summaries show Motorola slopes around +100 to +136 nT/min during recording versus generally smaller pre/post slopes; Samsung shows a weaker version of the same pattern.
- Battery temperature is too coarse and is often flat, so it neither explains nor rules out internal electronics/sensor heating.
- Room position and tiny orientation effects remain possible because positions were not randomized.
- Pixel is operationally preferable today for five-minute stability, but another Pixel 7 install historically drifted about -0.3 to -0.6 µT; do not generalize from model name alone.

## Local code changes
- Added `magno-sensor/analyzer/devices.py`: committed-style seed registry for friendly handset labels keyed by install ID.
- Added `magno-sensor/analyzer/tests/test_devices.py`.
- Updated `magno-sensor/analyzer/webui.py` so dashboard and advanced table expose friendly phone, model, app version, and install-ID prefix.
- Targeted device/participant tests and `analyzer/deploy/smoke.py` pass.
- Changes are local only. Live deployment was requested for confirmation but was not authorized in this chat.
- Preserve unrelated existing dirty-worktree changes, especially analyzer reports, `run_metadata.py`, `study.py`, and `test_study.py`.

## Next work
1. If authorized, deploy the local Analyzer change with `deploy-maglab-analyzer.ps1`, then verify the live API/page shows Motorola and Samsung labels.
2. Run synchronized phones in a fixed-orientation jig after 15 minutes stabilization.
3. Rotate every phone through every physical position and alternate five minutes ambient-idle with five minutes recording without touching it.
4. Analyze phone, position, order, recording state, calibrated/uncalibrated vector drift, battery current, and temperature separately.
