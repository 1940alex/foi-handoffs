# MagLab FTPS run-fetch handoff

Updated: 2026-09-17T00:25:00Z

## Purpose
Continue work on the Future of Inquiry MagLab analyzer after making “Check for new runs” use FTPS instead of the rate-limited public web route.

## Current state
- Project: `C:\Users\atsak\Downloads\ad-worktree\foi-projects-roots\SheldrakeFieldProjectRoot`
- Branch: `cursor/tae-v9`, clean and pushed to `origin`.
- Live analyzer: https://futureofinquiry.org/maglab/
- Analyzer v53 is deployed and tagged `analyzer-v53`.
- Implementation commit: `14390790`; release commit: `da5a9996`; durable-doc commit: `2f27eb1c`.
- Current branch HEAD also includes later commit `14fdb698`.

## What changed
- `magno-sensor/analyzer/fetch.py` now uses FTPS whenever `/root/maglab-analyzer/.ftp_runs` exists; public `runs.php` is the automatic fallback only when it is absent.
- The credential file is three lines (host, username, password), owned by root and mode `600`. Never print or commit it.
- The Hostinger FTP account is jailed to `public_html/field-meter/maglab-session-console/data/runs`; it cannot reach its parent.
- SHA-256 extraction and verification are identical for FTPS and HTTPS.
- Tests: `magno-sensor/analyzer/tests/test_fetch_transport.py`.
- Durable instructions are in `AGENTS.md`, `magno-sensor/MAGLAB.md`, and `magno-sensor/docs/maglab/{hosts,analysis,data}.md`.
- `magno-sensor/docs/CONSOLE_FETCH_RATE_LIMIT.md` is marked diagnostic history; do not implement its old HTTP proposals.

## Live verification
- Test run 101653 fetched over FTPS in 0.61 s; all 18 files passed `SHA256SUMS.txt`.
- The jail stayed at `/` after `cwd ..`; `../upload.php` was denied.
- A later refresh fetched runs 102013–102020 by FTPS, verified every run, rebuilt `maglab.db`, and rewrote the public page in 27 s.
- Public page contains run 102020; `/maglab/api/job` ended in state `done`.
- `maglab-analyzer` and unrelated `reading-app` remained active.

## Timing gotcha
The first refresh after v53 deployment began before `.ftp_runs` had been installed, so that already-running process remained in the old HTTPS backoff loop and caused the UI to say “job already running.” It finished normally. Subsequent jobs use FTPS and complete quickly; do not clear or recreate the account.

## Receiving conversation
Read root `AGENTS.md`, then `magno-sensor/MAGLAB.md`; open only the routed docs needed. Before changing or deploying, inspect git status and the live job endpoint. Never modify raw run data or touch Signal Beyond.
