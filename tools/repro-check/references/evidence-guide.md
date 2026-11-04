## Evidence guide: where proof lives in a reproduction package

## Environment

It lives in an an eval bundle, you should check the issue context for the environment or version requirements, then compare them with the repo-facts block and the environment details in the repro report. In live mode, check the GitHub issue for stated OS, runtime, dependency, or version requirements, compare them with the repository’s README, setup instructions, and configuration files, and inspect the draft comment for the environment they actually used.
What "Good" looks like: the report identifies the relevant OS, runtime, dependency versions, and configuration details needed to understand the reproduction, where applicable. The recorded environment matches the issue's stated target, or explicitly identifies any differences that could affect the result.



## Steps
It lives in an eval bundle, check the reproduction report's steps section and compare it with the issue context and any setup details in the repo-facts block. In live mode, check the GitHub issue's reproduction instructions and the student's draft comment for the starting state, setup commands, actions, inputs, and triggering conditions.

To be considered "good", make sure that the steps specify the prerequisites, starting state, exact commands or actions, and inputs needed to trigger the reported behavior. A stranger with the stated environment can follow the steps in order without having to guess at missing instructions or unstated assumptions.



## Behavior shown



Where it lives: In an eval bundle, inspect the artifacts included in the reproduction report, such as output excerpts, error messages, logs, screenshots, and expected-versus-actual results. Compare them with the issue context and repo-facts block to establish what behavior should occur. In live mode, inspect the evidence attached to the GitHub issue or linked in its thread, together with the student's draft comment describing the observed result.

What good looks like: The artifacts show the specific behavior described in the issue and connect it to the documented reproduction steps. The evidence distinguishes the actual result from the expected result and contains enough relevant context to identify the failure, rather than showing an unrelated error or an artifact that merely suggests the issue might exist. 

## Honesty

Where it lives: In an eval bundle, compare the claim comment with the reproduction report, including its environment, steps, results, and artifacts. Check the issue context and repo-facts block for facts that support or contradict the claim. In live mode, compare the student's draft comment against the GitHub issue, the repository's documented behavior, and the evidence produced by the student's own reproduction attempt.

What good looks like: Every factual claim about reproduction, observed behavior, and verification is supported by the available evidence. The report distinguishes confirmed results from assumptions, clearly states limitations or environment differences, and says "cannot reproduce" when appropriate without presenting an unsuccessful or incomplete attempt as proof that the issue is invalid. A cannot-reproduce that admits its setup may not have hit the trigger, and names the likely missing condition, is honest, not incomplete.

## Comms

 Where it lives: In an eval bundle, inspect the claim comment, the issue context, the repo-facts block, and any contribution or AI-use policies represented in the bundle. Compare the comment's content and disclosures with the repository's stated requirements. In live mode, inspect the student's draft comment alongside the GitHub issue thread, the repository's contribution guidelines and issue or pull-request templates, and any documented AI-use disclosure requirements.

What good looks like: The comment addresses the specific issue, identifies the relevant reproduction result, and makes concrete claims that agree with the supporting evidence. It follows applicable repository templates and contribution policies, includes any required AI-use disclosure, and avoids generic boilerplate, unsupported certainty, or claims of completed work that the evidence does not establish. 
