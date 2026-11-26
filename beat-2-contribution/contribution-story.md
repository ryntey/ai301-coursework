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

**Unit 6: REPRO CHECK

[2-4 sentences: the voice-guard, the repo-memory file. What broke or
fell short, and what changed because of it. "Nothing broke" counts when
it is true - say how you know.]

I used repro-check as it was on my computer. to check my reproduction on my computer. I

**Unit 7: Plan Check

[2-4 sentences: the ship-gate and your gate manifest. What it produced
on your real branch, what fought you, what changed because of it.]

I kept that plan-check tool as it was originally. 

**Unit 8: Pr-precheck

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

I chose this issue based on what my issue-scout deemed appropriate. I specifically curated my issue-scout on my scope of being a senior computer science major and needing to be eased into things as I had taken a year off, looking specifically for a Python issue as it is my strongest language. Issue scout. I think I may have ultimately trusted it a bit too much, as I don't know that it was completely aligned with all I wanted. My desire was to remain, but I the issue ended up being dead without much activity, ran pretty slowly on Mac specificlly (which I have)

https://github.com/ArduPilot/ardupilot/issues/8503#issuecomment-7168305085 

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

- Tool:
- What it produced:
- Difficulties:
- What changed:

---

## Reproduction Process

**Environment Setup**

Environment
ProductName: macOS
ProductVersion: 15.3.1
BuildVersion: 24D70
arm64
Python 3.10.18, MAVProxy 1.8.75
ArduPilot master at dc9c4e2 Sat Nov 21 09:03:57 2026 +1100, Rover SITL

I felt that the tool ran somewhat awkwardly on my computer. I had trouble deciphering what everything I was looking at meant on my own, and the project felt much bigger than me at first, so I used Claude to help me figure out all that was being said so that I could move forward. 


**Steps to Reproduce**

Steps
sim_vehicle.py -v Rover --console --map -w
At the MAVProxy prompt:
wp load rover-8503.txt
param set FS_GCS_ENABLE 1
param set FS_TIMEOUT 5
param set FS_ACTION 2
arm throttle
mode auto
After the rover passes WP 2, set heartbeat 0 to stop GCS heartbeats.
Wait ~10 s, then set heartbeat 1.
Repeat steps 2–4 with param set FS_ACTION 1 (switching back to mode auto first; the failsafe does not act while already in HOLD unless FS_OPTIONS bit 0 is set).

3. Observed
FS_ACTION=2: mode changed AUTO → HOLD.
FS_ACTION=1: mode changed AUTO → RTL and the rover drove back towards home.
In both cases the rover stopped following the mission before item 5 (DO_LAND_START).
The FS_ACTION values on this commit are 0:Nothing,1:RTL,2:Hold,3:SmartRTL or RTL,4:SmartRTL or Hold,5:Terminate,6:Loiter or Hold (Rover/Parameters.cpp). None of them continues to a mission item.


**Reproduction Evidence**

- **Commit showing reproduction:** [link to commit in your fork]
- **Screenshots/logs:** [if applicable]
- **My findings:** [what you discovered during reproduction; an
  honest, evidenced cannot-reproduce that led you to re-pick or ask
  the maintainer belongs here and costs you nothing]

**Toolkit**

- Tool:
- What it produced:
- Difficulties:
- What changed:

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

dd failsafe action `7: Auto DO_LAND_START or RTL` to Rover. On failsafe,
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
- I had 

[Record deviations here as they happen: what changed mid-build and
why. A recorded deviation is honest work; silent drift is not.]

**Toolkit**

- Tool:
- What it produced:
- Difficulties:
- What changed:

---

## Testing Strategy

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

- Tool:
- What it produced:
- Difficulties:
- What changed:

---

## Implementation Notes

[A running log, added to as you work. Date each entry. What you
built, decisions you made when you made them, wrong turns included.]

Issue-Scout: 11/19/2026

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

I asked Claude for help with the steps, and I understood and thought it had gone well and was able to give a presentation I felt confident in, but I realized when I went to create this document, it kind of took over and ran without me seeing the intricacies I needed to answer these questions. I had no idea how to move forward at first, but I eventually settled on needing to start over. 

**2026-11-24*: 


**2026-11-25: Proper reproduction, build, and fix on my own computer. 

Repro-Check
Plan-Check
Pr-Precheck

[Entry]

**Toolkit**

- Tool:
- What it produced:
- Difficulties:
- What changed:

---

## Pull Request

**PR Link:** [GitHub PR URL when opened]

**PR Description:** **PR Description:** draft in `beat-2-contribution/ardupilot-8503/pr-description.md`,
following ArduPilot's PR template (Summary / Classification & Testing /
Description). It discloses AI assistance, as ArduPilot's `AGENTS.md`
requires.

**Maintainer Feedback:**

- [11/25]: None Yet
- [11/25]: Continued working 

**Status:** Awaiting review

**Toolkit**

- Tool:
- What it produced:
- Difficulties:
- What changed: 

---

## Learnings and Reflections

**Technical Skills Gained**

I was completely unfamiliar with the idea of open-source contributions prior to this class, let alone the process. I also had never created a tool made for search that I didn't manually plug a database or file into. This semester and class have expanded my understanding of the process and I feel 10x more capable than I did when starting, even if not 100%.

**Challenges Overcome**

ArduPilot builds on Linux, and is pretty slow to set up on Mac. I also ran out of storage space on my computer right in the middle of testing. I had to stop, delete miscellaneous items, and then continue. That was really frustrating as I was already having some difficulty with the project.

**What I'd Do Differently Next Time**

For the specific issue, I may have went in a different direction. I was glad to challenge myself, but I think was far enough away for what my intention was that I believe I should've second guessed issue scout a bit more before going full steam ahead. In a general sense, I would prioritize reaching out for help and starting earlier. I got really caught up in my lack of understanding in the first few weeks and found it difficult to get back on m feet from there. During the middle of the semester, I spoke to my professor to open a line of communication about where I was struggling and figure out a path forward and I think that's where things started shifting for the better for me. I'd also use Claude to help me break down the steps from assignment one. Those two things reframed the grip I had on the material, especially with the very tools I ended up using for this contribution, and I would implement them sooner. 

---

## Resources Used

[What actually helped, not a reading list. List things this story
already shows you using: the doc you followed to set the environment
up, the thread that explained the bug, the person who answered you.
One real entry beats ten plausible ones.]

- The related open PR that deals with a similar base issue (https://github.com/ArduPilot/ardupilot/pull/35171), being that when something goes wrong/a failsafe is triggered, the mission is supposed to be resume from a particular marker as opposed to returning home. It's not the same exact thing, but it help provide context into what my specific issue was and why it was a problem in the first place.
- [ArduPilot/ardupilot#8503](https://github.com/ArduPilot/ardupilot/issues/8503):
  IamPete1's comment pointing at `DO_LAND_START` set the design.
- `AGENTS.md` in the ArduPilot repo: commit and PR rules for AI-assisted work.
- [Building ArduPilot / SITL](https://ardupilot.org/dev/docs/building-setup-linux.html)
  and the build section of `AGENTS.md` (`./waf configure --board sitl`).
- Existing code used as the model: `ArduCopter/mode_auto.cpp`
  (`jump_to_landing_sequence_auto_RTL`, `enter_auto_rtl`) and
  `ArduPlane/GCS_MAVLink_Plane.cpp` (the `set_force_resume` pattern).
