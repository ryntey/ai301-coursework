# Unit 4 — Test and Submit

Path: `beat-1-sandbox/unit-4/pull-request.md`

Record of the pull request you opened against the Path Review repo, and of the evaluation
runs that produced `eval-run.txt`. This file is graded at the path above; a copy kept
anywhere else in the repository is not read.

Complete every labelled field below. Each is graded on its own; content placed under the wrong
label is not graded.

---

## Your pull request

**Pull request**

https://github.com/codepath/pathreview-ai301-fa26-howard/pull/97

**Branch**

fix/75-env-example-openrouter-key

**My pr-precheck verdict on my draft**

I ran pr-precheck in live mode on my branch and draft description before
opening the PR. Verdict: **accept**. Every required check passed:

- diff-within-plan: pass. Only `.env.example` changed, and its lines
  match the plan's Files / approach block. The `DATABASE_URL` port issue
  is left untouched, as the plan's not-in-scope list says.
- description-matches-diff: pass. All three Changes bullets map to diff
  hunks.
- repro-rerun-before-after: pass. `grep -c OPENROUTER` goes from 0 to 4
  (the 3-vs-4 difference is recorded as a deviation), and `Settings()`
  `openrouter_api_key` goes from `''` to `'sk-or-your-key-here'`, with
  `llm_provider` still `'mock'`. Both runs are shown on main and on the
  branch.
- repo-checks-shown: pass. `make check` (ruff, black and mypy all pass);
  `make test-unit`: "375 passed, 53 xfailed, 5 warnings in 7.14s".
- diff-clean: pass.
- template-asks-met: pass.
- ai-disclosure: pass.
- thread-direction-engaged (preferred): pass.

```json
{
  "item": "fix/75-env-example-openrouter-key",
  "checks": [
    {"name": "diff-within-plan", "grade": "pass", "evidence": "Only .env.example changed; its 5 added/edited lines match the plan's Files/approach block line for line; DATABASE_URL port left untouched as the plan's not-in-scope list says."},
    {"name": "description-matches-diff", "grade": "pass", "evidence": "All three Changes bullets map to diff hunks; 'The diff itself is exactly the planned change' is true."},
    {"name": "repro-rerun-before-after", "grade": "pass", "evidence": "grep -c OPENROUTER 0 -> 4 (3-vs-4 recorded as a deviation) and Settings() openrouter_api_key '' -> 'sk-or-your-key-here' with llm_provider 'mock', both shown on main and branch."},
    {"name": "repo-checks-shown", "grade": "pass", "evidence": "make check: ruff/black/mypy pass; make test-unit: '375 passed, 53 xfailed, 5 warnings in 7.14s'."},
    {"name": "diff-clean", "grade": "pass", "evidence": "No debug output, TODOs, or reformatting; the two commented OPENROUTER_* lines are planned optional-setting template lines, not dead code."},
    {"name": "template-asks-met", "grade": "pass", "evidence": "Summary/Issue ('Closes #75')/Changes/Testing/Screenshots/Notes all filled; checklist items answered or N/A with reasons, except the CI item, which can only be met once the PR is open."},
    {"name": "ai-disclosure", "grade": "pass", "evidence": "No AI policy in repo; draft still discloses 'I used Claude (Claude Code) to help review the repo, draft this description, and check it against my plan.'"},
    {"name": "thread-direction-engaged", "grade": "pass", "evidence": "No maintainer direction in #75 beyond 'Make the two files agree'; the open question on the 'openrouter' value is raised in Notes for Reviewers."}
  ],
  "verdict": "accept"
}
```

What I changed because of it: the tool flagged that the CI checkbox was
unticked with no note. I added a note about CI once the PR was open. It
also caught that my local `plan.md` test plan said "expected after 4",
while the plan I posted on #75 said 3. I set it back to 3 so the
Deviations section ("I predicted 3, it returned 4") is true.

Earlier, my own `diff-clean` check led me to fix the branch before
opening the PR. My original commit had deleted the blank line before
`# App settings` in `.env.example`, which is formatting churn the plan
never mentions. I restored the blank line and amended the commit, so the
final diff is 5 lines added and 1 line changed, all of them planned.

**Reflection**

What the tool caught that I would have missed: two things I would not
have noticed by reading my own description. First, a whitespace-only
deletion in my diff. Second, a disagreement between my local plan.md and
the plan I actually posted. Neither would have shown up in my before and
after output, because both were about whether the PR matches the plan,
not about whether the fix works.

What I learned about "ready to submit": a correct fix is not enough. In
the eval set, several packages had the right code and still had to be
held: pkg-01 for an ignored checklist, pkg-20 for a missing AI
disclosure, and pkg-12 for debris. Others were held for evidence that
re-ran the wrong path (pkg-07, pkg-14). And honest limits are not
failures: pkg-13 and pkg-16 are accepts because their deferred cases are
written down.

What I would do differently: run the precheck earlier, right after
building the branch rather than right before opening the PR, and keep
plan.md frozen as posted, putting every later correction in Deviations.

## Eval iterations

Answer all four sections. Quote source text directly; paraphrase does not satisfy these
fields.

**Run history**

1. My first attempt was refused by the harness before grading anything,
   so it has no score. The four files in
   `~/.claude/skills/pr-precheck/` were still the blank templates,
   because I had edited the starter repo's `skill/` copy instead of the
   installed copy. I copied my files to the installed folder and ran
   again.
2. Run 1 (full, 20 packages, not saved): **20/20**. Categories:
   clear-accept 7/7, not-tested 4/4, silent-drift 4/4, standards-wall
   2/2, unreviewable 3/3. PASS.
3. Run 2 (full, confirming run with `--save-run eval-run.txt`, same
   tool files): **20/20**, with the same category tallies. PASS. This is
   the run in `eval-run.txt`.

I did no `--only` re-runs, because no package disagreed in either run.

**Package analysis**

pkg-13 (neovim/neovim#41337). My rubric decided **accept**. The gold
label is **accept**, so they agree.

The PR caret-escapes cmd metacharacters (`& ^ | < >`) so that
`vim.ui.open` on Windows stops truncating URLs at `&`. It deliberately
does not handle `%VAR%` expansion. That makes it the kind of package a
rubric could wrongly reject for doing "less than everything".

My rubric reads it as ready because the shortfall is recorded, not
silent:

- diff-within-plan passes. Its condition allows a deliverable to be
  "explicitly deferred in a recorded note", and the plan's deferral note
  covers `%VAR%`. The description restates it: "percent-sign environment
  expansion (`%VAR%`) is NOT handled here".
- description-matches-diff passes, because the description never claims
  the `%VAR%` case is fixed.
- repro-rerun-before-after passes. The evidence re-runs the failing URL
  (`?q=hello&t=brave`), showing before `code = 1` with the browser at
  `?q=hello`, and after `code = 0` with the full URL, plus an unchanged
  no-ampersand control.
- repo-checks-shown passes on
  `TEST_FILE=test/functional/lua/ui_spec.lua make functionaltest`
  passing.
- template-asks-met passes because the Problem and Solution sections
  neovim's template asks for are both filled.
- ai-disclosure passes because neovim states no AI policy.

What decides it is the last paragraph of my verdict rule: "A case the
plan defers in a recorded note, and that the description does not claim
to have fixed, passes every check above; 'less than everything' is only
drift when it is silent." Compare pkg-17, where the description claims
docs were updated and the diff has none. That one is silent drift, and
my rubric rejects it.

**Check rationale**

| repro-rerun-before-after | The test-evidence section, read against the plan's test plan and the repro evidence's failing case. | Pass when the evidence re-runs the plan's failing repro case (the same input and path that broke, not a control and not a different path) and shows an observable before result and an observable after result, with the after matching the plan's stated expected-after; and every distinct failing case the plan's test plan names is re-run, unless the plan or description records it as deferred. Fail when the evidence is subjective ("works now", "verified on my machine", "colors look right"), runs only a control or a path the fix does not change, re-runs only some of the failing cases the test plan names, or shows only an after with no before. A passing regression test does not replace the repro re-run. | required |

Why it reads this way: the simple version, "the PR shows test evidence",
would pass almost every not-tested package, because they nearly all
contain some evidence.

- pkg-07 shows `go test ./pkg/goanalysis/...` passing and a run that
  builds. But the run is the single-file control, which is the path that
  never broke. That is why the check names "not a control and not a
  different path", and why it says a passing regression test does not
  replace the repro re-run.
- pkg-14 runs a GET with header casing when the bug is a POST
  Content-Type. That is the "path the fix does not change" clause.
- pkg-04 and pkg-10 are the subjective clauses ("tested locally, colors
  work now", "verified working on my machine for a full day").

I rejected writing it as "tests pass" plus a judgement call. Instead, the
check asks for the before and the after on the same failing input, which
someone else can check by matching commands.

The clause "unless the plan or description records it as deferred" is
there so the check does not undo my honest-shortfall rule. Without it,
pkg-13 and pkg-16 would fail for not re-running the cases they openly
deferred.

**Trade-offs**

The "every distinct failing case the plan's test plan names" clause is
strict. A PR whose fix is solid, but whose evidence re-runs only the
main failing case, gets held. The case I accept it will miss is calib-04
(fd#2078). It is an unscored calibration package whose gold note calls
it "defensible both ways": a genuine cap-and-error fix whose evidence
re-runs repro 1 only, and says nothing about the `--exec` abort the plan
also named. My rubric holds it. The cost is that a real contributor with
a working fix would be told to go back and run one more command. I
accept that, because a test plan that names a case and then skips it is
exactly how an untested path reaches review.

Nothing else moved because of this. I never loosened or rewrote the
check between runs: both full runs used the same rubric and both scored
20/20, with all four not-tested packages rejected and all seven
clear-accepts accepted. The clear-accepts include pkg-13 and pkg-16, the
two whose deferred cases are exempt from the clause. So the strictness
did not cost any scored accept.

---

Related paths: `eval-run.txt` in this directory; your skill's files in
`tools/pr-precheck/`.
