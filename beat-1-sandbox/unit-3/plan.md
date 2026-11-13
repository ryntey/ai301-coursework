# Plan: issue #75 — README and `.env.example` disagree about which LLM API key to set

Issue: https://github.com/codepath/pathreview-ai301-fa26-howard/issues/75

## Repro evidence this plan builds on

From my reproduction (fresh clone at commit `99673c7`, following the
README's Quick Start exactly; macOS, Python 3.11.1)

1. The README's setup block says:
   `# Configure environment (add your OPENROUTER_API_KEY to .env)` then
   `cp .env.example .env`. `docs/SETUP.md` repeats it:
   `# Edit .env and set your OPENROUTER_API_KEY (required for AI features)`.
2. `grep -c OPENROUTER .env.example` → `0`. The file's LLM block is:
   ```
   # LLM provider
   # Options: "mock" (default, no API key needed), "openai"
   LLM_PROVIDER=mock
   OPENAI_API_KEY=sk-your-key-here
   ```
3. `core/config.py` defines both keys plus two OpenRouter settings:
   `openai_api_key`, `openrouter_api_key`,
   `openrouter_base_url` (default `https://openrouter.ai/api/v1`),
   `openrouter_model` (default `google/gemma-3-27b-it:free`).
4. Real code path: with `.env` copied from `.env.example`, loading
   `core.config.Settings()` prints:
   ```
   llm_provider= 'mock'
   openrouter_api_key= ''
   openai_api_key= 'sk-your-key-here'
   ```
   A contributor who follows the README has no `OPENROUTER_API_KEY` line to
   fill in, and the template only offers the OpenAI key.

Expected: `.env.example` lists the key the README tells you to set, and its
`LLM_PROVIDER` comment matches the settings `core/config.py` defines.
Actual: it lists only `OPENAI_API_KEY` and only `mock`/`openai`.

## Diagnosis

`.env.example` was not updated when OpenRouter settings were added to
`core/config.py` and the README/SETUP docs were pointed at
`OPENROUTER_API_KEY`. The README and SETUP.md already match the code;
`.env.example` is the file that is out of date.

## Scope

One bounded change: bring `.env.example`'s LLM block in line with
`core/config.py`.

- In scope: `.env.example` only. Add `OPENROUTER_API_KEY`, add
  `OPENROUTER_BASE_URL` and `OPENROUTER_MODEL` commented out with the
  defaults from `core/config.py`, and update the `LLM_PROVIDER` comment so it
  says which key goes with which provider. Keep `OPENAI_API_KEY`, because
  `config.py` still defines it and `ingestion/embeddings/provider.py` says it
  needs it.
- Not in scope: any change to `core/config.py`; wiring `llm_provider` into
  code; README or SETUP.md (they already say what the code expects); the
  unrelated `DATABASE_URL` port difference (`5433` in `.env.example`, `5432`
  default in `config.py`). I'll mention that one in the PR notes as a
  separate possible issue, not fix it here.

## Files / approach

1. `.env.example`, `# LLM provider` block, change to:
   ```
   # LLM provider
   # Options: "mock" (default, no API key needed), "openai", "openrouter"
   # The README's Quick Start uses OpenRouter: set OPENROUTER_API_KEY for AI features.
   LLM_PROVIDER=mock
   OPENROUTER_API_KEY=sk-or-your-key-here
   # OPENROUTER_BASE_URL=https://openrouter.ai/api/v1
   # OPENROUTER_MODEL=google/gemma-3-27b-it:free
   OPENAI_API_KEY=sk-your-key-here
   ```
2. Branch `fix/75-env-example-openrouter-key` on my fork; commit as
   `docs: add OPENROUTER_API_KEY to .env.example` (Conventional Commits, per
   CONTRIBUTING.md).
3. Run `make check && make test-unit` before pushing (CONTRIBUTING.md's PR
   step 1); open the PR in week 4 with the PR template filled in.

## Test plan

Re-run my repro steps against the branch:

- Step 2 again: `grep -c OPENROUTER .env.example` → before `0`,
  expected after `4` (one comment line naming the key, one active key line, two commented settings).
- Step 4 again (real code path): `cp .env.example .env`, then load
  `core.config.Settings()`. Before: `openrouter_api_key= ''`.
  Expected after: `openrouter_api_key= 'sk-or-your-key-here'`, with
  `llm_provider= 'mock'` unchanged so the default setup still needs no key.
- Following the README's Quick Start word for word, the
  `OPENROUTER_API_KEY` line it tells me to fill in now exists in `.env`.
- `make check && make test-unit` pass. No test reads `.env.example` today,
  so no test changes are expected.

## Risks and unknowns

- **Unknown:** I could not find any code that reads `settings.llm_provider`
  (searching the repo for `llm_provider`/`LLM_PROVIDER` only finds
  `config.py`, `.env.example`, and CI workflows setting `mock`). So I can't
  prove from the code that `"openrouter"` is the exact provider value the app
  will expect. I'm listing it because the issue asks the comment to match
  `config.py`'s keys, and I'm asking in the thread whether maintainers prefer
  a different value name, or leaving the value list alone and only adding the
  key.
- Low-risk change: `.env.example` is a template, and nothing imports it.
  The real risk is giving contributors wrong guidance, which is what the
  re-run of the README steps checks.


## Deviations

The change I built is the one I planned: same file, same lines, same
scope. One correction to the test plan: I predicted the
`grep -c OPENROUTER .env.example` re-run would return 3, and it
returned 4, because the new comment line names OPENROUTER_API_KEY too.
I counted wrong; the code did nothing unexpected. The check that
matters went as planned: loading Settings() from a fresh copy of the
template gives openrouter_api_key = 'sk-or-your-key-here' (before: ''),
and llm_provider stays 'mock'.
