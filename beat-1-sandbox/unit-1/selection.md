# Unit 1 — Issue Selection

Path: `beat-1-sandbox/unit-1/selection.md`

Record of the issue carried into Unit 2, and of the evaluation runs that produced
`eval-run.txt`. This file is graded at the path above; a copy kept anywhere else in
the repository is not read.

Complete every labelled field below. Each is graded on its own; content placed under the
wrong label is not graded.

---

## Selected issue

**Issue link**

[[The individual Path Review issue page. A link to the repository or the issue list
does not satisfy this field.]]
(https://github.com/codepath/pathreview-ai301-fa26-howard/issues/64)

**Verdict output**

[Your skill's live-mode output for this issue, pasted verbatim and ending with the
fenced JSON verdict block. A summary does not satisfy this field.]

**The verdict must record `accept` for this issue.** Choose an issue your own skill
accepts. If your skill rejects every candidate you try, that is a signal about your
rubric rather than about the issues: revise it and re-run — retries are unlimited and a
partial re-run costs about $0.20 — or run the skill on different candidates. Output
recording `reject` for the issue you chose earns no credit for this field.

Ran 3 shell commands

I graded the three issues against your rubric. Two pass and one fails.

Accepted, best fit first. 

#64: Relevance storer "partial overlap" test fixture has full overlap. This is the best match for your profile. It's a small Python/pytest fix: change the test's query so it only partly overlaps the chunk, then rerun pytest tests/unit/test_relevance_scorer•py. That's a good way to get back into Python and testing without much risk. It has a good first issue label.

```

---

## Eval iterations

Quote source text directly in each field below. Paraphrase does not satisfy them.

**Run history**

[The agreement score of each run you did, in order. A single run is a complete answer if
only one run occurred. **The last score in your list must match the agreement line in the
`eval-run.txt` you committed** — that file is the record of your final run.]


I no longer have the exact command history, but my score progression was like this:
16/20 16/20, 16/20.., 17/20, 17/20, 17/20..., fixed Claude connection-- 19/20



**Issue analysis**

[One scored issue, identified by id (`issue-01` through `issue-20`; the `calib-`
issues are not scored). State your rubric's decision, the gold label, and the
reasoning that produced your rubric's result.]

  issue-12: reject

item      gold    verdict  agree  note
issue-12  reject  reject   yes

The required ai-policy check caused it to fail. Issue 12 has an outright ban on it, so that's an automatic rejection. 

**Check rationale**

[One check from the `rubric.md` uploaded to `tools/issue-select/`, quoted as it is
currently written, with the reasoning behind its current form.]

| gfi-label | Issue labels | Has a "good first issue", "help wanted", or "easy" label | preferred |
Reasoning: Originally, this was how this check was formatted:
| right-fit | tags | Has a "good first issue" tag | preferred |
 , and I asked for Claude's help to make it more thorough and the above is the result. The new version covered more bases. 

**Trade-offs**

[What the quoted check gives up. Any one of these is a complete answer: an issue whose
result it changes, a canary you re-ran with `--only`, a case you accept it will miss, or a
stated reason nothing changed elsewhere. "Nothing changed, and here is how I know" earns
the point in full when the reason follows.]

Issue 13 doesn't have any comments, but is still claimed via the repo facts instead. Since I've fixed Claude and am now passing the checks with a 19/20, I am accepting that it is a case I will miss.

---

## Selection rationale

Graded on whether all three are answered, in your own words. Not on how good the
reasoning is, and not on length — a short honest answer to each earns the full marks.
This is also the basis for the claim comment you write in Unit 2.

**Selection rationale**

[Answer all three:

1. The issue's fit to your interests and to the time available.
I specifically stated that I wanted an issue to The other accepted issue was #75, a docs and config edit, and specifically didn't have much to do with python. I considered it because it seems like an easier fix, however for my goal of wanting to refresh my skills, the issue I chose is best. 
2. What the verdict identified correctly, and what you weighed that the rubric could
   not.
The verdict correctly identified everything about the fit pretty well, in its explanation it explained that this issue wasn't a super extensive fix, but still offered me a good place to start with practice. However, I couldn't be sure if the rubric underestimated my abilities. I did want to take it slow, but did want to simultaneously offer myself enough of a challenge. In that I had to use my own judgement in not picking the low hanging fruit. Even then, the tool did still consider the idea that I wasn't completely new to things, which I was impressed with. 
3. The anticipated difficulty in claiming it.
I think because I'm in need of a refresh, it will be harder for me. I estimate about a 3/5 in personally difficulty with all things considered. Because of it being a good first issue and meant to refresh, which is something I emphasized in my rubric, I am a little concerned about getting beat out to the issue, but I don't think it will be a large deterrent. In that case, I may be able to pivot to issue #75.

---

Related paths: `eval-run.txt` in this directory; your skill's files in
`tools/issue-select/`.
