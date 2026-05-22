# Chapter 5 — The Five Supervisory Capacities

*These are the five things you do that Claude cannot. Name them. Practice them. Never delegate them.*

---

Seth is forty minutes into a build and the code in front of him compiles.

It is a Friday in late April. He's building a short-answer quiz grader for his AP Biology study site — a Python function that takes a student's typed answer and an answer key and returns a score between zero and one. He has been working with Claude for half an hour. He has a function. He has twelve test pairs, and on those twelve pairs the function returns 85% accuracy. The tests pass. The code is clean. The function uses `difflib.SequenceMatcher` with a 0.7 ratio threshold, which Claude has explained in a comment Seth has read twice.

He almost ships it.

What stops him is not a test failure. What stops him is an itch — a small, specific, hard-to-articulate wrongness located somewhere between his eye and the screen. He stares at the test pairs. They all involve things like *mitochondria* versus *mitochondria*, or *the powerhouse of the cell* versus *the powerhouse of the cell*. Sensible things. Things that look like what the function is supposed to handle.

He types a new pair into the test file by hand. `("Paris", "Parris")`. The function returns 0.91.

*Paris* and *Parris* are not the same answer. One is a city. The other is a character from *The Crucible*, which he read in October. The function is doing string similarity. What Seth needed was semantic equivalence. Those are not the same problem. Claude wrote tests for the problem the function solves, not for the problem Seth was trying to solve. He almost shipped string similarity disguised as grading.

He doesn't ship it.

This chapter is about what to call the move he just made — and the four other moves that competent supervisors make on the way to a clean build. Together these five moves are *the things you do that Claude does not do*. They have always existed; this chapter just gives them names.

<!-- → [DIAGRAM: The five supervisory capacities as a pentagon or five-column layout. Each: label (PA, PF, TO, IJ, EI), plain-language name, one-sentence definition. Editorial style. No color.] -->

---

## The Five, In One Breath

Before I take them one at a time, here is the whole list, because the list is the argument:

**[PF] Problem Formulation** — deciding what the build is before Claude sees it. Claude optimizes within the frame you give it. If the frame is wrong, the output is wrong, elegantly.

**[TO] Tool Orchestration** — choosing which Claude task, in what order, with what trust level. The sequencing that makes a good result possible.

**[PA] Plausibility Auditing** — hearing the wrong note in output before any test confirms it. Checking what Claude returned against domain knowledge that isn't in the prompt.

**[IJ] Interpretive Judgment** — supplying the meaning Claude's output cannot carry. What does this result mean for this project, this deadline, this user?

**[EI] Executive Integration** — holding the whole build toward a single goal. Noticing, at prompt seventeen, that the current move undermines the decision made at prompt three.

The labels — **PA, PF, TO, IJ, EI** — are shorthand specific to this book. They're short because you'll be marking them on transcripts, and because Chapter 6 turns each one into a scoring axis. Learn them now. The next five sections are what's behind each label.

One word I will use that deserves a flag: *currently*. Every time the question comes up of whether some future model will do one of these for you, the honest sentence is: we don't know the schedule. Harry Collins, in *Tacit and Explicit Knowledge* (Collins, 2010), divides tacit knowledge into kinds — some codifiable in principle, some only sometimes, some irreducibly social. Some of the five capacities are probably the first kind. Others probably aren't. What I can tell you with confidence is that *currently*, in 2026, all five require you. That is the claim this chapter is making.

<!-- → [TABLE: Five supervisory capacities — three columns: Label / Plain name / What it catches. Five rows. No color.] -->

---

## [PF] Problem Formulation

Seth's quiz grader is a textbook **[PF]** failure that almost shipped.

The frame he gave Claude was: *write a function that scores a student's answer against an answer key.* Claude returned a function that scores a student's answer against an answer key. The output is excellent *inside that frame*. Outside that frame — in the world where students write *Paris* when the answer is *Parris*, where the error mode is semantic substitution rather than spelling drift — the frame is the wrong question.

Claude is, in a deep and well-documented way, an optimizer within a frame. You hand it a specification and it produces the cleanest output it can for that specification. The orchestra plays the piece you handed them. If you handed them the wrong piece, what you get back is not a mistake — it is *the wrong piece, played beautifully*. The error is upstream of the output and invisible in it.

Lucy Suchman spent two hundred pages in *Plans and Situated Actions* (Suchman, 1987) arguing that this is structural to machines, not incidental. Her field site was a Xerox copier with an "intelligent" help system. Users would fail at the copier; the system would faithfully offer the next step of the plan it had, which was the wrong plan, because the plan did not know what the user was actually trying to do. Suchman's conclusion is that plans are *resources for situated action*, not substitutes for it. The irreducible work of the human is to bring the situation into the plan. You bring the situation. Claude does not have it.

Problem formulation, at student scale, is mostly four questions you ask before you type. What is the actual outcome I want — not the function, the outcome? What would count as a wrong answer that still passes the obvious tests? What knowledge does this task require that isn't in the prompt I'm about to send? Whose problem is this — what user, what deadline, what constraint do I have that Claude does not?

These are not exotic questions. Every competent engineer asks them before starting. The reason they appear on this list is that **Claude produces output whether you've asked them or not**, and the output will look fine. The cost of skipping **[PF]** is invisible until something fails downstream — at which point the build is shaped around the wrong frame, and the fix is a rebuild, not a patch.

---

## [TO] Tool Orchestration

If **[PF]** decides what the build is, **[TO]** decides how to assemble it from Claude's actions.

Claude Code is not one tool. You can ask it to plan, to write, to read, to refactor, to run tests, to run scripts, to search, to commit. Each of those is a different task type with a different failure profile. A planning prompt produces a list; if the list is wrong, you waste a build. A writing prompt produces code; if the code is plausible-but-wrong, you waste a debug. A run-tests prompt produces a number; if the number is misleading, you waste a decision. The orchestration question — which task, in what order, trusted how far — is the difference between a build with a clean diff and a build with a tangled one.

The textbook tangled-diff failure: a refactor that touches `models.py`, `routes.py`, and `tests/`, asked of Claude in a single prompt — *please update the model with these new fields and adjust routes and tests accordingly.* Claude produces a 400-line diff. The diff compiles. The tests pass — Claude wrote the tests. The diff is unreviewable, because the changes in `routes.py` reference fields in `models.py` you didn't check, and the changes in `tests/` are testing those fields against themselves. You have produced a closed loop of correlated correctness. The student who orchestrates instead — first the model, review and commit; then the routes, review and commit; then the tests, review and commit — gets three small reviewable diffs and a git history that names the steps. Same model, same task, different orchestration, entirely different artifact.

**[TO]** is also where *trust level* lives. Some Claude tasks are cheap to verify — search the web, read a file, summarize text — and you can run them on long leash. Some are expensive to verify — write code that touches the database, modify CI configuration, install packages — and should run on short leash. A 2025 industrial study at Tencent examined 433 LLM-flagged static-analysis alarms and found that the raw LLM had a false-alarm rate above 76%, with inspectors averaging ten to twenty minutes per alarm (Wen et al., 2025). When the team gated LLM output with a human filter, false positives dropped below 6%. Trust level is a knob. You set it per task, not per session.

The temptation students give in to once — and then never again — is to delegate **[TO]** to a second model. Have Claude check Claude. The empirical evidence is bad. Two LLMs do not make a supervisor. They make correlated error. Suchman's argument applies here too: situated sequencing — the actual ordering of work in the world — is not specifiable in advance. You cannot write the orchestration down before the build because you don't yet know which step will fail. Orchestration is mid-flight steering. Claude can hand you a plan. What you do with it on the next step is yours.

---

## [PA] Plausibility Auditing

The move Seth made when he typed *Paris* and *Parris* — before any formal verification, before the test runner confirmed anything — is **[PA] Plausibility Auditing**. It is the capacity to hear the wrong note before the proof is ready.

Michael Polanyi, who was a physical chemist before he became a philosopher, built a career on a single sentence: *we can know more than we can tell* (Polanyi, 1966). His evidence was connoisseurship — the trained perceptual capacity of an expert to recognize quality, error, or wrongness in their domain before they can articulate the rule that was violated. The wine taster who knows the year. The radiologist who sees the shadow. The student who reads a generated function and knows something is off before they know *what* is off. Polanyi's claim, which sixty years of expert-performance research has not displaced, is that this is real knowledge. Not gut feeling. Knowledge built by exposure to many cases, especially labeled wrong cases.

Plausibility auditing is connoisseurship applied to Claude's output. It is trained perception, not intuition, and it can be deliberately built.

The headline failure mode that **[PA]** prevents is package hallucination. Spracklen and colleagues, in a 2024–25 study across 576,000 code samples from sixteen language models, identified roughly 440,000 hallucinated package references and 205,474 unique invented names (Spracklen et al., 2024). Commercial models hallucinated at about 5.2%; open-source models at about 21.7%. Worse, 58% of fabricated package names recurred across queries — which means an attacker can register the fabricated name on npm or PyPI, and your `pip install` will fetch someone else's code. The attack has a name: *slopsquatting*. It exists because **[PA]** was skipped at the import line.

At student scale, plausibility auditing looks like reading the imports at the top of a generated file and asking, of each one, *have I seen that before? Does that sound like something that exists?* It looks like noticing a constant — `0.7`, `512`, `"v2"` — and asking *where did that number come from, is it justified or is it vibes?* It looks like the itch that a function is doing extra work, or not enough work, or the right work on the wrong thing. None of those moves are verification. All of them are *perception trained by exposure*.

The training of **[PA]** is the part no chapter can do for you. You build connoisseurship by looking at many Claude outputs and noticing what was right and what was wrong — especially what was *plausibly* wrong. The case studies and worked examples in this book are training data for your own perception. Seth's *Paris*/*Parris* probe was not luck. It was the correct application of domain knowledge — *what are the hardest cases for a string-similarity scorer on short answers?* — to a function that had passed every test in front of it.

Plausibility auditing fails in exactly one way: when you delegate it. The version Seth almost committed at the top of this chapter — *the tests passed, so it must be right* — is the complete failure mode. Tests are downstream verification. They do not replace upstream perception. If the wrong note in the score is *the tests are testing the wrong thing*, the test runner cannot hear it. Only you can.

---

## [IJ] Interpretive Judgment

Suppose Seth's new grader reports 92% accuracy on a fresh test set of 200 graded answers.

What does that mean?

On its own: not enough to make any decision. 92% is a number. It does not say what the class distribution was. It does not say whether the 8% errors are random spelling drift or systematic semantic substitution. It does not say whether 92% is good *for this use case*, given *this student population*, on *this deadline*. The number is a measurement. **[IJ] Interpretive Judgment** is the act of deciding what the measurement means.

Donald Schön called this *reflection-in-action* (Schön, 1983). Watching architects, doctors, engineers, and therapists work, his central observation was that experts adjust during the task, not after — meaning the interpretation of what they are seeing arrives in the same act as seeing. The radiologist sees the scan; the meaning for *this patient* with *this history* arrives in the same moment. The expert's knowledge and the expert's situation are not separated. The professional's duty, Schön argued, is to surface that interpretation explicitly enough that it can be argued with.

For a student working with Claude, **[IJ]** is the paragraph you write under the number. Not "the model is 92% accurate," but: of the 8% error, how many are spelling drift and how many are semantic substitution? What are the worst-case failures? Is 92% good enough for multi-word answers but not for single-word concepts where the error mode is dangerous? Would I be comfortable with my name on this if the 8% turned out to be systematic? That paragraph is interpretive judgment. Nothing in it can be produced by Claude alone — not because Claude cannot write paragraphs about numbers, but because the interpretation depends on the specific project, the specific deadline, and the acceptability threshold you carry in your head and have not written down anywhere.

Claude has the number. You have the situation. **[IJ]** is what happens where they meet.

The failure mode is shipping the number without the paragraph. Reporting *the model is 92% accurate* without the distribution of error, the acceptability threshold, or the decision that follows. The number is the first act. The decision is the second act. They are performed by different agents. Claude performs the first. You perform the second.

---

## [EI] Executive Integration

There is one more, and it catches the failure that hurts most when it gets through.

Builds drift. A long Claude session accumulates small decisions, small concessions, small *let's just for now*s, and the accumulated drift between where you started and where you are at minute ninety is rarely visible in any single step. Each individual move is locally reasonable. Each individual move is, in isolation, a sensible answer to the immediate problem. And ninety minutes later you are looking at an architecture that has nothing to do with the one you agreed on, and nobody in the loop — not you, not Claude — was holding the whole.

**[EI] Executive Integration** is the act of holding the whole.

Three prompts in, you and Claude have agreed that the app will use a `Config` dataclass injected at construction time — no globals, no environment-variable reads scattered through modules, configuration is a parameter the caller supplies. Prompt seven: you ask Claude to add logging. Claude proposes adding `import os; LOG_LEVEL = os.environ.get("LOG_LEVEL", "INFO")` at the top of `logger.py`, "for convenience." The change is local, it works, it is even good practice in many codebases. It also violates the architecture you agreed on three prompts ago. Nobody flagged it. The drift is in.

The **[EI]** move is: *Three prompts ago we agreed on a `Config` dataclass injected at construction. Reading from the environment at module load undoes that. Roll the logger change back. Re-prompt: add logging that takes a `level` parameter from the existing `Config`.*

That sentence is what holding the whole sounds like in practice. It refers backward to a prior decision; identifies the current move as inconsistent with it; stops the current move; re-prompts. None of those four acts is something Claude performs on its own. Claude does not hold your architectural decisions as constraints on subsequent moves unless you put those constraints into the prompt every time. Even then, drift slips through, because constraints in prose are softer than constraints in code, and the model will sometimes optimize toward the immediate task at the cost of the global one.

Spurr, Vasilescu, and colleagues, in a 2025 field study of LLM-assisted code review, identified the dominant failure modes of human-AI collaboration as context insufficiency, plausible-but-wrong suggestions, and — the one that matters here — *over-trust by reviewers* (Spurr and Vasilescu, 2025). Over-trust is what happens when the reviewer stops holding the whole. Each individual suggestion is reasonable. The reviewer accepts each one. The accumulated suggestions are an architecture nobody chose. The fix is not better suggestions from the model. The fix is a reviewer who holds the whole across all of them.

---

## Seth's Build, Labeled

Here is the full arc of Seth's forty minutes, with every capacity named where it appeared.

He starts before he opens Claude. He spends two minutes writing down what *correct* means for his grader: spelling drift is acceptable, semantic substitution is not, wrong answers go to a review queue rather than auto-failing. He writes this as a comment block in an empty file. That is **[PF]**. The frame is on the page in his words before Claude sees the task.

He decides to build in three Claude tasks — signature and test plan, then function, then review queue — committing between each. That is **[TO]**. Small, reviewable steps.

Claude returns a function using `difflib.SequenceMatcher`. Seth reads it, says "looks reasonable," moves on. **[PF] should have re-engaged here and did not.** The signature tests string similarity; the frame Seth wrote said semantic equivalence. The wrong frame entered the build at this step.

He runs the tests. Twelve pairs, 85% accuracy, all pass. **[PA] has not yet engaged.** The output is plausible. He almost accepts it.

He pauses. He cannot say what's wrong. He types `("Paris", "Parris")`. The function returns 0.91. That is **[PA]** — connoisseurship producing a probe, the probe producing confirmation, the wrong note caught before any test was ready to flag it.

He identifies the failure: *string similarity is not semantic equivalence.* He could ship the 85% number. He doesn't, because he asks what 85% means for *this* use case — single-word biology concepts, a study site where wrong-marking a right answer is the worst possible UX, a student population prone to spelling drift but not to synonym substitution. The interpretation kills the ship. That is **[IJ]**.

He re-prompts with a new frame: exact-match against a curated variants list, with normalized-string secondary check, routing ambiguous cases to a review queue rather than auto-grading. Same Claude. Different formulation. **[PF]** again, this time correct.

Midway through the new implementation, Claude offers an improvement: a module-level cache to speed up rubric loading. Seth looks at his original comment block. It says the rubric will be edited by teachers in production and the system should pick up changes within a minute. A module-level cache breaks that constraint. He declines the improvement and re-prompts with the constraint stated. That is **[EI]** — a backward reference to a prior decision, a rejection of local optimization at the cost of the global constraint.

All five capacities appeared in forty minutes. **[PA]** caught the near-failure. **[PF]** reformulated the frame. **[TO]** kept the diffs reviewable. **[IJ]** decided what the number meant. **[EI]** rejected an architectural drift. No one of them was sufficient. All five were needed.

The lesson is not the sequence. The sequence will differ every build. The lesson is that all five capacities are *available to you* and none of them is being supplied by Claude. When a build goes wrong, it has gone wrong because one of these five failed. Learning to name which one failed — and at which step — is the diagnostic skill the rest of this book will assume you have.

---

## 🕰️ AI Wayback Machine

**Douglas Engelbart (1925–2013)** is the person who, in 1962, wrote the report that put the structural argument behind what this chapter is saying. *Augmenting Human Intellect: A Conceptual Framework* (AFOSR-3223, SRI Project 3578) was a sixty-six-page document prepared for the Air Force Office of Scientific Research, written before Engelbart had invented the mouse, before the 1968 "Mother of All Demos," before most of the computing infrastructure we now take for granted existed. Its argument: the proper goal of computing is not to replace human capability but to *augment* it — to extend what Engelbart called the **H-LAM/T system**: Human, using Language, Artifacts, Methodology, with Training. The artifact handles execution and storage. The human contributes framing, judgment, and integration. The five supervisory capacities in this chapter — **PA, PF, TO, IJ, EI** — are the 2026 student-scale operationalization of Engelbart's human portion. He did not have the empirical evidence we now have for where models hallucinate and where they succeed. He did have the structural argument, sixty-four years ago, that the human is not optional. The augmentation tradition, of which this book is a descendant, starts in that report.

**Run this:** *"Read pages 1–15 of Engelbart's 1962 SRI report* Augmenting Human Intellect: A Conceptual Framework *(AFOSR-3223). Then write a 300-word argument: how do the five supervisory capacities in Chapter 5 of* Claude Code for Students *— Plausibility Auditing, Problem Formulation, Tool Orchestration, Interpretive Judgment, Executive Integration — operationalize Engelbart's H-LAM/T model for a 2026 high-school student working with Claude Code? Be specific about which capacity corresponds to which element of H-LAM/T, and where the mapping is imperfect."*

---

## Bridge

The reader knows the five capacities. Chapter 6 introduces the tool that makes exercising them systematic: Gru.
