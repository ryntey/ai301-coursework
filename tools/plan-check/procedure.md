# Procedure: how this skill grades a plan package

## Read order

1. Repo facts first. Write down: (a) the exact AI-use / contribution
   policy wording, and whether it requires disclosing AI use in
   comments (yes / only in PRs / no policy); (b) template asks.
2. Issue next. Write down the reported behavior in one sentence:
   that is the scope boundary.
3. Thread highlights. List every statement by an OWNER, MEMBER,
   COLLABORATOR, or CONTRIBUTOR that gives direction: an isolated
   culprit, a proposed approach, a rejected approach, a patch to
   test, or an open PR. If none, write "no maintainer direction".
4. Repro evidence BEFORE the plan. List each control run and
   comparison and what it rules in or out. Write down the Expected
   line. Reading the evidence first means the plan's confident
   wording cannot set the frame for the diagnosis check.
5. Candidate plan, then candidate plan comment.

## Evidence gathering

- diagnosis-grounded: the plan's cause sentence vs. the control list
  from step 4. For each control, ask: does it show the failure
  without the blamed part, or the blamed part working under failing
  conditions? Record any control that does.
- scope-bounded: list every unit of work the plan commits to (each
  approach step, each in-scope item). Mark each as "needed for the
  fix/test" or "extra". Deferrals are not commitments.
- executable: record the chosen approach and the named code location,
  or the hedge words that replace them.
- test-decisive: record the observable outcome the test plan names
  and whether it ties to the repro steps or issue cases.
- honest-unknowns: record stated risks/unknowns and any certainty
  claim not backed by the evidence.
- comms-thread-aware: for each maintainer direction from step 3,
  record whether the comment or plan follows it, names and departs
  from it with a reason, or ignores it.
- comms-policy-compliant: from step 1(a), record whether disclosure
  is required in comments; then quote the comment's disclosure
  sentence or record "none".

## Check execution

- Run the checks in rubric order. Each check reads only its gathered
  evidence; do not let a pass on one check soften another (a
  polished, well-scoped plan can still fail comms; a correct core fix
  can still fail scope).
- Grade each check strictly by its pass condition, not by overall
  impression or length. Terse plans that meet the conditions pass.
- If the evidence for a check is genuinely absent from the package
  (e.g. the plan names no test plan), grade fail. If the package
  simply contains nothing that violates the pass condition (e.g. no
  maintainer direction exists), grade pass.
- Do not invent requirements the rubric does not state.

## Verdict assembly

1. Apply the rubric's verdict rule: accept only if every required
   check is pass; any required fail (or unclear) means reject.
2. Preferred checks are listed but never change the verdict.
3. In each check's evidence field, quote or cite the specific fact
   that decided it; for a reject, the failing check's evidence must
   quote the package text (control run, plan step, thread statement,
   or policy line) that caused the fail.
