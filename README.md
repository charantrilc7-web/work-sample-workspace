# Work Sample Workspace

Two small, runnable pages that show how an AI-assisted work sample could be run and then reviewed fairly. Unofficial demo by Charan T R, built from a public job post. It is not affiliated with, and does not use any code, rubric or data from, Touchstones.

**Live pages**

- Candidate side: https://charantrilc7-web.github.io/work-sample-workspace/
- Reviewer side: https://charantrilc7-web.github.io/work-sample-workspace/evaluator.html

No build step, no dependencies. Open `index.html` in a browser to run it locally. The only network request is Google Fonts.

## 1. Work Sample Workspace (`index.html`)

The part where the interview happens. The candidate gets a brief (a rate limiter for a public API), a code editor, a test runner, an AI assistant panel and a README to explain their decisions.

- **Test runner:** candidate code runs in a Web Worker with a 4 second timeout. Syntax errors, runtime errors and infinite loops are caught and mapped to file and line.
- **Work trail:** every prompt, AI answer, edit (as a line diff), test run and note is logged with a timestamp. The candidate can read the whole trail before submitting.
- **Candidate stays in control:** an up-front consent screen says what is recorded and that a human reviews it. Code from the assistant only enters the editor when the candidate clicks.
- **Submit:** locks the files and exports the trail as JSON.
- **Layout:** works on desktop and phone, in light and dark mode.

## 2. Work Trail Evaluator (`evaluator.html`)

The part after the interview. A reviewer reads a fictional candidate's trail next to a five-criterion rubric and makes the call.

- **Evidence-cited scoring:** each score points to trail events. A citation only counts if its quote is a substring of the cited event.
- **Eval harness:** three retrievers (keyword baseline, stemmed, stemmed with an event-type prior) scored on precision and recall at k against a golden set, with a regression gate.
- **Injection guard:** rules for text inside a candidate's trail that tries to instruct the judge (normalisation, zero-width stripping, base64 decode and re-check, delimiter spoofing). It includes one attack it deliberately misses, so the limits are visible.
- **Tool-call limits:** allowlist, path normalisation, egress allowlist and a call budget.
- **Reviewer assist:** summary, gaps and templated follow-up questions.
- **Cost calculator:** single call versus per-criterion with retrieval. Prices are placeholders.

## What is live and what is recorded

| Part | Status |
| --- | --- |
| Editor, test runner, diff logging, work trail, JSON export | Live |
| Citation check, eval metrics, injection rules, tool-call policy, cost maths | Live |
| AI assistant on this site | Scripted offline stand-in with a few topics. Nothing leaves the page. It uses Claude only when the page is opened inside Claude and the viewer consents |
| Judge scores in the evaluator | Recorded fixtures, not a live model call |
| Candidate, trail and golden set | Fictional |

## Principles

- AI advises, humans decide.
- Every score cites evidence that can be checked.
- No biometric or webcam signals.

## Next steps

- Replace the recorded judge with a live model call that returns structured scores, with the citation check run on its output.
- Build the golden set from real trails and track the metrics per release.
- Add reviewer calibration across several reviewers.

Unofficial demo. Fictional candidate and data. Nothing on these pages is stored or sent anywhere.
