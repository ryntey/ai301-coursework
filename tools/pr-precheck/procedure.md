# Procedure: how this tool grades a PR package

## Read order

1. Read the plan first (eval: the "Plan context" block; live:
   `plan.md`). Write down: the stated cause, every in-scope item, every
   file in the Files list, every not-in-scope item, every deliverable
   (code change, test, docs, changelog), the test plan's failing cases
   and expected-after results, and any deviation or deferral note. The
   plan goes first because the diff, the description, and the evidence
   are all graded against it; reading the description first anchors on
   the author's own summary.
2. Read the issue and repro evidence. Write down the failing input and
   any control the repro used.
3. Read the repo facts (eval: "Repo facts" block; live: the PR
   template, CONTRIBUTING.md, AI policy). Write down each concrete PR
   ask as a numbered list, and the AI policy's requirement for pull
   requests (or "none stated").
4. Read the thread highlights. Write down any maintainer direction or
   open question about the approach.
5. Read the diff and the commit list. Write down every changed file
   and, per file, a one-line summary of each hunk.
6. Read the description and title last. Write down each factual claim
   about what the PR changes.
7. Read the test evidence. Write down each command run, whether it ran
   on the before code, the after code, or both, and its output.

## Evidence gathering

- diff-within-plan: pair the changed-file list (step 5) with the plan's
  Files list and deliverables (step 1). Mark each hunk "planned",
  "deviation note", "required by repo ask", or "unplanned". Mark each
  plan deliverable "in diff", "deferred in a note", or "missing".
- description-matches-diff: pair each description claim (step 6) with
  the hunk that delivers it, or mark it "not in diff".
- repro-rerun-before-after: pair each failing case in the test plan
  (step 1) with a command in the evidence (step 7). For each, record
  whether it is the failing input (not the control), whether a before
  output and an after output are both shown, and whether the after
  matches the plan's expected-after.
- repo-checks-shown: from step 7, record the repo test or check
  commands run and their stated outcomes.
- diff-clean: scan every `+` and `-` line from step 5 for the debris
  tells in the evidence guide's Diff quality section. Record each one
  found with its file and line text.
- template-asks-met: for each numbered ask from step 3, record where
  it is met (description sentence or diff file) or "absent".
- ai-disclosure: record the policy requirement from step 3 and quote
  any disclosure sentence from the description, or record "none".
- thread-direction-engaged: for each direction from step 4, quote the
  description sentence that engages it, or record "not engaged".

## Check execution

Run the checks in the rubric's table order. Grade each one only from
the evidence recorded above, applying the pass condition literally.
Each check's evidence line quotes or names the single fact that
decided it (a file, a missing ask, a quoted line). Grade `unclear`
only when the evidence the check needs is not in the package at all;
if it is present and shows no violation, grade `pass`. Grade every
check even after a required check fails: eval mode always runs the
full rubric. Do not re-read the package to second-guess a grade the
recorded evidence already decides.

## Verdict assembly

Apply the rubric's verdict rule: accept only if every required check
is `pass`; any required `fail` or `unclear` gives reject. Preferred
checks never change the verdict. In the readable summary, name the
deciding check: the first failing (or unclear) required check in
rubric table order, with its evidence. For an accept, say that every
required check passed. Then emit the JSON block exactly as SKILL.md
specifies, with nothing after it.
