# Unit 1 — Issue Selection

Path: `beat-1-sandbox/unit-1/selection.md`

Record of the issue carried into Unit 2, and of the evaluation runs that produced
`eval-run.txt`.

---

## Selected issue

**Issue link**

https://github.com/codepath/pathreview-ai301-fa26-s3/issues/34

**Verdict output**

````
I graded all five with your rubric, and all five are accepted. None has a comment, an assignee or a linked PR, so nobody has started on any of them yet.

The repo-level checks pass for all five. The newest commit is from 2026-09-16, 18 days ago, by Aburke225, a person rather than a bot, and the repo isn't archived. Neither docs/CONTRIBUTING.md nor the PR template says anything about AI, which your rubric counts as a pass. The one preferred check, maintainer replies, fails for all five: in the 15 most recently updated issues, no maintainer has commented. That lowers ranking only and never changes a verdict.

Ranked by your fit profile:
1. #34 — add X-RateLimit-Remaining headers and a 429 response. This is the best fit. It's FastAPI middleware, which you've used, and the fix is clear: call the existing check_rate_limit, set two headers, and return 429 when the limit is hit. Each of those is easy to test, and it's the smallest of the five (3–5h). You'd also learn how api/ connects to safety/, which is good practice finding your way around a codebase.
2. #19 — add a contribution_streak field. The core of it is a pure Python function (longest run of consecutive days with commits), which is ideal for practising tests. It's 4–6h, all in agent/tools/github_tool.py.
3. #28 — rate limiting per IP address. It's also FastAPI middleware, but it overlaps #34: both add middleware in api/main.py that calls check_rate_limit. Pick one of the two, not both.
4. #3 — skip re-embedding when a document hasn't changed. It changes core/models/ingested_source.py, which probably means a database migration. That's more moving parts than a first issue needs.
5. #20 — DependencyAuditTool. It's the largest (5–8h) and has to read three file formats and look up current versions from outside sources.

None of the five involves React or CSS.

Your rubric is passing larger issues than its intent suggests. bounded_scope only rejects umbrella issues, unsettled designs and changes to core internals. It has no limit on estimated effort, so these 3–8 hour new features pass the same way the 1–2 hour bugs did. If you want first issues kept small, add an effort limit to that row in rubric.md.

```json
[
  {
    "item": "https://github.com/codepath/pathreview-ai301-fa26-s3/issues/34",
    "checks": [
      {"name": "maintainer_commits", "grade": "pass", "evidence": "Newest default-branch commit 2026-09-16 by Aburke225 (human), 18 days before 2026-10-04"},
      {"name": "repo_in_use", "grade": "pass", "evidence": "archived=false; no release; newest commit 2026-09-16 (within 180 days)"},
      {"name": "bounded_scope", "grade": "pass", "evidence": "Add middleware calling check_rate_limit and setting two headers; files api/middleware/, safety/rate_limiter.py, api/main.py; est. 3-5h"},
      {"name": "not_support_question", "grade": "pass", "evidence": "Asks for a code change: 'Add middleware that calls it and sets both headers'"},
      {"name": "no_abandoned_history", "grade": "pass", "evidence": "No PRs cross-referenced"},
      {"name": "not_assigned", "grade": "pass", "evidence": "assignees: []"},
      {"name": "no_open_pr", "grade": "pass", "evidence": "No linked PRs; 0 comments"},
      {"name": "no_live_claim", "grade": "pass", "evidence": "0 comments"},
      {"name": "contribution_policy", "grade": "pass", "evidence": "docs/CONTRIBUTING.md and PR template have no AI policy (silence)"},
      {"name": "maintainer_responds", "grade": "fail", "evidence": "0 maintainer first-responses across 15 most recently updated issues"}
    ],
    "verdict": "accept"
  }
]
```
````

(The skill graded five issues in this run: #34, #19, #28, #3 and #20, all accepted. The JSON above is the entry for the chosen issue, #34. The skill's full JSON also had entries for the other four, each with "verdict": "accept".)

---

## Eval iterations

**Run history**

Run 1 (first draft rubric, full run, rubric unchanged afterward):
`agreement: 18/20 scored items  (bar: 18/20: PASS)`

This is the only graded run and the one saved in `eval-run.txt`. An earlier attempt
printed `ERROR (claude exited 1: )` on every issue because my claude CLI was not
logged in. Nothing was graded, so it has no score.

**Issue analysis**

issue-19. The harness printed:
`issue-19  accept  reject  NO     failed: bounded_scope, maintainer_responds (preferred)`

My rubric's decision: reject. Gold label: accept. The harness only prints which
checks failed, not the grader's evidence, so the reasoning below is my reading of
the issue text against my rubric, not a quote from the grader.

The issue (zxcalc/zxlive#517, "Selecting large subgraphs in proof mode freezes the UI")
has a body that begins "There are two potential causes which should be fixed:" and
then lists "Additional suggestions" such as multi-processing and running matchers in
a separate thread. My `bounded_scope` check fails an issue that is "an umbrella or
tracking issue (a list of sub-items meant to be split)", and I think the numbered
list of five items looked like that, and the threading and multi-processing
suggestions may have looked like core internals.

I now think the gold label reads it better. The evidence guide says "Grade the size
of the work being asked for, not the polish of the writeup." This is one bug
reported by a maintainer (labels `Type: bug`, `Priority: High`), and the suggestions
are optional ideas, so a newcomer could fix one cause. My check judges the shape of
the text (a list) instead of whether the work could be split off and finished.
`maintainer_responds` also failed (2 of 5 sampled issues had no maintainer comment),
but it is `preferred`, so it could not change the verdict.

**Check rationale**

Quoted from `tools/issue-select/rubric.md`:

> | bounded_scope | Issue body and Comments section | None of these appear: the issue is an umbrella or tracking issue (a list of sub-items meant to be split), a maintainer says the design is undecided or debated, a maintainer says the fix touches core internals. A terse body alone is not a fail | required |

It is worded this way because the evidence guide names exactly these three conditions
as how scope fails, and says "Short is not the same as unscoped." I listed the three
conditions instead of using an adjective like "small", and added the last sentence so
a terse body would not be rejected by itself. It is `required` because "the scope
fits a newcomer" is one of the four families that kills a first contribution.

**Trade-offs**

This check gives up issues like issue-19, where a real bug has a numbered list in its
body. My rubric rejected it (`issue-19  accept  reject  NO`) even though gold says
accept, because "a list of sub-items meant to be split" is something a grader can
read into any list. It also has no effort limit, so in live mode it accepted features
the skill estimated at 3-8 hours alongside the 3-5 hour #34, and I chose the smallest
one myself. I left the rubric unchanged because 18/20 meets the bar and any edit
would stop matching the fingerprint in `eval-run.txt`. I re-ran issue-19 alone with
`--only` and it rejected again, so the miss is stable and not a one-off.

---

## Selection rationale

1. Fit: #34 adds rate-limit headers and a 429 response in FastAPI middleware, which I
   have used. It was the smallest of the five accepted issues (3-5h in the skill's
   estimate), so it fits the time I have.
2. The verdict correctly found that #34 had no comments, assignee or linked PR, and
   that the repo had a recent commit by a person. It could not weigh how big a first
   issue should be, because `bounded_scope` has no effort limit and also accepted
   5-8h features. It also could not weigh my interests. I made both calls myself.
3. Difficulty claiming: #28 overlaps #34 (both add middleware in api/main.py that
   calls check_rate_limit), so a classmate may claim one of them. No maintainer
   replied in the 15 most recently updated issues, so I may wait a while for feedback.
   I will recheck for a new PR right before I claim.

---

Related paths: `eval-run.txt` in this directory; your skill's files in
`tools/issue-select/`.