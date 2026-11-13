# Evidence guide: where evidence lives in a plan package

## Diagnosis and grounding

- Where it lives (eval): the candidate plan's "Diagnosis" (or cause)
  line; the repro evidence block's steps, artifacts, **control runs**,
  comparisons (other versions, other flags, other platforms), and its
  Expected/Actual lines; maintainer root-cause statements in the
  thread highlights.
- Where it lives (live): the draft `plan.md` diagnosis; the student's
  own posted repro comment on the issue (or, on the house issue, the
  repro pack as quoted in the drafts); maintainer comments in the
  issue thread.
- What good looks like: every control and artifact is consistent with
  the stated cause. A control run is the decisive evidence: if the
  control shows the same input working when the blamed part is still
  present, or shows the failure already happening before the blamed
  step runs, the diagnosis is contradicted, however confident or long
  the plan is. A diagnosis that just restates what the repro isolated
  is grounded.

## Scope

- Where it lives: the plan's in-scope / not-in-scope lines, its
  "Change" or "Scope" section, and every numbered approach step (scope
  creep often hides in the later steps: "also", "while we're here",
  "migrate", "rewrite", "add an option", "CI matrix").
- What good looks like: one bounded change at the site the repro
  isolates, plus the tests for it. Anything else is either absent or
  explicitly deferred. Scoping DOWN (deferring a platform the author
  cannot test, or a larger rework the maintainers may prefer) with a
  reason is good scoping, not a gap. A drive-by rewrite bundles work
  the issue never asked for next to the fix.

## Executability

- Where it lives: the plan's "Files", "Approach", and "Change"
  sections.
- What good looks like: one chosen approach and a named place in the
  code (file, function, code path, or command builder). A terse plan
  that names the file and the change is executable. Hedges like "not
  sure which layer", "somewhere", "whichever is easier", "profile
  first then decide" mean the real decision has not been made.

## Test plan

- Where it lives: the plan's "Test plan" section, read against the
  repro evidence's steps and Expected line.
- What good looks like: the repro steps or the issue's test cases
  re-run with a stated expected-after (an exit code, a printed match,
  a passing named test, a timing number, no panic), and/or a
  regression test for exactly that case. A vague plan says "run the
  suite", "CI is green", "feels fast", "nothing else broken".

## Honesty

- Where it lives: the plan's "Risk", "Unknowns", or "Open question"
  lines, and confident claims anywhere in the plan; live, a
  "Deviations" section in `plan.md` records mid-build changes.
- What good looks like: unverified things are labeled as unknowns
  with how they will be checked. False confidence asserts results for
  cases the evidence never touched.

## Comms

- Where it lives (eval): the candidate plan comment, read against the
  thread highlights (who is OWNER/MEMBER/COLLABORATOR/CONTRIBUTOR and
  what they said: isolated culprit, proposed approach, rejected
  approach, patched binary to test, open PRs) and against the
  repo-facts block's "contribution policy" line (AI policy wording).
- Where it lives (live): the draft comment, the live issue thread, and
  the repo's CONTRIBUTING / AI policy files.
- What good looks like: the comment names the maintainer's direction
  and either follows it or says why it departs; it mentions related
  open PRs instead of racing them. If the AI policy requires
  disclosing AI usage in any form or in comments, the comment says AI
  was used (tool / extent). If the policy only asks for disclosure in
  PRs, or asks comments to be in the contributor's own words with no
  disclosure ask, no disclosure line is required in the comment.
