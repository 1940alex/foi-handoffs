# AEP one-minute app, results list, and command center

Updated: 2026-10-01T12:50Z. Purpose: continue the Attention Effect Project minute instrument without mixing it up with TAE.

Receiving chat: open `C:\Users\atsak\Downloads\ad-worktree\foi-projects-roots\SheldrakeFieldProjectRoot` on branch `cursor/tae-v9`. Run `git pull`. Read `AGENTS.md`, `magno-sensor/MAGLAB.md`, then `magno-sensor/docs/MINUTE_ANDROID.md` and the One minute section of `magno-sensor/docs/IOS_CLIMB.md`. Recordings are never edited after upload. Do not publish the AEP APK on the TAE install page. Do not run `release.ps1 -Tae` for this app.

## What this is

AEP is a separate instrument from TAE. The Android package is `com.futureofinquiry.minute`. TAE stays `com.futureofinquiry.quietcheck`. The public name is AEP, Attention Effect Project. Current app version is 5, protocol `one-minute-accel-v1`. Install page: https://futureofinquiry.org/field-meter/maglab-aep/ (the old `maglab-minute` address serves that same page). The APK is `AEP-vN.apk`. Version key is `minute` in `magno-sensor/RELEASE.json`.

The phone records accelerometer only, 50 Hz, m/s², columns `wall_clock_utc_ms,x,y,z`. Each file keeps 60 seconds. The next minute starts at second 60. Nothing between minutes is thrown away. `session.json` has `gap_ms` 0, the session id, and the minute number. It has no attended flag and no run number. The screen is black, with the word AEP and a stop link. Stop ends the recording. There is no beep and no vibration. The clock stops if the sensor sends nothing for a whole minute. The phone POSTs the ZIP to `minute.php` while the next minute is already recording. A slow send does not stop the clock. The upload is retried until it is stored.

iPhone app 22 used the same protocol with a 5-second gap. iPhone app 25 and later use the 60-second clock and may leave `gap_ms` out of the file. Those later iPhone minutes belong on the no-gap list with Android app 5.

## Where the files live

The original ZIP is on Hostinger shared hosting, which is the instrument and the archive. There is no shell and no Python there.

`/public_html/field-meter/maglab-session-console/data/minutes/<session-id>/<minute>/phone-export.zip`

Door: https://futureofinquiry.org/field-meter/maglab-session-console/minute.php

`action=index` lists sessions. `action=fetch` returns one ZIP. A repeat of the same bytes is accepted. Different bytes for the same session and minute are refused.

The analyzer on the VPS keeps a second copy at `/root/maglab-analyzer/minutes/` and scores that copy. `analyzer/fetch.py` `fetch_new_minutes()` copies only minutes that are not already unpacked. The public website does not read shared hosting itself. `session-console/public/index.php` proxies `/maglab/public/...` to the analyzer at `https://signalbeyondproject.com:8443/maglab/`.

Keep it that way. The phone keeps posting to shared hosting. The console and the recipe stay on the VPS, reading the local copy. Do not score by pulling each ZIP off shared hosting on every click. Do not move the archive onto the VPS as the only copy. Two later cleanups, not done: the catalog re-hashes every ZIP on each index, and the fast FTPS login can see `data/runs` only, so minute files still come down through PHP.

## Results console

https://futureofinquiry.org/maglab/

This is the list, not the climb the person takes. It opens on **One minute**: no-gap minutes, health, quiet percent, wiggle, and recipe 01 lean. Health (`minute-qc-v1`) asks whether the file is a whole minute. Quiet percent is how many seconds were still after each second's mean is removed. Wiggle is the hardest shake, in milli-g; the stored field is still `movement_mg`. Lean is the median tilt of that minute from its own first 30 seconds, in millidegrees. A quiet minute with a large lean stays on the list. Older gapped minutes and the iOS climb list are behind Archive. Code: `analyzer/db/ios_minute.py`, command `python -m db.ios_minute`. These rows stay off `ios-climb-v16`.

**Check for new runs** copies new minute ZIPs and rewrites the page. It does not rebuild `maglab.db` for a one-minute file. A three-letter phone name is a label on the hardware serial (`analyzer/db/phone_names.py`). The Pixel in this session is **PXW**. Renaming a phone renames its sessions on the list. The recording is not edited.

Committed on `cursor/tae-v9` through `983e44bc`. The working tree is dirty and must not be thrown away or published by accident. Uncommitted local edits bump the results console from 57 to 58, drop the Run column on the minute list, and treat iPhone app 25+ with no `gap_ms` as no-gap (`no_gap()` in `ios_minute.py`, plus `IOS_CLIMB.md`, `browser_template.html`, `export_browser.py`, `RELEASE.json`, and `tests/test_ios_minute.py`). That 58 is not what this handoff verified live. Untracked analysis scripts and reports under `analyzer/` are also still uncommitted.

## Command center

This is the page a person uses for one climb. It does not start the phone. The phone should already be open on AEP, sitting still.

- Gap clock, recipe `aep-lean-v1`: https://futureofinquiry.org/maglab/public/aep — 65-second cycle, minutes whose `gap_ms` is not 0. Page `analyzer/aep_control.html`.
- No-gap clock, recipe `aep-lean-v2`: https://futureofinquiry.org/maglab/public/aep-live — 60-second cycle, no countdown. This is the current Android clock. Page `analyzer/aep_live.html`.

Both are served by `analyzer/webui.py` and scored in `analyzer/aep_score.py`. The score is the average tilt of minutes 4 and 5, in millidegrees, from the first 30 seconds of minute 1. A minute is calm at 100% quiet, hardest second at most 2 milli-g, minutes 4 and 5 within 4 millidegrees, and wobble of those two at most 4 millidegrees. The line is the 90th percentile of calm back-to-back minutes on that same phone, and it needs at least 20 such pairs. This is a smoke-test rule, not a published TAE score. The two pages keep separate boards: `analyzer/derived/aep-board.json` and `aep-board-live.json`.

The button calls `GET .../api/aep-live/next`. It will start only when the newest good minute on the VPS copy began within the last two cycles (120 seconds on the live clock). Otherwise the page says the phone has not sent a new minute. A sealed minute is already about 60 seconds old when it arrives, so that window is short on purpose: it means the phone's phase is still known.

## What this chat fixed

On 2026-10-01 the live page stayed on that message while AEP was open and the results list already showed new minutes. Shared hosting had the new session (minute 9 was about 80 seconds old). The VPS copy still stopped at minute 2. `watch()` never copied before it decided, so the two-minute gate treated a recording phone as silent. `score()` only copies after a climb has already started, and the stale page never gets that far.

`983e44bc` makes `watch()` call `fetch_new_minutes()` before it decides. That file was copied to `/root/maglab-analyzer/aep_score.py` and `maglab-analyzer` was restarted. Signal Beyond (`reading-app`) stayed active. The next live answer was `stale: false` for PXW, recipe `aep-lean-v2`, with a boundary on the next minute. Press **I want to do a run** again with the phone still open. The page then waits for that boundary, runs a five-minute climb and a one-minute break, and asks the VPS to score.

## Still open

- Results console 58 and the iPhone-25 no-gap rule are local only. Finish, test, and publish them on purpose, or leave them uncommitted. Do not ship them by deploying the whole dirty analyzer tree.
- The live page can still refuse a start if the newest sealed minute is older than 120 seconds, which happens while the just-finished minute has not arrived yet. Copy-on-click fixes the missed archive. It does not widen that gate.
- The catalog re-hash and the FTPS jail are the remaining host-speed issues. They were discussed and not changed.
- A climb still needs enough calm minutes on that phone before it will draw a line.
