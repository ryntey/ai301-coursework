# Unit 3 — Plan and Build

Path: `beat-1-sandbox/unit-3/plan-and-implement.md`

Record of your plan, the branch you built it on, and the evaluation runs that produced
`eval-run.txt`. This file is graded at the path above; a copy kept anywhere else in the
repository is not read.

Complete every labelled field below. Each is graded on its own; content placed under the wrong
label is not graded.

---

## Posted upstream

**GitHub username**

ryntey

**Plan comment**

[[Link to the comment where you posted your plan on the issue. Use the comment's own
permalink. **Then paste the text of that comment underneath the link** — the pasted text is
what this field is graded on, so copy across what you actually posted.]]
(https://github.com/codepath/pathreview-ai301-fa26-howard/issues/75#issuecomment-6845260501)

From my reproduction (fresh clone at commit 99673c7, following the
README's Quick Start exactly; macOS 15.3.1, Python 3.11.1)

The README's setup block says:
# Configure environment (add your OPENROUTER_API_KEY to .env) then
cp .env.example .env. docs/SETUP.md repeats it:
# Edit .env and set your OPENROUTER_API_KEY (required for AI features).
grep -c OPENROUTER .env.example → 0. The file's LLM block is:
# LLM provider
# Options: "mock" (default, no API key needed), "openai"
LLM_PROVIDER=mock
OPENAI_API_KEY=sk-your-key-here
core/config.py defines both keys plus two OpenRouter settings:
openai_api_key, openrouter_api_key,
openrouter_base_url (default https://openrouter.ai/api/v1),
openrouter_model (default google/gemma-3-27b-it:free).
Real code path: with .env copied from .env.example, loading
core.config.Settings() prints:
llm_provider= 'mock'
openrouter_api_key= ''
openai_api_key= 'sk-your-key-here'
A contributor who follows the README has no OPENROUTER_API_KEY line to
fill in, and the template only offers the OpenAI key.
Expected: .env.example lists the key the README tells you to set, and its
LLM_PROVIDER comment matches the settings core/config.py defines.
Actual: it lists only OPENAI_API_KEY and only mock/openai.

Diagnosis
.env.example was not updated when OpenRouter settings were added to
core/config.py and the README/SETUP docs were pointed at
OPENROUTER_API_KEY. The README and SETUP.md already match the code;
.env.example is the file that is out of date.

Scope
One bounded change: bring .env.example's LLM block in line with
core/config.py.

In scope: .env.example only. Add OPENROUTER_API_KEY, add
OPENROUTER_BASE_URL and OPENROUTER_MODEL commented out with the
defaults from core/config.py, and update the LLM_PROVIDER comment so it
says which key goes with which provider. Keep OPENAI_API_KEY, because
config.py still defines it and ingestion/embeddings/provider.py says it
needs it.
Not in scope: any change to core/config.py; wiring llm_provider into
code; README or SETUP.md (they already say what the code expects); the
unrelated DATABASE_URL port difference (5433 in .env.example, 5432
default in config.py). I'll mention that one in the PR notes as a
separate possible issue, not fix it here.
Files / approach
.env.example, # LLM provider block, change to:
# LLM provider
# Options: "mock" (default, no API key needed), "openai", "openrouter"
# The README's Quick Start uses OpenRouter: set OPENROUTER_API_KEY for AI features.
LLM_PROVIDER=mock
OPENROUTER_API_KEY=sk-or-your-key-here
# OPENROUTER_BASE_URL=https://openrouter.ai/api/v1
# OPENROUTER_MODEL=google/gemma-3-27b-it:free
OPENAI_API_KEY=sk-your-key-here
Branch fix/75-env-example-openrouter-key on my fork; commit as
docs: add OPENROUTER_API_KEY to .env.example (Conventional Commits, per
CONTRIBUTING.md).
Run make check && make test-unit before pushing (CONTRIBUTING.md's PR
step 1); open the PR in week 4 with the PR template filled in.
Test plan
Re-run my repro steps against the branch:

Step 2 again: grep -c OPENROUTER .env.example → before 0,
expected after 3 (one active key line, two commented settings).
Step 4 again (real code path): cp .env.example .env, then load
core.config.Settings(). Before: openrouter_api_key= ''.
Expected after: openrouter_api_key= 'sk-or-your-key-here', with
llm_provider= 'mock' unchanged so the default setup still needs no key.
Following the README's Quick Start word for word, the
OPENROUTER_API_KEY line it tells me to fill in now exists in .env.
make check && make test-unit pass. No test reads .env.example today,
so no test changes are expected.


---

## Your branch

**Branch**

fix/75-env-example-openrouter-key

**Evidence**
**Before (main):**

```
$ git checkout main
Switched to branch 'main'
Your branch is up to date with 'origin/main'.
$ grep -c OPENROUTER .env.example
0
$ T=$(mktemp -d); cp .env.example "$T/.env"; cp -R core "$T/"; (cd "$T" && python3 -c "from core.config import Settings; s=Settings(); print('llm_provider =', repr(s.llm_provider)); print('openrouter_api_key =', repr(s.openrouter_api_key)); print('openai_api_key =', repr(s.openai_api_key))")
llm_provider = 'mock'
openrouter_api_key = ''
openai_api_key = 'sk-your-key-here'
```

**After (fix/75-env-example-openrouter-key):**

```
$ git checkout fix/75-env-example-openrouter-key
$ grep -c OPENROUTER .env.example
4
$ T=$(mktemp -d); cp .env.example "$T/.env"; cp -R core "$T/"; (cd "$T" && python3 -c "from core.config import Settings; s=Settings(); print('llm_provider =', repr(s.llm_provider)); print('openrouter_api_key =', repr(s.openrouter_api_key)); print('openai_api_key =', repr(s.openai_api_key))")
llm_provider = 'mock'
openrouter_api_key = 'sk-or-your-key-here'
openai_api_key = 'sk-your-key-here'
```

Before the change, a `.env` copied from `.env.example` gives the app no OpenRouter key, even though the README says to set one. After it, the key is in the template and `Settings()` loads it, and `LLM_PROVIDER` stays `mock`, so the default setup still needs no key.

## Eval iterations

Answer all four sections. Quote source text directly; paraphrase does not satisfy these
fields.

**Run history**

Run history: 
The default interactive shell is now zsh.
To update your account to use zsh, please run `chsh -s /bin/zsh`.
For more details, please visit https://support.apple.com/kb/HT208050.
MacBook-Pro-98:ai301-unit3-starter laurynallotey$ mkdir -p ~/.claude/skills/plan-check && cp -R "$(git rev-parse --show-toplevel)/skill/." ~/.claude/skills/plan-check/
MacBook-Pro-98:ai301-unit3-starter laurynallotey$ ls -R ~/.claude/skills/plan-check
SKILL.md        procedure.md    references      rubric.md       scope.md        voice-guide.md

/Users/laurynallotey/.claude/skills/plan-check/references:
evidence-guide.md
MacBook-Pro-98:ai301-unit3-starter laurynallotey$ cp ../skill/rubric.md ~/.claude/skills/repro-check/rubric.md
cp ../skill/references/evidence-guide.md ~/.claude/skills/repro-check/references/evidence-guide.mdcp: ../skill/rubric.md: No such file or directory
MacBook-Pro-98:ai301-unit3-starter laurynallotey$ cp ../skill/references/evidence-guide.md ~/.claude/skills/repro-check/references/evidence-guide.md
cp: ../skill/references/evidence-guide.md: No such file or directory
MacBook-Pro-98:ai301-unit3-starter laurynallotey$ cd /Users/laurynallotey/Documents/GitHub/ai301-unit2-starter/eval
MacBook-Pro-98:eval laurynallotey$ python3 run_eval.py --rubric ~/.claude/skills/repro-check/rubric.md --evidence ~/.claude/skills/repro-check/references/evidence-guide.md
grading 20 package(s) with rubric.md + evidence-guide.md, model sonnet, 5 worker(s)...
  pkg-04: reject
  pkg-02: reject
  pkg-03: accept
  pkg-01: accept
  pkg-05: accept
  pkg-07: accept
  pkg-08: reject
  pkg-06: reject
  pkg-10: reject
  pkg-09: reject
  pkg-11: accept
  pkg-12: accept
  pkg-14: reject
  pkg-13: reject
  pkg-15: reject
  pkg-17: reject
  pkg-19: reject
  pkg-16: reject
  pkg-20: accept
  pkg-18: reject

item    gold    verdict  agree  note
pkg-01  accept  accept   yes    
pkg-02  reject  reject   yes    
pkg-03  accept  accept   yes    
pkg-04  reject  reject   yes    
pkg-05  accept  accept   yes    
pkg-06  reject  reject   yes    
pkg-07  accept  accept   yes    
pkg-08  reject  reject   yes    
pkg-09  accept  reject   NO     failed: behavior-matches
pkg-10  accept  reject   NO     failed: behavior-matches
pkg-11  accept  accept   yes    
pkg-12  accept  accept   yes    
pkg-13  reject  reject   yes    
pkg-14  reject  reject   yes    
pkg-15  reject  reject   yes    
pkg-16  reject  reject   yes    
pkg-17  reject  reject   yes    
pkg-18  reject  reject   yes    
pkg-19  reject  reject   yes    
pkg-20  reject  accept   NO     graded accept

categories: clear-accept 6/8  disclosure 0/1  no-evidence 4/4  unfollowable-comms 3/3  wrong-target 4/4
agreement: 17/20 scored items  (bar: 18/20: below the bar; category floor unmet: no match in disclosure)
MacBook-Pro-98:eval laurynallotey$ run_eval.py --rubric ~/.claude/skills/repro-check/rubric.md --evidence ~/.claude/skills/repro-check/references/evidence-guide.md --only pkg-09, pkg-10, pkg-20
bash: run_eval.py: command not found
MacBook-Pro-98:eval laurynallotey$ run_eval.py --rubric ~/.claude/skills/repro-check/rubric.md --evidence ~/.claude/skills/repro-check/references/evidence-guide.md --only pkg-09 pkg-10 pkg-20
bash: run_eval.py: command not found
MacBook-Pro-98:eval laurynallotey$ run_eval.py --rubric ~/.claude/skills/repro-check/rubric.md --evidence ~/.claude/skills/repro-check/references/evidence-guide.md --only pkg-09 pkg-10 pkg-20
bash: run_eval.py: command not found
MacBook-Pro-98:eval laurynallotey$ cp ~/Downloads/rubric.md ~/.claude/skills/plan-check/rubric.md
cp ~/Downloads/procedure.md ~/.claude/skills/plan-check/procedure.md
cp ~/Downloads/evidence-guide.md ~/.claude/skills/plan-check/references/evidence-guide.md
cp ~/.claude/skills/repro-check/voice-guide.md ~/.claude/skills/plan-check/voice-guide.mdMacBook-Pro-98:eval laurynallotey$ cp ~/Downloads/procedure.md ~/.claude/skills/pcheck/procedure.md
MacBook-Pro-98:eval laurynallotey$ cp ~/Downloads/evidence-guide.md ~/.claude/skills/plan-check/references/evidence-guide.md
MacBook-Pro-98:eval laurynallotey$ cp ~/.claude/skills/repro-check/voice-guide.md ~/.claude/skills/plan-check/voice-guide.md
MacBook-Pro-98:eval laurynallotey$ cd ~/Documents/GitHub/ai301-unit3-starter/eval
python3 run_eval.py --rubric ~/.claude/skills/plan-check/rubric.md \
  --evidence ~/.claude/skills/plan-check/references/evidence-guide.md \
  --save-runMacBook-Pro-98:eval laurynallotey$ python3 run_eval.py --rubric ~/.claude-check/rubric.md \
>   --evidence ~/.claude/skills/plan-check/references/evidence-guide.md \
>   --save-run eval-run.txt
grading 20 package(s) with rubric.md + evidence-guide.md + procedure.md, model sonnet, 5 worker(s)...
  pkg-05: accept
  pkg-03: accept
  pkg-02: accept
  pkg-01: reject
  pkg-04: reject
  pkg-06: reject
  pkg-07: reject
  pkg-08: accept
  pkg-10: reject
  pkg-09: accept
  pkg-12: reject
  pkg-11: reject
  pkg-13: accept
  pkg-15: reject
  pkg-14: accept
  pkg-17: reject
  pkg-16: reject
  pkg-18: reject
  pkg-19: reject
  pkg-20: reject

item    category           gold    verdict  agree  note
pkg-01  wrong-cause        reject  reject   yes    
pkg-02  clear-accept       accept  accept   yes    
pkg-03  clear-accept       accept  accept   yes    
pkg-04  thread-convention  reject  reject   yes    
pkg-05  clear-accept       accept  accept   yes    
pkg-06  scope-creep        reject  reject   yes    
pkg-07  wrong-cause        reject  reject   yes    
pkg-08  clear-accept       accept  accept   yes    
pkg-09  clear-accept       accept  accept   yes    
pkg-10  unbuildable        reject  reject   yes    
pkg-11  wrong-cause        reject  reject   yes    
pkg-12  scope-creep        reject  reject   yes    
pkg-13  clear-accept       accept  accept   yes    
pkg-14  clear-accept       accept  accept   yes    
pkg-15  scope-creep        reject  reject   yes    
pkg-16  wrong-cause        reject  reject   yes    
pkg-17  unbuildable        reject  reject   yes    
pkg-18  unbuildable        reject  reject   yes    
pkg-19  scope-creep        reject  reject   yes    
pkg-20  thread-convention  reject  reject   yes    

categories: clear-accept 7/7  scope-creep 4/4  thread-convention 2/2  unbuildable 3/3  wrong-cause 4/4
agreement: 20/20 scored items  (bar: 18/20: PASS)
run written to eval-run.txt


**Package analysis**

[pkg-20 (ghostty-org/ghostty#11261). My rubric decided reject; the gold
label is reject, so they agree.

This is the hardest reject in the set, because the plan itself is
strong. My rubric passed it on diagnosis-grounded (the no-hyperlink
control passes, which fits the stale-`prev` cause), scope-bounded
(unconditional recompute and other cached pointers are explicitly out
of scope), executable (one chosen approach: a page generation counter
checked in Terminal.print), test-decisive (both fuzz cases as
regression tests, control unchanged), and comms-thread-aware (the
comment says "My plan follows the direction proposed here", matching
mitchellh's suggestion to recompute only when capacity changes).

It failed on one required check, comms-policy-compliant. The repo-facts
block quotes ghostty's policy: "All AI usage in any form must be
disclosed, stating the tool used and the extent of the assistance,"
and the plan comment contains no disclosure. The eval treats every
package as AI-assisted, so a good plan whose comment breaks the repo's
stated AI policy is not ready to post. My rubric reads it this way
because the policy check is required and never balanced against how
good the rest of the plan is.

**Check rationale**

| comms-policy-compliant | The repo-facts block's contribution policy and AI-use policy, read against the candidate plan comment. Treat every package as AI-assisted work. | Pass when the repo states no AI disclosure requirement for comments, or the comment satisfies what the policy requires. Fail when the policy requires disclosing AI usage (in any form, or in comments/issues specifically) and the plan comment contains no disclosure. A policy whose disclosure ask applies only to pull requests, or which states no disclosure ask for comments, does not require one in the plan comment. | required |

Why it reads this way: my Unit 2 repro-check rubric had an
ai-disclosure check that just said "if the policy requires disclosing
AI use, the comments disclose it." That was too blunt for plan
packages, because the repos here word their policies differently.
ghostty (pkg-20) requires disclosure of AI use in any form, while fd
(pkg-09) requires it only in pull requests and "states no disclosure
ask for issue comments." A blunt version would fail pkg-09, a gold
accept, for skipping a disclosure fd never asked for. So I made the
check say where the requirement applies: it fails only when the policy
covers comments, and the last sentence explicitly carves out PR-only
policies. I also kept "Treat every package as AI-assisted work" so the
grader can't pass a comment by assuming no AI was used.



**Trade-offs**

The PR-only carve-out in comms-policy-compliant is the trade-off. It is
what lets pkg-09 pass (fd's policy asks for disclosure in pull
requests, not issue comments), but it means my rubric accepts some
comments with no disclosure at all. A case I accept it will miss: a
repo whose policy is vague about scope ("disclose AI use in your
contributions"). The grader has to decide whether a comment counts as
a contribution, and my check doesn't settle that, so it could go
either way between runs.

Nothing else changed because of this: in the full run, the two
thread-convention packages (pkg-04 and pkg-20) still both rejected,
and pkg-03 (ripgrep, where the comment discloses AI use and says it is
in the author's own words) still accepted.

---

Related paths: `plan.md` and `eval-run.txt` in this directory; your skill's files in
`tools/plan-check/`.
