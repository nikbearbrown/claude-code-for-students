# Chapter 5 — The Five Supervisory Capacities

*These are the five things you do that Claude cannot. Name them. Practice them. Never delegate them.*

---

**Learning outcomes**

1. **(Remember)** Name and define the five supervisory capacities.
2. **(Apply)** Identify which supervisory capacity is being exercised at each step of a provided build sequence.
3. **(Analyze)** Diagnose a build that went wrong by identifying which supervisory capacity was absent.

---

Seth is forty minutes into a build and the code in front of him compiles.

It is a Friday in late April. The build is the short-answer quiz grader for his AP Biology study site — a Python function that takes a student's typed answer and the answer key and returns a score between zero and one. He has been working with Claude for half an hour. He has a prompt. He has a function. He has a test file with twelve example pairs, and on those twelve example pairs the function returns 85% accuracy. The tests pass. The code is clean. The function is forty lines long and uses `difflib.SequenceMatcher` with a 0.7 ratio threshold, which Claude has explained in a comment Seth has read twice.

He almost ships it.

He doesn't ship it, but the reason he doesn't ship it is the chapter you are reading. Something is *off*. He cannot, in the moment, point at the line that is wrong. The function compiles. The tests pass. The variable names are reasonable. The threshold of 0.7 has a comment next to it that says *empirically chosen — tunable*. And still: something. A small, specific, hard-to-articulate wrongness, located somewhere between his eye and the screen.

He stares at the test file. The pairs Claude was tested on are: `("mitochondria", "mitochondria")` → 1.0. `("mitochondrion", "mitochondria")` → 0.93. `("the powerhouse of the cell", "the powerhouse of the cell")` → 1.0. `("powerhouse of the cell", "the powerhouse of the cell")` → 0.91. Twelve more like that. All sensible. All passing.

He types a new pair into the test file by hand. `("Paris", "Parris")`. The function returns 0.91. He stares at it. *Paris* and *Parris* are not the same answer. One is a city. The other is a misspelled person's name from *The Crucible*, which he read in October, and which he is now seeing clearly because he typed two unrelated things into a function and the function called them ninety-one percent the same.

The function is doing string similarity. The problem is *semantic equivalence*. They are not the same problem. The function passes every test Claude wrote because Claude wrote tests for the problem the function solves, not for the problem Seth was trying to solve. The threshold of 0.7 isn't wrong; the *threshold* is wrong, because the *thing being thresholded* is wrong. He almost shipped string-similarity disguised as grading.

He doesn't ship it.

This chapter is about why he didn't, and what to call the move he just made. The move has a name. It is one of five. The other four would have caught other things, on other days, in other builds — and three of them, Seth has already exercised in this same forty minutes without knowing they had names. This is the chapter where they get names.

<!-- → [DIAGRAM: The five supervisory capacities as a pentagon or five-column layout. Each: label (PA, PF, TO, IJ, EI), plain-language name, one-sentence definition. Editorial style. No color.] -->

## The five capacities, in one breath

The argument of this chapter is that there are, currently, exactly five things a competent human supervisor does that Claude does not do. They are not skills in the way "Python" is a skill; they are *stances* a person takes during a build. They show up at different moments, against different failure modes, and a build can pass through any one of them without exercising another. Together they are what you bring to the orchestra that the orchestra does not bring to itself.

In the order they tend to appear in a real build:

- **[PF] Problem Formulation** — deciding what the build *is* before Claude sees it. Claude optimizes within the frame you give it. If the frame is wrong, the output is wrong, elegantly.
- **[TO] Tool Orchestration** — choosing which Claude task, in what order, with what trust level. The sequencing that makes a good result possible.
- **[PA] Plausibility Auditing** — hearing the wrong note in output before verification confirms it. Checking output against domain knowledge that isn't in the prompt.
- **[IJ] Interpretive Judgment** — supplying the meaning Claude's output cannot carry. What does this result mean for *this* specific project, *this* specific deadline, *this* specific user?
- **[EI] Executive Integration** — holding the whole build toward a single goal. *Three prompts ago we agreed on an architecture. This new output is undermining it. Stop.*

The labels (**PA, PF, TO, IJ, EI**) are notation specific to this book. They are short because you are going to mark them on transcripts in the exercises, and because Chapter 6 will turn each one into an axis of a score. Read them once. The next five sections take them one at a time.

I am going to insist on the word *currently* every time the question comes up of whether some future model will do one of these for you. Harry Collins, in the most careful book on the topic — *Tacit and Explicit Knowledge* (Collins, 2010) — divides tacit knowledge into three kinds: relational (codifiable in principle), somatic (sometimes codifiable), and collective (irreducibly social). Some of the five capacities are probably relational in Collins's sense and may be approximated by a future model. Others probably are not. We do not know the schedule. *Currently*, all five require you. That is the honest sentence.

<!-- → [TABLE: Five supervisory capacities — three columns: Label / Plain name / What it catches. Five rows. No color.] -->

## [PF] Problem Formulation — what is the build, actually

Of the five, **[PF] Problem Formulation** is the one that determines whether everything that follows is worth doing.

Claude is, in a deep and well-documented way, an optimizer within a frame. You give it a frame — a prompt, a specification, a function signature — and it produces output that fits the frame as cleanly as it can. The orchestra plays the piece you handed them. If you handed them the wrong piece, what you get back is not a mistake; it is *the wrong piece, played beautifully*. The error is invisible at the point of output, because the output is internally coherent. The error lives upstream, in the act of choosing which piece.

Seth's quiz grader is a textbook **[PF]** failure that almost shipped. The frame he gave Claude was *write a function that scores a student's answer against an answer key*. Claude returned a function that scores a student's answer against an answer key. Inside that frame, the function is excellent. Outside that frame — in the world where students write *Paris* when the answer is *Parris* and *the mitochondria* when the answer is *mitochondria* — the frame is the wrong question. The right question was something like: *given a rubric and a typed answer, decide whether the student has demonstrated knowledge of this concept, where "demonstrated" tolerates spelling variation but not semantic substitution.* Same model. Different frame. The output you get back is categorically different.

This is the kind of failure Lucy Suchman, in *Plans and Situated Actions* (Suchman, 1987), spent two hundred pages arguing was structural to machines. Her field site was a Xerox copier with an "intelligent" help system. Users would fail at the copier; the system would dutifully give them the next step *of the plan it had*, which was the wrong plan, because the plan did not know what the user was actually trying to do. The machine could not formulate the problem. It could only execute the formulation it was given. Suchman's argument is that plans are *resources for situated action*, not substitutes for it — and the irreducible work of the human is to bring the situation into the plan. You bring the situation. Claude does not have it.

Problem formulation, at student scale, is mostly a small set of questions you ask yourself before you type:

- *What is the actual outcome I want?* (Not the function. The outcome.)
- *What would count as a wrong answer that still passes the obvious tests?*
- *What knowledge does this task require that is not in the prompt I am about to send?*
- *Whose problem is this — what user, what deadline, what constraint do I have that Claude does not?*

These questions are not exotic. They are the questions any good engineer asks before they start. The reason they belong on a list of supervisory capacities, rather than on a list of "good habits," is that **Claude will produce output whether you have asked them or not**, and the output will look fine. The cost of skipping **[PF]** is invisible until something fails downstream. By then the build is shaped around the wrong frame, and the fix is not a patch — it is a rebuild.

A second example, smaller. *Build a script to find duplicate students in this CSV.* Frame Claude with that, get an elegant exact-string-match deduplicator. Now run it on a real registrar's file where *Jonathan Smith* and *Jon Smith* and *J. Smith* are the same student in three records. The script finds zero duplicates. The frame was wrong. The output is elegantly wrong. The fix is upstream of the function — you reformulate: *find probable duplicates by name, where probable means edit-distance below threshold AND matching birth year, output as a review queue for human confirmation.* Now the script can do its job. Now the orchestra has the right piece.

## [TO] Tool Orchestration — which task, in what order, trusted how far

If **[PF]** decides what the build is, **[TO] Tool Orchestration** decides how to assemble it from Claude's actions.

Claude Code is not one tool. It is a small fleet. You can ask Claude to plan, to write, to read, to refactor, to run tests, to run scripts, to search the web, to commit. Each of those is a different *task type* with a different failure profile. A planning prompt produces a list; if the list is wrong, you waste a build. A writing prompt produces code; if the code is plausible-but-wrong, you waste a debug. A run-tests prompt produces a number; if the number is misleading, you waste a decision. The orchestration question — *which task, in what order, with what trust level* — is the entire difference between a build with a clean diff and a build with a tangled one.

The textbook tangled-diff failure: a refactor that touches `models.py`, `routes.py`, and `tests/`, asked of Claude in one prompt as *please update the model with these new fields and adjust the routes and tests accordingly*. Claude produces a 400-line diff. The diff compiles. The tests pass — Claude wrote the tests. The diff is unreviewable, because the changes in `routes.py` reference fields that exist somewhere in `models.py` that you did not check, and the changes in `tests/` are testing those same fields against themselves. You have produced a closed loop. The student who orchestrates instead — *first regenerate the model with the new fields; show me the diff; I commit; then update the routes; show me the diff; I commit; then update the tests; show me the diff; I commit* — gets three small reviewable diffs and a git history that names the steps. Same model. Same task. Different orchestration. Different artifact.

Tool orchestration is also where the **trust level** decision lives. Some Claude tasks are cheap to verify, and you can run them on long leash — search the web, read a file, summarize. Some are expensive to verify and you should run them on short leash — write code that touches the database, modify CI, install packages. A 2025 industrial study at Tencent looked at this empirically: across 433 LLM-flagged static-analysis alarms, the raw LLM had a false-alarm rate above 76%, and inspectors averaged ten to twenty minutes per alarm (Wen et al., 2025). When the team gated the LLM output with a human filter, false positives dropped to under 6%. The conclusion is operational, not theoretical: *trust level is a knob, and you set it per task, not per session*.

The temptation that almost every student gives in to once — and then never again — is to delegate **[TO]** to a second LLM. *Have Claude review Claude.* The empirical evidence on this is bad. A 2025 paper found that LLM verifiers exhibit high false-positive *and* false-negative rates when judging code against natural-language specs, and that *adding detail to the verification prompt increases misjudgment* (the paper documents systematic failures of LLMs as verifiers; see the *Uncovering Systematic Failures* line of work cited in the Spurr and Vasilescu literature on human-AI oversight, Spurr and Vasilescu, 2025). Two LLMs do not make a supervisor. They make correlated error.

Suchman's argument applies here too. She showed that situated action — the actual sequencing of work in the world — is not specifiable in advance. You cannot write the orchestration down before the build because you do not yet know which step will fail. Orchestration is mid-flight steering. It is something you do, not something you plan. Claude can produce the plan. You decide what to do with it on the next step.

## [PA] Plausibility Auditing — the wrong note before the proof

**[PA] Plausibility Auditing** is the capacity Seth exercised, at the top of this chapter, when he typed *Paris* and *Parris* into his test file.

Michael Polanyi, who was a physical chemist before he was a philosopher, spent the second half of his career on a single sentence. The sentence is: *we can know more than we can tell* (Polanyi, 1966). His evidence was *connoisseurship* — the trained perceptual capacity of an expert to recognize quality, error, or wrongness in their domain before they can articulate the rule that was violated. The wine taster who knows the year. The radiologist who sees the shadow. The grader who knows a chemistry answer is wrong before they have the rubric in front of them. Polanyi's claim, the one that survived sixty years of expert-performance research and has not been displaced, is that this is *real knowledge*, not gut feeling — and it is built by exposure to many cases, *especially labeled wrong cases*.

Plausibility auditing is connoisseurship for Claude's output. It is the trained perception that something is off before the test runner is ready to confirm it. It is hearing a wrong note in a piece you have heard played correctly enough times that the wrong note sounds different in your ear.

The headline empirical case for **[PA]** is package hallucination. Spracklen and colleagues, in a 2024–25 study across 576,000 code samples from sixteen language models, identified roughly 440,000 hallucinated package references and 205,474 unique invented names (Spracklen et al., 2024). Commercial models hallucinated at about 5.2%; open-source models at about 21.7%. Worse, 58% of fabricated package names recurred across queries — which means an attacker can register the fabricated name on npm or PyPI, and your `pip install` will install someone else's code. The attack has a name. It is called *slopsquatting*. It exists because plausibility auditing was skipped.

What does plausibility auditing actually look like at student scale?

It looks like reading the imports at the top of a generated file and asking, of each one, *have I ever seen that before? Does that sound like something that exists?* It looks like skimming a generated function for a constant — `0.7`, `512`, `"v2"` — and asking *where did that number come from, is it justified or is it vibes?* It looks like noticing that a function signature has a parameter Claude has not used, or returns a tuple whose second element is never read, or imports a module that the rest of the file does not need. It looks like the small itch that something is doing extra work, or not enough work, or the right work on the wrong thing. None of those are verification. All of them are *perception trained by exposure*.

The training of **[PA]** is the part of this book that no chapter can do for you. You build connoisseurship by looking at many Claude outputs and labeling, for each one, what was right and what was wrong — especially what was *plausibly* wrong. The exercises in this chapter, and in every chapter that follows, are training data for your own perception. When Seth re-reads his quiz grader and feels that something is off, what he is doing is recognizing a pattern (string similarity in a place that wanted semantic similarity) against a library of cases he has seen labeled. He has been building that library since Chapter 1. He will build it the rest of his life.

Plausibility auditing fails in exactly one way: when you delegate it. The second-LLM-as-supervisor failure is one version. The other version is the one Seth almost committed at the top of this chapter — *the tests passed, so it must be right*. The tests are a verification step downstream of plausibility. They do not replace it. If the wrong note in the score is *the score is testing the wrong thing*, the test runner cannot hear it. Only you can.

## [IJ] Interpretive Judgment — what does this number mean

Suppose Claude finishes the quiz grader and reports that, on a fresh test set of 200 graded answers, the model achieves 92% accuracy.

What does that mean?

The honest answer is: not enough, on its own, to make any decision. A 92% accuracy number is a piece of arithmetic, not a finding. It does not say what the class distribution was. It does not say what kinds of errors the 8% are. It does not say whether the 92% is on student answers like the ones the system will see in production, or on a curated test set drawn from the same source as training. It does not say whether the eight wrong answers in a hundred are random spelling drift (acceptable) or systematic failure on the answers that matter most for the grade (unacceptable). It does not say whether 92% is good *for this specific use case*, given *this specific student population*, on *this specific deadline*.

The number does not carry that meaning. **You supply the meaning.** That is **[IJ] Interpretive Judgment**.

Donald Schön, in *The Reflective Practitioner* (Schön, 1983), gave this capacity its name. Schön spent twenty years watching architects, doctors, engineers, and therapists work, and his central observation was that experts adjust *during* the task, not after — what he called **reflection-in-action** and **knowing-in-action**. The radiologist sees the scan; the meaning of the scan for *this patient* with *this history* arrives in the same act as seeing. The architect looks at the model; the meaning of the model for *this site* with *this budget* arrives in the same act as looking. The expert "knew more than they could say." The duty of the professional, Schön argued, is to surface that knowing — to render the interpretation explicit enough that it can be argued with.

For a 2026 student working with Claude, **[IJ]** is the paragraph you write *under* the number. Claude reports 92%. You write:

> 92% on a test set of 200 graded answers, where the answer-key distribution is roughly 60% multi-word phrases and 40% single-word concepts. Of the 8% error, six are spelling drift on single-word answers (acceptable — students misspell mitochondria all the time), nine are semantic substitution that scored as similar (unacceptable — *Paris*/*Parris* class), and one is a key entered wrong in the test set (data error, not model error). I am willing to ship for the multi-word case where errors are mostly spelling. I am not willing to ship for the single-word case where the error mode is semantic substitution. The number is good enough; the *distribution of error* is not.

That paragraph is **[IJ]**. Nothing in it could be produced by Claude alone — not because Claude cannot write paragraphs about numbers, but because the *interpretation* depends on the specific project, the specific deadline, the specific user, and the specific acceptability threshold you carry in your head and have not written down. Claude has the number. You have the situation. The paragraph happens where they meet.

The student-scale failure mode here is shipping the number without the paragraph. It is reporting *the model is 92% accurate* to a teacher, a teammate, or a future self, and not noticing that the sentence is doing none of the interpretive work. The number is a measurement. The decision is what you do with the measurement. Those are different acts, performed by different agents. Claude produces the first. You produce the second.

## [EI] Executive Integration — holding the build toward one goal

There is one more, and it is the one that catches the failure that hurts the most when it gets through.

**[EI] Executive Integration** is what you do when you remember, at prompt seventeen, what was decided at prompt three.

Builds drift. A long Claude session — and any session that produces real software is long — accumulates small decisions, small concessions, small *let's just for now*s, and the drift between where you started and where you are at minute ninety is rarely registered in any single step. Each individual move is locally reasonable. Each individual move is, in isolation, an answer to the immediate problem. And ninety minutes later you are looking at an architecture that has nothing to do with the one you agreed on, and the reason it has nothing to do with that one is that nobody in the loop — not you, not Claude — was holding the whole.

Executive integration is the act of holding the whole.

A small example. Three prompts in, you and Claude have agreed that the app will use a `Config` dataclass injected at construction time — no globals, no environment-variable reads scattered through modules, configuration is a parameter the caller supplies. Prompt seven: you ask Claude to add logging. Claude proposes a small change. The change adds `import os; LOG_LEVEL = os.environ.get("LOG_LEVEL", "INFO")` at the top of `logger.py`, "for convenience." The change is local, the change works, the change is even *good practice* in many codebases. The change also violates the architecture you agreed to three prompts ago. Nobody flagged it. The drift is in.

The **[EI]** move is: *Three prompts ago we agreed on a `Config` dataclass injected at construction. Reading from the environment at module load undoes that. Roll the logger change back. Re-prompt: add logging that takes a `level: str` parameter from the existing `Config`.*

That sentence is what holding the whole sounds like in practice. It refers backward to a prior decision; it identifies the current move as inconsistent with that decision; it stops the current move; it re-prompts. None of those four sub-acts is something Claude does on its own. Claude does not, in any current architecture, hold the architectural decisions of the build as constraints on subsequent moves unless you put those constraints into the prompt every time. Even then, drift slips through, because constraints in prose are softer than constraints in code, and the model will sometimes optimize toward the immediate task at the cost of the global one.

Schön's *reflection-in-action* lives here too. The professional, mid-task, asks: *is this consistent with the whole?* The asking is the act. The answer is sometimes no, and the value of the asking is precisely that it can produce a *no*. **[EI]** is the structural place in your build where *no* gets to come out of your mouth.

There is empirical weight behind the importance of this. Spurr, Vasilescu, and colleagues, in a 2025 field study of LLM-assisted code review, identified the dominant failure modes of human-AI collaboration as *context insufficiency*, *plausible-but-wrong suggestions*, and — the one that matters here — *over-trust by reviewers* (Spurr and Vasilescu, 2025). Over-trust is what happens when the reviewer stops holding the whole. Each individual suggestion is reasonable. The reviewer accepts each one. The accumulated suggestions are an architecture nobody chose. The fix is not better suggestions. The fix is a reviewer who holds the whole.

## Worked example: Seth's quiz grader, with every capacity labeled

Here is the full forty-minute build sequence from the top of the chapter, broken into moves and labeled with the capacity each one exercised — or, where it did not, with the capacity that was missing.

**Move 1.** Seth decides to build a short-answer quiz grader for his AP Bio study site. He spends two minutes asking himself: *what counts as right?* He decides: *spelling drift is OK, semantic substitution is not, and I want to see the wrong answers in a review queue, not auto-mark them.* He writes that down in a comment at the top of an empty file before he opens Claude.

→ **[PF] Problem Formulation.** This is the move he did *before Claude saw the task*. The frame is on the page in his words.

**Move 2.** He decides to break the build into three Claude tasks: first, draft a function signature and the test plan; second, write the function; third, write the review-queue output. He commits between each.

→ **[TO] Tool Orchestration.** He chooses the task ordering before he chooses the prompt. Each step is small and reviewable.

**Move 3.** He prompts Claude for the function signature and test plan. Claude returns a signature using `difflib.SequenceMatcher` and a test plan with twelve sensible-looking input pairs.

→ This is the move where **[PF] should have re-engaged and did not.** The frame Seth wrote at the top of the file said *semantic substitution is not OK*. The signature Claude returned tests *string similarity*. Seth read the signature, said "looks reasonable," and moved on. The wrong frame entered the build here.

**Move 4.** Seth prompts Claude for the function. Claude writes a forty-line function using `SequenceMatcher` with a 0.7 ratio threshold. The function compiles. The twelve tests pass at 85%.

→ **[TO] is operating** (small step, reviewable diff) but **[PA] has not yet engaged.** The output is plausible. Seth almost accepts it.

**Move 5.** Seth pauses. He cannot say what is wrong. He stares at the file. He types a new test pair by hand: `("Paris", "Parris")`. The function returns 0.91.

→ **[PA] Plausibility Auditing.** This is the move. Connoisseurship — *something is off* — produces a probe — *type a probably-wrong pair* — that produces verification — *the function calls them similar*. The wrong note was heard before the test runner was ready to confirm it.

**Move 6.** Seth identifies the failure mode. *String similarity is not semantic equivalence.* He realizes the **[PF]** failure from Move 3 — the frame was wrong, the function is elegantly wrong, the fix is not a patch.

→ **[IJ] Interpretive Judgment.** He could have shipped the 85% number. He instead asks what the 85% *means* for his use case (single-word concepts, where the error mode is semantic substitution, on a study site where wrong-marking a right answer is the worst possible UX). The number is fine; the *distribution of error* is unacceptable. The interpretation kills the ship.

**Move 7.** Seth re-prompts Claude. New formulation: *given a rubric with one accepted answer and a list of acceptable spelling variants, AND given a typed student answer, AND given a tolerance for spelling drift but NOT for semantic substitution, return either ACCEPT, REVIEW, or REJECT — and route REVIEW to a review queue rather than auto-grading.* He writes the new spec in a comment block before prompting.

→ **[PF]** again, this time correctly. Same Claude. Different frame.

**Move 8.** Claude returns a new function using exact-match with a curated variants list plus a normalized-string secondary check. It compiles. Seth tests it on the *Paris/Parris* case — REJECT. On the *mitochondria/mitochondrion* case — ACCEPT (because *mitochondrion* is in the variants list). On the *the mitochondria/mitochondria* case — ACCEPT (because the normalizer strips leading articles).

→ **[PA]** running cleanly now — Seth probes with three known cases. **[TO]** running cleanly — small reviewable function.

**Move 9.** Mid-implementation, Claude offers an "improvement": *I notice the rubric loading is slow because we read the file every call. Want me to add a module-level cache?* Seth looks at his Move 1 comment. It does not say anything about a module-level cache. It does say *the rubric will be edited by a teacher in production, and the system should pick up changes within a minute*. A module-level cache breaks that constraint.

→ **[EI] Executive Integration.** Seth refers backward: the rubric edits matter. The cache contradicts the constraint. He declines the improvement. He re-prompts with the constraint stated.

**Move 10.** Final read-through. Seth re-reads the entire function, the tests, the variants list, and the comment block at the top of the file. He writes one paragraph in the README describing what the grader does, what it does not do, what kinds of errors are acceptable, and what kinds are not.

→ **[IJ]** once more, in the final paragraph — the interpretation that makes the function legible to a future Seth who will not remember any of this in four weeks.

**The lesson.** All five capacities appeared in this build. **[PA]** is the one that caught the near-failure, but **[PF]** was the one that, exercised correctly at Move 7, made the second draft work. **[TO]** kept the diffs reviewable. **[IJ]** decided what the number meant. **[EI]** rejected an architectural drift at Move 9. No one of them is sufficient. All five were required.

**The limit.** This is one build. The capacities will show up in different orders, with different weights, on different days. The point is not the sequence. The point is that all five are *available to you* and that none of them is being supplied by Claude.

## Exercises

**1. (Apply) Label the build.** Below is a six-prompt build sequence in which a student builds a Flask login route. Mark, in the margin of each prompt, which supervisory capacity (PA / PF / TO / IJ / EI) the student exercised — and which ones were absent. At minimum, one prompt should be labeled with two capacities; one prompt should be labeled with a missing capacity that produced a downstream failure.

> *Prompt 1.* "Add a login route to my Flask app."
> *Prompt 2.* (Claude returns code using `flask.RouteRegistry`, which does not exist.) Student commits.
> *Prompt 3.* "The app crashes on startup with `ImportError`."
> *Prompt 4.* (Claude apologizes, regenerates with `@app.route`.) Student commits.
> *Prompt 5.* "Add password hashing."
> *Prompt 6.* (Claude adds `hashlib.md5`. Student commits.)

Identify, for each numbered move, which capacity was exercised, missed, or actively reversed. At which prompt did the absence of **[PA]** ship a real failure? At which prompt did the absence of **[PF]** ship a worse one?

**2. (Analyze) Diagnose the missing capacity.** A classmate gives you the transcript of a 90-minute build. The transcript starts with a clear specification, proceeds through fifteen prompts, and ends with code that passes every test, compiles, and runs — but produces silently wrong results on the production data set. Read the transcript and identify which one of the five supervisory capacities is *systematically* missing across the session. Cite at least three specific prompts where the missing capacity should have engaged, and describe what the student should have asked, typed, or stopped at each point.

**3. (Create) Design your supervisory checklist.** Take a build you are currently working on — a homework project, a personal site, a class assignment, anything you have written or will write Claude into. Produce a one-page checklist with the five capacity labels (**PA, PF, TO, IJ, EI**) as headings. Under each heading, write *three* specific questions you will ask yourself, in this project, when the time comes to exercise that capacity. The questions must be specific to *your* project — not generic. ("Did I check the imports?" is generic. "Did I check that the `flask-login` import actually exists and is not the hallucinated `flask.RouteRegistry` from the last build?" is specific.) The checklist must be pinnable above your screen.

---

## 🕰️ AI Wayback Machine

**Douglas Engelbart (1925–2013).** American electrical engineer and inventor. Worked at the Stanford Research Institute from 1957 to 1977, where he founded the Augmentation Research Center, invented the computer mouse, and co-led the team that gave the 1968 demonstration now remembered as the "Mother of All Demos." But before any of that, in 1962, Engelbart wrote a sixty-six-page report for the Air Force Office of Scientific Research titled *Augmenting Human Intellect: A Conceptual Framework* (AFOSR-3223, SRI Project 3578). The report's argument is that the proper goal of computing is not to replace human capability but to *augment* it — to extend what Engelbart called the **H-LAM/T system**: Human, using Language, Artifacts, Methodology, with Training. The artifact (the computer) handles execution and storage. The human contributes framing, judgment, and integration. The five supervisory capacities in this chapter — PA, PF, TO, IJ, EI — are the 2026 student-scale version of Engelbart's human portion. He did not have the empirical evidence we now have for what the artifact does well and where it hallucinates; he did have the structural argument, sixty-four years ago, that the human is not optional. The augmentation tradition, of which this book is a descendant, started in that 1962 report.

**Run this:** *"Read pages 1–15 of Engelbart's 1962 SRI report* Augmenting Human Intellect: A Conceptual Framework *(AFOSR-3223). Then write a 300-word argument: how do the five supervisory capacities in Chapter 5 of* Claude Code for Students *— Plausibility Auditing, Problem Formulation, Tool Orchestration, Interpretive Judgment, Executive Integration — operationalize Engelbart's H-LAM/T model for a 2026 high-school student working with Claude Code? Be specific about which capacity corresponds to which element of H-LAM/T, and where the mapping is imperfect."*

---

The reader knows the five capacities. Chapter 6 introduces the tool that makes exercising them systematic: Gru.
