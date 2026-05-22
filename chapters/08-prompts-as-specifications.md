# Chapter 8 — Writing Claude Prompts That Are Specifications

*"Write me a login function" is not a prompt. A prompt names the thing, the invariants, the output format, and what not to touch.*

---

**Learning outcomes**

1. **(Understand)** Distinguish a prompt (request) from a specification (complete task definition).
2. **(Apply)** Rewrite a weak prompt as a complete specification using the five-element format.
3. **(Analyze)** Identify what is missing from a set of provided prompts that would cause Claude to produce incorrect output.

---

## Opening: The Same Task, Twice

Seth has an SDD now. Five sections, written down, version-controlled. He sits down on a Tuesday evening to write the next piece of his task tracker — a small thing, the function that loads grades from a CSV the teacher exports each week. He opens Claude Code. He types:

> *Help me write a CSV parser for my grades file.*

Claude obliges. Forty seconds later there is a file called `parser.py`, eighty lines, importing `pandas`, defining a class called `GradeParser`, and announcing — in a docstring Seth did not ask for — that it "robustly handles a wide variety of CSV dialects." It does, in fact, parse a CSV. It also imports a library Seth doesn't have installed. It also returns rows as dictionaries when Seth's SDD §1.4 says rows are `Grade` dataclasses. It also silently drops the leading byte-order mark on UTF-8 files instead of tolerating it, which means the first column header in Seth's file — `"Student ID"` — comes back as `"﻿Student ID"` and breaks every downstream lookup.

Seth deletes the file. He thinks for a minute. He types again:

> *Write `parse_grades(path: str) -> list[Grade]` in `parsers/grades.py`. Invariants: standard library only — no pandas; quoted fields containing commas must parse correctly; UTF-8 BOM must be tolerated; empty cells become `None`, not the empty string. Context: the `Grade` dataclass is defined in `models/grade.py`; SDD §1.4 specifies the file format; a sample input is at `data/grades_sample.csv`. Output: one Python file; then run `python -m pytest tests/test_parser.py -q` and report the exit code. Do not modify `models/grade.py`. Do not add new dependencies. Do not touch any file outside `parsers/`.*

Claude obliges this time too. The file is forty lines. It uses the `csv` module. It opens the file with `encoding="utf-8-sig"` — which is the standard-library way to tolerate a BOM. Empty cells come back as `None`. The tests pass. The exit code is reported. The file `models/grade.py` is untouched.

The model did not change between the two prompts. The week did not change. Seth's project did not change. Only the **precision of the instruction** changed.

This chapter is about what changed.

<!-- → [TABLE: Prompt vs. specification — two columns, five rows. Each row: one element of the specification format. Left column: weak prompt version. Right column: specification version.] -->

The table above is the chapter's spine. Two columns, five rows. The same task — "validate a student record" — held constant across the rows so the only variable is the **format of the instruction**. Read down the left column and you see a request: vague, conversational, optimistic. Read down the right column and you see a specification: a named function, an invariants list, a context pointer, an output contract, a list of things not to touch. The chapter's claim is that the right column is not a *better-written* prompt. It is a *different artifact*. It is a specification, and that distinction is what the rest of this chapter develops.

A note on definitions before we start, because two words are about to do a great deal of work. A **prompt**, in the common usage of 2026, is a request — a sentence or two of natural language directed at a model, asking it to produce something. A **specification**, formally, is a *complete task definition*: a statement of what is to be produced, under what constraints, against what acceptance criterion, with what scope of effect. The IEEE 830-1998 *Recommended Practice for Software Requirements Specifications* — still the canonical reference, even though it has been superseded by ISO/IEC/IEEE 29148:2011 — lists eight characteristics of a good specification: correct, unambiguous, complete, consistent, ranked for importance, verifiable, modifiable, traceable. The five-element format you are about to learn is the student-scale operational subset of those eight properties, sized for one prompt at a time.

The thesis of this book is that Claude Code is *instrument*, not *shortcut*. The student who draws the line builds real capability; the student who delegates without drawing it polishes output while thinking atrophies. The prompt is **where the line lives, every single day**. The five elements you are about to learn are five places the student decides — in advance, in writing, before Claude reads a token — what counts as done. Skip any of the five and Claude makes the decision instead. Make all five and the prompt is no longer a request. It is a specification.

## The Five Elements of a Specification Prompt

The five elements are: the **specific task**, the **invariants**, the **context**, the **output format**, and the **negative constraint**. They go in roughly this order in the prompt, though strict ordering matters less than presence. Each element does a different cognitive job. Each one, omitted, produces a different failure mode. We will walk them in turn, with the same task — a CSV parser — held constant across all five, so the only variable is which element you are reading about.

### 1. The Specific Task

The specific task is the one thing you are asking Claude to do. Not "help me with X." Not "work on Y." The one thing.

A weak specific task is a topic: "help me with parsing." Claude obliges by *interpreting the topic*, which means Claude picks one thing in the space of "parsing" — usually the most popular thing in its training data — and builds that. Sometimes the popular thing matches what you wanted. More often it does not. The student then re-prompts to nudge Claude toward the intended thing, and three rounds later the student has spent more time correcting Claude's interpretation than it would have taken to specify the task in the first place.

A strong specific task names the artifact: the function name, the file path, the return type, the language. *"Write `parse_grades(path: str) -> list[Grade]` in `parsers/grades.py`."* That is one sentence. It names the function. It names the file. It names the signature. There is no interpretive surface area; Claude either produces that artifact or it doesn't, and either outcome is detectable.

The discipline here is to **resist topic phrasing**. "Help me with the parser" is topic phrasing. "Write `parse_grades(path: str) -> list[Grade]`" is artifact phrasing. The difference between the two is the difference between a prompt that has a definite answer and one that does not.

White et al. (2023), in their *Prompt Pattern Catalog* of sixteen patterns derived from empirical work with GPT-4 and Claude predecessors, separate this move into what they call the **Output Automater** pattern: phrase the request as a *function* with named inputs and outputs, not as a topic. Their ablations — where the same task was tested with and without the Output Automater pattern — show the freeform version producing suggestions and explanations instead of executable code, even when the prompt was otherwise well-specified. The function-naming move is not stylistic. It changes what the model produces.

### 2. The Invariants

The invariants are what *must not change*. They are the load-bearing assumptions of the project — the decisions that, if violated, would force a redesign — written in the prompt so Claude inherits them rather than re-deriving them.

A weak prompt has zero invariants. The student assumes Claude knows what stays fixed. Claude does not. Claude makes a fresh guess every turn, weighted by what is popular in training data. Popular-in-training-data is often *not* what is true in this project. The popular CSV parser uses pandas. Seth's project does not. Without an invariant naming "standard library only — no pandas," Claude will reach for pandas every time, because that is what most public CSV parsers do.

A strong invariant is specific and *defensive*. It names the thing that must stay fixed and, ideally, the reason. *"Standard library only — no pandas. Quoted fields containing commas must parse correctly. UTF-8 BOM must be tolerated. Empty cells become `None`, not the empty string."* Four invariants. Each one names a decision the project has already made. Each one, if violated, would break something Seth has already built or specified.

The cognitive move that produces good invariants is **enumeration of what could drift**. Ask: across runs of this same prompt with different mood, time of day, and version of the model, what would I want to be *the same*? Anything that would *not* be the same without explicit instruction is an invariant. The exercise is uncomfortable at first because it forces the student to articulate decisions that have been implicit. Articulating them is the point. Once they are in writing they survive the prompt boundary, and they survive future prompts too if they migrate to `CLAUDE.md`.

There is a *thinness* in the published literature worth naming here. Empirical work on prompt-pattern ablations — White et al. (2023), Khattab et al. (2023) on DSPy — measures the contribution of structural moves like output formatting, persona, and chain-of-thought, but there are very few clean studies isolating the specific contribution of explicit invariants versus implicit assumption. The closest empirical anchor is DSPy's signature-based specifications, which produced ~25–65% absolute improvement on GSM8K and HotPotQA relative to ad-hoc prompts — but the signature format collapses invariants, output format, and task naming into one structural change, so the contribution of invariants alone is not separated. The chapter's claim that invariants matter is on solid footing in *practice* and in *expert opinion*; the precise ablation against an otherwise-identical prompt is, as of 2026, thin. Worth flagging honestly.

### 3. The Context

The context is the **set of facts the model needs in order to do the task correctly** and that the model does not already have. It is the SDD sections that govern this step. It is the file paths of related code. It is the sample input. It is the schema. In the chapter's vocabulary, the context is **working memory externalized** — the student is deciding what the model needs to know, and supplying it explicitly, rather than hoping the model will infer it.

A weak prompt assumes the model can see the project. The model cannot. The model sees only what is in the prompt and, in agent-mode tools like Claude Code, what it actively reads from the filesystem during the turn. The model has no memory of yesterday's session. It has no memory of the SDD you wrote last week. If you want the SDD in this turn's context, the prompt must point at it.

A strong context section names the artifacts and *where they live*. *"The `Grade` dataclass is defined in `models/grade.py`. SDD §1.4 specifies the file format. A sample input is at `data/grades_sample.csv`."* Three pointers. Each one tells Claude where to look. In Claude Code, "tells Claude where to look" is a literal operation — Claude will read the named files before producing output, and the contents of those files will appear in the model's context window.

Anthropic's own long-context guidance recommends a specific cognitive move when the context is large: **have the model quote the relevant parts of the context before producing the output**. The reason is mechanical — by the time the model is generating the answer, the relevant context has scrolled past in the attention pattern unless something has caused it to re-attend. Quoting forces re-attention. For student-scale prompts the long-context concern is usually overkill, but the principle is general: *naming what is relevant is not optional when the model has to choose what to attend to*.

### 4. The Output Format

The output format is **what done looks like**. It is the artifact, the destination, and the verification step. Not "write the parser." *"One Python file; then run `python -m pytest tests/test_parser.py -q` and report the exit code."* The output format names the deliverable and the success criterion in the same breath.

A weak prompt has no output format. The student gets back whatever Claude thought looked good — sometimes a file, sometimes a code block, sometimes a tutorial, sometimes all three. Each of these is *plausible* output. None of them is *the* output. The student then has to do the cognitive work of deciding which part is the artifact, which part is commentary, and what counts as done.

A strong output format pre-decides those questions. *"Single file at `parsers/grades.py`. Then run `pytest tests/test_parser.py -q` and report exit code."* Two sentences. The first names the deliverable. The second names the verification. The success criterion is now testable — not by Seth's judgment, but by the exit code of a command. A pass is a pass; a fail is a fail; "looks good" is not in the vocabulary.

This is also the place where structured-output modes and tool-use APIs have moved the format question from suggestion to contract. OpenAI's JSON-mode, Anthropic's tool definitions, and the typed signatures of DSPy (Khattab et al. 2023) all share a common move: **the output format is no longer a suggestion the model may or may not honor — it is a constraint enforced by the API**. For Claude Code, the analogous move is to specify *what file* and *what test command* in the prompt. The student is encoding the success criterion in machine-checkable form. The model is not being asked to judge whether the work is done. The exit code judges.

### 5. The Negative Constraint

The negative constraint is **what Claude must not do**. It is the scope discipline of the prompt — the explicit list of things that are out of scope and must not be touched. *"Do not modify `models/grade.py`. Do not add new dependencies. Do not touch any file outside `parsers/`."* Three sentences. Each one names a thing Claude has *opportunity* to do, and refuses it.

The negative constraint is the element students most often skip, because it feels like over-specification. "Why would I tell Claude not to do something it has no reason to do?" The answer is that Claude *does* have reason to do those things — the model has training data full of CSV parsers that import pandas, projects that update related models when parsers change, and refactors that touch adjacent files because that is what conscientious engineers do. Without an explicit "do not," Claude will sometimes do the conscientious thing. Sometimes is enough to cause harm.

A useful diagnostic: every negative constraint in your prompt should correspond to a thing Claude *plausibly might do* if you don't say not to. "Do not delete the database" is a useless negative constraint because Claude has no reason to delete the database in a parser-writing task; including it just adds noise. "Do not modify `models/grade.py`" is a *useful* negative constraint because Claude has reason to do exactly that — the parser's return type lives in that file, and Claude might decide the dataclass needs a new field to support the parse.

The empirical face of the negative constraint shows up in Anthropic's own context-engineering documentation: **agents drift when the success criterion is ambiguous, and without an explicit scope statement, agentic loops accumulate side effects**. The negative constraint is the per-turn antidote. Each "do not" closes a door Claude might otherwise wander through.

And here is the honest gap, repeated because it matters: the published literature is thin on **clean ablations of the negative-constraint element**. White et al. (2023) document that the negative-constraint pattern reduces unintended file edits in tested code-generation prompts, but they do not run the controlled comparison where the same prompt is tested with and without the negative constraint while every other element is held fixed. The case for negative constraints is strong in practice and strong in mechanism — agentic systems do drift without scope statements — but the precise quantitative effect size of *just* the negative-constraint element, isolated, is not yet on the table. A student who builds a small ablation experiment around this gap would have a publishable result.

## Why Claude Has No Memory Between Prompts

The five-element format presumes a fact about Claude that is worth stating directly, because the misconception that violates it is the most common reason students' prompts fail.

**Claude has no memory between prompts.** Every prompt is read fresh. Every turn is a new beginning from the model's point of view. The conversation that scrolls up your screen is, mechanically, the *context window* that the model re-reads every time you press Enter. Drop a fact out of the context window and it is gone from the model's awareness. Mention a fact in a prompt last week and then assume Claude remembers it this week and you are wrong — last week's prompt is not in this week's context, and the model has no separate memory store from which to retrieve it.

This is mechanical, not a deficiency. Transformer-based language models do not have a persistent state between API calls. The state is the context window. The context window is what you pass in. Nothing else exists.

The operational consequence for prompt writing is large. **Invariants must be re-supplied every time, or pointed at via a file that gets included in context.** The two strategies in 2026 are:

1. **Per-prompt restatement.** Repeat the invariants in every prompt. Cheap to type if there are two invariants; expensive if there are twenty.
2. **Project-context files.** Put the durable invariants in `CLAUDE.md` (or `.cursorrules`, or `AGENTS.md`), which Claude Code reads at the start of each session. The per-prompt invariants then become *deltas* — what's true for this prompt that wasn't already true for the project.

The right answer for student-scale projects is usually both. The project-wide invariants — "standard library only," "Python 3.11+," "tests live in `tests/`" — go in `CLAUDE.md`. The per-task invariants — "UTF-8 BOM must be tolerated," "empty cells become `None`" — go in the prompt itself. The union of the two is the *operative specification* for the turn.

The disputed question — flagged honestly because it is genuinely unsettled — is whether `CLAUDE.md` and per-prompt specifications are converging into the same artifact, with the prompt acting only as a pointer ("do task X under the project rules"), or whether the per-prompt specification will remain a distinct, fully-written artifact even as project-context files mature. The empirical answer in 2026 is that both patterns coexist; project files carry the durable parts, prompts carry the per-task parts, and the seam between them is a place where craft still matters.

## The Dangerous Middle

The most insidious failure mode in prompt writing is not the prompt with zero specification. That prompt fails *visibly* — Claude produces something obviously wrong, and the student knows to re-prompt. The failure mode that costs hours is the prompt that has **three or four of the five elements** and feels complete.

Three of five is the dangerous middle. The student wrote the specific task. The student named the output format. The student even listed two invariants. The student omitted the context and the negative constraint, because at the moment of writing the prompt those two felt obvious — *Claude knows about my SDD, right? Claude won't touch unrelated files, right?* Claude doesn't, and Claude will.

The output of a three-of-five prompt looks plausible. It compiles. It runs. It does *most* of the task. The defect — the wrong return type because the context was missing, the modified unrelated file because the negative constraint was missing — is buried one layer in. The student commits the change, moves on, and discovers the defect three hours later when the next piece of code can't find the function it was expecting.

The diagnostic for the dangerous middle is the **five-question self-check** below. Run it before pressing Enter. Five seconds of friction at the prompt-writing stage prevents three hours of debugging later. The asymmetry is unreasonable in your favor.

## Prompt Quality Self-Check

Before pressing Enter, ask:

1. **Have I named the one specific artifact?** (Function name, file path, return type, or equivalent. Topic phrasing — "help with X" — fails this check.)
2. **Have I listed the invariants?** (What must not change. Standard-library-only, signature stability, dependency policy, whatever is true and load-bearing for this project.)
3. **Have I pointed at the context?** (SDD section, related files, sample input. If Claude would need to read a file to do this correctly, name the file.)
4. **Have I named what done looks like?** (Output format and verification step. Bonus: is the verification step a command Claude can run?)
5. **Have I said what Claude must not do?** (Files not to touch, dependencies not to add, related code not to refactor.)

If any answer is no, the prompt is incomplete. The student who runs the self-check is the student who, three hours from now, is *not* debugging a defect that shouldn't have happened.

A subtler version of this check, worth introducing once the five questions are reflex: ask whether the prompt would be unambiguous to a *different* student who has not been in your head all week. If the answer is no, the prompt depends on context that exists in your head but not in the prompt. The model is in the position of the different student — it has not been in your head all week.

## Give Claude a Way to Verify Its Own Work

The fourth element — output format — has a deeper move that is worth its own section, because it is the single highest-leverage thing in this chapter.

**Whenever possible, include in the prompt a test, a linter, or a bash command Claude can run to check the output before reporting done.**

The reason is empirical. Claude iterating against a feedback mechanism produces significantly better results than single-pass generation. This is not a stylistic preference — it is a structural feature of how agentic language models work. Given a verifier (a test that fails, a lint error, an exit code), the model can detect that its first attempt was wrong, revise, and try again, often within the same turn. Without a verifier, the model has no signal that the first attempt was wrong, and "done" reduces to "I produced something that looks reasonable."

The practical translation:

- For code: include a test command. *"Run `pytest tests/test_parser.py -q` and report exit code."*
- For data validation: include an example input and its expected output. *"For `row = {...}`, output should be `[...]`."*
- For text: include a check the model can run. *"Verify that no sentence in the output exceeds 25 words by counting; report any violations."*
- For schemas: include a sample document and validate against it. *"Validate the produced JSON against `schema.json` using `jsonschema` and report errors."*

The cost is one line in the prompt. The benefit is that Claude's "done" is now anchored to a check, not to the model's self-evaluation. The student is no longer relying on Claude's judgment about whether the work is good. The exit code judges. The schema judges. The example judges.

This move shows up across the literature under different names. Anthropic calls it *tool-verifiable success criteria*. DSPy calls it *metric-driven optimization* — every signature has a metric attached, and the compiler optimizes against the metric. Promptbreeder (Fernando et al. 2023) uses task-specific verifiers to score evolved prompts; the verifier *is* what makes the evolution possible, because without a scoring function there is nothing to evolve toward. The unifying observation: **language models work better when they can grade themselves with something other than themselves**.

## Worked Example: Building the Grade Parser, Five Prompts Deep

The chapter has been abstract. Here is the concrete version. Seth's task: build the CSV parser for grades, end to end, in five specification-shaped prompts. Each prompt below shows all five elements. Each one builds on the previous. Read down the sequence and watch the line stay drawn.

**Prompt 1 — Define the data model.**

> *Specific task: write the `Grade` dataclass in `models/grade.py`. Fields: `student_id: str`, `assignment: str`, `score: float`, `out_of: float`, `submitted_at: datetime | None`. Invariants: `score` is non-negative; `out_of` is positive; `score <= out_of` is a class invariant enforced in `__post_init__`. Context: SDD §1.4 names these five fields as the canonical grade schema. Output format: one Python file; then run `python -c "from models.grade import Grade; print(Grade.__annotations__)"` and report the output. Negative constraint: do not add fields not listed in SDD §1.4; do not import any non-stdlib package; do not create other files.*

**Prompt 2 — Write the parser.**

> *Specific task: write `parse_grades(path: str) -> list[Grade]` in `parsers/grades.py`. Invariants: standard library only; quoted fields containing commas parse correctly; UTF-8 BOM is tolerated (use `encoding="utf-8-sig"`); empty cells become `None`, not `""`; `submitted_at` parses ISO-8601 timestamps or `None`. Context: `Grade` is defined in `models/grade.py`; SDD §1.4 specifies the file format; sample at `data/grades_sample.csv`. Output format: one Python file; then run `python -c "from parsers.grades import parse_grades; print(len(parse_grades('data/grades_sample.csv')))"` and report the integer printed. Negative constraint: do not modify `models/grade.py`; do not add dependencies; do not touch any file outside `parsers/`.*

**Prompt 3 — Write the tests.**

> *Specific task: write `tests/test_parser.py` with `pytest`-style functions covering: (a) a 3-row sample parses to 3 `Grade` instances; (b) a row with a quoted comma in the assignment field parses correctly; (c) a UTF-8 BOM at the start of the file is tolerated; (d) an empty `submitted_at` cell becomes `None`; (e) the class invariant `score <= out_of` is enforced. Invariants: pytest only; one assert per test; use temporary CSV files created in fixtures, do not edit `data/grades_sample.csv`. Context: `parse_grades` is in `parsers/grades.py`; `Grade` is in `models/grade.py`; SDD §7.2 specifies the test policy. Output format: one Python file; then run `pytest tests/test_parser.py -q` and report exit code and number of tests collected. Negative constraint: do not modify the parser or the model; do not skip any test; do not add mocks for I/O — use real temp files.*

**Prompt 4 — Handle an edge case revealed by the tests.**

> *Specific task: in `parsers/grades.py`, fix the bug revealed by `test_score_with_trailing_whitespace` (currently failing). The cause is that the `score` column is being stripped before float conversion in some code paths but not others. Invariants: existing passing tests must continue to pass; the function signature does not change; standard library only. Context: see the failing test in `tests/test_parser.py`; the bug is localized to `parsers/grades.py`. Output format: one modified file; then run `pytest tests/test_parser.py -q` and report exit code; all tests must pass. Negative constraint: do not modify the test; do not modify `models/grade.py`; do not refactor unrelated code paths in `parsers/grades.py`.*

**Prompt 5 — Document the parser interface in the SDD.**

> *Specific task: add a new subsection `§1.4.1 — Grade parser interface` to `SDD.md`. Contents: function signature, file path, exact list of CSV format rules supported, exact list of CSV format rules NOT supported. Invariants: SDD section numbering is sequential; the parser's actual behavior, not aspirational behavior, is what gets documented. Context: the parser is in `parsers/grades.py`; the tests are in `tests/test_parser.py`; the existing SDD is at `SDD.md`. Output format: one diff against `SDD.md`; then re-read the subsection and confirm it accurately describes what the code does. Negative constraint: do not modify code; do not renumber existing SDD sections; do not add aspirational features the parser does not yet support.*

Five prompts. Each one specifies one artifact. Each one names invariants, context, output format, and negative constraint. Each one builds on the previous *without* assuming Claude remembers the previous — the relevant facts are re-supplied or pointed at.

Notice what is *not* in these prompts. There is no "please." There is no "I would like." There is no "if you could." The tone is not curt — it is *specified*. The same tone the SDD has. The same tone an IEEE standard has. The tone is a side effect of the precision; once the precision is there, the politeness becomes noise.

Notice also what is *in* each prompt: a way for Claude to verify the output. A `python -c` one-liner, a pytest command, a re-read of the produced subsection. Five prompts, five verifiers. The cost of including each verifier is one line. The benefit is that "done" means something specific in every case.

## Exercises

1. **(Apply)** Open your Claude history from the last week. Pull out three prompts you actually sent. For each one, rewrite it as a complete five-element specification. Compare side by side. Note in writing — one sentence per prompt — which element you had been omitting and what that omission was costing you.

2. **(Analyze)** Below is a prompt that is missing two of the five elements. Identify which two are missing. For each missing element, write one sentence describing what Claude is likely to do wrong as a result.

   > *Write a function `summarize_grades(grades: list[Grade]) -> dict` that returns the mean, median, and standard deviation of the `score / out_of` ratios. Invariants: ignore grades where `out_of == 0`; use only the standard library. Context: `Grade` is defined in `models/grade.py`.*

   (After identifying the missing elements, rewrite the prompt as a complete five-element specification.)

3. **(Create)** Identify the next step in your current project. Write a complete five-element specification for it. Then run the five-question self-check on your own specification. If any question gets a "no," revise until all five get a "yes." Send the prompt to Claude. Compare what came back to your output-format and negative-constraint elements. Note any drift in writing — what did Claude do that your specification did not authorize?

## 🕰️ AI Wayback Machine

**Ada Lovelace** (1815–1852) was the first person to write what we would today recognize as a computer program — and, more pointedly for this chapter, the first person to write what we would recognize as a *specification*. Her Notes on Menabrea's *Sketch of the Analytical Engine* (Lovelace 1843), and particularly Note G, contain a precise, indexed table of operations for computing the Bernoulli numbers on a machine that did not yet exist. The table names the variables, the operations on them, the order of operations, the dependencies between steps, and the expected outputs. It is a five-element specification written 183 years ago: the artifact (Bernoulli number computation), the invariants (the variable indices, the operation order), the context (the engine's architecture as described in Notes A–F), the output (a column of results), and — implicit in the table's rigor — the negative constraint that the machine must perform *these* operations, in *this* order, and no others. The five-element format you have learned in this chapter descends in a straight line from Lovelace's table. The machine has changed. The discipline has not.

**Run this:**

> *Specific task: write `bernoulli(n: int) -> Fraction` in `math/bernoulli.py` returning the nth Bernoulli number. Invariants: input is a non-negative integer; return type is `fractions.Fraction`; B(1) = +1/2 (the convention used in Lovelace's Note G); standard library only. Context: SDD §2 specifies the math module's interface; reference values for n=0..8 are in `data/bernoulli_reference.json`. Output format: one Python file; then run `python -c "from math.bernoulli import bernoulli; [print(n, bernoulli(n)) for n in range(9)]"` and compare against `data/bernoulli_reference.json`; report any mismatches. Negative constraint: do not import `sympy` or any non-stdlib package; do not modify the reference data; do not implement only the first few cases and stop.*

## Bridge

The reader can write specifications. Chapter 9 addresses what happens when the specification is right and the output is still wrong.
