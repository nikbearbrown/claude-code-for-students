# claude-code-for-students-a-practitioners-guide
## Full TOC Draft — All Phase Outputs Compiled

**Working title:** Claude Code for Students: A Practitioner's Guide
**Series:** Practitioner Guides for the AI Classroom · Bear Brown & Company
**Author:** Nik Bear Brown · bear@bearbrown.co · Bear Brown, LLC
**Co-author:** Seth Brown
**Document:** Full TOC Draft — compiled from all phase outputs
**Version:** 1.0
**Status:** Pre-production — ready for chapter drafting

---

## Document structure

1. Book Concept and Thesis
2. Learner Profile
3. Book Type and Deployment Specification
4. Field Positioning
5. Three-Act Learning Arc
6. Prerequisite Map
7. Build Arc and Terminal Deliverables
8. Chapter-by-Chapter TOC
9. Chapter Anatomy Template
10. Case Study Strategy
11. Hard Topics, Contested Claims, Aging Risk
12. Market Positioning
13. Feature List
14. Out of Scope
15. Adoption Risk Register
16. Open Questions

---

# PART 1 — BOOK CONCEPT AND THESIS

## Book concept summary

> This book teaches **technically fluent high school students how to
> use Claude Code as a Conductor** — directing the machine through
> real builds while protecting the cognitive struggle that builds
> their own capability — **by showing exactly where to draw the line
> between what Claude executes and what only the student can do**,
> through the story of a student who learned the discipline by
> practicing it. It fills the gap left by generic AI literacy courses
> (which treat students as problems to manage) and prompt engineering
> guides (which optimize for output, not for learning). It succeeds
> when the reader can **run a complex build with Claude Code and
> finish it knowing more than when they started, not less**.

**One-sentence logline:**
The student who learns to conduct Claude Code builds faster and
thinks better; the student who lets it run unattended builds a
convincing artifact and an atrophying mind.

## Central thesis

"This book argues that Claude Code is not a shortcut but an
instrument — that the student who draws an explicit line between
what Claude executes and what only the human can do will build
real capability, while the student who delegates without that line
will produce polished output while their own thinking atrophies,
and this matters because the homework/quiz gap is already visible
in every classroom where AI tools are used without a conducting
discipline."

## Thesis test

The TOC reflects the thesis at every act:

- ACT ONE: Seth watches his friends ace homework and freeze on
  quizzes. The reader feels the cost before they need the solution. ✓
- ACT TWO: The Boondoggling framework, the five supervisory
  capacities, Gru, CLAUDE.md, Brutalist — the line is drawn
  operationally, tool by tool. ✓
- ACT THREE: The reader runs their own first fully Boondoggled
  build, from /v0 through verified output. The discipline is
  practiced, not described. ✓

**Thesis test: PASS**

---

# PART 2 — LEARNER PROFILE

## Primary reader

A technically fluent high school student, 2026. Already uses
Claude daily. Ahead of teachers on tooling. Watching friends
borrow capability they haven't built. Technically fluent,
domain-shallow, honest enough about the gap to want a discipline
that closes it.

**Specific person:** Seth Brown — the Cautious Builder. AP
Computer Science student who noticed that his friends were acing
homework with AI and freezing on unassisted quizzes. He reached
out not for a lecture on plagiarism but for a concrete operational
discipline that would let him build real software with Claude Code
without hollowing out his own thinking. He is now co-authoring
this book by practicing the discipline as he writes it.

## Prior knowledge assumed

- Basic prompt engineering: how to get Claude to produce code
- Familiarity with Claude.ai or similar conversational AI
- Some coding experience (AP CS level or equivalent)
- Can run a terminal command if given the exact syntax

## Prior knowledge NOT assumed

- Claude Code in the terminal
- CLAUDE.md, Skills, Hooks, Subagents
- The five supervisory capacities by name
- The Boondoggling framework
- Software Design Documents
- Any formal software engineering methodology

## Prior misconceptions (what they think they know that is wrong)

1. "If Claude writes correct code, I learned something" — the
   Bastani RCT shows the opposite: 48% higher practice scores,
   17 percentage points lower on the unassisted exam
2. "Using AI more is always better" — the Kosmyna EEG study
   shows up to 55% reduction in brain connectivity during
   AI-assisted writing vs. brain-only writing
3. "I'm ahead of my teachers on AI, so I know enough" — technical
   fluency without domain depth is the specific danger zone; the
   student who can run Claude cannot necessarily audit it
4. "Claude Code is just a better chatbot" — it is an agentic
   system that reads files, runs commands, and makes changes;
   the autonomy is the feature and the risk simultaneously

## Motivation type

**Primary:** Intellectual — Seth and his reader have already
noticed the problem and want the repair kit.
**Secondary:** Professional — they want to build real things, not
just homework assignments, and the conducting discipline is what
makes that possible.
**Not primary:** Academic (course requirement) — this book is
found independently, not assigned. The school adoption play is
secondary.

## Prerequisite map

| Prerequisite | Safe to assume? | If not: where addressed |
|---|---|---|
| Basic Claude prompting | Yes | — |
| Some coding experience | Yes | — |
| Terminal familiarity | Probably | Ch 1 first session walkthrough |
| Claude Code installed | No | Ch 1 installation |
| CLAUDE.md concept | No | Ch 6 (Gru chapter) |
| SDD concept | No | Ch 7 (SDD chapter) |
| Boondoggling framework | No | Ch 4 (conducting chapter) |
| Five supervisory capacities | No | Ch 5 (capacities chapter) |

**Front-loading decision:** Terminal installation is addressed in
Chapter 1 as primary instruction. No prerequisite chapter required
— the one technical gap is covered at first use with exact
commands provided.

---

# PART 3 — BOOK TYPE AND DEPLOYMENT SPECIFICATION

## Book type

**PRIMARY TYPE:** Practitioner handbook with course-textbook
bones. The student reads it cover to cover once, then consults
it chapter by chapter during builds.

**NOT:** A course textbook adopted for a semester (though it can
be assigned); a field-defining monograph (chapters build skills,
not an argument); a reference work (the sequence dependency is
too high for random access).

## Deployment specification

**Primary adoption context:**
Self-directed high school student finding it independently —
through recommendation, through the $1 Kindle price, through
a teacher who assigned it recognizing their technically fluent
students in Seth's story. Not a classroom adoption play primary.

**Secondary adoption context:**
K-12 CS teachers using it alongside
`claude-code-for-teachers-a-practitioners-guide` — the teachers
book references this book explicitly and at $1 on Kindle, it is
assignable without a budget conversation.

**Tertiary adoption context:**
University intro CS courses that want to address AI-assisted
coding discipline in the first semester.

**What the book is NOT designed for:**
Generic AI literacy courses; students who want prompt engineering
tips; courses where the teacher wants to ban AI use.

**How the TOC signals book type to a reader:**
Three-act structure with Seth's story as the vehicle, terminal
deliverable named on page one, $1 Kindle price signals
accessibility. A student who opens this book knows in the first
chapter whether they are the reader it was written for.

**Price point:** $1 Kindle — designed for assignment without
budget friction. Print edition at standard series pricing.

---

# PART 4 — FIELD POSITIONING

## The positioning statement — consolidated

The competitive landscape has a clean gap:

- Generic AI literacy courses treat students as problems to
  manage. This book treats them as builders to equip.
- Prompt engineering guides optimize for output quality.
  This book optimizes for capability retention.
- The Vanderbilt/Coursera Claude Code course targets developers.
  This book targets students.
- No existing book takes a technically fluent student seriously,
  hands them a production discipline, and is co-authored by one
  of them.

## Positioning statements by comparable

**vs. generic AI ethics / responsible use curriculum:**
"Unlike responsible AI use curricula, which address students as
potential plagiarists to be managed, this book addresses students
as builders who need a concrete operational discipline — the
Boondoggling framework — that protects their own learning while
enabling ambitious builds."

**vs. prompt engineering guides:**
"Unlike prompt engineering guides, which teach students to get
better outputs from AI, this book teaches students to protect
their own thinking while using AI — the discipline that makes
the output worth having."

**vs. Vanderbilt/Coursera Claude Code:**
"Unlike the Vanderbilt Claude Code course, which teaches
developers to delegate like a tech lead, this book teaches
students to conduct like a composer — preserving the cognitive
struggle that builds capability while directing the machine
through real builds."

**vs. the teachers book in this series:**
"This book is the student's half of a two-book pair.
`claude-code-for-teachers` is the teacher's half. Together they
give both sides of the classroom the same discipline in
role-appropriate form. At $1 on Kindle, this book is assignable
by any teacher who reads the companion."

## Research validation

The Anthropic RCT (January 2026) provides the empirical
foundation: 17 percentage point decline in conceptual test
performance for AI-assisted groups vs. hand-coders (Cohen's
d = 0.738, p = 0.01). The three low-scoring interaction
patterns (AI Delegation, Progressive AI Reliance, Iterative
AI Debugging) are the behaviors this book's discipline prevents.

The Bastani finding and Kosmyna EEG study (up to 55% reduction
in brain connectivity during AI-assisted writing) ground the
neurobiological argument in Chapter 1.

---

# PART 5 — THREE-ACT LEARNING ARC

## The arc statement

This book takes the reader from **technically fluent but
cognitively unguarded** to **disciplined conductor** — first
by naming the risk Seth saw in his friends before he had
vocabulary for it, then by building the operational framework
piece by piece through real build tools, then by running a
complete project from problem formulation through verified
output using the full discipline.

## The pebble-in-the-pond opening

Chapter 0 gives the reader Seth in AP Computer Science, watching
a friend rip through a problem set and freeze on the quiz.
Chapter 1 gives them the neurobiological mechanism — the Bastani
finding and Kosmyna EEG study. The problem is felt before the
framework is named. Act One ends with the reader understanding
exactly what is at stake before Act Two gives them the tools.

## Act One — The Problem (Chapters 0–3)

**Starting state:** The reader is technically fluent but has no
discipline for where to draw the line. They may not yet know
the line exists.
**Ending state:** The reader understands the homework/quiz gap,
the teacher-student AI gap, and why technical fluency without
domain depth is the specific danger zone.
**Inciting question:** "If I can get Claude to write the code,
why am I learning anything?"
**Act One → Act Two transition:** The reader must feel the cost
of undisciplined AI use and want a concrete alternative — not
just "AI is risky" but "this specific thing I do is costing me
something I can't see yet."

## Act Two — The Discipline (Chapters 4–10)

**Starting state:** The reader wants the discipline but has no
framework.
**Ending state:** The reader can run a structured build with
explicit handoff conditions, naming which cognitive work they
keep and which they delegate.
**Hardest conceptual moment:** Chapter 9 — the dangerous middle.
The moment Claude produces output that passes every handoff
condition the student wrote and is still wrong. This is the
Act Two crisis.
**Act Two → Act Three transition:** The reader must be able to
state, for any build step, whether it belongs to Claude or to
them, and why. They have the full toolkit. Now they use it.

## Act Three — The Build (Chapters 11–14)

**Starting state:** The reader has the framework but has not
applied it to a complete real project.
**Ending state:** The reader has completed their first fully
Boondoggled build — planned with an SDD, executed with a
Boondoggle Score, verified against SDD needs, documented in
a post-build learning record.
**Terminal deliverable:** Not Seth's build. The reader's build.
The book ends with the reader, not Seth. Seth's arc closes at
the bridge between Act Two and Act Three. The reader owns the
final chapter.

## Act Two → Act Three bridge

No formal bridge chapter (unlike the teachers book). Seth's
retrospective voice in Chapter 11 carries the transition: the
moment his builds stopped being about homework and started
being about what he could make that didn't exist before.

---

# PART 6 — PREREQUISITE MAP

## Prerequisite chain by chapter

| Chapter | Prerequisite capabilities | Source |
|---|---|---|
| Ch 0 (Introduction) | None | — |
| Ch 1 (Homework/Quiz Gap) | None | — |
| Ch 2 (Division of Labor) | Ch 1 risk established | Ch 1 |
| Ch 3 (Teacher-Student Gap) | Ch 1 + Ch 2 | Ch 1–2 |
| Ch 4 (Conducting) | Ch 1–3 problem understood | Ch 1–3 |
| Ch 5 (Five Capacities) | Ch 4 conducting metaphor | Ch 4 |
| Ch 6 (Gru) | Ch 4–5 framework | Ch 4–5 |
| Ch 7 (SDD) | Ch 6 Gru installed | Ch 6 |
| Ch 8 (Specifications) | Ch 7 SDD concept | Ch 7 |
| Ch 9 (Handoff Conditions) | Ch 8 specifications | Ch 8 |
| Ch 10 (Brutalist) | Ch 4–9 full framework | Ch 4–9 |
| Ch 11 (Planning) | Ch 4–10 full framework | Ch 4–10 |
| Ch 12 (Running the Build) | Ch 11 plan complete | Ch 11 |
| Ch 13 (Verification) | Ch 12 build in progress | Ch 12 |
| Ch 14 (Full Build) | Ch 11–13 full sequence | Ch 11–13 |

## Load-bearing chapters

Chapter 4 (Conducting): if skipped, every subsequent chapter
loses its organizing metaphor.

Chapter 5 (Five Capacities): if skipped, the Boondoggle Score
in Chapter 6 cannot be interpreted.

Chapter 7 (SDD): if skipped, the Boondoggle Score in Chapter 11
has no document to generate from.

Chapter 11 (Planning): the transition chapter. If skipped,
Chapters 12–14 have no plan to execute.

---

# PART 7 — BUILD ARC AND TERMINAL DELIVERABLES

## The three build tiers

This is a practitioner handbook. Assessment is structured around
builds, not grades. Three tiers of increasing complexity run
across the book's arc.

| Tier | Chapters | Build | Complexity | Terminal artifact |
|---|---|---|---|---|
| Foundations | 1–3 | None — observation only | — | Personal AI audit (Ch 3 exercise) |
| Framework | 4–10 | Tool-level exercises | Low-Medium | Labor separation rule (Ch 2); DESIGN.md (Ch 10) |
| Full build | 11–14 | Complete Boondoggled project | High | Post-build learning document (Ch 14) |

## Terminal deliverable specification

**The reader's first fully Boondoggled build:**

Required components:
1. `/v0` one-sentence problem formulation
2. Minimum viable SDD (five core sections)
3. Boondoggle Score for Phase 1
4. Build execution log (specification per step, handoff
   condition evaluation per step, supervisory capacity label
   per step)
5. Verification pass against SDD needs
6. Post-build learning document (five sections)

**Post-build learning document — five sections:**
1. What I built (one paragraph, plain language)
2. What I delegated to Claude and why
3. What I kept for myself and why
4. What I learned that I didn't know before
5. What I would do differently

**Success criterion:** Not a perfect build. A build where the
reader can account for every decision — which cognitive work
was Claude's, which was theirs, and what the handoff condition
was at every step.

## Chapter exercise structure

Every chapter has three assessable exercises minimum:
- One at Apply level or above (Bloom's)
- One requiring the student to produce something (not just answer)
- One connecting the chapter concept to their own current project

Bloom's distribution across the full book:
- Remember/Understand: Ch 0–3 (establishing the problem)
- Apply: Ch 4–9 (framework tools)
- Analyze/Evaluate: Ch 9–13 (dangerous middle, verification)
- Create: Ch 11–14 (full build)

---

# PART 8 — CHAPTER-BY-CHAPTER TOC

---

## ACT ONE — THE PROBLEM
*Chapters 0–3: From technically fluent to cognitively aware*

---

### Chapter 0 — Introduction: The Cautious Builder

**One-line:** Meet Seth. He noticed something his friends didn't.

**Learning outcomes:**
1. (Remember) Name the difference between borrowing capability
   and building it.
2. (Understand) Explain why a high homework grade with a low
   quiz grade is a signal, not a coincidence.
3. (Understand) Describe what "conducting" Claude Code means
   vs. letting it run.

**Opening:** Seth in AP Computer Science, watching a friend rip
through a problem set in thirty seconds. The friend gets an A on
the homework. Two weeks later, same friend freezes on the in-class
quiz. Seth already knows something is wrong. He doesn't yet know
what to call it.

**Core content:**
- Seth's observation: the homework/quiz gap
- Why fluency with the tool is not the same as fluency in the domain
- What this book is and is not (not AI ethics, not prompt tips)
- How to read this book: Seth's voice and your own experience side
  by side

<!-- → [DIAGRAM: Seth's arc from observer to practitioner — simple
two-point timeline showing "watches friends" → "builds the discipline".
Minimal. Editorial style. No color.] -->

**Assessable exercises:**
1. (Remember) Before reading Chapter 1: write down three things
   you have built with AI in the last month. For each: could you
   explain every decision in it to someone who has never seen the
   code?
2. (Understand) What is the difference between using a calculator
   and learning arithmetic? Write one paragraph. Keep it.
   You will return to it in Chapter 14.
3. (Understand) Name one thing Seth noticed that you have also
   noticed in your own class. One sentence.

**Wayback Machine:** 🕰️ **Norbert Wiener** (1894–1964) — founder of
cybernetics; the first systematic thinker about the feedback loop
between human and machine. His question — what does the machine do
to the human who uses it? — is the question this chapter asks.

**Bridge:** The feeling Seth has is real. Chapter 1 gives it a
name, a number, and a neurobiological mechanism.

---

### Chapter 1 — The Homework/Quiz Gap: What's Actually Happening

**One-line:** Students who use AI freely during practice score
dramatically lower on unassisted tests — and feel like they
learned more, not less.

**Learning outcomes:**
1. (Understand) Explain why AI-assisted practice can produce the
   feeling of mastery without the cognitive events that constitute it.
2. (Analyze) Distinguish between capability borrowed from the
   machine and capability built in the learner.
3. (Evaluate) Assess their own recent AI use against this
   distinction.

**Opening:** The Bastani finding stated plainly: 48% higher scores
during AI-assisted practice, 17 percentage points lower on the
unassisted exam. Not slightly worse. Dramatically worse.

<!-- → [TABLE: Bastani RCT results — two columns: AI-Assisted group
vs. Hand-Coding group. Rows: practice score, exam score, score gap,
Cohen's d, p-value. No color. Editorial style.] -->

**Core content:**
- What actually happens in the brain when you struggle with a
  problem (plain language — no neuroscience jargon)
- What happens when the AI does the struggling for you
- The fluency trap: AI output that looks finished and feels like
  understanding
- The Kosmyna EEG result: up to 55% reduction in brain
  connectivity during AI-assisted writing
- The three low-scoring interaction patterns from the Anthropic
  RCT: AI Delegation, Progressive AI Reliance, Iterative
  AI Debugging
- The debugging gap: why the largest performance disparity
  appears specifically on diagnostic questions
- Start with questions before code: before your first build
  session, spend time asking Claude questions about the codebase.
  This teaches you the boundary — what Claude can one-shot, what
  needs precise instruction — before you delegate anything.

<!-- → [DIAGRAM: The fluency trap — a simple two-path diagram.
Path A: struggle → consolidation → durable capability.
Path B: delegate → fluent output → no consolidation → atrophy.
Editorial style. No color.] -->

**Worked example:** Two students, same problem set, same final
grade. One builds a database schema by working through it. One
asks Claude for the schema and reads it. Six weeks later: one
can design a database. One can describe what a database looks like.

**Assessable exercises:**
1. (Apply) Identify three recent AI interactions of your own.
   For each: did you build the capability or borrow it?
2. (Analyze) Given two assignment transcripts (provided), identify
   which student is building and which is borrowing. Defend.
3. (Evaluate) Design a rule for your own AI use that would prevent
   the homework/quiz gap in your next unit.

**Wayback Machine:** 🕰️ **William James** (1842–1910) — psychologist
who first described habit formation as the nervous system's
mechanism for consolidating repeated struggle into durable
capability. His 1890 account of what repetition does to the
brain is the mechanism the Bastani finding measures.

**Bridge:** The reader knows the risk. They don't yet know which
specific cognitive capacities are at stake — or that Claude is
structurally unable to supply them.

---

### Chapter 2 — What You're Actually Good At (And What Claude Is Better At)

**One-line:** Pattern recognition is Claude's domain. Supervisory
intelligence is yours. Knowing which is which is the whole game.

**Learning outcomes:**
1. (Understand) Distinguish pattern recognition (where AI is
   superhuman) from supervisory intelligence (where AI is
   structurally weak).
2. (Apply) Classify a set of build tasks as Claude work or human
   work.
3. (Analyze) Identify the specific supervisory capacity being
   exercised at a given step in a build.

**Opening:** Seth asks Claude to write a function. Claude produces
something that compiles, passes basic tests, and looks correct.
Seth runs it. Edge case: silent failure. Claude didn't know the
edge case existed. Seth almost didn't catch it. That near-miss
is the chapter.

<!-- → [TABLE: Division of labor — two columns: Claude does /
Human does. Rows: pattern completion, code generation, syntax
resolution, test execution (Claude) vs. plausibility auditing,
problem formulation, interpretive judgment, tool orchestration,
executive integration (Human). No color.] -->

**Core content:**
- What pattern recognition means and why Claude is superhuman at it
- What supervisory intelligence means: the five capacities in
  plain language (named formally in Chapter 5)
- Why AI is structurally weak at supervision: same weights that
  produced the output are doing the audit
- The solve-verify asymmetry: Claude solves faster, that gap
  won't close; verification against domain reality is
  irreducibly human
- The dangerous middle: tasks that look like pattern work but
  require supervisory judgment

<!-- → [DIAGRAM: The solve-verify asymmetry — simple timeline.
Claude's solve speed increasing over time. Human verification
capacity stable. The gap widens. The human's job is not to
solve faster but to verify better.] -->

**Worked example:** The same build step analyzed twice — once
with Claude running unattended, once with the student exercising
each supervisory capacity explicitly. Same Claude output.
Different results.

**Assessable exercises:**
1. (Apply) Given a list of 10 build tasks, classify each as
   Claude work, human work, or "dangerous middle."
2. (Analyze) Read a provided Claude transcript. Identify every
   moment where a supervisory capacity should have been exercised
   but wasn't.
3. (Create) Write your own labor separation rule for a project
   you are currently working on.

**Links:** [irreducibly.xyz](https://irreducibly.xyz) — the full
seven-tier taxonomy for readers who want the complete architecture.

**Wayback Machine:** 🕰️ **Frederick Winslow Taylor** (1856–1915) —
the first systematic analyst of the division of labor between
human judgment and mechanical execution. His question — which
cognitive work belongs to the engineer and which to the machine?
— is the question this chapter answers for AI.

**Bridge:** The reader can name the capacities. Chapter 3 explains
why school isn't teaching them — and why the student is on their
own.

---

### Chapter 3 — The Teacher-Student AI Gap: Why You're On Your Own

**One-line:** You know more than your teachers about the tools,
and less than you need to about the domains. That gap is exactly
where AI is most dangerous.

**Learning outcomes:**
1. (Understand) Explain the teacher-student AI gap and why it
   produces a specific kind of risk for technically fluent students.
2. (Analyze) Distinguish technical fluency (knowing how to run
   the tool) from domain depth (knowing when the tool is wrong).
3. (Evaluate) Assess their own domain depth in a subject they
   use AI for regularly.

**Opening:** The frustration named directly. Your teachers are
trying to ban the thing you already know better than they do.
The curriculum is three years behind the tools. You're on your
own — which means you need a discipline that works without
institutional scaffolding.

**Core content:**
- Why technical fluency without domain depth is the specific
  danger zone
- The hallucination problem stated precisely: Claude produces
  confident output in domains where the student has no basis
  to audit it
- Nicholas's observation: polished output with no soul, no
  aesthetic stance, no genuine human intent — the creative
  version of the same problem
- Why the answer is not to use AI less but to build the
  supervisory capacity that makes AI use safe
- What "domain depth" means in practice and how you build it
  (the struggle is the mechanism)

**Worked example:** Seth encounters a Claude explanation of a
concept he doesn't know well enough to audit. He accepts it.
It's wrong. He doesn't find out until the test.

**Assessable exercises:**
1. (Apply) Choose a subject where you use AI regularly. List
   three claims Claude has made that you accepted without
   verification. Go verify them now.
2. (Analyze) Identify a domain where your depth is sufficient
   to audit Claude and a domain where it isn't. What's the
   difference?
3. (Evaluate) Design a personal audit protocol for Claude
   outputs in your weakest domain.

**Wayback Machine:** 🕰️ **Ivan Illich** (1926–2002) — philosopher
who argued in *Tools for Conviviality* (1973) that tools become
counterproductive when they outpace the human capacity to use
them wisely. His concept of "counter-productivity" is the
teacher-student gap applied to institutions.

**Bridge:** The reader understands the problem completely. They
are ready for the solution. Chapter 4 introduces conducting.

---

## ACT TWO — THE DISCIPLINE
*Chapters 4–10: From awareness to operational framework*

---

### Chapter 4 — Conducting, Not Prompting: The Core Idea

**One-line:** Programming as conducting. Claude does what it's
superhuman at. You do what only you can.

**Learning outcomes:**
1. (Understand) Explain the difference between prompting Claude
   and conducting a build with Claude.
2. (Apply) Identify whether a given interaction is prompting
   or conducting.
3. (Understand) Explain what a handoff condition is and why
   it matters.

**Opening:** The orchestra metaphor in Seth's voice. You are
the conductor. Claude is the orchestra. The orchestra is
excellent. They will play exactly what they understood you to
mean. The gap between what you meant and what they understood
is where everything breaks.

<!-- → [DIAGRAM: The conductor/orchestra model. Human = conductor
(holds intent, sequences, verifies). Claude = orchestra (executes
specifications, returns output). Between them: specification +
handoff condition. Editorial style.] -->

**Core content:**
- The difference between a prompt and a specification
- What a handoff condition is: not "looks good" but a specific,
  testable condition that must be true before the next step begins
- Why dependency order matters: some Claude tasks cannot run until
  a human task is complete
- The Boondoggling name and origin
- The Minion Part and the Gru Part: Claude's tasks and your tasks,
  sequenced separately
- The Explore → Plan → Implement → Commit workflow: before your
  first specification, use plan mode to read and understand before
  touching anything

**Worked example:** One build step done twice. First: "write me
a login function." Second: a complete specification with
invariants, field names, output format, what not to touch, and
a handoff condition. Same Claude. Completely different results.

**Assessable exercises:**
1. (Apply) Take a prompt you've used in the past week. Rewrite
   it as a specification using the format introduced here.
2. (Apply) Write a handoff condition for a Claude task in a
   current project.
3. (Analyze) Given a provided Claude transcript, identify where
   the handoff condition was missing and what broke as a result.

**Links:** [boondoggling.ai](https://boondoggling.ai)

**Wayback Machine:** 🕰️ **Herbert Simon** (1916–2001) — Nobel
laureate who formalized the concept of bounded rationality and
satisficing: the human decision-maker who works within real
cognitive limits by designing systems that extend those limits.
The Boondoggling framework is bounded rationality applied to
AI-assisted development.

**Bridge:** The reader has the metaphor and the basic mechanics.
Chapter 5 names the five things the human must never delegate.

---

### Chapter 5 — The Five Supervisory Capacities

**One-line:** These are the five things you do that Claude cannot.
Name them. Practice them. Never delegate them.

**Learning outcomes:**
1. (Remember) Name and define the five supervisory capacities.
2. (Apply) Identify which supervisory capacity is being exercised
   at each step of a provided build sequence.
3. (Analyze) Diagnose a build that went wrong by identifying
   which supervisory capacity was absent.

**Opening:** Seth mid-build. Something is wrong with Claude's
output but he can't tell what. It compiles. It seems fine. He
almost ships it. The chapter asks: what capacity would have
caught this? Answer: plausibility auditing.

<!-- → [DIAGRAM: The five supervisory capacities as a pentagon or
five-column layout. Each: label (PA, PF, TO, IJ, EI), plain-
language name, one-sentence definition. Editorial style.
No color.] -->

**Core content:**

**[PA] Plausibility Auditing:** Hearing the wrong note before
verification. "This compiles and the tests pass. But why does
it feel wrong?" Checking output against domain knowledge that
isn't in the prompt.

**[PF] Problem Formulation:** Deciding what the build IS before
Claude sees it. Claude optimizes within the frame you give it.
If the frame is wrong, the output is wrong, elegantly.

**[TO] Tool Orchestration:** Choosing which Claude task, in what
order, with what trust level. The sequencing that makes a good
result possible.

**[IJ] Interpretive Judgment:** Supplying meaning Claude's output
cannot carry. What does this result mean for this specific project,
this specific deadline, this specific user?

**[EI] Executive Integration:** Holding the whole build toward
a single goal. "Three prompts ago we agreed on an architecture.
This new output is undermining it. Stop."

<!-- → [TABLE: Five supervisory capacities — three columns:
Label / Plain name / What it catches. Five rows. No color.] -->

**Worked example:** A complete build sequence analyzed step by
step, with each supervisory capacity labeled where it appears.

**Assessable exercises:**
1. (Apply) Label the supervisory capacity required at each step
   of a provided build plan.
2. (Analyze) A build transcript is provided where one supervisory
   capacity is systematically missing. Identify which one and
   trace the consequences.
3. (Create) Design a personal checklist for a current project
   using the five capacity labels.

**Wayback Machine:** 🕰️ **Douglas Engelbart** (1925–2013) —
inventor of the mouse and pioneer of human-computer interaction
who spent his career asking: how do we augment human intellect,
not replace it? His 1962 framework for augmenting human
capability is the conceptual ancestor of the five supervisory
capacities.

**Bridge:** The reader knows the five capacities. Chapter 6
introduces the tool that makes exercising them systematic: Gru.

---

### Chapter 6 — Gru: The Tool Built for This

**One-line:** Gru is what happens when the Boondoggling framework
is built into a Claude Project. Here's what it does and why it
was built.

**Learning outcomes:**
1. (Understand) Explain what Gru does that a plain Claude
   conversation does not.
2. (Apply) Use /v0 and /v1 to formulate and document a problem
   brief.
3. (Apply) Generate a Boondoggle Score for a small project
   using /claude.

**Opening:** Why was Gru built? Because "write me a login
function" is not a specification. Because Claude has no memory
between prompts. Because every prompt that goes to Claude is
a decision about what Claude can be trusted to do at this step —
and most people are making that decision unconsciously.

<!-- → [DIAGRAM: The Gru phase sequence — /v0 → /v1 → SDD →
/claude → Boondoggle Score → Minion Brief. Linear with phase
gates marked. Editorial style.] -->

**Core content:**
- What a Claude Project is and how system prompts work
- The Gru identity: senior architect, not a code generator
- /v0: the problem formulation gate — one sentence before
  intake begins
- /v1: problem intake — what are you building?
- /claude (/boondoggle): the Boondoggle Score generator
- The Minion Brief: stripped prompts for execution
- Silent mode vs. interactive mode
- CLAUDE.md: the file Claude reads at every session start —
  project conventions, naming rules, what Claude must never
  touch. Introduced here as part of the Gru system; full
  treatment in the teachers book.

**Worked example:** Seth uses Gru to plan a small project. The
/v0 gate stops him from proceeding until he can name the thing
he is building in one sentence. He discovers he cannot. The
chapter walks through the reformulation until the sentence exists.

**Assessable exercises:**
1. (Apply) Use /v0 on a project you're currently working on.
   Can you produce the one-sentence formulation? If not, what's
   missing?
2. (Apply) Complete a /v1 intake for a small project and produce
   the Problem Summary.
3. (Create) Generate a Boondoggle Score for a 3-step build
   using /claude.

**Links:**
[humanitarians.ai/tools](https://www.humanitarians.ai/tools) ·
[boondoggling.ai](https://boondoggling.ai)

**Wayback Machine:** 🕰️ **Frederick Brooks** (1931–2022) — author
of *The Mythical Man-Month* (1975), who established that software
development is primarily a design problem, not a coding problem,
and that the most expensive bugs are the ones that come from
building the wrong thing. The /v0 gate is Brooks's insight
operationalized.

**Bridge:** The reader has Gru. Chapter 7 builds the document
that makes Gru's output trustworthy: the Software Design
Document.

---

### Chapter 7 — The Software Design Document: Building the Mission Before the Build

**One-line:** The SDD is not bureaucracy. It is the decision
that prevents you from making the same decision twice under
deadline pressure.

**Learning outcomes:**
1. (Understand) Explain why a Software Design Document is a
   prerequisite for a productive Boondoggle Score.
2. (Apply) Complete the core sections of an SDD for a
   student-scale project.
3. (Analyze) Identify the sections of an SDD most likely to
   reveal a problem formulation gap.

**Opening:** Seth starts a build. Three hours in, he realizes
he is building the wrong thing. Not the wrong code — the wrong
system. The decision he needed to make was at the beginning,
not the middle.

<!-- → [DIAGRAM: The SDD as decision record — five sections
as labeled boxes in sequence: Problem Statement → Architecture
Principles → Core User Flows → User Needs → Component List.
Arrows showing dependency. Editorial style.] -->

**Core content:**
- What an SDD is and is not
- The five sections that matter most at student scale
- Why the /v0 sentence is the SDD's foundation
- The minimum viable SDD
- What the SDD unlocks: a Boondoggle Score that is
  actually trustworthy

**Worked example:** A complete minimum viable SDD for a student
project — a task tracker with a specific user, a specific
workflow, and three architecture principles.

<!-- → [TABLE: Minimum viable SDD — section name, one-sentence
description, what a weak version looks like, what a strong
version looks like. Five rows.] -->

**Assessable exercises:**
1. (Apply) Write the problem statement section of an SDD for
   a project you want to build. It must pass the /v0 one-sentence
   test.
2. (Apply) Write three architecture principles for the same
   project. Run the Principle Collision Test.
3. (Analyze) A provided SDD has a weak needs section. Identify
   which needs are feature descriptions and rewrite them as
   testable outcomes.

**Wayback Machine:** 🕰️ **Edsger Dijkstra** (1930–2002) —
computer scientist who argued that programming is not primarily
about writing code but about constructing a proof of correctness
before the first line is written. His concept of program
correctness as a design property, not a testing property, is
the intellectual foundation of the SDD as a prerequisite to
building.

**Bridge:** The reader can build an SDD. Chapter 8 teaches them
to write Claude prompts that are specifications, not requests.

---

### Chapter 8 — Writing Claude Prompts That Are Specifications

**One-line:** "Write me a login function" is not a prompt. A
prompt names the thing, the invariants, the output format,
and what not to touch.

**Learning outcomes:**
1. (Understand) Distinguish a prompt (request) from a
   specification (complete task definition).
2. (Apply) Rewrite a weak prompt as a complete specification
   using the five-element format.
3. (Analyze) Identify what is missing from a set of provided
   prompts that would cause Claude to produce incorrect output.

**Opening:** Side by side: the same task written as a prompt
and as a specification. Claude's output for each. The difference
is not Claude — it's the precision of the instruction.

<!-- → [TABLE: Prompt vs. specification — two columns, five rows.
Each row: one element of the specification format. Left column:
weak prompt version. Right column: specification version.] -->

**Core content:**
- The five elements of a specification prompt:
  1. The specific task (not "help me with X" — the one thing)
  2. The invariants (what must not change)
  3. The context (the SDD sections that govern this step)
  4. The output format (what done looks like)
  5. The negative constraint (what Claude must not do)
- Why Claude has no memory between prompts and what this means
- The dangerous middle: prompts that are almost specifications
- Prompt quality self-check: five questions before sending
- Give Claude a way to verify its own work: whenever possible,
  include a test, a linter, or a bash command Claude can run
  to check output before reporting done — Claude iterating
  against a feedback mechanism produces significantly better
  results than single-pass generation

**Worked example:** A complete build sequence of five prompts,
each written as a full specification with all five elements.

**Assessable exercises:**
1. (Apply) Take three prompts from your recent Claude history.
   Rewrite each as a specification.
2. (Analyze) A provided specification is missing two elements.
   Identify which ones and explain what Claude will do wrong.
3. (Create) Write a complete five-element specification for
   the next step in your current project.

**Wayback Machine:** 🕰️ **Ada Lovelace** (1815–1852) — the first
person to write what we would recognize as a computer program:
a precise specification of operations in dependency order,
written before any machine existed that could run them. Her
Notes on Babbage's Analytical Engine are the conceptual ancestor
of the five-element specification format.

**Bridge:** The reader can write specifications. Chapter 9
addresses what happens when the specification is right and the
output is still wrong.

---

### Chapter 9 — Handoff Conditions and the Dangerous Middle

**One-line:** Not "looks good." A specific, testable condition
that must be true before the next step begins.

**Learning outcomes:**
1. (Understand) Explain what a handoff condition is and why
   "looks good" fails as one.
2. (Apply) Write handoff conditions for a set of provided
   Claude tasks.
3. (Analyze) Identify the "dangerous middle" — tasks where
   Claude's output requires specific human verification that
   isn't in the obvious checklist.

**Opening:** Seth approves a Claude output. It looks correct.
It compiles. It passes every test he thought to write. Six days
later: silent failure in production. The condition that wasn't
met was the one he didn't know to check. That's the dangerous
middle.

<!-- → [DIAGRAM: The handoff condition as a gate between build
steps. Step N → [Handoff condition check] → Step N+1. If check
fails: /rewind to step N checkpoint. If check passes: proceed.
Editorial style.] -->

**Core content:**
- What a handoff condition is: specific, testable, binary
- The dangerous middle: tasks where Claude produces plausible
  but domain-incorrect output that passes surface checks
- The scope creep prompt: "while I'm here" improvements that
  break things
- How to write a handoff condition that catches what you'd
  otherwise miss
- The STOP block: explicit conditions under which Claude must
  pause and wait for human verification
- When a handoff condition fails: use /rewind, not forward
  correction. Press Esc twice or type /rewind, select the
  checkpoint before the failed step, write a better specification
  that includes the failed condition as a negative constraint,
  and run the step again from clean state. After two failed
  corrections on the same step, the context is polluted —
  /rewind to before the failure started.

<!-- → [TABLE: Strong vs. weak handoff conditions — two columns.
Five examples. Left: weak ("looks good," "tests pass," "works
for me"). Right: strong (specific, testable, binary). No color.]
-->

**Worked example:** Three handoff conditions analyzed — one
strong, one weak, one that missed the dangerous middle.

**Assessable exercises:**
1. (Apply) Write handoff conditions for five Claude tasks in
   a provided build sequence.
2. (Analyze) A provided build transcript shows Claude crossing
   into the dangerous middle. Identify the exact moment and the
   handoff condition that would have caught it.
3. (Create) Add a STOP block to a Claude specification for a
   task in your current project that touches a dangerous middle.

**Wayback Machine:** 🕰️ **Grace Hopper** (1906–1992) — computer
scientist who first documented the importance of explicit
verification criteria in software and coined the term "debugging"
after finding an actual moth in the Mark II computer. Her
insistence that "the most dangerous phrase in the language is
'we've always done it this way'" applies directly to handoff
conditions that default to "looks good."

**Bridge:** The reader has the full framework for code builds.
Chapter 10 applies the same discipline to creative work.

---

### Chapter 10 — Brutalist: When the Build Is Creative

**One-line:** The technical barrier is now low enough that any
student can produce ambitious creative work. The question is
whether the creative judgment stays theirs.

**Learning outcomes:**
1. (Understand) Explain how the fluency trap manifests in
   creative AI use.
2. (Apply) Apply the Brutalist three-file system (CLAUDE.md,
   DESIGN.md, PROJECT.md) to a creative project.
3. (Analyze) Distinguish creative judgment (irreducibly human)
   from technical execution (Claude's domain) in a provided
   creative build.

**Opening:** Nicholas's observation: his classmates let AI
generate the prose, the graphics, the music. The output is
polished. It has no soul. He can feel the void underneath it.

<!-- → [DIAGRAM: The three-file system as three nested layers.
Outer: CLAUDE.md (technical constitution — what Claude never
improvises). Middle: DESIGN.md (visual constitution — every
aesthetic decision specified or escalated). Inner: PROJECT.md
(project state — Intent Layer is human, always). Editorial.] -->

**Core content:**
- Why creative work has the same problem as code: the fluency
  trap is about the feeling of authorship, not the domain
- The Brutalist governing principle: maximally informed,
  minimally autonomous, by design
- The three files and what each protects
- The six principles in plain language
- Refusal behavior: Brutalist says no — this is the feature,
  not a limitation
- Nicholas's principle: AI handles technical execution, you
  keep creative judgment

<!-- → [TABLE: Labor separation in creative builds — two columns.
AI handles / Human keeps. Examples from visual, writing, and
music domains. No color.] -->

**Worked example:** A student data visualization project built
twice — once with Claude running unattended on aesthetic
decisions, once with the Brutalist system enforcing the boundary.
Same Claude. Same technical output. Different authorship.

**Assessable exercises:**
1. (Apply) Create a DESIGN.md for a creative project. Name
   six design decisions that are fully specified and two that
   are escalated to you.
2. (Analyze) A provided creative build transcript shows Claude
   making an aesthetic judgment. Identify the moment and name
   which file would have prevented it.
3. (Create) Write the Intent Layer of a PROJECT.md for a
   creative project. Have a classmate read it. Can they tell
   what you are trying to make?

**Links:** [brutalist.art](https://brutalist.art)

**Wayback Machine:** 🕰️ **Sol LeWitt** (1928–2007) — conceptual
artist who created works defined by written instructions that
others executed. His argument that the idea is the art — that
the person who holds the intent and writes the instruction is
the author, regardless of who executes — is the Brutalist
principle stated fifty years before AI made it urgent.

**Bridge:** The reader has the full discipline. Act Three begins.
Chapter 11 is the planning phase of their first complete build.

---

## ACT THREE — THE BUILD
*Chapters 11–14: From framework to first fully Boondoggled build*

---

### Chapter 11 — Planning Your First Boondoggled Build

**One-line:** Before Claude sees a single prompt, you know
exactly what you are building, why, and which steps belong
to you.

**Learning outcomes:**
1. (Apply) Complete a minimum viable SDD for a student-scale
   project.
2. (Apply) Generate a Boondoggle Score for the first phase.
3. (Analyze) Identify the three steps in the build most likely
   to hit the dangerous middle.

**Opening:** Seth planning his first fully Boondoggled build.
The /v0 sentence. The SDD. The Boondoggle Score. The moment
he realizes: he is not thinking about Claude yet. He is thinking
about the problem. That is the discipline working.

<!-- → [DIAGRAM: The planning sequence — /v0 → /v1 → SDD core
sections → Boondoggle Score → Minion Brief. Phase gates labeled.
What must be true before each gate opens. Editorial style.] -->

**Core content:**
- The planning sequence: /v0 → /v1 → SDD → Boondoggle Score
- How to scope a student project for a first Boondoggled build
- Reading a Boondoggle Score: critical path, highest-risk
  handoffs, supervisory capacity distribution
- What to do when the score reveals a thin SDD section
- The planning gate: what must be true before the first prompt
- Start with codebase Q&A, not code editing: if you are
  working in an existing project, spend at least one session
  asking questions before writing any specification

**Worked example:** Seth's complete planning document for a
real project — /v0 sentence, abbreviated SDD, Boondoggle Score
for Phase 1.

<!-- → [TABLE: Boondoggle Score summary — columns: Step number,
Phase, Labor (Claude/Human), Supervisory capacity (if human),
Handoff condition. Five rows showing a Phase 1 example.] -->

**Assessable exercises:**
1. (Apply) Produce a /v0 sentence, minimum viable SDD, and
   Boondoggle Score for the project you will build in Chapter 12.
2. (Analyze) Review the provided Boondoggle Score. Identify
   the two highest-risk handoffs. Write handoff conditions.
3. (Evaluate) Is your SDD ready to govern a build? What is
   the weakest section? Fix it before Chapter 12.

**Wayback Machine:** 🕰️ **Christopher Alexander** (1936–2022) —
architect and design theorist whose *A Pattern Language* (1977)
argued that good design begins with a clear statement of the
problem before any solution is attempted. His concept of
"timeless way of building" — design that begins with what
the inhabitant needs, not what the builder wants to make —
is the SDD principle applied to architecture.

**Bridge:** The plan is complete. Chapter 12 executes it.

---

### Chapter 12 — Running the Build: Claude Tasks and Human Tasks

**One-line:** The score is the plan. Now you execute it — one
step at a time, with explicit handoff conditions between every
step.

**Learning outcomes:**
1. (Apply) Execute a Boondoggle Score build sequence, completing
   Claude tasks and human tasks in dependency order.
2. (Apply) Apply each of the five supervisory capacities at
   the steps requiring them.
3. (Analyze) Identify when a build is going off-script and stop
   before it breaks.

**Opening:** Seth mid-build. The plan is on the screen. Claude
is running. He is doing his job — not approving output, but
evaluating it against the handoff condition. He rejects one
output. Asks for a revision. Approves the revision. The build
continues. This is what conducting feels like.

**Core content:**
- Running the Minion Part: executing Claude prompts in
  dependency order
- Running the Gru Part: human tasks labeled by supervisory
  capacity
- What to do when Claude fails a handoff condition: /rewind,
  not forward correction
- What to do when Claude passes but feels wrong: plausibility
  auditing — trust the feeling, investigate it
- The scope creep moment: how to handle "while I'm here"
- When to stop and reformulate vs. when to push through
- Give Claude a way to verify its own work: include tests,
  linters, or bash commands Claude can run. Claude iterating
  against a feedback mechanism produces better output than
  single-pass generation. Build the verification mechanism
  first; it is Claude's handoff condition, not yours.
- /clear between unrelated tasks: if you have corrected Claude
  more than twice on the same issue, the context is polluted.
  /clear and write a better specification from scratch.

**Worked example:** A complete build session documented in real
time — Seth's actual prompts, Claude's actual outputs, Seth's
handoff evaluations, one rejection and revision, the final
accepted output for Phase 1.

<!-- → [DIAGRAM: The build loop — Specification → Claude executes
→ Handoff condition check → [Pass: next step] / [Fail: /rewind
and respecify]. Loop annotation showing supervisory capacity
at the check step. Editorial style.] -->

**Assessable exercises:**
1. (Apply) Execute the first phase of your Boondoggle Score.
   Document each handoff evaluation.
2. (Analyze) At least one Claude output in your build will
   require revision. Document the failure and the revision
   specification.
3. (Evaluate) At the end of Phase 1: what did you learn that
   you didn't know at planning? What would you change in the SDD?

**Wayback Machine:** 🕰️ **W. Edwards Deming** (1900–1993) —
statistician whose Plan-Do-Check-Act cycle became the foundation
of quality management. His argument that quality is built into
a process, not inspected in at the end, is the handoff condition
principle applied to manufacturing — and the reason the
Boondoggle Score sequences verification into every step.

**Bridge:** The build is done when it passes the handoff
conditions. Chapter 13 defines what "done" actually means.

---

### Chapter 13 — Verification: How You Know It Works

**One-line:** The build is done when it passes the handoff
conditions — not when Claude says it's done.

**Learning outcomes:**
1. (Apply) Run a structured verification pass on a completed
   build using explicit criteria from the SDD.
2. (Analyze) Distinguish build failures from test quality gaps.
3. (Evaluate) Produce a post-build assessment.

**Opening:** Seth's build passes all his tests. He is about
to declare it done. He runs one more check — the one he almost
skipped. It fails. The failure is not a bug in the code. It's
a gap in the original SDD.

<!-- → [DIAGRAM: The verification sequence — three passes in
order. Pass 1: functional verification (does it run?). Pass 2:
edge case verification (does it handle what the SDD defines
as out of bounds?). Pass 3: SDD needs verification (does it
do what the user needs?). Each pass has a binary result and a
path to resolution.] -->

**Core content:**
- Verification against SDD needs, not just "does it run"
- The verification sequence: functional, edge case, SDD needs
- Test quality vs. build quality: a test that passes because
  it's a bad test is not a passing test
- The post-build learning document: the most important output
  of the build
- Why the post-build document matters more than the code

**Worked example:** Seth's verification pass on Phase 1 —
three passes, one failure, a bug fix, and the post-build
document.

**Assessable exercises:**
1. (Apply) Run a structured verification pass on your
   Chapter 12 build. Document each check and its result.
2. (Analyze) One of your tests passes but you're not sure
   it's testing the right thing. Diagnose and rewrite the test.
3. (Create) Write a post-build learning document for your
   Chapter 12 build.

**Wayback Machine:** 🕰️ **Barbara Liskov** (born 1939) —
computer scientist who formalized the Liskov Substitution
Principle: a behavioral contract specifying exactly what a
component must do to be considered correct. Her contribution
to formal specification — the idea that "correct" must be
defined before it can be verified — is the SDD needs
verification pass stated as a principle.

**Bridge:** The reader has the discipline. Chapter 14 hands
them the build.

---

### Chapter 14 — Your First Full Build: From Problem to Verified Output

**One-line:** You have the discipline. Here is the project.
Run it.

**Learning outcomes:**
1. (Create) Plan, execute, and verify a student-scale project
   using the complete Boondoggling framework.
2. (Evaluate) Assess their own build against the five
   supervisory capacities.
3. (Create) Produce a post-build document.

**Opening:** Not Seth's build. Yours. The chapter gives the
student the project brief, the tools, and the sequence.
Everything else is their decision.

**Core content:**
- The project brief: a student-scale project requiring all
  five supervisory capacities
- The complete sequence: /v0 → SDD → Boondoggle Score →
  build → verification → post-build document
- What success looks like: a build where you can account for
  every decision
- What the post-build document should contain
- Where to go next: Irreducibly Human series, Boondoggling
  community, the next project

**Terminal deliverable:** The reader's first fully Boondoggled
build — planned with an SDD, executed with a Boondoggle Score,
verified against SDD needs, documented in a post-build
learning record.

<!-- → [DIAGRAM: The complete arc — a minimal timeline from
Chapter 0 (Seth observes) to Chapter 14 (reader builds).
Four milestones labeled: Problem named / Discipline learned /
First build planned / First build verified. Editorial style.] -->

**Closing:**
"You built it. You know what every line does. You know why
every decision was made. You know what you would do
differently. That is what learning looks like. That is what
Claude Code is for."

**Assessable exercises:**
1. (Create) Complete your full Boondoggled build. Submit the
   post-build learning document alongside the code.
2. (Evaluate) Return to the paragraph you wrote in Chapter 0
   exercise 2 (the calculator and arithmetic). Rewrite it now.
   What changed?
3. (Evaluate) Which of the five supervisory capacities was
   hardest to exercise consistently in your build? Design a
   practice exercise that targets that specific capacity.

**Wayback Machine:** 🕰️ **John Dewey** (1859–1952) — philosopher
of education who argued in *Experience and Education* (1938)
that learning is not the transmission of information but the
transformation of the learner through purposeful experience.
His concept of "learning by doing" is the post-build learning
document — the record that the experience changed the person,
not just the repository.

---

# PART 9 — CHAPTER ANATOMY TEMPLATE

All 14 chapters follow this structure. The Cowork enrichment
pass processes items marked with comments.

1. **One-line descriptor** (capability build, not topic)
2. **Learning outcomes** (3–5, Bloom's level labeled)
3. **Chapter opening** (failure case or Seth narrative —
   problem before framework)
4. **Figure comments** — `<!-- → [DIAGRAM: ... ] -->` or
   `<!-- → [TABLE: ... ] -->` embedded at the point of use
5. **Core content sections** (4–6): concept → example →
   application
6. **Worked example** (Seth's story or provided transcript;
   Act Three: real build session)
7. **Assessable exercises** (minimum 3; at least one at Apply
   or above; at least one requiring production)
8. **Links** (where applicable: boondoggling.ai, brutalist.art,
   irreducibly.xyz, humanitarians.ai/tools)
9. **AI Wayback Machine** — `## 🕰️ AI Wayback Machine` — one
   pre-2000 figure per chapter, connected to chapter's
   intellectual substance
10. **Bridge question** (one sentence; raises what next chapter
    answers)

**Enforcement:** A draft chapter missing items 4 (figures),
7 (exercises), 9 (Wayback Machine), or 10 (bridge) is an
incomplete draft. Do not advance to peer review without
resolving it.

**Seth's voice rule:** Chapters 0–3 and 11–12 are primarily
Seth's narrative voice. Chapters 4–10 are framework-forward
with Seth as illustration. Chapters 13–14 transition to the
reader as primary actor. The voice shift is structural, not
stylistic.

---

# PART 10 — CASE STUDY STRATEGY

## Domain coverage map

| Domain | Chapters | Primary use |
|---|---|---|
| AP Computer Science / homework | 0, 1, 3 | Seth's observation |
| Software function / login system | 4, 8 | Prompt vs. specification |
| Task tracker / student project | 7, 11 | SDD and planning |
| Data visualization | 10 | Brutalist creative build |
| Generic student build | 12, 13, 14 | Full build arc |
| Creative work (writing, music, graphics) | 10 | Nicholas angle |

## Case escalation

Act One cases: observation only — Seth watching, no build.
Act Two cases: single-concept worked examples — one tool
applied to one scenario.
Act Three cases: Seth's real build session — multi-step,
real prompts, real Claude outputs, one documented failure
and revision.

## Worked example format (all chapters)

Situation → The problem as Seth sees it → The cognitive work
required → What happens when it is skipped → What happens
when it is exercised → The lesson (one sentence) → The limit
(where this approach fails).

## Sourcing requirement

Every factual claim (Bastani finding, Kosmyna EEG, Anthropic
RCT numbers, Cohen's d) requires a citation. Seth's narrative
is explicitly labeled as such. No factual claims presented
without attribution.

---

# PART 11 — HARD TOPICS, CONTESTED CLAIMS, AGING RISK

## Contested claims

| Claim | Status | Book's position |
|---|---|---|
| AI use always reduces learning | Disputed | "Without the conducting discipline" — the RCT shows this specifically for unstructured delegation |
| The five supervisory capacities are permanent | Emerging dispute | "Currently requires human judgment" — "currently" qualifier throughout |
| Boondoggling prevents the homework/quiz gap | Unproven | The discipline is grounded in the mechanism; the specific claim awaits the study |
| Claude Code is the right tool for all student builds | Platform-specific | Framework applies to any agentic coding tool; Claude Code is the current implementation |

## Hard chapters

**Chapter 9 (Dangerous Middle):** The Act Two crisis. The
chapter must produce genuine discomfort — the student who
reads it should recognize something they have already done.
Requires a worked example where the failure is specific and
the missed handoff condition is non-obvious.

**Chapter 1 (Homework/Quiz Gap):** The empirical foundation
chapter. The Bastani finding must be stated precisely — not
overstated ("AI always hurts learning") or understated
("one study found..."). The Cohen's d and p-value must be
present.

## Aging risk

| Content type | Risk | Review cadence |
|---|---|---|
| Claude Code feature references | High | Before each edition |
| Gru command syntax | High | Before each edition |
| Bastani / Kosmyna / Anthropic RCT numbers | Low-Medium | On new major studies |
| The five supervisory capacities | Low | On major framework revision |
| Boondoggling framework | Low | On major framework revision |
| Brutalist three-file system | Medium | On major tool change |
| Seth's narrative | None | N/A |

**Aging mitigation:** The book teaches the discipline, not the
feature set. All Claude Code feature references are in Appendix A,
which can be updated without touching main text. All tool URLs
are live references.

---

# PART 12 — MARKET POSITIONING SUMMARY

## The gap this book fills

No book takes a technically fluent student seriously, hands
them a production discipline, and is co-authored by one of them.
The Boondoggling framework has no student-facing educational
equivalent. The gap is documented and closing — independent
researchers are arriving at phase-gated AI use simultaneously —
but no practitioner handbook yet exists for this specific reader.

## Market size estimate

Primary: 2–5 million technically fluent high school students
in the US who use AI coding tools regularly. Kindle price
($1) removes the adoption barrier entirely.

Secondary: K-12 CS teachers assigning it alongside the
teachers companion (~100–500 course adoptions per year at
steady state, ~15–35 students per adoption).

Tertiary: University intro CS courses, professional development
programs.

---

# PART 13 — FEATURE LIST

| Feature | Priority | Production effort |
|---|---|---|
| 14-chapter architecture | ESSENTIAL | Low |
| Three-act learning arc | ESSENTIAL | Low |
| Seth's co-authored narrative | ESSENTIAL | Medium |
| Five supervisory capacities framework | ESSENTIAL | Low |
| Boondoggling framework / Gru integration | ESSENTIAL | Medium |
| Assessable exercises (3+ per chapter) | ESSENTIAL | Medium |
| Terminal deliverable (post-build document) | ESSENTIAL | Low |
| AI Wayback Machine (14 figures) | IMPORTANT | Low |
| Worked examples (Seth's real builds) | IMPORTANT | High |
| Figure comments for Cowork enrichment | IMPORTANT | Medium |
| Brutalist three-file system chapter | IMPORTANT | Medium |
| Appendix A (Gru command reference) | IMPORTANT | Low |
| Appendix B (Brutalist quick-start) | IMPORTANT | Low |
| Appendix C (post-build document template) | IMPORTANT | Low |
| Medhavy integration (Kindle/online edition) | VALUABLE | High |
| Companion website | ASPIRATIONAL | High |

**Minimum Viable Book:** ESSENTIAL features only. Adoptable
at $1 Kindle by a self-directed student reader.

---

# PART 14 — OUT OF SCOPE

Permanently excluded:

| Topic | Reason | Covered better in |
|---|---|---|
| Generic prompt engineering tips | Optimizes for output, not capability | Any prompt engineering guide |
| AI ethics and academic integrity policy | Treats students as problems, not builders | *ai-for-teachers* companion |
| Full Gru command library | Too advanced for student scale | humanitarians.ai/tools |
| Advanced Claude Code features (hooks, subagents, MCP) | Teachers book scope | *claude-code-for-teachers* |
| Walker (Unity refactoring) | Second edition / game development supplement | Future edition |
| Server-side deployment | Too complex for first build | Advanced follow-on |
| Agent teams / parallel sessions | Power user pattern | Advanced follow-on |

All exclusions acknowledged in the introduction. The teachers
book is named explicitly as the resource for CLAUDE.md full
treatment, hooks, subagents, and classroom deployment.

---

# PART 15 — ADOPTION RISK REGISTER

| # | Risk | Likelihood | Impact | Mitigation |
|---|---|---|---|---|
| 1 | "Just tell me the prompts" reader | High | Medium | Seth's voice in Ch 0–1 must land; Bastani finding is the hook |
| 2 | Aging risk: Claude Code specifics | High | Medium | Discipline in main text; features in appendices; live URLs |
| 3 | School adoption barrier | Medium | Low | Primary reader self-directs; $1 Kindle; teachers companion bridges |
| 4 | Seth's co-authorship scope | Medium | Medium | Clear delineation: Seth drafts Ch 0, narrative sections, real build logs |
| 5 | Boondoggling framework evolution | Low | Medium | "Currently" qualifier throughout; framework URL as live reference |
| 6 | Kindle price sustainability | Low | Low | Series pricing strategy; print edition at standard pricing |
| 7 | Nicholas's role insufficient | Low | Low | Nicholas provides feedback on Ch 10 and Ch 3; not co-author |

---

# PART 16 — OPEN QUESTIONS

| # | Question | Stakes | Decision deadline | Owner |
|---|---|---|---|---|
| 1 | What is Seth's specific terminal build project for Chapters 11–13? | Worked example quality; must be real, must be reproducible | Before chapter drafting begins | Seth + Author |
| 2 | Does the book include an index or rely on Medhavy for search? | Production cost; print edition usability | Publisher proposal stage | Publisher |
| 3 | What is the project brief for Chapter 14's terminal deliverable? | Complexity calibration; must require all five supervisory capacities | Before manuscript drafting | Author |
| 4 | Seth's co-authorship credit: cover attribution and royalty structure? | Legal; contract | Before contract | Author + Publisher |
| 5 | Nicholas's feedback credit: acknowledgments or contributor credit? | Attribution | Before manuscript | Author + Nicholas |
| 6 | Should Appendix A (Gru command reference) be maintained on a separate web page rather than in the book, given command evolution? | Aging risk | Before production | Author |

---

## Appendix A — Gru Command Reference for Students

Student-facing subset of the full Gru command library.
Commands most useful at the student scale:

| Command | What it does | When to use it |
|---|---|---|
| /v0 | Problem formulation gate | Before any intake — one sentence |
| /v1 | Problem intake | What are you building? |
| /v2 | Architecture principles | Three non-negotiable design commitments |
| /v4 | User and business needs | Testable outcomes, not feature descriptions |
| /claude (/boondoggle) | Boondoggle Score | After SDD is complete |
| /p1 | Feature list with MoSCoW | After component documentation |
| /p5 | Open Questions Log | Any stage |
| /g2 | SDD audit against 7 failure modes | Before build begins |

Full library: [humanitarians.ai/tools](https://www.humanitarians.ai/tools)

---

## Appendix B — Brutalist Quick-Start for Student Creative Projects

Three-file setup in under an hour. Templates:

**CLAUDE.md template (student creative projects)**
**DESIGN.md template (student visual projects)**
**PROJECT.md template with Intent Layer prompts**

Full documentation: [brutalist.art](https://brutalist.art)

---

## Appendix C — Post-Build Learning Document Template

Five sections. One document. Honest.

1. What I built (one paragraph, plain language)
2. What I delegated to Claude and why
3. What I kept for myself and why
4. What I learned that I didn't know before
5. What I would do differently

---

## Series Links

[boondoggling.ai](https://boondoggling.ai) ·
[brutalist.art](https://brutalist.art) ·
[frictional.xyz](https://frictional.xyz) ·
[irreducibly.xyz](https://irreducibly.xyz) ·
[nikbearbrown.com](https://nikbearbrown.com) ·
[humanitarians.ai/tools](https://www.humanitarians.ai/tools)

---

*Full TOC Draft v1.0 — compiled from all phase outputs*
*All phases complete: Vision (i1–i4), Learning Architecture
(l1–l4), Chapter Architecture (c1–c4), Scope & Market (m1–m4)*
*One blocker before production: OQ-1 (Seth's terminal build
project for Chapters 11–13)*
