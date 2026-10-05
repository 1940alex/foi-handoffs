# AEP climb grade

Updated: 2026-10-05T14:00:00Z. Purpose: continue the Attention Effect collector at attentioneffectproject.com. This is not the older one-minute MagLab app.

Receiving chat: repo `https://github.com/1940alex/aep.git`, folder `c:\Users\atsak\Downloads\ad-worktree\foi-projects-roots\aep`. Read `ANALYSIS.md` first, especially "The words" and "How the climb is measured." Owner is Alex Tsakiris. Do not name anyone else as owner.

## What is running

Phones talk only to https://attentioneffectproject.com. Results: https://attentioneffectproject.com/ . The clock is settle 5s, Quiet 10s, climb 300s, relax 90s. The screen word Attention means the climb. Kind Control, including Auto control, is a desk climb: phone on the desk, no observer, no practitioner. A wreck (bump, knock, or tilt) stops the sitting. The phone does not score. Q1 on the page is that wreck check, separate from the grade.

AAA is the Pixel. AAB is the iPhone 12 mini. AAC is an iPhone 17 Pro Max with too few runs for a floor. The calibration batch is version 4 desk climbs 800301 through 800341. Wrecked sittings are not in it.

## The grade, as agreed

The short Quiet sets level for that sitting only. Skip the first second. It is not the noise floor. Do not move the floor onto the short Quiet unless that change is written down on purpose.

The noise floor is the middle climb of that phone's control runs during the 300-second desk time. A climb is the highest 10-second moving average of lean, in millidegrees, away from that sitting's own level. AAA's floor is 4.0. AAB's floor is 6.7. Do not recalculate those floors for this recipe change.

Direction is the way that high point leaned. It belongs to that run only. It does not pass or fail.

Springback is the share of the climb that came back along that direction, before the 300 seconds end or during the 90-second relax. The current recipe asks for one half. The climb must also reach 2 times the floor. Recalculate can change the climb multiple and the springback share. A desk run that clears both is a false positive, shown in the Result column.

## Not done yet

Committed `ANALYSIS.md` on origin still says 3 times the floor and a springback of 1.5 times the floor in millidegrees. Local uncommitted edits in `ANALYSIS.md`, `calibration/2026-10-04.json`, `recipes/desk_climb.py`, and `results.py` switch the recipe to 2 times and a half share. Those edits are not deployed and not committed. An earlier Recalculate trial set AAA's climb target and springback target to 1.0 under the old millidegree meaning. Do not treat 1.0 as the new half-share.

Load, Hold, Recovery, and Pattern are the older October 3 recipe. They are not this grade. On cal rows, Load was replaced with the new climb.

## Next

Finish tests, deploy the results page, and commit the 2-times climb and half-share springback. Keep the existing floors. After deploy, confirm a desk run's Result is false positive only when both new targets are cleared.
