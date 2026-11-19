# PR draft: ArduPilot/ardupilot — "Rover: add Auto DO_LAND_START or RTL failsafe action"

Paste everything below the line into the PR body. Before opening, check
every box against what you actually did, and rewrite anything you can't
explain yourself (ArduPilot's AGENTS.md: the human submitter must
understand and defend every line).

---

### Summary

Adds failsafe action 7, "Auto DO_LAND_START or RTL", to Rover (FS_ACTION, BATT_FS_LOW_ACT/BATT_FS_CRT_ACT, FENCE_ACTION). On failsafe the rover jumps to the nearest DO_LAND_START in its mission and continues in Auto, falling back to RTL and then Hold. Fixes #8503.

### Classification & Testing (check all that apply and add your own)

- [ ] Checked by a human programmer
- [ ] Non-functional change
- [ ] No-binary change
- [ ] Infrastructure change (e.g. unit tests, helper scripts)
- [x] Automated test(s) verify changes (e.g. unit test, autotest)
- [x] Tested manually, description below (e.g. SITL)
- [ ] Tested on hardware
- [ ] Logs attached
- [x] Logs available on request

New autotest `test.Rover.FailsafeDoLandStart` (RC failsafe, FS_ACTION=7):
- in Auto, the failsafe jumps to the item after DO_LAND_START and stays in Auto;
- from Manual with MIS_RESTART=1, it enters Auto at the DO_LAND_START rather than restarting at WP1;
- with no DO_LAND_START in the mission, it falls back to RTL.

The test fails on master (failsafe does nothing; Auto prints "Skipping invalid cmd #189") and passes with this PR. Also run locally in SITL, all passing: ThrottleFailsafe, GCSFailsafe, DriveMission, Rally, SmartRTL, PolyFence, MAV_CMD_DO_FENCE_ENABLE. `check_branch_conventions.py` and flake8 are clean.

### Description

As suggested in the issue, this uses the mission's DO_LAND_START marker (as Copter's FS_ACTION=6 and Plane already do) instead of a "waypoint number" parameter, so the return route lives in the mission.

- `ModeAuto::jump_to_landing_sequence()` calls `AP_Mission::jump_to_landing_sequence()` and switches to Auto. It sets `set_force_resume(true)` when Auto is about to (re)start the mission, so MIS_RESTART=1 doesn't send the rover back to the first item. If Auto is already running the DO_LAND_START sequence, it leaves it alone.
- Auto now treats `MAV_CMD_DO_LAND_START` as a no-op marker instead of an invalid command.
- The new action sits after RTL in `_failsafe_priorities`, matching Copter.
- Parameter docs updated for FS_ACTION, BATT_FS_*_ACT (Rover) and FENCE_ACTION (Rover). No parameter indices or defaults change.

Not included: handling `MAV_CMD_DO_LAND_START` sent as a command from a GCS, which could be a follow-up.

AI disclosure: this change was written with AI assistance (Claude Code), which helped trace the code, write the implementation and autotest, and run the SITL tests. I reviewed the diff and the test results before submitting.
