# Rubric: is this pull request ready to submit?

<!--
THIS IS THE PART YOU WRITE (fourth week running; this is the rubric's
final form in the sandbox). Your frame in SKILL.md executes whatever
checks you define here, via your procedure.md. It ships empty on
purpose: the judgment is your work.

A filled rubric must contain:

1. At least one row in the checks table. Each row needs all four
   columns:
   - Check: a short name (used in the output JSON).
   - Evidence: exactly what to look at, and where in the package. Name
     the part (the diff read against the plan's scope, the test
     evidence read against the plan's test plan, the description read
     against the diff, the repo-facts block's template asks) or a
     location from your references/evidence-guide.md. "The PR" is not
     a source; "the diff's changed files read against the plan's
     stated boundary" is.
   - Pass condition: a decision rule about the OUTCOME that someone
     else could apply and get your answer. Judge the thing itself
     (does the diff fall inside the plan plus its deviation notes? is
     the claimed evidence observable?), never the write-up's shape
     (how long the description is, how many commits there are).
     Structure-shaped checks are what make graders disagree with
     themselves.
   - Weight: `required` (a fail here holds the package) or `preferred`
     (never changes the verdict).

2. A verdict rule below the table: how the check grades combine into
   accept (submit) or reject (hold), including how `unclear` is
   treated. The verdict space is binary. State the `unclear`
   treatment explicitly: the frame here is YOUR SKILL.md, so a rubric
   that stays silent is only covered if your frame's grading
   discipline says what happens (the contract's own default is that
   an unverifiable claim fails).

Cover what actually gets bad PRs submitted. The failure families the
lecture named ARE the harness's scoring categories, same names as the
eval README: silent drift (the diff silently does more or less than
the posted plan, or the description claims fidelity the diff
contradicts), not tested (the evidence proves nothing observable, or
the repo's own checks were never run), unreviewable (debris or
unrelated hunks bury the change), and standards wall (the repo's
stated template and disclosure asks are ignored). Your evidence
guide's four headings map onto these one to one (plan fidelity =
silent drift, test evidence = not tested, diff quality =
unreviewable, standards and comms = standards wall), and the category
floor is scored on exactly these names plus clear accept. A rubric
that ignores a category will fail the eval packages built around
that category. And remember the honest-outcome
rule, fourth week running: a PR that honestly discloses a shortfall
can be ready; a rubric that equates "less than everything" with
"hold" fails the set.
-->

## Checks

| Check | Evidence | Pass condition | Weight |
|---|---|---|---|
| diff-within-plan | Every file and hunk in the candidate PR's diff, read against the plan context's scope (in-scope work, Files list, approach), its not-in-scope list, and any deviation or deferral note recorded in the plan or restated in the description. | Pass when every hunk does work the plan names (the fix, its tests, or a file the repo's stated PR asks require, such as a changelog or whatsnew entry) or work a recorded deviation note names, AND every deliverable the plan commits to (each code change, test, docs or changelog edit it lists) is in the diff or is explicitly deferred in a recorded note. Fail when any hunk adds work the plan does not name with no deviation note: a new option, flag, or setting, a rename or refactor pass, a rewrite of code the fix does not need, or an edit to a file outside the plan's Files list. Fail also when a deliverable the plan promises is missing from the diff with no note. Adjacent or well-meant extra work still fails; a correct core fix does not excuse it. | required |
| description-matches-diff | Each factual claim in the candidate PR's title and description about what the PR changes (files, behavior, tests, docs, "implements the plan exactly", "no other changes"), read against the diff. | Pass when every claim about the change is visible in the diff, and every limitation or deferral the plan records is not contradicted by the description. Fail when the description claims a change the diff does not contain (for example, says docs were updated and no docs file is in the diff), or claims fidelity or "no changes beyond X" while the diff contains work outside X. | required |
| repro-rerun-before-after | The test-evidence section, read against the plan's test plan and the repro evidence's failing case. | Pass when the evidence re-runs the plan's failing repro case (the same input and path that broke, not a control and not a different path) and shows an observable before result and an observable after result, with the after matching the plan's stated expected-after; and every distinct failing case the plan's test plan names is re-run, unless the plan or description records it as deferred. Fail when the evidence is subjective ("works now", "verified on my machine", "colors look right"), runs only a control or a path the fix does not change, re-runs only some of the failing cases the test plan names, or shows only an after with no before. A passing regression test does not replace the repro re-run. | required |
| repo-checks-shown | The test-evidence section, read against the plan's test plan and the repo-facts block's contributing asks. | Pass when the evidence names at least one of the repo's own test or check commands (for example `pytest ...`, `cargo test`, `go test ./...`, `make check`) and states its outcome. Fail when no repo test or check command appears, or one is named with no outcome. | required |
| diff-clean | The unified diff (every added and removed line) and the commit list. | Pass when every added or removed line belongs to the fix, its tests, its explanatory comments, or a file the repo's PR asks require. Fail when the diff contains any of: debug output added outside tests (`print`, `eprintln!`, `console.log`, `dbg!`, "DEBUG" lines), commented-out code, a dead or unused function (including one kept alive by an allow-dead-code attribute), a newly added TODO or FIXME, or formatting, re-indent, import-reordering, or re-printed-identical-line hunks on code the fix does not change. Explanatory comments describing the fix are not debris. Commit message wording alone never fails this check. | required |
| template-asks-met | The repo-facts block's "pull requests" line (the PR template and its required sections, checklist, and issue-link line) and contributing asks, read against the candidate PR's description and diff. | Pass when the repo states no PR template, or every concrete ask it states is met in substance: each required section is present with real content (the heading need not match word for word), each required line (for example "closes #N" or "fixes #N") is present with the issue number, each checklist item is addressed or marked not applicable with a reason, and each required file entry (changelog, whatsnew, regenerated docs) is in the diff. Subjective quality asks ("concise", "release-notes-quality title") pass unless plainly violated. Fail when any concrete required ask is visibly absent from both the description and the diff. | required |
| ai-disclosure | The repo-facts block's contribution policy and AI-use policy, read against the candidate PR's description. Treat every package as AI-assisted work. | Pass when the repo states no AI policy, or states one that asks nothing of pull request text, or the description contains a disclosure that meets what the policy asks (for example, the tool used and the extent of assistance when the policy asks for both). Fail when the policy requires disclosing AI use in pull requests or "in any form" and the description contains no disclosure, or the disclosure leaves out something the policy explicitly requires. | required |
| thread-direction-engaged | The thread highlights (any maintainer or owner direction, open question, or rejected approach), read against the description. | Pass when the thread has no explicit maintainer direction or open question about the approach, or the description follows it or names it and says why it departs. Fail when a maintainer gave explicit direction or asked an open question about the approach and the description proceeds as if it were never said. | preferred |

## Verdict rule

Accept (ready to submit) only if every required check passes. Reject
(hold) if any required check fails or is graded `unclear`. Preferred
checks are reported but never change the verdict.

Grade `unclear` only when the package lacks the evidence a check needs
to decide (for example, there is no test-evidence section at all).
When the evidence is present and shows nothing that violates the pass
condition, grade `pass`: the absence of a problem is not a problem.

An honestly recorded shortfall is not a failure. A case the plan
defers in a recorded note, and that the description does not claim to
have fixed, passes every check above; "less than everything" is only
drift when it is silent.
