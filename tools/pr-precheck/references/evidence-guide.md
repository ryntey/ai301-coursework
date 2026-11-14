# Evidence guide: where evidence lives in a PR package

## Plan fidelity (harness category: silent-drift)

Where it lives:

- Eval bundle: the "Plan context" block holds the plan's scope
  ("Scope, one bounded change: ..."), the not-in-scope list ("Not in
  scope: ..."), the Files list ("Files: ..."), the test plan, and any
  deviation or deferral note. The candidate PR's "Diff" section shows
  what changed (each `+++ b/<path>` line is one changed file), and the
  "Description" section shows what the author claims changed.
- Live mode: `plan.md` in the student's course repo (its Scope, Files /
  approach, and Deviations sections), the branch diff from
  `git diff main...HEAD` in the working copy, and the draft PR title
  and description.

What good looks like: list the changed files from the diff and the
files and deliverables from the plan, then match them both ways.
Every changed file and hunk maps to a plan item or a recorded
deviation note, and every plan deliverable maps to a hunk or a
recorded deferral. A deviation counts only when it is written down in
the plan or the description; an unexplained extra (a new flag, a
renaming pass, a "while I was here" rewrite) is drift in the
more-than-plan direction, and a promised deliverable that is missing
is drift in the less-than-plan direction. A description that says
"exactly as planned", "no other changes", or "docs updated" is a claim
to check against the diff, not evidence on its own.

## Test evidence (harness category: not-tested)

Where it lives:

- Eval bundle: the candidate PR's "Test evidence" section (and any
  before/after blocks in the Description), read against the plan
  context's "Test plan" and its "Repro evidence" paragraph, which says
  which input breaks and which control does not.
- Live mode: the test output the student captured on main and on the
  branch (for this course, the before/after commands in
  `beat-1-sandbox/unit-3/plan-and-implement.md` re-run for the PR),
  plus the output of the repo's own checks named in CONTRIBUTING.md
  (Path Review: `make check && make test-unit`).

What good looks like: the same failing input the repro used, run on
main and on the branch, with output for both, and the after output
matching the expected-after the plan stated. If the test plan names
two failing cases, both appear. A control run is welcome as an extra
but never replaces the failing case. The repo's own test or check
command is named with its outcome visible ("2841 passed", "passes",
an exit code). Red flags: "tested locally", "works on my machine",
"looks right now", only a single-file or happy-path control, a test
that exercises a path the diff does not touch.

## Diff quality (harness category: unreviewable)

Where it lives:

- Eval bundle: the candidate PR's "Diff" section (every `+` and `-`
  line) and "Commits" section.
- Live mode: `git diff main...HEAD` and `git log --oneline main..HEAD`
  on the branch.

What good looks like: every `+`/`-` line is part of the fix, its
tests, or a comment explaining the fix. Debris tells, any one of which
fails the diff: debug output outside tests, commented-out code (a
line starting with a comment marker that contains code), a function
nothing calls or one marked to silence unused warnings, a new TODO or
FIXME, re-indented or reformatted lines with no logic change, import
reshuffles, identical lines removed and re-added. A `print` call inside
a test file that is part of the test's setup is not debris.

## Standards and comms (harness category: standards-wall)

Where it lives:

- Eval bundle: the "Repo facts" block. Its "pull requests" line states
  the PR template's sections, checklist, and required lines; its
  "contribution policy" line states the contributing guide's asks and
  any AI-use policy. The "Thread highlights" block holds maintainer
  direction. Compliance shows up in the candidate PR's Description
  (sections, issue-link line, checklist answers, disclosure) and Diff
  (required changelog or whatsnew files).
- Live mode: the Path Review repo's `.github/pull_request_template.md`
  (or `.github/PULL_REQUEST_TEMPLATE/`), `CONTRIBUTING.md`, and any AI
  policy file, plus the issue thread on GitHub.

What good looks like: write down each concrete ask (a section name, a
"closes #N" line, each checklist item, a changelog file, a disclosure
requirement) and find each one in the description or the diff. A
section can be labelled in prose ("Requirements: ...") rather than as
a heading and still count. When the policy requires AI disclosure, the
description says AI was used and gives whatever details the policy
names (tool, extent, own words). Missing a required item, leaving the
checklist out entirely, or no disclosure under a policy that demands
one is a wall, however good the code is.
