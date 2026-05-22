# Chapter 6 — Gru: The Tool Built for This

The first time I typed "write me a login function" into a plain Claude chat, I got a login function. It compiled. It ran. It was not my login function — the one my project actually needed, in the system I was actually building, against the user model I actually had. It was *a* login function. A competent, fluent, generic login function that a thousand students before me had received in response to the same sentence. The gap between *a* login function and *my* login function is the thing every chapter of this book has been about. That gap is also the reason Gru exists.

---

There are three reasons the tool was built. I want to say them plainly, in the order they bite.

The first is the login function problem. "Write me a login function" is not a specification. It is a sentence about a feeling. It does not say what counts as a user, what counts as a session, what counts as failure, what counts as done. A senior engineer handed that sentence would respond with questions. Claude handed that sentence responds with code. The code is fine. The fit is a different problem.

The second is memory. Claude has no memory between prompts. Every new chat begins again. Whatever you established in the last session — your variable naming conventions, the fact that your user identifier is `user_score`, the fact that your dataset encodes missing values as the string `"N/A"` rather than as null — is gone. A project you return to seven times across eight weeks accumulates seven separately-briefed strangers, each making slightly different decisions in slightly different directions. The codebase that results is shaped less like an artifact and more like a committee where no member ever met another. In software engineering this has a name. The name is *drift*.

The third reason is the one most students do not see until it has cost them. Every prompt you send to Claude is a decision. It is a decision about what Claude can be trusted to do at this step, with this scope, under this much supervision. *Should Claude write the function? Should Claude propose the architecture? Should Claude decide whether the y-axis is linear or log?* These look like similar prompts from the outside. They require very different amounts of you on the inside. Most people are making this decision unconsciously, prompt by prompt, by feel — which means that on a tired Wednesday at 11 p.m. the decision drifts toward Claude does more and I check less, and on a Saturday morning with coffee it drifts back, and the build is the sum of those drifts. Gru is a tool that forces the decision to be conscious by refusing to advance until you have made it in writing.

<!-- → [DIAGRAM: The Gru phase sequence — /v0 → /v1 → SDD → /claude → Boondoggle Score → Minion Brief. Linear with phase gates marked. Editorial style.] -->

Each arrow in that diagram is not a step. It is a gate — a refusal point that does not pass until a specific artifact exists. The artifacts are concrete: a sentence, a summary, a document, a score, a brief. The gates are checkable by you, on a Saturday afternoon, in front of a laptop. The point of this chapter is to walk that spine, gate by gate.

---

Before the walk, two definitions that the rest of the chapter depends on.

A **Minion Brief** is the stripped-down execution prompt that gets handed to Claude after the design work is done. The name is borrowed from the *Despicable Me* films — Gru is the architect; the minions execute the small scoped tasks under direction. In practice, a Minion Brief is what you copy into a fresh Claude Code session to do *one specific thing*: build this function, refactor this file, write this test. It contains the minimum context required for that specific thing and nothing else. The SDD is strategy. The Minion Brief is one tactical order.

The **Gru identity** — the persona the tool projects in every interaction — is *senior software architect, not code generator*. That sentence is short and load-bearing. A code generator's job is to produce code from a description. An architect's job is to refuse to produce code until the description holds up. Gru is configured to be the second. When you ask it to write a login function, it responds the way the senior engineer would: with a question, then another question, then a request for a one-sentence problem statement. This is not politeness. It is the mechanism.

---

A Claude Project is how Gru is built, so it is worth a moment.

A Claude Project bundles a system prompt, a body of reference documents, and a default behavior into a named container. Every new chat inside that container starts with the same setup already loaded. You do not re-paste the instructions. You do not re-upload the documents. You open the project, start a chat, and it is there.

Two pieces matter. The **system prompt** is what Claude reads before you say anything. It is invisible in the chat window — the user never sees it — but it shapes everything that follows. A good system prompt does not only set the topic. It sets the role, specifies the refusals, defines what kinds of outputs are acceptable and which are not. Gru's system prompt tells Claude to be an architect, to refuse code generation until specific gates have passed, and to ask for the one-sentence problem statement before anything else.

The **Project Knowledge** is the body of reference documents loaded at the start of every chat. For Gru, this includes the Boondoggling framework's definitions, the rubric for the Boondoggle Score, the SDD template, and worked examples. It is what allows Gru to invoke the Boondoggle Score by name without re-explaining what the score is.

The combination — system prompt plus Project Knowledge, both versioned, both in force at every chat — is what makes the framework enforceable. None of this is magic. Anthropic released the Projects feature in mid-2024. What is new is not the technology but the realization that a discipline you would normally enforce by self-control, or by checklist, or by a teacher hovering over your shoulder, can be encoded as a system prompt and made to enforce itself.

One honest caveat, because this book does not oversell: a 2024 study by Zheng and colleagues found that persona prompts do not reliably improve raw task accuracy — they shape behavior and refusal patterns. Which is exactly what Gru is built to do. Gru is not trying to make Claude smarter. Gru is trying to make Claude refuse, at specific moments, until the human does the human work.

---

If you opened Gru right now and typed *hi*, here is approximately what would happen.

Gru would respond positionally — not warmly, not coldly, but as an architect responds. It would not offer to help with whatever is on your mind. It would ask what you are building. If you typed back *an app for my cross-country team*, it would not begin sketching code or proposing architecture. It would ask you to put the project into one sentence — a sentence that names what the system does, for whom, and what makes it count as done. If you could not produce that sentence, it would refuse to proceed.

The refusal is the whole point. A plain Claude conversation is helpful by default — the model is trained to be useful, to make progress, to give the user something concrete. Gru is trained by system prompt to suspend that helpfulness at specific gates. The unhelpfulness is the feature.

There is a fifty-year reason for this design. Fred Brooks, managing the IBM System/360 operating system in the 1960s — at the time the largest software project ever attempted — separated two kinds of complexity. *Essential* complexity is the difficulty of figuring out what the system must do, in what world, under what constraints. *Accidental* complexity is the difficulty of getting it written, compiled, deployed, debugged. Brooks's claim, confirmed repeatedly across five decades of software engineering, is that the hardest part of software is essential, not accidental. Claude eats accidental complexity. It is excellent at it. What Claude cannot do — what no model has been shown to do reliably — is the essential work of deciding what to build. Gru's identity exists to make sure that work happens first. The architect refuses. The minions, later, execute.

---

`/v0` is the first command in the Gru sequence. It does one thing: it refuses to let you continue until you can name what you are building in one sentence — a sentence that says what the system does, for whom, and how you will know it is done.

This sounds like a small ask. It is not.

The one-sentence test is famous in requirements engineering as the elevator test: if you cannot describe what the system does in the time it takes an elevator to reach the floor, you do not yet know what you are building, and any work done before you know is a guess. Barry Boehm's 1988 spiral model formalized this as anchor-point milestones — you do not advance from one phase to the next without producing the deliverable for that phase. `/v0` is Boehm's first anchor-point milestone, sized for a high-school project, enforced by a system prompt.

Students at the `/v0` gate fail in a predictable way. They produce a sentence that contains an *and*. *"A flashcard scheduler and a study group matchmaker."* *"A Discord bot that does homework reminders and also moderates the server."* *"A website that tracks my mileage and shares it with my coach and ranks me against my teammates."* The `/v0` gate refuses these sentences because the *and* is a tell. Two ands and you are almost certainly building two systems. One and and you should be suspicious. The reformulation that passes is the one where the student has *chosen* — flashcard scheduler *or* study group matchmaker, not both.

The second reason the one-sentence constraint matters is conceptual integrity — Brooks's term for the property that all parts of a system reflect a single coherent design mind. The one-sentence formulation is the moment you take ownership of that integrity in writing. You are saying: *this, specifically, is the thing.* Everything Claude produces from here gets measured against that sentence. If a Claude output drifts away from the sentence, you bring it back. If it cannot be brought back, you change the sentence — but deliberately, with a new `/v0`, not by accident in the middle of a build.

Gru audits the sentence against three questions: Is the subject a single system, not two? Does it name the user — the specific person or role this is for? Does it name the done-condition — what makes this count as finished? If any answer is no, Gru returns a specific refusal, points to which question failed, and asks for a revision. The loop continues until the sentence passes.

---

Once `/v0` passes, `/v1` begins. Where `/v0` is a single sentence, `/v1` is an intake interview. Gru asks a structured sequence of questions and produces a *Problem Summary* — a one-page document naming the project, the user, the inputs, the outputs, the success criterion, and the explicit non-goals.

The questions are not arbitrary. They are drawn from the requirements elicitation tradition and sequenced so that early answers constrain later ones. A representative sequence: Who is the user, named specifically — not "people who like running" but "my cross-country coach, Coach Romero"? What does the user currently do that this replaces — even if the current workflow is "writes mileage on a clipboard"? What input does the system take, in concrete data terms? What output does it produce, again concrete? Under what condition does the user say *yes, this is what I needed*? And crucially: what is explicitly not in scope?

That last question is what catches the second and. If Coach Romero says *and could it also email the parents*, that gets logged as not in scope and filed as a future `/v0`. The non-goals section is not a polite gesture. It is a boundary drawn in ink before the build begins.

Two things about the Problem Summary deserve direct attention.

It is negotiable. You will revise it as you learn things. The point is not that it is permanent — the point is that every revision is deliberate, visible, and dated. The version you started with is preserved; the version you ended with is preserved; the diff between them is the record of what you learned during the project. The FBI Virtual Case File project, which the U.S. government wrote off at $170 million in 2005, failed in part because requirements changed more than once per day with no tracking. Negotiable and uncontrolled are not the same thing.

It is checkable. Anyone — your teacher, a friend, you yourself in three weeks — can read the Problem Summary and ask whether the thing being built matches the thing being described. The Problem Summary is the document that makes the question askable.

---

Once the SDD is complete — Chapter 7 builds it; here we treat it as a thing that exists — the next command is `/claude`, equivalently `/boondoggle`. They produce the **Boondoggle Score**.

The Boondoggle Score is a step-by-step plan in which each step has been assessed for how much of it should be Claude's and how much must be yours. The output is a numbered list. Each step has a *what*, a *who* — Claude-only, Claude-with-human-review, or human-only — and a *why*, a brief justification in terms of the supervisory capacities from Chapter 5.

Here is a representative score for a small project — a script that takes a CSV of weekly mileage and returns each runner's acute-to-chronic workload ratio, a sports-science measure of injury risk:

| # | Step | Who | Why |
|---|---|---|---|
| 1 | Parse the CSV into a DataFrame | Claude-only | Pure accidental complexity |
| 2 | Validate input — required columns, expected types | Claude-with-review | Decisions about missing data require domain judgment |
| 3 | Define the acute-to-chronic formula | Human-only | The choice of 7-day acute / 28-day chronic window is a decision, not a fact |
| 4 | Implement the formula | Claude-only | Once the formula is fixed, implementation is mechanical |
| 5 | Set the threshold for elevated risk | Human-only | The threshold encodes a coaching judgment Claude does not have |
| 6 | Generate the per-runner table output | Claude-only | Mechanical |
| 7 | Sanity-check outputs against last week's known cases | Human-only | Plausibility audit requires domain memory |

Step 3 is human-only because the formula choice is essential complexity. Step 5 is human-only because the threshold encodes domain judgment Claude cannot supply. Everything else, Claude can do — but the human signs off on Step 2 and audits Step 7 because those are the points where Claude's confidence outruns Claude's actual coverage.

If you run `/claude` and the score comes back with every step marked Claude-only, you have either built something trivial or you have not been honest. The most common failure at this gate is the score that *looks* rigorous but has quietly handed the human work to Claude. Catching that is itself a supervisory act. The Boondoggle Score is a prompt for your own judgment, not a substitute for it.

---

The final artifact in the sequence is the **Minion Brief**.

A Minion Brief is the prompt you paste into a fresh Claude Code session to execute one specific step from the Boondoggle Score. It is deliberately stripped. It contains the single step this brief executes, the minimum context Claude needs for that step — usually a short excerpt from the SDD and the relevant function signatures — the done-condition for this specific step, and a pointer to CLAUDE.md for global project conventions.

It does not contain the whole SDD. It does not contain the whole Boondoggle Score. It does not contain unrelated context. The reason is twofold. A fresh Claude session given the whole SDD will helpfully try to implement the whole SDD, and will produce something that compiles and is wrong, because the SDD is strategy and the session is being asked for one tactical task. And keeping the Minion Brief small means the resulting output is bounded — you know what you asked for, you know what you got, you can audit the gap.

Without Minion Briefs, the SDD becomes a document the student wrote once and then ignored while typing freehand prompts into Claude. With them, every prompt that goes to Claude is a deliberate slice of the SDD, written down before sending, auditable after returning. The discipline is annoying. The discipline works.

---

`CLAUDE.md` is a file that lives at the root of your project folder and which Claude Code reads at the start of every session. Anthropic introduced it as part of the Claude Code memory architecture, and it is Gru's external memory — the thing that survives between chats.

What goes in it: project conventions (*variables are snake_case; dates are ISO-8601*); architecture notes (*data layer in* `data/`; *logic in* `lib/`); what Claude must never touch (*do not modify* `data/raw/`; *do not delete* `tests/regression/`); and the commands needed to run, test, and build the project.

The most important property of CLAUDE.md is not that it is comprehensive. It is that it is *honest* — it reflects the conventions you actually want enforced. A CLAUDE.md copied from a friend's project is worse than no file at all, because it states conventions you do not follow, and Claude will then enforce conventions you have not chosen. For a student-scale project, under fifty lines is right. Write what is true.

---

It is a Tuesday in early May. Seth has decided he wants to build a thing for his cross-country team. He has been thinking about it on the drive home from practice for three days. He has not written anything down. He opens Gru.

He types: `/v0 I want to build an app for my cross-country team.`

Gru refuses. The refusal is specific. All three questions failed: no single system, no named user, no done-condition. The sentence contains a gesture in the direction of a system.

Seth types again.

`/v0 An app that helps the cross-country team track mileage and find each other for group runs and rank against other teams.`

Gru refuses. It flags two ands. It observes that the sentence describes three different systems — a mileage tracker, a group-run matchmaker, and a leaderboard — with different users, different inputs, and different done-conditions. It asks which one Seth is building first.

Seth has not been challenged in this specific way before. He had thought he was building one thing. He was sketching three things and calling them one because they all involved cross-country. He stares at the screen. He thinks about which of the three he actually wants. The leaderboard sounds fun, but he does not have access to other teams' data. The group-run matchmaker is a social problem dressed in code — what stops people from showing up to group runs is not lack of an app. The mileage tracker is the one Coach Romero has complained about, on the clipboard, for two seasons.

He types: `/v0 A web page where Coach Romero pastes weekly mileage and the page returns each runner's training-load ratio compared to last week, so he can identify runners at elevated injury risk.`

Gru audits. One system. One user, named. One done-condition. The sentence passes.

The whole interaction has taken fourteen minutes. No code. No design. No prompt. One sentence, twenty-six words long. It is the most expensive sentence Seth has written this semester, in the sense that it cost fourteen minutes when he could have been typing. It is also, by a large margin, the most valuable, in the sense that it revealed he was about to build three things — did not have data for one of them, did not have a real product problem for another, and had been postponing the only one that mattered.

He proceeds to `/v1`. Gru begins the intake. The first question is: *Who is the user, named specifically?* Seth answers: *Coach Romero.* Gru asks the next question.

We will return to Seth in Chapter 7, where he builds the SDD around this sentence.

---

One note on aging, because it is necessary. This chapter describes a tool — Gru — and commands — `/v0`, `/v1`, `/claude`, `/boondoggle` — that exist in a specific form as of mid-2026. The framework itself is robust; it reflects fifty years of software engineering practice. The specific command syntax is not guaranteed. Treat the syntax as illustrative. Treat the framework as load-bearing. The current canonical command list lives at [humanitarians.ai/tools](https://www.humanitarians.ai/tools).

---

## 🕰️ AI Wayback Machine

**Frederick Brooks (1931–2022)** managed the IBM System/360 operating system project in the 1960s — at the time the largest software project ever attempted — and emerged from it with a book that has been continuously in print since 1975. *The Mythical Man-Month* did two things. First, it killed the assumption that adding programmers to a late project speeds it up — Brooks's Law: *adding manpower to a late software project makes it later.* Second, and more importantly for Gru, it argued that software development is primarily a design problem, not a coding problem. The most expensive bugs are not bugs in the code. They are bugs in the decision about what to build. Brooks named this *conceptual integrity* and claimed it was the single most important quality of any software system. Eleven years later, in *No Silver Bullet* (1986), he sharpened the claim: software has *essential* complexity — figuring out what to build — and *accidental* complexity — getting it built — and no tool eliminates the essential. The `/v0` gate is Brooks's insight operationalized as a single shell command. Forty years on, with Claude eating accidental complexity for breakfast, Brooks's distinction is sharper, not weaker.

**Run this:**

```text
You are a study partner. I have read in a textbook that Fred Brooks
separated "essential" from "accidental" complexity in software, and
that Claude Code eliminates accidental complexity nearly completely.

Explain to me, in two short paragraphs, why that makes Brooks's
/v0-style problem-formulation discipline more important rather than
less.

Then ask me one question about a project I'm working on that would
test whether I have done the essential work.
```

---

**Links:** [humanitarians.ai/tools](https://www.humanitarians.ai/tools) · [boondoggling.ai](https://boondoggling.ai)
