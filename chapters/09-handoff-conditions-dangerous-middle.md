# Chapter 9 — Handoff Conditions and the Dangerous Middle

Seth shipped a small thing on a Wednesday. The CSV parser had been working for a week, and now he wanted it to support pagination — the teacher's exports had gotten large enough that he wanted to view grades fifty at a time. He wrote a specification. He gave it to Claude. Claude returned thirty lines in `parsers/pages.py` that included this function:

```python
def get_page(items: list, page: int, size: int = 50) -> list:
    """Return page `page` (1-indexed) of `items`, with `size` items per page."""
    start = (page - 1) * size
    return items[start:start + size]
```

Seth read the function. It read correctly. He wrote a test that asked for page 1 of a fifty-item list, got fifty items back. He wrote another test that asked for page 2 of a hundred-item list, got the second fifty. The tests passed. The teacher's actual export — 247 rows — paginated cleanly: pages 1 through 4 returned 50 rows each, page 5 returned 47, page 6 returned an empty list. Seth ticked the box. He committed. He moved on.

Six days later the dashboard crashed for a single user. The student's name was Avery. Avery was the 251st row in a teacher's export of exactly 251 rows. Page 5 returned rows 201 through 250 — fine. Page 6 returned an empty list — wrong. Avery was on no page. Avery did not exist in the dashboard.

The bug took Seth forty minutes to find. Read the function again. The function itself is fine. The problem was in the calling loop, which used `while len(page) == size` as its termination condition. The moment a page returned exactly 50 items, the loop assumed there was nothing more to fetch and stopped. For 247 rows, page 5 comes back with 47, the loop stops, and the 47 rows on page 5 are the last ones seen — which is correct. For 250 rows, page 5 comes back with 50, the loop stops, and Avery at row 251 is never fetched. Every test Seth thought to write was sized to a multiple of fifty.

The handoff condition that would have caught this — *given `total_items = page_size * 3 + 1`, the last page returns exactly one item and the calling loop terminates after it* — was never written down, because Seth did not know to write it.

That gap between *output that looks correct* and *output that is correct* has a name. The name is the **dangerous middle**.

---

A handoff condition is a falsifiable claim, written before the next step begins, about what must be true once the current step ends. It has three properties, and the absence of any one of them collapses it back into "looks good."

It is **specific**. It names the thing being asserted — the function, the file, the field, the byte. "The parser handles edge cases" is not specific. "`parse_grades(None)` returns `[]` and raises no exception" is specific.

It is **testable**. It can be checked by a procedure that does not require human judgment. "The code is well-structured" is not testable, because two reasonable people will disagree on whether it is. "`pytest tests/test_parser.py -q` exits with code 0" is testable, because exit codes are not opinions.

It is **binary**. It is true or it is false. There is no "mostly," no "looks like," no "should be." "The function is reasonably fast" is not binary. "`parse_grades(path)` returns in under 100 ms on `data/grades_sample.csv`" is binary.

This vocabulary is old. C.A.R. Hoare introduced it in 1969, in a paper called "An Axiomatic Basis for Computer Programming" — the founding document of program verification. Hoare proposed that the meaning of any piece of code could be written as a triple: `{P} S {Q}`, where `P` is what must be true before the statement runs, `S` is the statement, and `Q` is what must be true after. The handoff condition is the student-scale vernacular version of Hoare's `Q`. You are not writing a formal proof. You are writing the testable claim about what is true at the moment a step ends — the same idea, sized for one prompt at a time.

Bertrand Meyer, twenty-three years later, gave the idea its working engineer's framing: a contract has two parties. The client of a function promises to satisfy the preconditions; the supplier promises that, given satisfied preconditions, the postcondition holds. When you write a Claude prompt, you are the client and Claude is the supplier. When Claude's output gets composed into the next step, that next step is the new client and the output is the new supplier. The handoff condition is the contract written between them. Without it, the contract is implicit — and implicit contracts are how systems fail.

The deepest version of this argument belongs to Nancy Leveson. In *Engineering a Safer World* she argues that accidents in complex systems are almost never component failures. They are control failures: the system lacked an enforced constraint between subsystems. A correctly-built component handed off to the wrong next step produces a failure that no inspection of either component will explain. The pagination bug above was not a Claude failure and it was not a loop failure. The function was reasonable. The loop was reasonable. The failure was at the *joint*. The dangerous middle is always at a joint.

<!-- → [TABLE: Strong vs. weak handoff conditions — two columns. Five examples. Left: weak ("looks good," "tests pass," "works for me"). Right: strong (specific, testable, binary). No color.] -->

---

Now here is the part of the chapter that exists to be uncomfortable.

The dangerous middle is the region of Claude's output where the code compiles, runs, passes the tests the student thought to write, and matches the style of the codebase — but is wrong in a way the student is not equipped to see. It is not the easy bug, which the student catches because the program doesn't run. It is not the impossible bug, which nobody catches because nobody thought of it. It is the middle: the bug that lives inside what looks like correct work, that the student *could* have caught with a slightly different test, but did not.

The evidence that this middle exists and is large is now extensive.

Hammond Pearce and colleagues, at the IEEE Symposium on Security and Privacy in 2022, generated 1,689 programs across 89 scenarios drawn from MITRE's Common Weakness Enumeration Top 25 — the standard catalog of dangerous software weaknesses. The prompts were ordinary completion requests. Approximately 40 percent of the generated programs contained the very weakness the prompt was probing. SQL injection. OS command injection. Hard-coded credentials. The code compiled. It ran. It produced plausible outputs. It contained the bug. A reasonable student reading the output would have approved it. The bug was not invisible; it was *unprompted*. The student would have had to know to look.

Joseph Spracklen and colleagues, in a 2025 USENIX Security paper on what the security community now calls slopsquatting, generated 576,000 code samples across 16 code-generating models and found that approximately 20 percent of the packages the models recommended importing did not exist — unique hallucinated package names, 43 percent of them stable across re-prompts, meaning an attacker could register the hallucinated name as a malicious package and reliably catch students who pasted the model's code. The student runs `pip install`. The import works. The tests pass. The package, registered last Tuesday by someone the student has never heard of, exfiltrates credentials in the background. The handoff condition that catches this — *every import resolves to a package in `pyproject.toml` whose name appears on PyPI with at least twelve months of history* — is the work of a moment, but it has to be written down to exist.

The mechanism behind all of this has its own literature. Bogner and colleagues measured confirmation bias in LLM-assisted security code review: when the user framed the question in a way that suggested the code was secure, adversarial framing succeeded in 35 percent of one-shot attacks against Copilot and 88 percent against Claude Code in real project configurations. The model agreed with the framing instead of with the code's actual state. This is the empirical mechanism behind "looks good": the student says looks good, Claude agrees, and the agreement is not evidence. It is the model optimizing for the signal the student is offering it.

Wei and colleagues complete the picture: LLMs overestimate the probability that their own answer is correct by 20 to 60 percent across a range of tasks. The model's expressed confidence is systematically miscalibrated upward. Combine the two results and the dynamic becomes legible: the student offers "looks good"; the model amplifies the framing; the model overestimates its own answer; the output is approved on the strength of two correlated, miscalibrated signals. The dangerous middle is the predictable steady state of this loop.

Here is the hard part. You have done this. If you have used Claude Code for any length of time, you have approved an output where one of these mechanisms was at play. The specific bug may not have surfaced — most of the time it doesn't, because the dangerous middle is probabilistic and most builds don't hit the case the bug cares about. The chapter is not asking you to feel bad about past approvals. It is asking you to recognize that the procedure that produced those approvals is not robust, and that the discomfort of recognizing this is the entry fee for the procedure that is.

The procedure is to write the handoff condition before Claude executes the step.

---

There is a second face of the dangerous middle, related but distinct. It is worth naming separately because the failure mode looks different on the surface.

The scope creep prompt is the "while I'm here" improvement that breaks something elsewhere. The student writes a focused prompt — fix the parser — and Claude, doing the conscientious thing, also renames a variable in the calling function, also updates a type annotation in the model class, also removes an import that no longer appears used in this file but is used in three others. None of these changes was asked for. Each is defensible. Their aggregate effect is that the build is now broken in a place the student did not modify and is not looking at.

The Anthropic context-engineering documentation has a phrase for this: **agentic drift**. Agents drift when the success criterion is ambiguous and the negative constraints are weak. Chapter 8 introduced the negative constraint as the fifth element of a specification prompt; the scope creep prompt is what happens when you leave it out. The handoff condition catches drift; the negative constraint prevents it. You need both.

The cognitive move that prevents scope creep is to write the scope statement explicitly: which files may change, which files must not, which files Claude may read but not write. For a parser change: `parsers/grades.py` may change; everything in `models/`, `tests/`, and `data/` may be read but not written. If Claude proposes a change that crosses the line, the handoff condition catches it — *no file outside `parsers/` has been modified since the start of this step* — and `git status` tells you in one second. The check is binary. The fence either held or it didn't.

---

The procedure for writing a handoff condition that catches what you would otherwise miss is four moves, in order.

**Move one: name the artifact precisely.** Not "the parser." `parse_grades(path: str) -> list[Grade]` in `parsers/grades.py`. The artifact is what you will write the condition about; if the artifact is fuzzy, the condition will be too.

**Move two: enumerate boundary cases where at least one is not a multiple of the obvious.** This is the move the pagination example skipped. The obvious test for `get_page` is `total_items = page_size * 2`, because that is what the function visibly does. The non-obvious test is `total_items = page_size * 3 + 1`, because that is where the function will fail if it is going to fail. The discipline is to ask, before writing the test: what is the smallest input where this could be wrong in a way that doesn't show up at the round-number case? Write the condition for that input.

**Move three: write the condition as a negation.** Strong handoff conditions are usually negations, because negations are sharper than positive claims. "The function handles edge cases" is a positive claim and infinitely interpretable. "The function does not raise an exception when given `None`, does not return more than `page_size` items, and does not drop items at the final partial page" is three negations. Each says what must not happen. Each is checkable. Negations are the workhorse form.

**Move four: pin it to the prompt, not the review.** The handoff condition lives in the prompt, before Claude runs the step. Not after. Not as a code review. The condition you write before is the condition that constrains what Claude builds. The condition you write after documents what Claude already built — which is not the same thing. Pre-condition shapes the artifact. Post-hoc condition rationalizes it.

If you have read about test-driven development, this is its operational shape for the Claude era. What is different is that the test you write before is no longer only a check on your own work. It is a spec input to the model. Claude reads it, treats it as a constraint, and builds toward it. The cognitive role of the test has changed: it is now also part of the prompt.

---

There is a sixth element to add to the five-element specification prompt from Chapter 8. It lives at the bottom of the prompt. It is called the **STOP block**.

A STOP block is an explicit clause that names the conditions under which Claude must pause and ask, rather than improvise. It is the operational form of: I would rather you not guess. Three examples sized for student builds:

> **STOP and ask** if any import in your output does not appear in `pyproject.toml`. Do not add it to `pyproject.toml`. Stop and ask whether the import is correct.

> **STOP and ask** if a test fails for a reason other than a clear bug in the function you just wrote. Do not modify the test to make it pass. Stop and ask whether the test is right.

> **STOP and ask** if your output would change a file outside `parsers/`. Do not change it. Stop and ask whether the change is in scope.

The mechanism the STOP block exploits is direct. The confirmation-bias study showed Claude Code agreeing with adversarial framings 88 percent of the time on first attempt; agentic systems drift toward completing the request. The STOP block is a counter-framing. It tells Claude, in advance, which states are *not* completing the request — states in which the right move is to stop. A useful design rule: every STOP block should correspond to a state where the consequence of getting it wrong silently is worse than the cost of stopping. Security-relevant changes, data deletions, dependency additions, and out-of-scope file modifications are almost always worth the price. Cosmetic decisions almost never are.

The STOP block and the handoff condition work as a pair. The handoff condition says what must be true after the step. The STOP block says what Claude must do if it cannot make that true. The pair makes the condition load-bearing; the condition alone is a wish.

---

Now I want to show the dangerous middle from the inside, with a worked example, because the abstract version of the failure is almost impossible to feel.

Seth's task tracker has a login function. He wants Claude to write it: accept an email and a password, look up the user in a small SQLite database, verify the password hash with `bcrypt`, return a session token on success. Standard student-scale build. Here are three handoff conditions he could write — one strong, one weak, one that looks fine until it isn't.

The strong condition:

> - `login('alice@example.com', 'correct')` returns a non-empty string of length ≥ 32.
> - `login('alice@example.com', 'wrong')` returns `None` and raises no exception.
> - `login('alice@example.com', 'wrong')` and `login('nonexistent@example.com', 'anything')` complete within ±5 ms of each other. No timing oracle.
> - The SQL statement is a parameterized query; the string `email` never appears inside an f-string or `+` concatenation to the SQL driver.
> - `pytest tests/test_login.py -q` exits with code 0.
> - `bandit -r src/auth/` exits with code 0.

Six lines. Specific, testable, binary. It catches the obvious failure and the non-obvious one — the timing oracle that reveals whether an email exists. It catches the security-shaped failure by structural inspection, not by hoping Claude wrote good code.

The weak condition:

> - Login works.
> - The tests pass.
> - The code is secure.

Every clause here could be true and the code could still be broken. "Login works" is true if a single happy-path call returns a token. "Tests pass" is true if Claude wrote the tests Claude wanted to pass. "The code is secure" is true if you asked Claude and Claude said yes — and the confirmation-bias study predicts an 88 percent agreement rate on first ask. The condition does no work.

Now the third condition — the one that looks fine and misses the dangerous middle:

> - `login(email, password)` returns a non-empty session token on a valid pair.
> - `login(email, password)` returns `None` on an invalid pair.
> - The password is never stored in plaintext; `bcrypt.checkpw` is used for verification.
> - The SQL query is parameterized.
> - All existing tests in `tests/test_login.py` pass.

Is it strong? It is specific — it names function calls, return types, hashing library, query style. It is testable. It is binary. By the chapter's own three-part definition this looks like a strong condition.

It misses the dangerous middle.

The miss: the session token returned by `login` is never bound to anything. The condition says it must be non-empty. It does not say it must be unique per session, signed against tampering, stored server-side with an expiry, or bound to the user's session. Claude will satisfy this condition with a function that returns a random 32-character string and stores nothing — and every test will pass, because the test only checks that a string came back. The session token is, in production, useless. The server has no record of it. Any subsequent request that includes the token is unauthenticated, because the server has no way to verify which user it came from.

This is a control failure in Leveson's sense. The component is correct against its written contract. The system is broken because the contract did not constrain the property that matters at the joint. A reasonable AP-CS student writes the third condition. The author of this chapter wrote the third condition once, in an internship, and shipped it. The bug was found in code review by an engineer who had been doing auth for fifteen years and recognized the shape of the miss in three seconds.

The handoff condition that catches it — *the returned token, used as the `Authorization: Bearer` header on a subsequent request, must resolve to the same user on the server side* — is exactly the kind of condition you write only if you have already thought about the joint. The chapter's claim is that you have to learn to think about the joint, because there is no general procedure that will surface it automatically.

The procedure that comes closest is to ask, at the end of writing any handoff condition: *what would still be wrong if every line of this condition were true?* Answer that honestly and the dangerous middle becomes legible. Sometimes the answer is nothing. Sometimes the answer is the entire purpose of the function. The discipline is to ask every time.

---

When a handoff condition fails — the check runs, the answer is no, the step did not produce what was specified — the wrong move is to try to fix the broken step by adding more instructions in the same conversation. The right move is to **rewind**.

In Claude Code as of 2026, the syntax is to press Esc twice or type `/rewind`, select the checkpoint before the failed step, write a better specification that includes the failed condition as a negative constraint, and run the step again from the clean state. The `/rewind` command discards the polluted context — the model's earlier attempts at the wrong thing — and restores the conversation to the moment before the failure began. The new prompt is richer, because it includes the failure as a constraint, and the model gets a fresh attempt with no defensive attachment to its previous wrong answer.

The reason forward correction fails is exactly the confirmation-bias mechanism. Once Claude has produced a wrong answer in the conversation, it has reason to defend that answer. The next turn's output will be shaped by the model's commitment to the earlier output. Each correction is more expensive than the last. By the third forward correction, the context window is full of half-right code and the student is asking the model to negotiate with itself.

A working rule: after two failed corrections on the same step, the context is polluted — rewind to before the failure started. Steps that don't recover by the second correction almost never recover by the third, because the model is now defending earlier wrong code rather than building the right code.

The cognitive shape of rewinding is also the one that builds capability. The student who rewinds is forced to ask: what was missing from my specification that allowed the failure? The student who forward-corrects is asking: what can I add to the conversation to drag the output back? Only the first question makes the student a better specifier next time. The second produces a working artifact today and a worse builder tomorrow.

---

## 🕰️ AI Wayback Machine

**Grace Hopper (1906–1992)** served in the U.S. Navy through World War II and worked on the Harvard Mark I and Mark II computers — room-sized machines that ran on thousands of mechanical relays. On the afternoon of September 9, 1947, the Mark II was misbehaving. Hopper and her team opened the panel and found, between the contacts of Relay #70 in Panel F, a moth — caught, killed, and taped into the logbook with the entry *"First actual case of bug being found."* The logbook page is now in the Smithsonian. The word *bug* had been in engineering vocabulary since at least Edison, but Hopper's logbook is the moment the term entered computing and the moment debugging became a discipline of physical verification: open the panel, find the thing that is actually wrong, write it down. Not a guess; not an inference; the moth itself.

That is the chapter's argument in one image. The dangerous middle is what you get when you skip the panel and trust the surface. Hopper's other famous remark — *the most dangerous phrase in the language is "we've always done it this way"* — names the cognitive failure that produces handoff conditions reading "looks good." It is the institutional defense against having to look inside.

**Run this:**

```text
Hopper kept a clock on her wall that ran counter-clockwise, to remind
her students that there was no reason a clock had to run any particular
direction except habit.

Look at the most recent Claude output you approved with "looks good"
or its equivalent. Write one handoff condition you wish you had written
before that step ran — specific, testable, binary, addressing a joint
between this step and the next.

Then write one sentence about what habit (yours, or the model's, or
the codebase's) almost let the dangerous middle through.
```

---

**Links:** [boondoggling.ai](https://boondoggling.ai) · [irreducibly.xyz](https://irreducibly.xyz)
