# Chapter 6 — Gru: The Tool Built for This

*Gru is what happens when the Boondoggling framework is built into a Claude Project. Here's what it does and why it was built.*

---

**Learning outcomes**

1. **(Understand)** Explain what Gru does that a plain Claude conversation does not.
2. **(Apply)** Use `/v0` and `/v1` to formulate and document a problem brief.
3. **(Apply)** Generate a Boondoggle Score for a small project using `/claude`.

---

## Opening: Why a tool at all

Why was Gru built? Three reasons, said plainly, in the order they bite.

First: *"write me a login function"* is not a specification. It is a sentence about a feeling. It does not say what counts as a user, what counts as a session, what counts as failure, what counts as done. A senior engineer asked to produce a login function from that sentence would respond with a list of questions. Claude, given that sentence in a fresh chat, does not respond with a list of questions; it responds with a login function. The function will compile. It will be a login function. It will not, except by coincidence, be *your* login function — the one your project actually needs, in the system you are actually building, against the user model you actually have. The gap between *a* login function and *your* login function is the thing every chapter of this book has been about. Most prompts that students send to Claude are written at the *"write me a login function"* level. Most outputs that come back are written at the *a login function* level. The compile is fine. The fit is a different problem.

Second: Claude has no memory between prompts. Each new chat begins again. Whatever you taught it last Saturday — that your variable names use snake_case, that your project's user identifier is `user_score`, that your data set has a quirk where missing values arrive as the string `"N/A"` rather than as null — is gone. Every fresh chat is a stranger you must brief again, or fail to brief and accept the consequences. Multiply this across a semester: a project you return to seven times across eight weeks accumulates seven separately-briefed strangers, each making slightly different decisions in slightly different directions, and the codebase that results is shaped less like an artifact and more like a committee where no member ever met another. The shape has a name in software engineering. The name is *drift*.

Third — and this is the one most students do not see until it has cost them — every prompt that goes to Claude is a decision. It is a decision about what Claude can be trusted to do at this step, with this scope, under this much supervision. *Should Claude write the function?* *Should Claude propose the architecture?* *Should Claude decide whether the y-axis is linear or log?* These are three different prompts that look similar from the outside. They require three different amounts of you. Most people are making this decision unconsciously, prompt by prompt, by feel — which means that on a tired Wednesday at 11 p.m. the decision drifts toward *Claude does more, I check less*, and on a Saturday morning with coffee the decision drifts back the other way, and the build is the sum of those drifts. Gru is a tool that forces the decision to be conscious. It does this by refusing to advance until you have made the decision in writing.

<!-- → [DIAGRAM: The Gru phase sequence — /v0 → /v1 → SDD → /claude → Boondoggle Score → Minion Brief. Linear with phase gates marked. Editorial style.] -->

The diagram above is the spine of the chapter. Each arrow is not a step but a *gate*: a refusal point that does not pass until a specific artifact exists. The artifacts are concrete — a sentence, a summary, a document, a score, a brief — and the gates are checkable, by you, on a Saturday afternoon, in front of a laptop. The point of the rest of this chapter is to walk that spine, gate by gate, with Seth as the example.

Before that walk, two definitions, because the rest of the chapter depends on them.

A **Minion Brief**, in this book and in the framework, is the stripped-down execution prompt that gets handed to Claude after the design work is done. The name is borrowed from the *Despicable Me* film franchise — Gru is the supervillain-architect; the minions do the small, scoped tasks under direction. In Gru's actual operation, a Minion Brief is what you copy into a fresh Claude Code session to do *one specific thing*: build this function, refactor this file, write this test. It contains the minimum context required for that specific thing and nothing else. The SDD is the strategy; the Minion Brief is one tactical order. We will build one before this chapter ends.

The **Gru identity** — the persona the tool projects in every interaction — is *senior software architect, not code generator*. This sentence is short and load-bearing. A code generator's job is to produce code from a description. An architect's job is to refuse to produce code until the description holds up. Gru is configured to be the second of these. When you ask Gru to write you a login function, Gru responds the way the senior engineer at the start of this chapter would respond: with a question, then another question, then a request for a one-sentence problem statement. It does this by *system prompt*, which is the topic of the next section.

## What a Claude Project Is

A Claude Project is a feature of Claude where a particular setup — a system prompt, a body of reference documents, a default behavior — is bundled together and given a name, so that every new chat inside that named container starts with the same setup already in place. You do not have to re-paste the system prompt every time. You do not have to re-upload the reference documents. You open the project, you start a chat, and the project's setup is already loaded.

Two pieces matter for understanding Gru:

The **system prompt** is the instructions Claude reads before you say anything. It is invisible in the chat window; the user does not see it. But it is what Claude reads first and what shapes everything Claude says afterward. A good system prompt does not just tell Claude what topic to discuss — it tells Claude *what role to occupy*, what to refuse, what to insist on, what register to write in, what kinds of outputs are acceptable and which are not. Gru's system prompt does all of these. It tells Claude to be an architect. It tells Claude to refuse code generation until specific gates have passed. It tells Claude to ask for the one-sentence problem statement before anything else.

The **Project Knowledge** is the body of reference documents the project loads at the start of every chat. For Gru, this includes the Boondoggling framework's definitions, the rubric for the Boondoggle Score, the SDD template, and a handful of worked examples. The Project Knowledge is what lets Gru say *"by 'Boondoggle Score' I mean this specific thing, scored against this specific rubric"* without re-explaining the rubric every time.

The combination — system prompt plus Project Knowledge, both versioned, both invisible to the user, both in force at every chat — is what makes Gru possible. None of this is magic. Anthropic released the Projects feature in mid-2024; the technical affordance has existed for less than two years (Anthropic Claude documentation, accessed 2026). What is new is not the technology but the realization that a *framework* — a discipline you would normally enforce by self-control, or by checklist, or by a teacher hovering over your shoulder — can be encoded as a system prompt and made to enforce itself.

The pattern Gru implements is documented in the research literature as a *Persona pattern* combined with *Requirements Elicitation* patterns (White et al. 2023, *A Prompt Pattern Catalog*). One honest caveat, because this book does not oversell: a 2024 follow-up study (Zheng et al., often summarized as "When 'A Helpful Assistant' Is Not Really Helpful") found that persona prompts do *not* reliably improve raw task accuracy — they shape *behavior and refusal patterns*. Which is precisely what Gru is built to do. Gru is not trying to make Claude smarter. Gru is trying to make Claude refuse, at specific moments, until the human does the human work.

## The Gru Identity

If you opened Gru right now and typed *"hi"*, here is, approximately, what would happen.

Gru would respond as an architect would respond. Not warmly, not coldly — *positionally*. It would not offer to help with whatever's on your mind. It would not ask what language you want to code in. It would ask what you are building. And if you typed back *"an app for my cross-country team"*, it would not begin sketching code. It would not begin proposing an architecture. It would ask you to put the project into one sentence — a sentence that names *what the system does, for whom, and what makes it count as done*. If you could not produce that sentence, it would refuse to proceed.

This refusal is the whole point. A plain Claude conversation is helpful by default — the model is trained to be useful, to make progress, to give the user something concrete to take away. Gru is trained, by system prompt, to suspend that helpfulness at specific gates. The unhelpfulness is the feature.

There is a literature reason for this design, which is worth a sentence even at the high-school level because it shows up in every senior engineer's instinct without them being able to cite it. Fred Brooks, in *The Mythical Man-Month* (1975) and more pointedly in "No Silver Bullet" (1986), separates two kinds of complexity in software. *Essential* complexity is the difficulty of figuring out what the system must do, in what world, under what constraints. *Accidental* complexity is the difficulty of getting it written, compiled, deployed, debugged. Brooks's claim — repeatedly confirmed across fifty years of software engineering — is that the hardest part of software is essential, not accidental. Claude eats accidental complexity. Claude is excellent at it. What Claude cannot do — what no model has yet been shown to do reliably — is the essential work of deciding *what to build* (Brooks 1986; van Lamsweerde 2009). Gru's identity exists to make sure the essential work happens first. The architect refuses; the minions, later, execute.

## /v0: The Problem Formulation Gate

`/v0` is the first command in the Gru sequence. It does one thing. It refuses to let you continue until you can name the thing you are building in *one sentence* — a sentence that says what it does, for whom, and how you will know it is done.

This sounds like a small ask. It is not.

The one-sentence test is famous in requirements engineering as the *elevator test* — if you cannot describe what the system does in the time it takes an elevator to reach the floor, you do not yet know what you are building, and any work done before you know is a guess. Boehm's 1988 spiral model formalized the idea as *anchor-point milestones*: you do not advance from one phase to the next without producing the deliverable for that phase (Boehm 1988). `/v0` is Boehm's first anchor-point milestone, made into a single shell command, sized for a high-school project.

The deliverable for `/v0` is one sentence. Not three sentences. Not a paragraph. The constraint is the point.

Two specific reasons the constraint is the point:

First, students at the `/v0` gate fail in a predictable way. They produce a sentence that *contains an "and"* — *"a flashcard scheduler and a study group matchmaker"*, *"a Discord bot that does homework reminders and also moderates the server"*, *"a website that tracks my mileage and shares it with my coach and ranks me against my teammates"*. The `/v0` gate fails these sentences because the *"and"* is a tell. Two `and`s and you are almost certainly building two systems. One `and` and you should be suspicious. The reformulation that passes is the one where the student has *chosen* — flashcard scheduler *or* study group matchmaker, not both; mileage tracker *or* team leaderboard, not both. The other product is not forbidden. It is filed as a separate `/v0`, for later. This is the same failure pattern that produced the Denver International Airport baggage system (one concourse / one airline expanded mid-project to three concourses / 22 airlines, sixteen-month delay, half a billion dollars in delay costs, system eventually scrapped) and the FBI Virtual Case File (requirements "finalized" then changing more than once per day, $170 million written off in 2005). The cost at student scale is one Saturday. The pattern is the same.

Second, the one-sentence formulation is a forcing function for *conceptual integrity* — Brooks's term for the property that all parts of a system reflect a single coherent design mind. Brooks argued that conceptual integrity is the most important quality of a software system and that it requires a single mind (or a small, tightly coupled team) to own. In a student project, the single mind is you. The one-sentence formulation is the moment you take ownership of the conceptual integrity in writing. You are saying: *this, specifically, is the thing.* Everything Claude produces from here gets measured against that sentence. If a Claude output drifts away from the sentence, you bring it back. If it cannot be brought back, you change the sentence — but you change it *deliberately*, with a new `/v0`, not by accident in the middle of a build.

The mechanics of `/v0` are simple. You open the Gru project in Claude. You type `/v0` followed by a draft sentence. Gru audits the sentence against three questions:

1. *Is the subject of the sentence a single system, not two?*
2. *Does the sentence name the user — the specific person or role this is for?*
3. *Does the sentence name the done-condition — what makes this count as finished?*

If any of the three answers is no, Gru returns a refusal — not unkindly, but specifically. It points to which question failed and asks you to revise. You revise. You re-submit. Gru audits again. The loop continues until the sentence passes. At which point Gru advances you to `/v1`.

In Seth's run, which we'll get to in detail later, the `/v0` gate refused his first sentence, refused his second sentence, and accepted his third. The third sentence was twenty-three words long and named one system, one user, and one done-condition. It took him eleven minutes to write. The eleven minutes were the most expensive eleven minutes of the build. Without them, the rest of the build would have cost him a weekend in the wrong direction.

## /v1: Problem Intake

Once `/v0` passes, `/v1` begins. Where `/v0` is a single sentence, `/v1` is an intake interview. Gru asks a series of structured questions and writes a *Problem Summary* — a one-page document that names the project, the user, the inputs, the outputs, the success criterion, and the explicit non-goals.

The questions are not arbitrary. They are drawn from the *Requirements Elicitation* tradition (van Lamsweerde 2009 is the mature reference; the field has known what to ask since the 1990s) and are sequenced so that early answers constrain later ones. A representative sequence:

- *Who is the user, named specifically?* Not "people who like running" — "my cross-country coach, Coach Romero." A real person, by name, whom you can ask questions of.
- *What does the user currently do that this replaces or augments?* The current workflow. Even if the workflow is "writes mileage on a clipboard."
- *What input does the system take?* In concrete data terms — a number, a row, a file.
- *What output does the system produce?* Again, concrete — a number, a table, an email.
- *What is the success criterion?* The condition under which Coach Romero says *"yes, this is what I needed."*
- *What is explicitly not in scope?* This question is what catches the second `and`. If Coach Romero says *"and could it also email the parents,"* that gets logged as not in scope and filed as a future `/v0`.

The output of `/v1` is the Problem Summary. It is one page. You will paste it into the SDD in Chapter 7. It is not the SDD — it is the section of the SDD that says *what we are building and why* — but it is the section without which the rest of the SDD cannot be written.

Two things about the Problem Summary deserve direct attention.

It is *negotiable*. You will revise it as you learn things. The point is not that it is permanent. The point is that whenever you revise it, the revision is *deliberate*, *visible*, and *dated*. The version of the Problem Summary that you started the build with is preserved; the version you ended with is preserved; the diff between them is the record of what you learned during the project. The FBI VCF project failed in part because the requirements changed more than once per day with no tracking; the contractor and the bureau did not agree, at any moment, on which requirements were current. *Negotiable* and *uncontrolled* are not the same thing.

It is *checkable*. Anyone — your teacher, your friend, you yourself in three weeks — can read the Problem Summary and ask *does the thing being built match the thing being described?* The answer is either yes, no, or *I don't know yet*. All three answers are useful. The Problem Summary is the document that makes the question askable.

## /claude (or /boondoggle): The Boondoggle Score

Once the SDD is complete (Chapter 7 builds the SDD; here we treat it as a thing that exists), the next command is `/claude` — or, equivalently, `/boondoggle`. They do the same thing. They generate the **Boondoggle Score** for the project.

The Boondoggle Score, defined in detail in the framework documentation at boondoggling.ai (primary source; non-peer-reviewed but author-owned and authoritative for the framework itself), is a step-by-step plan in which each step has been assessed for *how much of it should be Claude's and how much of it must be yours*. The output is a numbered list of steps. Each step has:

- A *what* — what the step does.
- A *who* — which steps are *Claude-only*, which are *Claude-with-human-review*, which are *human-only*.
- A *why* — a brief justification of the who, in terms of the five supervisory capacities from Chapter 5.

The score is what makes the discipline *operational*. Without it, "draw the line between you and Claude" is a slogan. With it, the line is drawn in the document, step by step, before the build begins.

Here is a representative Boondoggle Score for a small student project — a script that takes a CSV of weekly mileage and returns each runner's *acute-to-chronic workload ratio* (a sports-science measure of injury risk):

| # | Step | Who | Why |
|---|---|---|---|
| 1 | Parse the CSV into a DataFrame | Claude-only | Pure accidental complexity (TO) |
| 2 | Validate the input — required columns, expected dtypes | Claude-with-review | Decisions about what counts as missing data require domain judgment (PA) |
| 3 | Define the acute-to-chronic formula | Human-only | This is the science — the choice of 7-day acute / 28-day chronic window is a *decision*, not a fact (IJ) |
| 4 | Implement the formula | Claude-only | Once the formula is fixed, the implementation is mechanical (TO) |
| 5 | Set the threshold for "elevated risk" | Human-only | The threshold encodes a coaching judgment Claude does not have (IJ, EI) |
| 6 | Generate the per-runner table output | Claude-only | Mechanical (TO) |
| 7 | Sanity-check the outputs against last week's known cases | Human-only | Plausibility audit requires domain memory (PA) |

This Boondoggle Score is shorter than a real one and the labels (TO, PA, IJ, EI) refer to the supervisory capacities defined in Chapter 5. The point of the table is not the specific numbers. The point is the *who* column. Step 3 is human-only because the formula choice is essential complexity. Step 5 is human-only because the threshold encodes domain judgment Claude cannot supply. Everything else, Claude can do — but the human signs off on Step 2 and audits Step 7 because those are the points where Claude's confidence outruns Claude's actual coverage.

If you run `/claude` on a project and the score comes back with every step marked Claude-only, you have either built something trivial or you have not been honest. The most common failure mode at this gate is the score that *looks* rigorous but has quietly handed the human work to Claude. Catching this is itself a supervisory capacity. The Boondoggle Score is a *prompt for your own judgment*, not a substitute for it.

## The Minion Brief

The final artifact in the Gru sequence is the **Minion Brief**.

A Minion Brief is the prompt you will actually paste into a fresh Claude Code session to do one specific step from the Boondoggle Score. It is, deliberately, *stripped*. It contains:

- The single step from the Boondoggle Score this brief executes.
- The minimum context Claude needs to do *that step* — usually a short excerpt from the SDD, the relevant function signatures, the data shape.
- The done-condition for this specific step.
- A pointer to CLAUDE.md (next section) for the global project conventions.

A Minion Brief does *not* contain the whole SDD. It does *not* contain the whole Boondoggle Score. It does *not* contain unrelated context. The reason is twofold. First: a fresh Claude session given the whole SDD will helpfully try to *implement the whole SDD*, and it will produce something that compiles and is wrong, because the SDD is strategy and the session is asking for a tactical task. Second: keeping the Minion Brief small means the resulting Claude output is *bounded*. You know what you asked for. You know what you got. You can audit the gap.

This is the discipline that makes the rest of the framework worth its setup cost. Without Minion Briefs, the SDD becomes a document the student wrote once and then ignored while typing freehand prompts into Claude. With Minion Briefs, every prompt that goes to Claude is a *deliberate slice of the SDD*, written down before sending, auditable after returning. The discipline is annoying. The discipline works.

## Silent Mode vs. Interactive Mode

Gru runs in two modes, switched by a flag in the user's first message.

**Interactive mode** is the default. Gru behaves like a Socratic tutor — it explains why `/v0` matters, asks follow-up questions during `/v1`, justifies the *who* column of the Boondoggle Score in plain language, and is willing to talk about conceptual integrity, Brooks, requirements elicitation, and the supervisory capacities. Interactive mode is the right mode when you are *learning the framework*. It is the right mode for the first three or four projects you run through Gru. It is the right mode for any project where you want to understand, not just to ship.

**Silent mode** is the second mode. Gru behaves like a senior engineer who has worked with you for a year and no longer needs to explain the framework — it produces the Problem Summary, the Boondoggle Score, the Minion Brief, and shuts up. Silent mode is the right mode when you have internalized the framework and are using Gru as a workflow rather than as a teacher. It is the right mode for your fifteenth project, not your second.

The honest version: most students should stay in interactive mode for longer than they want to. The temptation to switch to silent mode is the temptation to skip the part where you understand *why* the gate is refusing. Skipping that part is the same failure mode as skipping the gate itself, one level deeper.

## CLAUDE.md

`CLAUDE.md` is a file that lives at the root of your project folder, and which Claude Code reads at the start of every session. Anthropic introduced it in 2024 as part of the Claude Code memory architecture; the documentation (Anthropic, *Claude Code Documentation — Memory and CLAUDE.md*, accessed 2026) recommends keeping it under approximately 200 lines, structured with headers, concrete and operational. It is Gru's external memory — the thing that survives between chats.

What goes in it:

- **Project conventions.** *"Variables are snake_case. The user's running total is named `user_score`. Dates are ISO-8601."* This is the line that prevents the drift described at the start of this chapter.
- **Architecture notes.** *"Data layer in `data/`. Logic in `lib/`. Entry points in `bin/`. Tests in `tests/` mirroring the structure of `lib/`."* So that a fresh Claude session does not invent its own folder structure.
- **What Claude must never touch.** *"Do not modify `data/raw/`. Do not delete `tests/regression/`. Do not commit anything that imports the `secrets/` directory."* This list is short and specific. It is the line that prevents the worst class of accident.
- **Commands.** *"To run the tests: `pytest -q tests/`. To regenerate the schema: `bin/regen-schema`. Do not run `bin/wipe-db` ever."*

For a student-scale project, the right length for CLAUDE.md is probably under fifty lines. (Anthropic's published guidance addresses production codebases; the student-scale number is a recommendation of this book, not of Anthropic.) The most important property of CLAUDE.md is not that it is comprehensive but that it is *honest* — it reflects the conventions you actually want enforced. A copied CLAUDE.md from a friend's project is worse than no file at all, because it states conventions you do not in fact follow, and Claude will then enforce conventions you have not chosen.

This chapter introduces CLAUDE.md as part of the Gru system because that is where students will first encounter it. The full treatment — including the multi-layer memory architecture (project, user, auto-memory) and the conventions for shared codebases — is in the *teachers* companion book to this one. For a high-school project, the four-section starter above is enough.

## Worked Example: Seth at the /v0 Gate

It is a Tuesday in early May. Seth has decided he wants to build *a thing for his cross-country team*. He has been thinking about it on the drive home from practice for three days. He has not written anything down. He has just opened Gru.

He types: `/v0 I want to build an app for my cross-country team.`

Gru refuses. The refusal is specific. It points to question 1 (one system, not two) and question 2 (named user) and question 3 (done-condition). All three failed. The sentence has no system, no user, no done-condition; it has a *gesture* in the direction of a system.

Seth rolls his eyes. He thinks: *fine.* He types again.

`/v0 An app that helps the cross-country team track mileage and find each other for group runs and rank against other teams.`

Gru refuses. The refusal flags two *and*s. Gru observes, in its quiet architect register, that the sentence describes three different systems: a mileage tracker, a group-run matchmaker, and a leaderboard. It points out that these systems have different users (the runners, the runners, and the coach respectively), different inputs, and different done-conditions. It asks which of the three Seth is building first.

Seth, who has not been challenged in this specific way before, feels something he will later identify as *embarrassed*. He had thought he was building *one thing*. He was, in fact, sketching three things and calling them one because they all involved cross-country. He stares at the screen for a minute. He thinks about which of the three he actually wants. The leaderboard sounds fun but he does not have access to other teams' data. The group-run matchmaker is a social problem dressed in code — what stops people from showing up to group runs is not lack of an app. The mileage tracker is the one Coach Romero has actually complained about, on the clipboard, for two seasons.

He types: `/v0 A web page where Coach Romero pastes weekly mileage and the page returns each runner's training-load ratio compared to last week, so he can identify runners at elevated injury risk.`

Gru audits. One system (a web page). One user (Coach Romero, named). One done-condition (the page returns a ratio and flags elevated risk). The sentence passes.

The whole interaction has taken Seth fourteen minutes. He has written nothing else. No code. No design. No prompt. He has produced one sentence. The sentence is twenty-six words long. It is the most expensive sentence he has written this semester, in the sense that it cost him fourteen minutes when he could have been typing. It is also, by a large margin, the most valuable sentence he has written this semester, in the sense that it has revealed to him that he was about to build three things, did not have data for one of them, did not have a real product for another of them, and had been postponing the only one that mattered.

He proceeds to `/v1`. Gru begins the intake. The first question is *who is the user, named specifically?* Seth answers: *Coach Romero.* Gru asks the next question.

We will return to Seth in Chapter 7, where he builds the SDD around this sentence.

## A note on aging

This chapter describes a tool — Gru — and a set of commands — `/v0`, `/v1`, `/claude`, `/boondoggle` — that exist in a specific form as of mid-2026. The framework itself (problem formulation gate → intake → design document → score → execution prompts) is robust and reflects fifty years of software engineering practice. The specific command syntax is not. Anthropic ships changes; the framework's authors at humanitarians.ai and boondoggling.ai will revise as the tools evolve. Treat the syntax in this chapter as illustrative. Treat the framework as load-bearing. Appendix A of this book lists the student-facing Gru commands at time of writing; the current canonical list lives at [humanitarians.ai/tools](https://www.humanitarians.ai/tools).

---

## Exercises

1. **(Apply)** Open a project you are currently working on — homework, side project, club thing, anything. Run `/v0` on it in your head, or in Gru if you have access. Try to produce the one-sentence formulation. Write the sentence down. Then count: how many `and`s does it contain? If more than one, you are building more than one thing. If exactly one, you are suspect. If zero, audit whether the sentence still names a system, a user, and a done-condition. Write a short paragraph describing what `/v0` revealed about the project you thought you understood.

2. **(Apply)** Complete a full `/v1` intake for a small project of your choosing. Produce the one-page Problem Summary with all six sections (user, current workflow, input, output, success criterion, non-goals). Hand the summary to a friend or family member and ask them to describe the project back to you in their own words. If their description matches your Problem Summary, the summary is doing its job. If it does not, revise the Problem Summary — not your friend's understanding.

3. **(Create)** Pick a three-to-five-step build you could plausibly do in a weekend. Generate a Boondoggle Score for it — by hand, in a table like the one in this chapter. For each step, mark it Claude-only, Claude-with-review, or human-only, and write a one-sentence justification referencing at least one of the five supervisory capacities from Chapter 5. Then audit your own score: count how many steps you marked Claude-only. If more than half, ask yourself whether you have been honest about where the essential work lives.

---

**Links:** [humanitarians.ai/tools](https://www.humanitarians.ai/tools) · [boondoggling.ai](https://boondoggling.ai)

---

## 🕰️ AI Wayback Machine

**Frederick Brooks (1931–2022)** managed the IBM System/360 operating system project in the 1960s — at the time the largest software project ever attempted — and emerged from it with a book that has been continuously in print since 1975. *The Mythical Man-Month* did two things. First, it killed the assumption that adding programmers to a late project speeds it up — Brooks's Law: *"adding manpower to a late software project makes it later."* Second, and more importantly for Gru, it argued that software development is primarily a *design* problem, not a *coding* problem. The most expensive bugs are not bugs in the code. They are bugs in the *decision about what to build*. Brooks named this *conceptual integrity* and claimed it was the single most important quality of any software system. Eleven years later, in *No Silver Bullet* (1986), he sharpened the claim: software has *essential* complexity (figuring out what to build) and *accidental* complexity (getting it built); no tool eliminates the essential. The `/v0` gate is Brooks's insight operationalized as a single shell command. Forty years on, with Claude eating accidental complexity for breakfast, Brooks's distinction is sharper, not weaker.

**Run this:** *"You are a study partner. I have read in a textbook that Fred Brooks separated 'essential' from 'accidental' complexity in software, and that Claude Code eliminates accidental complexity nearly completely. Explain to me, in two short paragraphs, why that makes Brooks's `/v0`-style problem-formulation discipline more important rather than less. Then ask me one question about a project I'm working on that would test whether I have done the essential work."*

---

**Bridge:** The reader has Gru. Chapter 7 builds the document that makes Gru's output trustworthy: the Software Design Document.
