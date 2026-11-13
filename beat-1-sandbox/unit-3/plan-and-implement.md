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

[The name of the branch you built the change on, exactly as it appears in your fork. The
naming shape is a type prefix, then the issue number, then a short description. **The issue
number in the branch name must be the number of the issue you claimed** — a name carrying
any other number does not satisfy this field.]

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

[The agreement score of each run you did, in order. A single run is a complete answer if
only one run occurred. **The last score in your list must match the agreement line in the
`eval-run.txt` you committed** — that file is the record of your final run.]

**Package analysis**

[Pick one scored package (`pkg-01` through `pkg-20` — the four `calib-` packages are never
scored). Name it by id, say what your rubric decided and what the gold label said, and
explain why your rubric read it that way.]

**Check rationale**

[Quote one check from the `rubric.md` you uploaded to `tools/plan-check/`, exactly as it reads now.
Then say why it reads that way — what you revised to get there, or what you rejected in
favour of it.]

**Trade-offs**

[Every check gives something up. Any one of these is a complete answer: a package whose
result it changes, a canary you re-ran with `--only`, a case you accept it will miss, or a
stated reason nothing changed elsewhere. "Nothing changed, and here is how I know" earns
the point in full when the reason follows.]

---

Related paths: `plan.md` and `eval-run.txt` in this directory; your skill's files in
`tools/plan-check/`.
