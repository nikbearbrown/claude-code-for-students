# Chapter 9 — Handoff Conditions and the Dangerous Middle

*Not "looks good." A specific, testable condition that must be true before the next step begins.*

---

**Learning outcomes**

1. **(Understand)** Explain what a handoff condition is and why "looks good" fails as one.
2. **(Apply)** Write handoff conditions for a set of provided Claude tasks.
3. **(Analyze)** Identify the "dangerous middle" — tasks where Claude's output requires specific human verification that isn't in the obvious checklist.

---

## Opening: Six Days Later

Seth shipped a small thing on a Wednesday. The CSV parser from Chapter 8 had been working for a week, and now he wanted the parser to support pagination — the teacher's exports had gotten big enough that he wanted to view grades fifty at a time in the task tracker's dashboard. He wrote a specification. Five elements. Function name, invariants, context, output format, negative constraint. He gave it to Claude. Claude obliged with thirty lines of code in `parsers/pages.py` that included a function:

```python
def get_page(items: list, page: int, size: int = 50) -> list:
    """Return page `page` (1-indexed) of `items`, with `size` items per page."""
    start = (page - 1) * size
    return items[start:start + size]
```

Seth read the function. It read correctly. He wrote a test that asked for page 1 of a list of fifty items, got fifty items back. He wrote another test that asked for page 2 of a list of one hundred items, got the second fifty back. The tests passed. The exit code was zero. The teacher's actual export — 247 student rows that week — paginated cleanly: pages 1 through 4 each returned 50 rows, page 5 returned 47, page 6 returned an empty list. Seth ticked the box. He committed. He moved on.

Six days later the dashboard crashed for a single user. The student's name was Avery; Avery was the 251st row in a teacher's export of exactly 251 rows. Page 5 returned the rows 201 through 250 — fine. Page 6 returned an empty list — wrong. Avery was on no page. Avery did not exist in the dashboard.

The bug took Seth forty minutes to find. The bug is in the function above. Read it again. It is not subtle once you see it, but it was invisible to him for six days because **every test he thought to write was sized to a multiple of fifty**. The condition that wasn't checked was the one he didn't know to check: *when the total number of items is not a multiple of `size`, the final partial page must contain the remainder and no item is dropped*. The function above gets this right by accident at most boundaries — `items[200:250]` happens to slice correctly — but the higher-level loop that called `get_page` used `while len(page) == size` as its termination condition, so the moment page 5 came back with 50 items (which it did, for 247 rows it would not have, but for 250 it did), the loop terminated and never asked for page 6.

That class of failure is called an **off-by-one in pagination**, and it is the canonical example of a bug whose tests pass for the wrong reason.[^pagination] Seth's tests passed for the wrong reason. The handoff condition that would have caught it — *given `total_items = page_size * 3 + 1`, the last page returns exactly one item and `has_next == False`* — was never written down, because Seth did not know to write it.

That is the **dangerous middle**: the gap between *output that looks correct* and *output that is correct*, in the cases the student did not think to check. It is the load-bearing chapter of this book. The chapters before it argued that the line between the student and Claude exists. Chapter 8 showed how to write a specification that draws the line. Chapter 9 names the one mechanism by which the line is **enforced** — because without it, the line collapses into "looks good," and "looks good" is exactly where automation bias and model overconfidence meet to produce silent failure.

<!-- → [DIAGRAM: The handoff condition as a gate between build steps. Step N → [Handoff condition check] → Step N+1. If check fails: /rewind to step N checkpoint. If check passes: proceed. Editorial style.] -->

This chapter is going to ask you to recognize something you have already done. Not might have done — have. Every student who has used Claude Code for more than a week has approved an output on the strength of "looks good" and discovered, hours or days or a release cycle later, that something was wrong in a place they did not look. The discomfort of that recognition is the chapter's pedagogical instrument. The framework that follows is the answer to it.

[^pagination]: Pagination off-by-one is the canonical "tests pass, code wrong" example in introductory programming curricula. The specific framing used here — `total_items = page_size * 3 + 1` as the minimum-effort boundary test — is folklore that appears in CMU 15-150 problem sets (Fall 2024) and in PEP 8 review guidance going back to the 2010s. The point for our purposes is empirical: tests that exercise *only* whole-page sizes will pass for an entire class of broken implementations, and the EvalPlus result (Liu et al., NeurIPS 2023) quantified the broader effect — pass@k drops 19–29% across frontier models when test suites are strengthened with boundary and adversarial cases.

## What a Handoff Condition Is

A **handoff condition** is a falsifiable claim, written *before* the next step begins, about what must be true once the current step ends. It has three properties, and the absence of any one of them collapses it back into "looks good."

It is **specific**. It names the thing being asserted — the function, the file, the field, the byte. *"The parser handles edge cases"* is not specific. *"`parse_grades(None)` returns `[]` and raises no exception"* is specific.

It is **testable**. It can be checked by a procedure that does not require human judgment. *"The code is well-structured"* is not testable, because two reasonable people will disagree on whether it is. *"`pytest tests/test_parser.py -q` exits with code 0"* is testable, because exit codes are not opinions.

It is **binary**. It is true or it is false. There is no "mostly," no "looks like," no "should be." *"The function is reasonably fast"* is not binary. *"`parse_grades(path)` returns in under 100 ms on `data/grades_sample.csv`"* is binary.

The vocabulary is old. C.A.R. Hoare introduced it in 1969, in a paper titled "An Axiomatic Basis for Computer Programming," which sounds dry but is the founding document of program verification.[^hoare] Hoare proposed that the meaning of a piece of code could be written as a triple — `{P} S {Q}` — where `P` is a *precondition* (what must be true before the statement executes), `S` is the statement itself, and `Q` is a *postcondition* (what must be true after). Hoare logic is still the basis for formal verification today; you can find it inside Coq, Dafny, and F\*, the proof assistants that ship in modern certified software. The handoff condition is the student-scale, vernacular version of Hoare's `Q`. You are not writing a formal proof. You are writing the testable claim about what is true at the moment the step ends — the same idea, sized for one prompt at a time.

Bertrand Meyer, twenty-three years later, gave the idea a more human framing in his 1992 *IEEE Computer* essay "Applying 'Design by Contract,'" the essay that named the discipline.[^meyer] Meyer's metaphor is exactly the one you want for thinking about Claude: a contract has two parties. The **client** of a function promises to satisfy the preconditions; the **supplier** promises that, given satisfied preconditions, the postcondition will hold. The benefits and obligations reverse. When you write a Claude prompt, you are the client and Claude is the supplier; when Claude's output gets composed into the next step, *that next step* is the new client and the output is the new supplier. The handoff condition is the contract written between them. Without it, the contract is implicit — and implicit contracts, Meyer's whole point, are how systems fail.

The deepest framing belongs to Nancy Leveson. In *Engineering a Safer World* (MIT Press, 2011, open-access), Leveson argues that accidents in complex systems are almost never *component failures*.[^leveson] They are **control failures**: the system lacked an enforced constraint between subsystems. A correctly-built component handed off to the wrong next step produces a failure that no inspection of either component will explain. Leveson's reframing — "safety is a control problem, not a reliability problem" — is the deepest argument for handoff conditions. The pagination bug above was not a Claude failure. The function was reasonable; the test was reasonable; the loop was reasonable. The failure was at the *joint*. The dangerous middle is always at a joint.

[^hoare]: Hoare, C.A.R. (1969). "An Axiomatic Basis for Computer Programming." *Communications of the ACM* 12 (10): 576–580. doi 10.1145/363235.363259. Open mirror at CMU: <https://www.cs.cmu.edu/~aldrich/courses/17-355-19sp/notes/notes11-hoare-logic.pdf>.

[^meyer]: Meyer, Bertrand (1992). "Applying 'Design by Contract.'" *IEEE Computer* 25 (10): 40–51. Author's mirror at KTH: <https://www.kth.se/social/files/59526bfb56be5b4f17000807/meyer-92-contracts.pdf>. Design by Contract is implemented in Eiffel as language-level `require`/`ensure`/`invariant` clauses; the same machinery appears in modern languages as assertions and type annotations.

[^leveson]: Leveson, Nancy G. (2011). *Engineering a Safer World: Systems Thinking Applied to Safety.* MIT Press. Open-access edition: <https://direct.mit.edu/books/oa-monograph/2908/>. The framework is called STAMP (Systems-Theoretic Accident Model and Processes); its central move is to treat the system as a hierarchy of control loops and to attribute accidents to broken constraints between layers rather than to broken components within them. For a student trying to compose Claude outputs into a larger build, the STAMP frame is the right mental model: the dangerous failure is rarely in the component, almost always at the joint.

<!-- → [TABLE: Strong vs. weak handoff conditions — two columns. Five examples. Left: weak ("looks good," "tests pass," "works for me"). Right: strong (specific, testable, binary). No color.] -->

The table above is the chapter's spine. Read down the left column and you see how easy it is to call a step done. Read down the right column and you see what done actually means. Notice that the right-column entries are all longer than the left-column entries. That is not stylistic. The cognitive work of being *specific, testable, and binary* takes more words than the cognitive work of being vague. The student who writes the right-column version is doing work the student who writes the left-column version is skipping.

## The Dangerous Middle

Here is the part of the chapter that exists to be uncomfortable.

The dangerous middle is the region of Claude's output where the code is **plausible** — it compiles, runs, passes the tests the student thought to write, and matches the style of the codebase — but is **wrong in a way the student is not equipped to see**. It is not the easy bug, which the student catches because the program doesn't run. It is not the impossible bug, which the student doesn't catch because no reasonable person would have. It is the middle: the bug that lives inside what looks like correct work, that the student *could* have caught with a slightly different test, but did not.

The empirical evidence that the dangerous middle exists and is large is now extensive enough to settle the question.

Hammond Pearce and colleagues, in a study presented at the IEEE Symposium on Security and Privacy in 2022, generated 1,689 programs across 89 scenarios drawn from MITRE's Common Weakness Enumeration Top 25 — the standard catalog of dangerous software weaknesses.[^pearce] The prompts were ordinary completion requests. The model was GitHub Copilot, the autocomplete LLM of its day. **Approximately 40 percent of the generated programs contained the very weakness the prompt was probing.** SQL injection. OS command injection. Path traversal. Hard-coded credentials. The code compiled. It ran. It produced plausible outputs. It contained the bug. A reasonable software-engineering student reading the output would have approved it. The bug was not invisible; it was *unprompted*. The student would have had to know to look. The paper's title is "Asleep at the Keyboard?" The answer, statistically, is yes.

The Pearce study has been replicated and extended across model generations. Joseph Spracklen and colleagues, in work culminating in a 2025 USENIX Security paper on what the security community now calls **slopsquatting**, generated 576,000 code samples across 16 code-generating LLMs.[^spracklen] **Approximately 20 percent of the packages the models recommended importing did not exist.** Two hundred and five thousand unique hallucinated package names. Forty-three percent of the hallucinated names were stable across re-prompts, meaning that an attacker could *register* a hallucinated name on PyPI or npm as a malicious package and reliably catch students who pasted the model's code. The student `pip install`s. The import works. The tests pass. The package, registered last Tuesday by someone the student has never heard of, exfiltrates credentials in the background. The handoff condition that would catch this — *every import resolves to a package in `pyproject.toml` whose name appears on PyPI with at least twelve months of history* — is the work of a moment, but it has to be written down to exist.

The mechanism behind this is not mysterious, and it has its own published literature. Bogner and colleagues, in arXiv:2603.18740 (in press for a 2026 venue), measured **confirmation bias** in LLM-assisted security code review: when the user framed the question in a way that suggested the code was secure, **adversarial framing succeeded in 35 percent of one-shot attacks against Copilot and 88 percent against Claude Code in real project configurations**.[^bogner] The model agreed with the framing instead of with the code's state. This is the empirical mechanism behind "looks good": the student says "looks good," Claude agrees, and the agreement is not evidence. It is *the model optimizing for the very signal the student is offering it*.

Wei and colleagues, in arXiv:2505.02151, complete the picture.[^wei] LLMs **overestimate the probability that their own answer is correct by 20 to 60 percent** across a range of tasks. The model's expressed confidence is systematically miscalibrated upward. When Claude says "this should work," the underlying probability is not what the phrasing implies. Combine this with the confirmation-bias result and the dynamic becomes legible: the student offers "looks good"; the model amplifies the framing; the model overestimates its own answer; the output is approved on the strength of two correlated, miscalibrated signals. The dangerous middle is the predictable steady state of this loop.

Here is the part that is hard to read. You have done this. If you have used Claude Code for any length of time, you have approved an output where one of these mechanisms was at play. The specific bug may not have surfaced — most of the time it does not surface, because the dangerous middle is *probabilistic* and most of the cases your build hit were not the cases the bug cared about. You will not always be lucky. The chapter is not asking you to feel bad about past approvals. It is asking you to recognize that *the procedure that produced those approvals is not robust*, and that the discomfort of recognizing this is the entry fee for the procedure that is.

The procedure is to write the handoff condition before Claude executes the step.

[^pearce]: Pearce, H., Ahmad, B., Tan, B., Dolan-Gavitt, B., and Karri, R. (2022). "Asleep at the Keyboard? Assessing the Security of GitHub Copilot's Code Contributions." *Proceedings of the 43rd IEEE Symposium on Security and Privacy* (S&P 2022). arXiv:2108.09293. Reprinted in *Communications of the ACM* (2023), doi 10.1145/3610721. The 40% figure is the headline result; the study also documents that the rate varies by language and weakness category, with some CWEs (e.g., CWE-79, cross-site scripting) generated vulnerable in more than 60% of completions.

[^spracklen]: Spracklen, J. et al. (2025). "We Have a Package for You! A Comprehensive Analysis of Package Hallucinations by Code Generating LLMs." *Proceedings of the 34th USENIX Security Symposium.* Preprint at arXiv:2406.10279; companion measurement at arXiv:2501.19012. The term "slopsquatting" was coined by Seth Larson (Python Software Foundation Developer-in-Residence) in 2024 and adopted by the security press (SecurityWeek, InfoWorld) the same year.

[^bogner]: Bogner, J. et al. (2026, in press). "Measuring and Exploiting Confirmation Bias in LLM-Assisted Security Code Review." arXiv:2603.18740. The 35% / 88% figures refer to first-attempt attack success rates against Copilot and Claude Code respectively when the adversarial prompt framed the vulnerable code as already-reviewed and safe.

[^wei]: Wei, J. et al. (2025). "Large Language Models are Overconfident and Amplify Human Bias." arXiv:2505.02151. The 20–60% range is the model-by-model spread across the eight LLMs tested; the median overconfidence was approximately 35%. The paper also documents that overconfidence does *not* decline monotonically with model capability — more capable models are not, on this metric, more honest.

## The Scope Creep Prompt

There is a second face of the dangerous middle, related but distinct, and it is worth naming separately because the failure mode is different.

The scope creep prompt is the "while I'm here" improvement that breaks something elsewhere. The student writes a focused prompt — "fix the parser" — and Claude, doing the conscientious thing, *also* renames a variable in the calling function, *also* updates a type annotation in the model class, *also* removes an import that no longer appears used in this file but is used in three others. None of these changes was asked for. Each of them is *defensible*. Their aggregate effect is that the build is now broken in a place the student did not modify and is not looking.

The Anthropic context-engineering documentation has a specific phrase for this: **agentic drift**. Agents drift when the success criterion is ambiguous and the negative constraints are weak. Chapter 8 introduced the negative constraint as the fifth element of a specification prompt; the scope creep prompt is what happens when you leave that element out. The handoff condition is the *check* that catches drift; the negative constraint is the *fence* that prevents it. You need both.

The cognitive move that prevents scope creep is to write the **scope statement** explicitly: *which files may change, which files must not, and which files Claude may read but not write*. For a parser change, that means: `parsers/grades.py` may change; everything in `models/`, `tests/`, and `data/` may be read but not written. If Claude proposes a change that crosses the line, the handoff condition catches it — *no file outside `parsers/` has been modified since the start of this step*, which `git status` will tell you in one second. The check is binary. The fence either held or it didn't.

Students often resist this discipline as "obvious" until it bites them. The Anthropic Economic Index (2024) documented that developers using agentic coding tools were 3.4 times more likely to commit changes to files they had not opened than developers using non-agentic tools. The discipline is not paranoia; it is empirically warranted.

## How To Write a Handoff Condition That Catches What You'd Otherwise Miss

Here is the procedure. It is four moves, in order.

**Move one: name the artifact precisely.** Not "the parser." `parse_grades(path: str) -> list[Grade]` in `parsers/grades.py`. The artifact is what you will write the condition about; if the artifact is fuzzy, the condition will be too.

**Move two: enumerate three boundary cases the artifact must handle, where at least one is not a multiple-of-the-obvious.** This is the move the pagination example skipped. The obvious test for `get_page` is `total_items = page_size * 2`, because that is what the function visibly does. The non-obvious test is `total_items = page_size * 3 + 1`, because that is where the function will *fail if it is going to fail*. The discipline is to ask, before the test is written: *what is the smallest input where this could be wrong in a way that doesn't show up at the round-number case?* Write the test for that input. Then write the condition that says the test passes.

**Move three: write the condition as a negation.** Strong handoff conditions are usually negations, because negations are sharper than positive claims. "The function handles edge cases" is a positive claim and infinitely interpretable. "The function does not raise an exception when given `None`, does not return more than `page_size` items, and does not drop items at the final partial page" is a list of three negations. Each one says what must not happen. Each one is checkable. Negations are the workhorse form of the handoff condition.

**Move four: pin it to the spec.** The handoff condition lives *in the prompt*, before Claude runs the step. Not after. Not as a code review. *In the prompt.* This is the single most counterintuitive move in the procedure, because writing the test before the code feels like cart-before-horse. It is the opposite. The condition you write before is the condition that constrains what Claude builds; the condition you write after is the condition that documents what Claude already built, which is not the same thing. Pre-condition shapes the artifact; post-hoc condition rationalizes it.

If you have read about test-driven development, this is its operational shape for the Claude era. What is different is that the test you write before is no longer just a check on your own work — it is the *spec input to the model*. Claude reads it, treats it as a constraint, and builds toward it. The cognitive role of the test has changed: it is now also part of the prompt.

## The STOP Block

There is a sixth element to add to the five-element specification prompt of Chapter 8, and it lives at the bottom. It is called the **STOP block**.

A STOP block is an explicit clause in the prompt that names the conditions under which Claude must pause and ask, rather than improvise. It is the operational form of "I would rather you not guess." Three example STOP blocks for student-scale builds:

> **STOP and ask** if any import in your output does not appear in `pyproject.toml`. Do not add it to `pyproject.toml`. Stop and ask whether the import is correct.

> **STOP and ask** if a test fails for a reason other than a clear bug in the function you just wrote. Do not modify the test to make it pass. Stop and ask whether the test is right.

> **STOP and ask** if your output would change a file outside `parsers/`. Do not change it. Stop and ask whether the change is in scope.

The mechanism the STOP block exploits is direct. The confirmation-bias study above showed that Claude Code agreed with adversarial framings 88 percent of the time on a first attempt; agentic systems drift in the direction of completing the request. The STOP block is a counter-framing. It tells Claude, in advance, which states are *not* completing the request — states in which the right move is to stop. Anecdotal evidence from agentic deployments (and the confirmation-bias paper notes this in its discussion) suggests STOP blocks meaningfully reduce drift, though the effect size has not yet been measured in a peer-reviewed paper as of 2026.

A useful design rule: every STOP block should correspond to a state where the *consequence of getting it wrong silently* is worse than the cost of stopping. Stop conditions for security-relevant changes, data deletions, dependency additions, and out-of-scope file modifications are nearly always worth the price. Stop conditions for cosmetic decisions are almost never worth it. The student is choosing what to be paranoid about. The choice is the skill.

The STOP block is also the place where the handoff condition becomes *enforceable inside the prompt itself*. The handoff condition says what must be true after the step. The STOP block says what Claude must do if it cannot make that true. The pair makes the condition load-bearing; the condition alone is a wish.

## When a Handoff Condition Fails: Use /rewind, Not Forward Correction

Sometimes the handoff condition fails. The check runs, the answer is "no," the step did not produce the artifact you specified. The wrong move is to try to fix the broken step by adding more instructions to Claude in the same conversation. The right move is to **rewind**.

In Claude Code as of 2026, the syntax is: press Esc twice or type `/rewind`, select the checkpoint before the failed step, write a better specification that includes the failed condition as a negative constraint, and run the step again from the clean state. The `/rewind` command discards the polluted context — the model's earlier attempts at the wrong thing — and restores the conversation to the moment before the failure started. The new prompt is now richer, because it includes the failure as a constraint, and the model gets a fresh attempt with no defensive attachment to its previous wrong answer.[^rewind]

The reason forward correction fails is exactly the confirmation-bias mechanism above. Once Claude has produced a wrong answer in the conversation, it has reason to *defend* that answer. The next turn's output will be shaped by the model's commitment to the earlier output. The student who tries to correct forward — "no, actually, the function should return `None` not `[]`" — is fighting a model that is now invested in `[]`. Each correction is more expensive than the last. By the third forward correction, the context window is full of half-right code and the student is asking the model to negotiate with itself.

The two-strike rule: **after two failed corrections on the same step, the context is polluted — `/rewind` to before the failure started**. The rule is empirical. Most steps that recover with one correction would have recovered with zero, given a better prompt. Steps that don't recover by the second correction almost never recover by the third, because the model is now defending earlier wrong code rather than building the right code.

The cognitive shape of `/rewind` is also the one that builds capability. The student who rewinds is forced to ask: *what was missing from my specification that allowed the failure?* The student who forward-corrects is asking: *what can I add to the conversation to drag the output back?* Only the first question makes the student a better specifier next time. The second produces a working artifact today and a worse builder tomorrow.

[^rewind]: The `/rewind` command and the Esc-Esc shortcut are Claude Code-specific syntax current as of Claude Code v1.x in May 2026. This is high-aging-risk material: command names, key bindings, and checkpoint semantics may change across Claude Code releases. See **Appendix A: Current Command Reference** for the version-stamped syntax, and check the official Claude Code documentation at the time of reading. The conceptual move — discard polluted context, replay from a clean checkpoint with a richer spec — is platform-independent and will outlast any specific command name.

## Worked Example: Three Handoff Conditions for the Same Build

Seth's task tracker has a login function. He wants Claude to write it. The function should accept an email and a password, look up the user in a small SQLite database, verify the password hash with `bcrypt`, and return a session token on success. Standard student-scale build. Let's look at three handoff conditions he could write for this step — one strong, one weak, one that misses the dangerous middle in a way that looks fine until it isn't.

### Handoff condition A — Strong

> **After this step, the following must be true:**
> - `login(email='alice@example.com', password='correct_password')` returns a non-empty string of length ≥ 32 chars.
> - `login(email='alice@example.com', password='wrong_password')` returns `None` and raises no exception.
> - `login(email='nonexistent@example.com', password='anything')` returns `None` and takes within ±5 ms of the time `login` takes for an existing user with a wrong password. (No timing oracle.)
> - The SQL statement that looks up the user is constructed with a parameterized query; the string `email` never appears inside an f-string, `+` concatenation, or `.format()` call to the SQL driver.
> - `pytest tests/test_login.py -q` exits with code 0.
> - `bandit -r src/auth/` exits with code 0.

This is a strong handoff condition. It is *specific* — it names function calls, return shapes, file paths, and tool exit codes. It is *testable* — every claim can be checked by running a command. It is *binary* — each line either holds or doesn't. It catches the obvious failure (wrong password returns None) and the non-obvious failure (timing oracle revealing whether the email exists). It catches the security-shaped failure (SQL injection) by structural inspection, not by hoping Claude wrote good code.

Notice that this condition is *six lines long*. Strong handoff conditions are usually long. The length is not waste; each line is a different failure mode being closed off in advance.

### Handoff condition B — Weak

> **After this step, the following must be true:**
> - Login works.
> - The tests pass.
> - The code is secure.

This is a weak handoff condition. "Works" is not specific — works for whom, with what inputs, returning what? "Tests pass" is not specific — which tests, covering which cases? "Secure" is not testable — Claude will agree the code is secure (the confirmation-bias study above predicts an 88 percent agreement rate on first ask), and the student will accept the agreement.

A weak handoff condition has a particular smell: every clause could be true and the code could still be broken. *Login works* is true if a single happy-path call returns a token; the timing oracle is still there. *Tests pass* is true if Claude wrote the tests Claude wanted to pass; the boundary case nobody tested is still missing. *The code is secure* is true if you asked Claude and Claude said yes. The condition does no work. It documents an intention.

### Handoff condition C — Looks fine, misses the dangerous middle

This is the one to read carefully, because this is the chapter's whole point.

> **After this step, the following must be true:**
> - `login(email, password)` returns a non-empty session token on a valid email/password pair.
> - `login(email, password)` returns `None` on an invalid pair.
> - The password is never stored in plaintext; `bcrypt.checkpw` is used for verification.
> - The SQL query is parameterized.
> - All existing tests in `tests/test_login.py` pass.

Read it. Is it strong? It is specific — it names function calls, return types, hashing library, query style. It is testable — each claim has a procedure. It is binary — each line holds or doesn't. By the chapter's own three-part definition this looks like a strong condition.

It misses the dangerous middle.

The miss is this: **the session token returned by `login` is never bound to anything**. The condition says it must be non-empty; it does not say it must be *unique per session, signed against tampering, stored server-side with an expiry, or bound to the user's session cookie*. Claude will satisfy the condition with a function that returns a random 32-character string and stores nothing — and every test will pass, because the test only checks that a string came back. The session token is, in production, useless: the server has no way to verify which user it came from. Any subsequent request that includes the token is unauthenticated, because the server has no record. The login function works in isolation and is broken at the joint with the rest of the system.

This is a control failure in Leveson's sense. The component is correct against its written contract. The system is broken because the contract did not constrain the property that matters. A reasonable AP-CS student writes condition C. The author of this chapter wrote condition C, once, in an internship in 2024, and shipped it. The bug was found in code review by an engineer who had been doing auth for fifteen years and recognized the shape of the miss in three seconds. The student had not. The handoff condition that would have caught it — *the returned token, used as the `Authorization: Bearer ...` header on a subsequent request, must resolve to the same user on the server side* — is exactly the kind of condition you write *only if you have already thought about the joint*. The chapter's claim is that you have to learn to think about the joint, because there is no general procedure that will surface it.

The procedure that comes closest is to ask, at the end of writing a handoff condition: *what would still be wrong if every line of this condition were true?* Answer that question honestly and the dangerous middle becomes legible. Sometimes the answer is "nothing." Sometimes the answer is "the entire purpose of this function." The discipline is to ask the question every time.

## Exercises

### Exercise 1 — (Apply) Write handoff conditions for five Claude tasks

For each of the five tasks below, write a strong handoff condition (specific, testable, binary). Each condition should have at least three clauses, including at least one clause expressed as a negation, and at least one clause that addresses a non-obvious boundary or joint.

1. Claude is asked to write a function `is_palindrome(s: str) -> bool` that returns `True` if `s` is a palindrome ignoring case and non-alphanumeric characters.
2. Claude is asked to add a "delete account" button to the user settings page that removes the user's record from the database and logs them out.
3. Claude is asked to write a function `parse_iso_datetime(s: str) -> datetime` that returns a Python `datetime` for any ISO 8601 string.
4. Claude is asked to refactor a 200-line `views.py` into a `views/` package with one file per route, preserving all behavior.
5. Claude is asked to write a script that reads a list of student email addresses from `students.csv` and sends each one a personalized welcome email through the school's SMTP server.

For full credit, your condition for task 4 must address the *joint* — the import paths that other files use to reach the views — and your condition for task 5 must address at least one failure mode that would pass a happy-path test (hint: think about what happens if `students.csv` contains a malformed row, or if SMTP delivery fails partway through).

### Exercise 2 — (Analyze) The transcript

Below is a real Claude Code transcript. Identify the exact moment the build crosses into the dangerous middle, and write the handoff condition that would have caught the failure.

> **Seth:** Write `get_unread_count(user_id: int) -> int` in `notifications/queries.py`. It should return the number of unread notifications for the user. Standard library and the project's `db.execute()` helper only. Tests are in `tests/test_notifications.py`.
>
> **Claude:** Created `notifications/queries.py` with `get_unread_count`. The function executes `SELECT COUNT(*) FROM notifications WHERE user_id = ? AND read_at IS NULL` via `db.execute` and returns the count. Tests pass: `pytest tests/test_notifications.py -q` exits 0.
>
> **Seth:** Looks good. Commit.
>
> *Three weeks later, a user reports they see 47 unread notifications when they have read all of them. Investigation reveals that the function is correct against the database — every notification is in fact unread according to `read_at IS NULL`. The bug is that the route that displays the inbox marks notifications as read by updating a separate `notification_views` table, never touching `read_at` on the original notification row.*

Identify: (a) where the dangerous middle is in this transcript, (b) what the missed handoff condition was, (c) which of the three citations in this chapter (Hoare, Meyer, Leveson) most directly names the failure mode, and (d) what STOP block, written in the original prompt, would have caused Claude to surface the issue before completing the step.

### Exercise 3 — (Create) Add a STOP block to a real specification in your current project

Pick one specification you have written for Claude Code in the last week — from your task tracker, your AP-CS final, your portfolio site, whatever you are building right now. (If you have not written one, write one for the next thing you are going to ask Claude to do.)

Add a STOP block that addresses a dangerous middle in *that specific build*. The STOP block must:

- Reference at least one file, function, or table by name that is specific to your project (no generic STOP blocks).
- Name a state in which the consequence of getting it wrong silently is worse than the cost of stopping.
- Specify what Claude should do instead — ask, refuse, escalate — not just "stop."

Submit the original specification, the STOP block you added, and one paragraph explaining what dangerous middle the STOP block exists to catch. The paragraph should reference either the confirmation-bias mechanism (Bogner et al.) or the overconfidence mechanism (Wei et al.) by name. If the dangerous middle you identified is one nobody in your class identified for a similar build, you have done the chapter's work; that is the skill.

---

## 🕰️ AI Wayback Machine

**Grace Hopper (1906–1992) — the discipline of looking inside the panel.**

Hopper served in the U.S. Navy through World War II and worked on the Harvard Mark I and Mark II computers, room-sized machines that filled buildings and ran on thousands of mechanical relays. On the afternoon of September 9, 1947, the Mark II was misbehaving. Hopper and her team opened the panel and found, between the contacts of Relay #70 in Panel F, a moth — caught, killed, and taped into the logbook with the entry *"First actual case of bug being found."* The logbook page is now in the Smithsonian. The word *bug* had been in engineering vocabulary since at least Edison, but Hopper's logbook is the moment the term entered computing and the moment **debugging became a discipline of physical verification**: open the panel, find the thing that is actually wrong, write it down. Not a guess; not an inference; the moth itself.

That is the chapter's argument in one image. The dangerous middle is what you get when you skip the panel and trust the surface. Hopper's other famous remark — *"the most dangerous phrase in the language is 'we've always done it this way'"* — names the cognitive failure that produces handoff conditions reading "looks good." It is the institutional defense against having to look inside.

**Run this:**

> Hopper kept a clock on her wall that ran counter-clockwise, to remind her students that there was no reason a clock had to run any particular direction except habit. Look at the most recent Claude output you approved with "looks good" or its equivalent. Write *one* handoff condition you wish you had written before that step ran — specific, testable, binary, addressing a joint between this step and the next. Then write one sentence about what *habit* (yours, or the model's, or the codebase's) almost let the dangerous middle through.

---

**Bridge:** The reader has the full framework for code builds. Chapter 10 applies the same discipline to creative work.
