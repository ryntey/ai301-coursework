---
name: pr-precheck
description: Grade a PR package (a candidate pull request read against the plan it claims to implement and the issue that plan belongs to) and decide whether it is ready to submit. Use when checking your own branch, draft PR title, and description before opening the pull request, or when grading an eval package bundle.
---

# pr-precheck: rubric-driven PR grading

## The question

Answer one question about one PR package: is this pull request ready
to submit? A PR package is a candidate pull request (its title,
description, commits, diff, and test evidence) read against three
things: the plan it claims to implement (scope, files, test plan, and
deviation notes), the issue and repro that plan belongs to, and the
target repo's stated standards (PR template, contributing guide, AI
policy, maintainer direction in the thread). Do not grade the plan,
the issue, or the code's general quality, and do not grade more than
one package per run.

## Inputs and modes

Live mode (checking the student's own PR before it is opened). Read:

- The plan: `beat-1-sandbox/unit-3/plan.md` in the student's course
  repo, including its Deviations section. A house-chain student reads
  the house plan and house repro pack instead, with the same checks.
- The diff: run `git diff main...HEAD` (three dots) from the working
  copy of the fix branch, and `git log --oneline main..HEAD` for the
  commits.
- The draft PR title and description, as the student provides them.
- The test evidence: the before/after output the student captured,
  and the output of the repo's own checks.
- The issue: the issue thread on GitHub, plus the repo's PR template
  (`.github/pull_request_template.md` or `.github/PULL_REQUEST_TEMPLATE/`),
  `CONTRIBUTING.md`, and any AI policy file.

If any of these is missing, say which one and grade the checks that
depend on it `unclear`.

Eval mode (grading a package bundle). The bundle is the whole world:
take every fact from the bundle text, fetch nothing, and read no other
file. Ignore `scope.md` and `voice-guide.md`. Always grade the complete
package: every check in the rubric and the full verdict rule.

## The scope seam (live mode only)

Before anything else in live mode, read `scope.md`. If its `Repo:`
line still holds the placeholder `<ORG>/<PATH-REVIEW-REPO>`, stop
without grading and tell the student to fill the `Repo:` line with
their section's Path Review repo. If the PR's target repo is not the
repo named there, refuse to grade and say why. Apply the scope's house
rules as facts about the repo: the PR comes from the student's own
fork on a `fix/<issue-number>-<slug>` branch, one PR per issue, and
the repo's PR template is always used. Never guess a scope. Eval mode
ignores `scope.md` entirely.

## The voice seam (live mode only)

In live mode, after grading, read `voice-guide.md` and hold the draft
PR title and description against each of its rules. In the readable
summary, list each rule the draft breaks, quoting the offending line
and the rule. The voice guide never changes the verdict on its own,
because no rubric check reads it; it is advice for the student's own
writing. Eval mode ignores `voice-guide.md` entirely.

## Component reads

- `rubric.md` defines the checks and the verdict rule. Grade exactly
  those checks; never add, drop, or rename one.
- `references/evidence-guide.md` says where each evidence family lives
  in a bundle and in live mode, and what good looks like.
- `procedure.md` gives the read order, the evidence gathering, how
  checks run, and how the verdict is assembled. Execute it as written.
  If it is silent on a step you need, report the gap in the summary
  and grade from the rubric's pass condition; do not invent a step.

If `rubric.md` has no checks or no verdict rule, or `procedure.md` has
no steps, refuse to grade and say which component is empty.

## Verdict and output

The verdict is binary: `accept` means ready to submit, `reject` means
hold. There is no third verdict and no score; reservations go in a
check's evidence line. Write a short readable summary (each check, its
grade, and the deciding fact; then the verdict and the check it turned
on), then end the reply with this fenced JSON block, valid and last,
with nothing after it. In eval mode, `item` is the bundle id; in live
mode it is the PR URL, or the branch name before the PR exists.

```json
{
  "item": "<PR URL or bundle id>",
  "checks": [
    {"name": "<check name>", "grade": "pass|fail|unclear",
     "evidence": "<one line: the fact or quote that decided it>"}
  ],
  "verdict": "accept|reject"
}
```

## Grading discipline

- Evidence first: grade no check without naming the fact or quote
  that decided it. "Looks fine" is not evidence.
- Grade the thing, not the polish: read the diff, the evidence, and
  the standards themselves. A terse PR can be ready; a confident,
  well-formatted description can be hiding drift, so check every claim
  against the diff.
- The rubric decides: if a check passes by its stated condition but
  feels wrong, it still passes. Report the feeling in the summary;
  never override the rubric.
- The procedure decides how: follow it in order and report its gaps
  instead of improvising.
- Unclear: follow the rubric's verdict rule, which treats `unclear` on
  a required check as a fail. Where the rubric is silent, an
  unverifiable claim fails.
