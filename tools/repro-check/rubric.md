# Rubric: is this reproduction package ready to post?

## Checks

|             Check               |          Evidence               |       Pass condition          |            Weight              |
|---------------------------------|---------------------------------|-------------------------------|--------------------------------|
|         env-recorded            |   The repro report's environment record, compared against the issue context and the repo-facts block for the right OS, runtime, dependency, version, and configuration requirements.   | The recorded environment identifies the relevant conditions needed to interpret the reproduction.|  required
|        steps-followable         |       The repro report's reproduction steps, read against the issue context and the repo-facts block for prerequisites, setup, commands, inputs, and expected starting state. | Pass when a stranger with public access could run steps as written, from the starting state to the final command, fail when any step depends on what the reader cannot access.  | required
|        behavior-matches         |     The repro report's output excerpts, logs, screenshots, error messages, and expected-versus-actual results, read against the issue context's description of the target behavior and the repo-facts block. | pass if the artifacts show the issue’s specific behavior, not a different error or a similar symptom. Do not fail a cannot-reproduce because the attempt may not have reached the trigger; when the report admits failures and names what it would take, that is an honest result and passes.| required
|        claim-specific           |  The claim comment, compared with the repro report's environment record, reproduction steps, results, artifacts, issue context, and repo-facts block.  |  The comment identifies the specific behavior actually observed and makes no material claim that contradicts or exceeds the evidence. A claim that the issue was reproduced passes only when the artifacts support that conclusion. A claim of inability to reproduce passes when it accurately describes the evidence-backed outcome; it does not establish that the issue is invalid.| required
ai-disclosure	 | the repo-facts block’s contribution and AI-use policy, read against the claim comment and repro report	if the policy requires disclosing AI use, the comments disclose it (treat every package as AI-assisted) | passes when the repo states no such requirement| required
## Verdict rule

Reject if any required check fails. Anything that comes across unclear counts as a full fail as it is not a certain pass. 
