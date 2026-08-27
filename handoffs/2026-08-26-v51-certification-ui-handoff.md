# v51 certification and recording UI

- Released Android app v51; analyzer remains 19, console remains 20, protocol remains 3.
- Release tag/commit: `apk-v51` / `12f0f232`; source commits `f3da3a81`, `71015932`.
- Public install page: https://futureofinquiry.org/field-meter/maglab/
- APK: `maglab-grok-v51.apk`, 17,165,526 bytes, SHA-256 `4b4f67d505d8c1fc2ec242fd2eeb1945bd231631479b9971633259dffd5393d9`.
- Certification is now one-touch: warm-up and 2–4 scored intervals auto-chain without touching the phone.
- The certification UI implements the circular Q gauge, interval count, good-streak progress, automatic countdown, and final pass/fail/quarantine result.
- Emergency stop retains the sitting, records a quarantine decision, and queues upload.
- Recording UI now leads with run number, truthful recording/starting state, time remaining, and the one instruction that matters; optional live feedback remains symmetric across conditions.
- Tests: 237 Android unit tests and 129 console tests passed; APK assembled and deploy script live-verified the install page and endpoints.
- Alex's first v50 adaptive sitting had not reached `qualification.php?action=index` by the final check; newest complete sitting remained Q0012/v48G. Check again before interpreting it.
- The large pre-existing dirty analyzer/report tree was stashed for release and restored afterward; do not stage it accidentally.
