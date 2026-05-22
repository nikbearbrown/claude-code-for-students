# Chapter 14 — Your First Full Build: From Problem to Verified Output

*You have the discipline. Here is the project. Run it.*

---

**Learning outcomes**

1. **(Create)** Plan, execute, and verify a student-scale project using the complete Boondoggling framework.
2. **(Evaluate)** Assess your own build against the five supervisory capacities.
3. **(Create)** Produce a post-build learning document that accounts for every decision in the build.

---

## Opening: The Chapter Is Yours

The previous thirteen chapters were Seth's. You watched him miscount a calculator problem in Chapter 0, watched him stare at a `v0.md` file for forty minutes in Chapter 11, watched him `/rewind` his way through a Saturday morning in Chapter 12, and watched him catch a Pass 3 failure he had nearly shipped at the end of Chapter 13. Seth's arc closed somewhere between the Boondoggle Score in Chapter 11 and the first prompt in Chapter 12, when the narrator stopped being the one composing prompts and started being the one designing a thing. You have been reading his transcripts since then because the discipline is easier to see in someone else's hands.

This chapter is not Seth's. It is yours.

The setup is short on purpose. You will pick a project. You will write a `/v0`. You will scope it to /v1. You will fill in a five-section Software Design Document. You will produce a Boondoggle Score for Phase 1. You will run the build. You will verify it. You will write a post-build learning document. Then you will commit, push, and — separately from the repository — write down what changed about you. The repository is the byproduct of the chapter. The post-build document is the chapter's actual deliverable. If you ship perfect code and cannot write the document, you have not done the assignment. If you ship modest code and *can* write the document, you have done it.

There is no Seth in what follows. There is the project brief, the sequence, the rubric for success, and the rubric for the document. Everything else — what you build, which capacity strains hardest, which row in the score sends you back to /v1 — is your decision. The book has been arguing, since Chapter 0, that Claude Code is an instrument and not a shortcut. The next eight to twenty hours of your life are the argument's test. Run it honestly.

<!-- → [DIAGRAM: The complete arc — a minimal timeline from Chapter 0 (Seth observes) to Chapter 14 (reader builds). Four milestones labeled: Problem named / Discipline learned / First build planned / First build verified. Editorial style.] -->

The diagram above is the book in one image. Four milestones. *Problem named* is Chapter 0 — the calculator paragraph you wrote in exercise 2, the day you noticed the gap between *typing keys* and *knowing arithmetic*. *Discipline learned* is Chapters 1 through 10 — the labor separation, the five supervisory capacities, the SDD, prompts-as-specifications, handoff conditions. *First build planned* is Chapters 11 through 13 — Seth's task tracker, end to end, with the verification pass that caught the Pass 3 failure. *First build verified* is this chapter — the build you are about to do, ending in a document that says, in plain language, what you decided and why.

There is an unmarked arrow continuing past Milestone 4. It is labeled *next project*. The terminus of the book is not the terminus of the practice.

## The Project Brief

You need a project. The project must be small enough to finish in eight to twenty hours of focused work over a week or two, and large enough to exercise all five supervisory capacities — *Specification, Direction, Verification, Boundary-setting, Synthesis* — at least once each. A project that exercises only three of the five is a chapter exercise, not a build. A project that exercises seven different concerns is a multi-week endeavor and not what this chapter is for.

This book takes a position on what to build, and the position is *exemplar, not mandate*. The book recommends one default and lists alternates. You may pick the default or swap to an alternate; you may, with your instructor's consent, propose a different project that meets the same constraint (eight to twenty hours, all five capacities). What you may not do is pick a project the night you start coding. Pick the project the day you start *planning*, which is the day before you start coding.

**The default project: a personal homework deadline tracker.** A local web page or CLI tool that lets you add an assignment with a due date and a class, mark assignments as done, list assignments due in the next seven days sorted by date, and persist across browser refreshes or invocations. No accounts. No server. No sync. One user — you. The data lives in `localStorage` (web) or a JSON file in your home directory (CLI). The whole project is one screen, one data model, one file of logic. Phase 1 is the add-list-toggle-persist loop. Phase 2 — out of scope for this chapter — is anything else.

The default is deliberately close in shape to Seth's task tracker from Chapters 11–13 because you have read its planning document, its execution log, and its verification pass. You will not be copying Seth's work; the *data model is different* (a homework assignment has a class and a due date; a task has neither), the *user need is different* (deadlines, not throughput), and *the display order is the user need*, which means the Pass 3 surprise Seth hit at the end of Chapter 13 is staring at you from the start. The familiarity is scaffolding. The novelty is the design decisions that scaffolding cannot make for you.

**Alternates, if the default does not fit your situation.** Each alternate exercises all five capacities; the one in the right column names the capacity that will strain hardest if you pick it. None is harder than another; they are different shapes.

| Alternate project | Hardest capacity to exercise |
|---|---|
| **CSV-to-summary utility for an AP class** — reads a `grades.csv` or `data.csv` from a class, computes column-wise summary statistics, prints a one-page report. One CSV dialect supported, one report format. | Verification — the numbers must match a hand-computed total, every time |
| **Hangman or 2048 clone in the terminal** — playable, scored, edge cases for tie / wall / win / lose handled. | Direction — game loops creep in scope; staying inside the /v0 sentence is the work |
| **Personal flashcard study app** — local web page, add/review cards, simple spaced-repetition rule (e.g., Leitner box), persistence. | Boundary-setting — *spaced repetition* is a research field; the project must touch one rule, not the field |
| **Chatbot wrapper for a single use case** — a CLI script that wraps an API call with a fixed persona prompt and a local conversation history file. One persona, one use case. | Synthesis — you are wrapping an LLM with an LLM; assessing whether the wrapper added value over the bare model is the project |

Pick *one*. Commit to the pick before you write the /v0 sentence. Switching projects after the /v0 is itself a failure of Direction — the capacity Chapter 5 names as *staying on the line you drew*. If you find yourself wanting to switch at hour three, do not switch. Re-scope the project you picked. Cut it in half. The point of this chapter is to finish *one* build, honestly. Two abandoned builds with a clean repo each is two failures of Direction, not two attempts.

A note on the choice itself. The book is being honest about this: *which exemplar belongs in this chapter* is an open question the authors have not fully settled, recorded in the table of contents as a soft decision.[^oq] We chose the homework deadline tracker as default because it is concrete to a high-school reader, because it shares scaffolding with the worked Chapter 11–13 example, and because the *display order is the user need* trap is pedagogically rich. We do not claim it is the only right pick. If your AP-CS teacher prefers a different project of similar shape, build that one. The framework — not the exemplar — is what this chapter is testing.

[^oq]: Listed in the book's table of contents as Open Question OQ-1: *what student-scale project belongs in Chapter 14 as the default exemplar?* The choice is a Soft Decision in the sense Chapter 11 used the term — defensible against the constraint, not provably optimal. The chapter framework should not break if a future edition swaps the default.

## The Complete Sequence

The sequence has six stages and they happen in order. Each stage has a gate; you do not cross the gate until the gate's condition is true. The gates are not bureaucracy — they are the book in execution form. The first five gates were named in Chapter 11 and run in Chapter 12; this chapter recombines them as one continuous flow with the sixth, verification, joined to the seventh, the post-build document. Read the sequence once now, before you begin.

**Stage 1 — `/v0`.** One sentence. One user. One outcome. No conjunctions hiding a second project. For the default exemplar: *"A local web page that lets me add homework assignments with a class and a due date, mark them done, see what is due in the next seven days, and remember them across browser refreshes."* Read your sentence out loud. If you used *and* to connect two outcomes, you have two projects; cut one and try again. *Gate 1:* one sentence, one user, one outcome.

**Stage 2 — `/v1`.** Two short lists: *in scope* and *out of scope*. The out-of-scope list is the load-bearing one. Five plausible features the project will *not* have — accounts, multiple users, server sync, calendar integration, mobile-native UI, push notifications, anything else you can name. Putting them on the out-of-scope list is the act of defending the project against itself. *Gate 2:* both lists non-empty, out-of-scope at least five items.

**Stage 3 — SDD.** Five sections, as Chapter 7 named them: *Data model, UI states, Persistence, Failure modes, Out of scope restated.* Every section has at least one concrete artifact — a field name, a UI state diagram, a file name, a named failure mode with a response. Sections that say "we will figure that out during the build" do not pass. *Gate 3:* every section names at least one concrete artifact.

**Stage 4 — Boondoggle Score.** A table. One row per step in Phase 1. Five columns: step number, phase, labor (Claude or human), supervisory capacity (named, if the human is laboring), handoff condition (what must be true to call the step done). Phase 1 should be three to seven rows. Fewer than three and you have not decomposed; more than seven and you are doing Phase 2 inside Phase 1. *Gate 4:* every row has a named handoff condition; no row says "Claude figures it out."

**Stage 5 — Build execution.** Run the score, one row at a time, in dependency order. For each row: write the prompt or do the human step; check the handoff condition; log it. The execution log is *not optional* and is written during the build, not after. A row's log entry is four lines: *what you asked Claude*, *what Claude returned* (or *what you produced*, if it is a human row), *what you did with it*, *which supervisory capacity governed the step*. If Pass 3 of the verification surfaces a need the score did not enroll as a row, add a row, log the addition, and continue. *Gate 5:* every row's handoff condition has been checked and the result recorded.

**Stage 6 — Verification.** Three passes in order, as Chapter 13 named them. Pass 1: does the build run on the happy path the SDD specifies? Pass 2: does it handle the edge cases the SDD names — empty list, oversized input, malformed persistence, missing storage? Pass 3: read the SDD's *User Needs* section out loud against the running build; does each sentence hold? *Gate 6:* all three passes return green against this version of the SDD.

**Stage 7 — Post-build learning document.** Five sections, drafted *during and immediately after* the build, not days later. Section template below. This is the document the chapter is grading. *Gate 7:* the document is complete, specific, and honest. *Honesty* here is not a feeling. It is the rubric in Chapter 13's worked example — what broke, how you knew, what you would do differently, concretely enough that it would change a row in the score for your next build.

The sequence is not a checklist you race through. It is the order in which decisions become non-reversible. Once you have a /v0, the /v1 must respect it. Once you have a /v1, the SDD must respect both. Once you have an SDD, the score must respect all three. Each gate is a *commitment*, and walking back across a commitment costs work. The cost is the point. The gates are cheap to honor and expensive to skip, in roughly the same way a building inspection is cheap if the foundation passes and expensive if it does not.

## What Success Looks Like

The book defined the success criterion in two places — the table of contents and Chapter 13 — and the definition is the same in both. Read it slowly.

*A build where you can account for every decision — which cognitive work was Claude's, which was yours, and what the handoff condition was at every step.*

Notice what success is not. Success is not *perfect code*. Success is not *no bugs*. Success is not *the build does everything the user might want*. Success is *accountability*. You can account for a build with a known bug if you can say *here is where the bug lives, here is why I did not fix it, here is the handoff condition that would have caught it if I had written it differently*. You cannot account for a build with no known bugs if Claude wrote the tests, Claude wrote the code, you typed *accept all*, and the tests passed. In that case the bugs may be zero or twelve; you do not know, and *you not knowing* is the failure, not the count.

The operational test for accountability, in this book, is a four-question instrument applied to every non-trivial row in your execution log:[^oq128]

1. *What did you ask Claude?* (Or — for human rows — what did you set out to produce?)
2. *What did Claude return?* (Or — what did you produce?)
3. *What did you do with the return?* (Accept as-is, accept with edits, reject and respecify, rewind?)
4. *Which supervisory capacity governed the decision?*

[^oq128]: This rubric is named in the book's open questions as the working operationalization of *accountable for every decision* at student scale. Listed as OQ-1 / sub-question (a) — *what constitutes "accountable" at student scale?* — with a tentative answer of these four questions.

If you can answer all four for every non-trivial row, you have drawn the line. If you cannot answer them for a row, the line was not drawn at that row. Mark the row in your execution log honestly and write that section of the post-build document with extra care. The book is not asking you to produce a perfect line; it is asking you to produce an honest one. A build where you can name the three rows where you stopped drawing the line is a *better* document than one that claims the line held everywhere.

There is a second test of success, less formal, that runs alongside the first. It is the test John Dewey made the foundation of *Experience and Education* in 1938. Dewey argued that a single experience earns the name *education* only when it modifies the next experience — when it produces *continuity*. The post-build document is the artifact of continuity. It is the record of what carries forward into the next project. If the document changes one row in the score for your next build — one prompt you will write differently, one capacity you will exercise sooner — the experience became education. If it does not, the experience was merely activity. Both are common. Only the first is what the book is for.

## What the Post-Build Document Should Contain

Five sections. One document. Honest.

The template is taken from Appendix C and matches what Seth produced at the end of Chapter 13. You will produce the same five sections about your build. Length per section: one short paragraph at minimum, one page at maximum. Aim for short and specific; long and vague is worse than short and specific, every time.

**Section 1 — What I built.** One paragraph, plain language, written for someone who will never read your code. Name the project, the data model, the persistence story, the user the build serves. Avoid jargon. If a reasonably smart non-programmer cannot understand the paragraph, rewrite it. The test of this section is whether your aunt, after reading it, could describe the project at dinner.

**Section 2 — What I delegated to Claude and why.** List the Claude rows from your Boondoggle Score. For each: name the row, name the supervisory capacity that governed the delegation, name the handoff condition, and state whether the delegation held. *Held* means the row passed the handoff condition on the first or second attempt. *Did not hold* means you rewound or rewrote the prompt or took the row back to a human row. Both outcomes are fine; the section is honest reporting, not self-promotion.

**Section 3 — What I kept for myself and why.** List the human rows. For each: name the supervisory capacity that did the work, name what you produced, and state whether the capacity strained. Two strains worth flagging: *the row I almost delegated* (the capacity that wanted to outsource itself) and *the row I almost skipped* (the capacity that wanted to disappear). These two strains are the data that will tell you, project by project, where your discipline is thin.

**Section 4 — What I learned that I didn't know before.** Two registers, both required. *About the domain:* one thing you now understand about the problem space — homework deadlines, CSV parsing, game loops, flashcards — that you did not understand at /v0. *About yourself as a builder:* one thing you now understand about your own decision-making with Claude that you did not understand at /v0. Both registers must be present and both must be specific. *"I learned a lot"* is not a sentence in this section. *"I learned that I use the score's handoff conditions as a substitute for re-reading the SDD's user needs section, and Pass 3 is the only place that substitution gets caught"* is a sentence in this section.

**Section 5 — What I would do differently.** One concrete change to your next build's score or SDD. Concrete enough that it would change a row, a section header, a gate condition. *"I would plan better"* is not concrete. *"I would copy the SDD's User Needs section as a literal block at the top of the score, above all rows, and re-read it before every Pass 3"* is concrete. The test of this section is whether a reader could, just from your sentence, add the change to *their* next project.

Two rules for the whole document. Schön's distinction — *reflection-in-action* during the work, *reflection-on-action* after the work — applies; both kinds of reflection should be visible. The execution log is the in-action reflection. The post-build document is the on-action reflection. Write the on-action document while the in-action reflection is still warm, ideally the same day you finish the build, certainly within the same week. Cold writing produces invention rather than memory. The PBL literature documents this clearly: post-build reflection consolidates learning when it is structured and immediate, and degrades into post-hoc rationalization when it is unstructured or delayed.[^zhang]

[^zhang]: Zhang & Ma's 2023 meta-analysis of project-based learning in *Frontiers in Psychology* reports aggregate positive effects on academic achievement and higher-order thinking, with effect sizes most reliable in *small-group, lab-based* settings — which a solo Boondoggled build approximates. The same review notes that effect depends heavily on *scaffolding* (template + prompts) and *immediacy* (reflection during or just after the work). The post-build document template in this chapter is the scaffold; the *during-or-immediately-after* rule is the immediacy. Both are necessary.

The second rule: the document is short. One page is plenty. The discipline is in what each sentence refuses to be vague about, not in length. A two-page document that is vague in each paragraph is worse than a half-page document that is specific in each sentence.

## Where to Go Next

The book ends with this chapter. The practice does not.

Three pointers, in order of how soon you should follow them.

**The next project.** Soonest. Once your post-build document is written, pick the next project. Smaller scope, harder capacity. If Verification strained on this build, pick a project where Verification is again the hardest capacity, and run it with the change you wrote in Section 5. The capability you built in this chapter is *transfer-tested* on the next project, not consolidated on this one. Kolb's experiential learning cycle names the four stages: concrete experience, reflective observation, abstract conceptualization, active experimentation.[^kolb] The build was the experience. The document is the observation and conceptualization. The next project is the experimentation. Without the fourth stage, the cycle is open.

[^kolb]: David Kolb's *Experiential Learning: Experience as the Source of Learning and Development* (Prentice Hall, 1984) names the four-stage cycle that the post-build document is the second and third stages of. Kolb: "learning is the process whereby knowledge is created through the transformation of experience." The transformation requires all four stages. The book's chapter is the first three; the reader's next project is the fourth.

**The Irreducibly Human series.** Second pointer, after one or two next projects. This book is one volume in a longer argument the series at *irreducibly.xyz* is making — that there are capacities the LLM era amplifies and capacities it cannot replace, and the second category is what education must concentrate on. The series treats domains beyond software: writing, design, scientific research, civic argument. The Boondoggling framework you have just used in code transfers to those domains with surface changes only. The underlying discipline — name the problem, scope it, set handoff conditions, run a verification pass against needs not tests, document what changed — is the same shape.

**The Boondoggling community.** Third pointer, ongoing. *boondoggling.ai* is the community where builders share post-build documents, critique each other's scores, debate which capacity strained on which build, and try to extend the framework into new project shapes. Your post-build document is your entry. The community is the standing peer-review structure the book cannot provide between covers. The most interesting open question the book leaves on the table — *does the post-build document predict future capability?* — is the question the community can answer over time, in a way no single book can.

The list is short on purpose. Three pointers, in order. The first is do-able tonight. The second is do-able after two more builds. The third is ongoing for as long as you keep building.

## Terminal Deliverable

The terminal deliverable for the book is the artifact set you produce by completing this chapter. Six required components, in order. Submit them as one bundle — a repository plus the documents below, either in the repo or alongside it. Your instructor's exact submission format may vary; the components do not.

1. **`/v0` — one-sentence problem formulation.** One sentence. One user. One outcome. Gate 1 satisfied. Save as `v0.md`.

2. **Minimum viable SDD — five core sections.** Data model, UI states, Persistence, Failure modes, Out of scope restated. Each section names at least one concrete artifact. Gate 3 satisfied. Save as `sdd.md`.

3. **Boondoggle Score for Phase 1.** A table with one row per step. Five columns: step number, phase, labor, supervisory capacity, handoff condition. Three to seven rows. Gate 4 satisfied. Save as `score.md`.

4. **Build execution log.** One entry per row in the score, written during the build, in order. Each entry contains: the specification you ran for the step (the prompt for Claude rows; the work plan for human rows); the handoff condition evaluation (the condition, the input you checked it against, the result, pass or fail or rewound); the supervisory capacity label for the step. Save as `execution-log.md`. Include any rows added during verification, with the row number, the reason for the addition, and the verification pass that surfaced the need.

5. **Verification pass against SDD needs.** All three passes, in order, against the running build. For each pass: the inputs you used, the expected behavior per the SDD, the actual behavior, the result (green, or green-after-fix, or amended-SDD). Pass 3 must include the *User Needs* sentences read against the build, with a per-sentence verdict. Save as `verification.md`.

6. **Post-build learning document — five sections.** What you built, what you delegated and why, what you kept and why, what you learned (domain register and self register), what you would do differently (concrete enough to change a row in your next score). Save as `post-build.md`.

The six files sit alongside the code repository. They are *the assignment*. The code is the byproduct.

## Exercises

1. **(Create) Complete your full Boondoggled build.** All six deliverables above, produced in order, against the project you picked from the brief. Submit the post-build learning document alongside the code; both are required. The success criterion is the one this chapter named — accountability for every decision — and the four-question instrument is the operational test. A build where you cannot answer all four questions for three or more rows is incomplete; mark the rows honestly in the post-build document rather than concealing the gap.

2. **(Evaluate) Return to the paragraph you wrote in Chapter 0, exercise 2 — the one about the calculator and arithmetic. Rewrite it now.** Do not look at the original until your rewrite is done. Then compare. What changed about the way you describe the gap between *using a tool* and *understanding the work the tool does*? What changed about which side of that line you locate yourself on? Submit both paragraphs, dated, with a one-paragraph commentary on the difference. The commentary, not the paragraphs, is the assessable artifact. *The change in how you describe the calculator paragraph is the change Dewey called continuity.*

3. **(Evaluate) Identify the supervisory capacity that was hardest to exercise consistently in your build. Design a practice exercise that targets it.** Pick one of the five — Specification, Direction, Verification, Boundary-setting, Synthesis — and name, from your execution log, the two or three rows where it strained or almost failed. Then design a stand-alone exercise — a short scoped task, ten to thirty minutes — that drills *only that capacity*. The exercise must be runnable by another student without the rest of your build. Submit the exercise as a one-page brief, with success criterion, time estimate, and the failure mode it is built to inoculate against. *Designing the exercise is itself an instance of Synthesis; you are building the next rung of your own ladder.*

## Closing

You built it. You know what every line does. You know why every decision was made. You know what you would do differently. That is what learning looks like. That is what Claude Code is for.

## 🕰️ AI Wayback Machine

**John Dewey** (1859–1952) was an American philosopher and educator whose two books on education — *Democracy and Education* (1916) and *Experience and Education* (1938) — make the case that learning is not the transmission of information from teacher to student but the *transformation of the learner* through purposeful experience. Dewey distinguished two principles that any genuinely educative experience must satisfy. *Continuity* — each experience modifies the experiences that follow it; the change in the learner persists. *Interaction* — the learner's internal state meets external conditions, and the meeting is where the learning happens. Information that the learner cannot put into use produces *miseducative* experience, in Dewey's vocabulary, because it leaves the learner less able to engage the next problem, not more.[^dewey] Dewey's most quoted line from the 1938 book, paraphrased by generations of teachers, is *education is not preparation for life; education is life itself.* The post-build learning document is the artifact that makes Dewey's principle operational for a 2026 high schooler with a Claude subscription. The document is the evidence that the experience modified you. Without it, the build is activity; with it, the activity becomes education.

[^dewey]: John Dewey, *Experience and Education* (Macmillan, 1938), and *Democracy and Education* (Macmillan, 1916). The two principles — continuity and interaction — are named in Chapter 3 of the 1938 book. The *miseducative* category is Dewey's; he is precise that not all experience educates, and the distinguishing test is whether the experience opens or closes the learner's capacity to engage the next problem. Donald Schön's *The Reflective Practitioner* (Basic Books, 1983) extends Dewey by distinguishing *reflection-in-action* (during the work) from *reflection-on-action* (after the work); the execution log is the first, the post-build document the second. Lorin Anderson and David Krathwohl's revision of Bloom's taxonomy (*A Taxonomy for Learning, Teaching, and Assessing*, Longman, 2001) names the apex level *Create* — *put elements together to form a coherent or functional whole; reorganize elements into a new pattern or structure* — which is what this chapter's terminal deliverable asks you to do.

**Run this:**

> *Read the post-build document I am pasting below. Find one sentence that describes a decision I would now make differently on my next build. Quote that sentence back to me. Then write the one-line change to my next Boondoggle Score that this sentence implies — a new row, a moved handoff condition, an added gate, or a rewritten supervisory-capacity label. Output: the quoted sentence, the implied score change, and one short paragraph explaining the connection between them.*

Paste your post-build document underneath the prompt. The output is the bridge between this build and the next. If the sentence Dewey would have called *experience becoming education* is in the document, Claude will surface it. If it is not, the post-build document is not yet finished.

---

*Where Seth's arc ended is where yours begins. The book has nothing else to say.*
