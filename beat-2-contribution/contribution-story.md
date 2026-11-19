# Contribution: Rover : Forward to waypoint # when failsafe triggered(opens in new tab)

Path: `beat-2-contribution/contribution-story.md`

This file is Assignment 5, and it is graded at the path above; a copy kept anywhere else in
the repository is not read. **Keep every section header below under its exact name** — the
format criteria grade exactly that. Your course repo link is the whole submission: no tool
files and no evidence uploads are part of Assignment 5.

**Contribution Number:** [1]
**Student:** Lauryn Allotey
**GitHub Username:** ryntey
**Issue:** [ArduPilot/ardupilot#8503](https://github.com/ArduPilot/ardupilot/issues/8503)
**Pull Request:** not opened yet (2026-11-19). The branch is ready: see `beat-2-contribution/ardupilot-8503/`

One repository, built across units 9 to 12, telling the whole story of your real
contribution. Fill it as you work, not at the end — the sections below ask for evidence that
is far easier to write down as it happens than to reconstruct in the last weekend.

Contribution Number stays `[1]` unless you make more than one contribution this term; it
just numbers them.

---

## Toolkit

Every story section below, from Why I Chose This Issue through Pull
Request, ends with a Toolkit block: which tool did that phase's work
and how you used it, what it produced, what difficulties or
shortcomings it presented, and what you changed or would change
because of them. Together the blocks are the thread the rubric's
Toolkit points read, and they are the raw material for your unit-9
portfolio, so keep them current as you work rather than
reconstructing them later.

A Toolkit block looks like this (copy it into each story section and
fill it there):

```text
**Toolkit**

- Tool: [which tool did this phase's work, and how you used it]
- What it produced: [described in a sentence or a short excerpt]
- Difficulties: [what fought you or fell short, or "none met"]
- What changed: [the patch, revision, or workaround, what you would
  update, or "nothing"]
```

This section also keeps one short dated entry per week of tool work,
starting below: what you built or changed that week, what broke, and
what you changed because of it. The blocks tell the story where it
happened; these entries are the toolkit's own timeline.

All four entries are graded. Keep every heading below, and fill each
one in the week it belongs to.

**Unit 9: Turned my issue selector into a scout with specifically Python search queries and ranking filters based on recent activity within the comments of the repo**

I installed the issue-scout skill, expanded my fit profile to name Python, small bug/docs/test work, and beginner-friendly projects, and wrote three `gh search issues` queries plus ranking filters that drop assigned or stale issues and sort by fewest comments. I did all of this based on my original fit profile, which covered my desire to find an issue that helps me brush up my Python skills after a year off. My first scan graded five issues; I checked them against the live pages and they held up for the most part, but when running with claude there was the suggestion of a few changes to clear up confusion. I looked over and approved this, and changes were logged in scan-notes.md.

**Unit 6: [one line on the tool work]**

[2-4 sentences: the voice-guard, the repo-memory file. What broke or
fell short, and what changed because of it. "Nothing broke" counts when
it is true - say how you know.]

**Unit 7: [one line on the tool work]**

[2-4 sentences: the ship-gate and your gate manifest. What it produced
on your real branch, what fought you, what changed because of it.]

**Unit 8: [one line on the tool work]**

[2-4 sentences: the PR-monitor and your playbook. What it read, what it
drafted, what you did with the draft.]

---

## Why I Chose This Issue

[1-2 paragraphs in your own words: how you searched, what your scan
surfaced, what your scout's verdict said, why this issue fits your
profile, and what you hope to learn. If your scout's verdict on it
was reject and you chose it anyway, say so here and say why; choosing
against your own tool silently is what loses points. Close with an
honest read of the tool itself: where the scout fell short on a real
wild search and what you changed or would change, or what convinced
you it held up. **Record the link to your claim comment in this section too.**
Not claimed yet? Write `claim pending` and today's date where the link goes,
and add the link the moment your claim lands.]

I chose this issue based on what my issue-scout deemed appropriate. 

**Claim comment:** claim pending, 2026-11-19

**Toolkit**

- Tool: issue-scout (your scout: your search strategy, your rubric),
  and how you used it
- What it produced: [the scan and its verdict on this issue, described]
- Difficulties: [where it fell short, or "none met" if none did]
- What changed: [what you patched or would update, or "nothing"]

---

## Understanding the Issue

**Problem Description**

[In your own words, what is broken or missing?]

**Expected Behavior**

[What should happen?]

**Current Behavior**

[What actually happens?]

**Affected Components**

[Which parts of the codebase are involved?]

**Toolkit**

- Tool: Claude Code (cloud session), used to read the issue thread and
  then search the Rover, Copter, Plane and AP_Mission source for how
  failsafe actions and `DO_LAND_START` work.
- What it produced: a map of the relevant code. Rover's `FailsafeAction`
  enum (`Rover/Rover.h`) has no "go to a mission item" action. Copter
  already has `AUTO_DO_LAND_START` (`ArduCopter/mode_auto.cpp`,
  `jump_to_landing_sequence_auto_RTL`). `AP_Mission::jump_to_landing_sequence()`
  is shared library code Rover can call.
- Difficulties: the issue is from 2018 and Rover 3.3. The only pointer to
  a design was IamPete1's 2021 comment ("This is the `DO_LAND_START`
  functionality that we now have on plane and copter"), so the current
  code had to be checked rather than trusting the thread.
- What changed: the approach followed the maintainer's comment: reuse
  `DO_LAND_START` instead of adding a new "waypoint number" parameter.

---

## Reproduction Process

**Environment Setup**

- OS: Ubuntu 24.04.5 LTS (cloud container), 4 cores, 15 GB RAM
- Compiler: gcc/g++ 13.3.0; Python 3.13
- Project: ArduPilot `master` at commit `5cdd616` (19 Nov 2026), ArduRover
  V4.8.0-dev, built for SITL (software-in-the-loop, no hardware needed)
- Setup:
  1. `git clone --depth 1 https://github.com/ArduPilot/ardupilot`
  2. `git submodule update --init --depth 1 modules/waf modules/mavlink modules/DroneCAN/* modules/littlefs modules/lwip modules/Micro-CDR modules/Micro-XRCE-DDS-Client`
  3. `pip install future lxml pexpect pymavlink MAVProxy dronecan`, plus `empy==3.3.4`
  4. `./waf configure --board sitl && ./waf rover`

Challenges:
- `pip install empy==3.3.4` (the version ArduPilot needs) failed to build
  a wheel on Python 3.13. Workaround: download the source tarball and copy
  its single `em.py` into site-packages.
- The first `./waf rover` failed in `AP_Networking` because the `lwip`
  submodule wasn't checked out. Fetching `lwip` (and the Micro-XRCE-DDS
  modules) fixed it; the next build took about 1.5 minutes.

**Steps to Reproduce**

1. Upload a Rover mission: WP1 40 m north, WP2 80 m north, WP3 120 m
   north, then `DO_LAND_START`, then WP5 (30 m east of home) and WP6
   (home). Items 4-6 are the "go home this way" route.
2. Set `FS_THR_ENABLE=1` and `FS_ACTION=7` (the value the fix adds; on
   master no `FS_ACTION` value means "go to a mission item").
3. Arm, switch to AUTO, wait until the rover is heading to WP2, then cut
   the RC signal (`SIM_RC_FAIL=1`).
4. Observed on unmodified master: "Radio Failsafe" is reported but
   nothing happens. The rover keeps driving WP2 then WP3, and when it
   reaches the `DO_LAND_START` item it prints `Skipping invalid cmd #189`
   (189 is `DO_LAND_START`). Rover can neither jump to a mission point on
   failsafe nor even recognise the marker command.

All of this is automated as the autotest `test.Rover.FailsafeDoLandStart`:
`./Tools/autotest/autotest.py --no-clean --speedup=10 test.Rover.FailsafeDoLandStart`

**Reproduction Evidence**

- **Commit showing reproduction:** the test-only commit `99e0748`
  ("autotest: add Rover FailsafeDoLandStart test"), saved as
  `beat-2-contribution/ardupilot-8503/patches/0001-autotest-add-Rover-FailsafeDoLandStart-test.patch`.
  Fork link pending: the fork does not exist yet.
- **Screenshots/logs:** `beat-2-contribution/ardupilot-8503/logs/before-fix-excerpt.log`.
  Key lines:
  ```
  AT-0006.5: AP: Radio Failsafe
  AT-0007.0: AP: Reached waypoint #2
  AT-0007.9: AP: Reached waypoint #3
  AT-0007.9: AP: Skipping invalid cmd #189
  FAILED: "FailsafeDoLandStart ...": Failed to receive text: landing sequence start
  ```
- **My findings:** the issue reproduces on current master, eight years
  after it was filed. An `FS_ACTION` value the enum doesn't know falls
  through the `switch` in `Rover::failsafe_trigger()` with no case, so the
  failsafe silently does nothing. Separately, Rover's Auto mode doesn't
  list `MAV_CMD_DO_LAND_START` at all, which is the "Skipping invalid
  cmd" message.

**Toolkit**

- Tool: Claude Code, which ran the build and wrote the SITL autotest
  that reproduces the issue, then ran it on unmodified master.
- What it produced: a failing autotest and the log excerpt above.
- Difficulties: the `empy` build failure on Python 3.13 and the missing
  `lwip` submodule (both above). The repro-check skill from unit 2 wasn't
  used. Its rubric targets small Python repos, not a C++ flight stack
  where reproducing means running a simulator.
- What changed: the test was written before the fix, so the same test
  serves as both the reproduction and the regression check.

---

## Solution Approach

**Analysis**

- `Rover/Rover.h`: `enum class FailsafeAction` only has None, RTL,
  Hold, SmartRTL, SmartRTL_Hold, Terminate and Loiter_Hold. None of them
  continues the mission from a chosen point.
- `Rover/failsafe.cpp`: `Rover::failsafe_trigger()` (RC/GCS failsafe) and
  `Rover::handle_battery_failsafe()` switch on that enum, and so does
  `Rover::fence_check()` in `Rover/fence.cpp`.
- `Rover/mode_auto.cpp`: `ModeAuto::start_command()` and
  `verify_command()` have no case for `MAV_CMD_DO_LAND_START`, so a mission
  holding one prints "Skipping invalid cmd".
- The building blocks already exist in shared code:
  `AP_Mission::jump_to_landing_sequence(loc)` (in
  `libraries/AP_Mission/AP_Mission.cpp`) finds the nearest
  `DO_LAND_START` and makes it the current item. Copter and Plane already
  use it from their failsafes.

**Proposed Solution**

Add failsafe action `7: Auto DO_LAND_START or RTL` to Rover. On failsafe,
jump to the nearest `DO_LAND_START` in the mission and continue in Auto.
If the mission has none, fall back to RTL, then Hold. The user marks the
waypoint the issue asks for with `DO_LAND_START` in the mission, which is
what IamPete1 suggested on the issue. This matches Copter's
`FS_ACTION=6` and Plane's `DO_LAND_START` handling.

Deliberately NOT changed:
- no new parameter holding a waypoint number (the mission marker does
  this job and can't go stale when the mission is edited);
- no new flight mode and no "Auto RTL" pseudo-mode like Copter has;
- no handling of the `MAV_CMD_DO_LAND_START` command sent from a ground
  station, which would be a separate follow-up;
- no change to `Loiter_Hold` being missing from the battery failsafe
  priority list (an existing gap, out of scope).

**Implementation Plan**

1. `Rover/Rover.h`: add `Auto_DoLandStart_RTL = 7` to `FailsafeAction`
   and to `_failsafe_priorities` (after RTL, the same place as Copter).
2. `Rover/mode.h`, `Rover/mode_auto.cpp`: add
   `ModeAuto::jump_to_landing_sequence(ModeReason)` (jump, then switch to
   Auto) and accept `DO_LAND_START` as a no-op marker command.
3. `Rover/failsafe.cpp`, `Rover/fence.cpp`: handle the new action in all
   three switches: try the jump, else RTL, else Hold.
4. Parameter docs: `FS_ACTION` (`Rover/Parameters.cpp`), `BATT_FS_LOW_ACT`
   and `BATT_FS_CRT_ACT` (`AP_BattMonitor_Params.cpp`), `FENCE_ACTION`
   (`AC_Fence.cpp`).
5. `Tools/autotest/rover.py`: the `FailsafeDoLandStart` test.

Deviations recorded during the build:
- `Rover/Rover.cpp` also had to change: it defines
  `_failsafe_priorities[7]` with a hard-coded size, so adding an entry was
  a compile error ("conflicting declaration"). Changed it to `[8]`.
- First version of the jump set `mission.set_force_resume(true)` only when
  the mission "was not running". The new third subtest (failsafe from
  Manual with `MIS_RESTART=1`) failed: the rover jumped to item 4 and then
  went back to WP1. Cause: `AP_Mission::jump_to_landing_sequence()` itself
  calls `resume()`, so the mission is always running by the time of the
  check, and entering Auto then restarted the mission from the beginning.
  Fixed by setting the flag whenever Auto is about to start the mission
  (not in Auto yet, or Auto still waiting to start).
- Split into four commits, one per subsystem, because ArduPilot CI
  rejects commits that touch more than one subsystem.

**Toolkit**

- Tool: Claude Code, which read the Copter/Plane implementations, made
  the edits, and rebuilt after each change. The plan-check skill from
  unit 3 wasn't run on this plan.
- What it produced: about 70 lines of C++ in `Rover/` plus three one-line
  parameter-doc changes.
- Difficulties: the first jump logic was wrong in a way that only showed
  when starting from a non-Auto mode. A test caught it; reading the code
  didn't.
- What changed: added the non-Auto/`MIS_RESTART=1` subtest permanently, so
  that path stays covered.

---

## Testing Strategy

1. **Before/after on the reproduction.** `test.Rover.FailsafeDoLandStart`
   fails on unmodified master (log above) and passes with the fix. After
   the fix the log shows, right after `Radio Failsafe`:
   ```
   AP: Mission: 4 LandStart
   AP: Mission: 5 WP
   AP: Landing sequence start
   AP: Reached waypoint #5
   ```
   (`logs/after-fix-excerpt.log`)
2. **Three behaviours, each asserted:** (a) in Auto, the failsafe jumps to
   WP5 and stays in AUTO; (b) from Manual with `MIS_RESTART=1`, the rover
   switches to AUTO and starts at WP5, not WP1; (c) with no
   `DO_LAND_START`, it prints `Unable to start landing sequence` and
   switches to RTL.
3. **Regression, the repo's own autotests:** ThrottleFailsafe,
   GCSFailsafe, DriveMission, Rally, SmartRTL, PolyFence and
   MAV_CMD_DO_FENCE_ENABLE all PASSED alongside the new test (8/8,
   `logs/regression-summary.log`). These cover the failsafe, mission and
   fence code paths the change touches.
4. **Repo checks:** `./waf rover` builds clean (no warnings);
   `flake8 Tools/autotest/rover.py` reports nothing;
   `param_parse.py --vehicle Rover` accepts the new `@Values`;
   `Tools/scripts/check_branch_conventions.py` reports "All checks passed".
5. **Not done:** no hardware test, and no full `test.Rover` suite run
   (only the related tests). ArduPilot's CI on the fork/PR runs the full
   suite.

**Toolkit**

- Tool: ArduPilot's own autotest framework (`Tools/autotest/autotest.py`),
  driven by Claude Code.
- What it produced: before/after logs and an 8/8 pass on the regression set.
- Difficulties: the first regression run listed a test named `Fence`,
  which doesn't exist. autotest stopped at it ("Failed to find tests
  {'Fence'}"), so later tests never ran. Rerun with the real names
  (PolyFence, MAV_CMD_DO_FENCE_ENABLE).
- What changed: check test names with `grep "def .*(self)" rover.py`
  before listing them.

---

## Implementation Notes

[A running log, added to as you work. Date each entry. What you
built, decisions you made when you made them, wrong turns included.]

Issue-Scout: 11/19/2026

[Entry]

**2026-11-19: reproduce, build, fix (Claude Code session)**

- Cloned ArduPilot master (`5cdd616`). Confirmed the issue is still open,
  unassigned, labelled `good first issue`, with no linked PRs, and that no
  open PR covers it. The closest, #35171, changes Copter/Plane fence
  actions only.
- Read `AGENTS.md`, ArduPilot's rules for AI-assisted contributions:
  disclose AI use in the PR, the human submitter must understand and
  defend every line, one subsystem per commit.
- Got SITL building (empy and lwip problems, see Environment Setup).
- Wrote the autotest first; it failed on master (reproduction).
- Implemented action 7. The first build failed on the hard-coded
  `_failsafe_priorities[7]` size in `Rover.cpp`.
- Added a third subtest for the non-Auto path, which caught the
  force-resume bug; fixed it (see Deviations).
- 8/8 related autotests pass. Committed as four subsystem commits on
  branch `rover-fs-do-land-start`. Saved them as patches in
  `beat-2-contribution/ardupilot-8503/patches/` because no
  `ryntey/ardupilot` fork exists yet to push to.
- Next: fork, `git am` the patches, push, check CI in the fork's Actions
  tab, review the diff line by line, post the claim comment, open the PR.

**Toolkit**

- Tool: Claude Code (cloud session) for the whole build day.
- What it produced: the four commits, the logs, and the PR draft.
- Difficulties: the container is temporary and had no access to a fork,
  so the work had to be saved as patch files to survive.
- What changed: patches and log excerpts now live in the course repo.

---

## Pull Request

**PR Link:** not opened yet (2026-11-19)

**PR Description:** draft in `beat-2-contribution/ardupilot-8503/pr-description.md`,
following ArduPilot's PR template (Summary / Classification & Testing /
Description). It discloses AI assistance, as ArduPilot's `AGENTS.md`
requires.

**Maintainer Feedback:**

- [Date]: [feedback received, or "none yet"; silence is a normal
  state and costs you nothing]
- [Date]: [how you addressed it]

**Status:** Not yet opened

**Toolkit**

- Tool: Claude Code drafted the PR description and ran
  `check_branch_conventions.py`; the pr-precheck skill from unit 4 has
  not been run on it yet.
- What it produced: a template-shaped PR description and four
  convention-checked commits.
- Difficulties: ArduPilot's `AGENTS.md` makes the human submitter
  responsible for every line, so the PR can't be opened straight from the
  session without me reviewing it first.
- What changed: [fill in after opening]

---

## Learnings and Reflections

**Technical Skills Gained**

[What you learned technically, named concretely.]

**Challenges Overcome**

[What was hard and how you solved it; point at the real events, the
Toolkit blocks above are full of them.]

**What I'd Do Differently Next Time**

[Reflection on your process.]

---

## Resources Used

[What actually helped, not a reading list. List things this story
already shows you using: the doc you followed to set the environment
up, the thread that explained the bug, the person who answered you.
One real entry beats ten plausible ones.]

- [ArduPilot/ardupilot#8503](https://github.com/ArduPilot/ardupilot/issues/8503):
  IamPete1's comment pointing at `DO_LAND_START` set the design.
- `AGENTS.md` in the ArduPilot repo: commit and PR rules for AI-assisted work.
- [Building ArduPilot / SITL](https://ardupilot.org/dev/docs/building-setup-linux.html)
  and the build section of `AGENTS.md` (`./waf configure --board sitl`).
- Existing code used as the model: `ArduCopter/mode_auto.cpp`
  (`jump_to_landing_sequence_auto_RTL`, `enter_auto_rtl`) and
  `ArduPlane/GCS_MAVLink_Plane.cpp` (the `set_force_resume` pattern).
