# Chapter 11 — Planning Your First Boondoggled Build

*Before Claude sees a single prompt, you know exactly what you are building, why, and which steps belong to you.*

---

**Learning outcomes**

1. **(Apply)** Complete a minimum viable SDD for a student-scale project.
2. **(Apply)** Generate a Boondoggle Score for the first phase of that project.
3. **(Analyze)** Identify the three steps in the build most likely to hit the dangerous middle.

---

## Opening: The Hour Before the First Prompt

The first time I ran a fully Boondoggled build from end to end, I sat at my desk for forty minutes and did not type a single word into Claude. I had the editor open. I had the project folder created. I had a fresh terminal in the right directory. The cursor blinked in the input box where I would, eventually, write the first prompt. I did not write it.

What I wrote instead was one sentence in a markdown file. The file was called `v0.md`. The sentence was: *"A web page that lets me add, check off, and delete tasks for one school day, and remembers them across browser refreshes."* I read it. I read it again. I deleted *one school day* and put it back. I deleted *across browser refreshes* and put it back. Then I sat with the sentence for a minute and made sure the sentence was the project — the whole project, no smaller, no larger.

This is the part of the discipline that is hardest to teach and easiest to skip. Forty minutes of staring at one sentence does not feel like work. It is the most important work of the build. While I was staring at the sentence, my brain stopped doing the thing it had been doing for years — narrating what I would type into the model — and started doing a different thing. It started thinking about *the problem*. About whether *one school day* meant Monday through Friday or twenty-four hours. About whether *delete* needed an undo. About what I would feel like, at 11 p.m. on a Sunday night, opening this page to plan a week. The narrator in my head — the one composing prompts — had been replaced, briefly, by an actual designer of an actual thing.

That replacement is the chapter. Everything that follows in Act Three — the SDD, the Boondoggle Score, the Minion Brief, the build execution log — is mechanical. But the cognitive shift the mechanics exist to *cause* is the shift I felt at minute forty, when I realized I had not been thinking about Claude for half an hour. The previous ten chapters were ten different ways of arguing for that shift. This chapter is where you do it.

What follows is the planning sequence in order, the gates between the steps, the test for whether a section is thin enough to break the build, and at the end, the complete planning document for the task tracker I actually built. The document is short. That is on purpose. The discipline is not in the length. The discipline is in what the document refuses to leave ambiguous.

<!-- → [DIAGRAM: The planning sequence — /v0 → /v1 → SDD core sections → Boondoggle Score → Minion Brief. Phase gates labeled. What must be true before each gate opens. Editorial style.] -->

The diagram above is the whole chapter in one image. Five nodes, four gates. The nodes are documents you produce. The gates are conditions that must be true before you move to the next node. You can walk back across a gate — the SDD often sends you back to /v1, the Boondoggle Score often sends you back to the SDD — but you cannot walk past a gate without satisfying its condition. The discipline is the gates, not the documents. The documents are evidence the gates were honored.

## The Planning Sequence

The sequence has five artifacts and four gates. I will walk them in order, then come back to read what each gate enforces.

**/v0 — the one-sentence problem statement.** One sentence. One user. One outcome. No conjunctions hiding a second project. *"A task tracker that's also a calendar and also a study planner"* is three projects pretending to be one and the /v0 gate exists to catch it. The single sentence is a brutal compressor. If you cannot say what you are building in one sentence, you do not yet know what you are building, and the next four artifacts will paper over the not-knowing in increasingly elaborate ways.

**/v1 — the scoped expansion.** Two short lists: *in scope* and *out of scope*. Both must be non-empty. The *out of scope* list is load-bearing. A /v1 that does not name what the project *will not do* is a /v1 that will quietly grow into something else under prompting pressure. The out-of-scope list is the document I refer back to most often during execution. Every time Claude proposes a "small addition," I check it against the list. If it is on the list, the answer is no. If it is not on the list, I think about whether it belongs on the list now.

**SDD — Software Design Document.** Five core sections. *Data model* (what the things in the system are, in concrete shapes). *UI states* (what the user sees in each state of the system — empty, populated, error). *Persistence* (what is saved, where, and when). *Failure modes* (one named failure mode per feature, with the response). *Out of scope, restated* (the /v1 out-of-scope list copied forward, so the SDD cannot disagree with /v1 by accident). The SDD is the document Claude will work from. The SDD is also the document you will defend against Claude. When Claude proposes a structure that does not match the data model section, the data model section wins.

**Boondoggle Score — the labor and handoff plan.** A table. One row per step in Phase 1. Five columns: step number, phase, labor (Claude or human), supervisory capacity (named, if the human is laboring), handoff condition (what must be true to call the step done). The score is the bridge between specification and execution. It is the document that answers the question *who does what next* without leaving any of the "next" ambiguous.

**Minion Brief — the per-Claude-step prompt specification.** For each row in the Boondoggle Score where labor is Claude, a short specification of what Claude needs to know to execute the step. Not the prompt itself — the specification *of* the prompt. This is where Chapter 8's "prompts as specifications" cashes out at the row level. You write the brief from the score. The brief is what you paste into Claude.

Now the gates.

**Gate 1 (/v0 → /v1).** *One sentence, one user, one outcome. No conjunctions hiding a second project.* Read the /v0 aloud. If you used the word *and*, check it is not hiding a second project. *"Add and check off tasks"* is one project; the *and* connects two operations on the same data. *"A task tracker and a calendar"* is two projects; the *and* connects two data models. Reject the second; accept the first.

**Gate 2 (/v1 → SDD).** *In-scope and out-of-scope lists are both non-empty.* A blank out-of-scope list means you have not yet noticed what the project might quietly grow into. Sit with the in-scope list and name five plausible additions that are *not* in this project. Put them in out-of-scope. They are now there to be defended.

**Gate 3 (SDD → Score).** *Every SDD section has at least one concrete artifact — a file name, a data shape, a UI state, a named failure mode.* Sections that say "we will design the data model later" do not pass this gate. Sections that say "the data model is one `Task` interface in `types.ts` with `id: string`, `text: string`, `done: boolean`" do.

**Gate 4 (Score → Minion Brief).** *Every Score row has a named handoff condition. No row says "Claude figures it out."* This is the gate Chapter 9 spent its entire length defending. A row whose handoff condition is *"Claude completes the function"* is a row that has not been planned; it has been delegated to the dangerous middle. A row whose handoff condition is *"three unit tests pass on a Task array of length 0, 1, 3"* has been planned.

The four gates are the planning gate. When all four are satisfied, the planning gate is open, and you may write the first prompt. Before all four are satisfied, you may not.

## How to Scope a Student Project

A first Boondoggled build should be smaller than the project you think you want to do. This is the single most reliable piece of advice in this book and it is also the advice students most reliably ignore.

The reason to start smaller is mechanical. The Boondoggling discipline has a per-step overhead — the SDD section, the score row, the handoff condition, the verification — that does not exist in ungoverned vibe-coding. If you choose a project at the upper limit of what you could build *without* the discipline, you will discover the overhead and quit. If you choose a project at the lower limit — something you could already build in a weekend without Claude — you will see the discipline produce a build that is *better* than your unaided weekend build, and you will keep going.

The right size, in my experience, is a project a fluent classmate could build in two unstructured weekends. You will build it in one Boondoggled week. The artifact you produce will be cleaner than the weekend version. The capability you produce will be substantially larger, because you will know, for every line in the project, whether it was your decision or Claude's. That knowledge does not exist in the weekend version.

Three scoping heuristics that work at student scale:

*One user, one device, one session.* Your first project has one user (you), runs on one device (your laptop or your phone), and is used in one session at a time. Multi-user, multi-device, and concurrency are three of the hardest things in software. Each of them is out of scope.

*No external dependencies you have not used before.* If you have never touched a Postgres database, your first Boondoggled build is not the project that introduces Postgres. Use the storage primitive you already know — files, `localStorage`, a JSON blob. Claude can absolutely write Postgres code. The question is not whether Claude can write it. The question is whether *you* can supervise it. Chapter 5's framework: supervisory capacity is the binding constraint, not technical capacity.

*One named user need, not three.* The /v0 sentence names one need. Resist adding the second need, the third need, the "while we're at it" feature. The most painful out-of-scope items to write are the ones you actually want. Write them anyway. They are why the project will finish.

A useful self-test: write your /v0 sentence and your /v1 lists, then estimate, honestly, how long the project would take a classmate who is fluent with the stack. If the answer is more than a weekend, scope down.

## Reading a Boondoggle Score

The Boondoggle Score is not a checklist. It is a *diagnostic*. A score that has been honestly produced tells you three things by inspection, before you have written a single line of code: where the critical path runs, where the highest-risk handoffs are, and whether your supervisory capacity is reasonably distributed across the build.

<!-- → [TABLE: Boondoggle Score summary — columns: Step number, Phase, Labor (Claude/Human), Supervisory capacity (if human), Handoff condition. Five rows showing a Phase 1 example.] -->

The table above is the Phase 1 score for the task tracker. Read it left to right, then read it top to bottom, and then read it diagonally for the pattern.

*Critical path* is the sequence of rows where each row's handoff condition is a precondition for the next row's labor. In the table, rows 1 → 2 → 3 are critical: the data model (row 1) must exist before the page scaffolds against it (row 2), and the page must exist before the mutation functions can be tested in it (row 3). Rows 4 and 5 are parallel paths off the spine. If a critical-path row fails its handoff condition, the build stops. If a parallel row fails, the build continues with that branch flagged.

*Highest-risk handoffs* are the rows where the cost of a missed handoff condition is largest. Row 1 is the highest-risk row in the build, even though it is the smallest in lines of code, because the data model is the row every other row inherits from. A wrong data model in row 1 produces a wrong UI in row 2, wrong mutation functions in row 3, wrong persistence in row 5, and a visual hierarchy in row 4 designed against a wrong model. A row whose handoff condition fails late looks like one mistake; it is really five.

The diagnostic test: ask, for each row, *what is the cost if this row's handoff condition turns out to be wrong?* Rows whose answer is *"the rest of the build inherits the wrong assumption"* are the highest-risk rows. They are typically the rows at the top of the score, and typically the rows assigned to the human.

*Supervisory capacity distribution* is the third thing the score tells you. Look down the *Labor* column, then look down the *Supervisory capacity* column for the human rows. The named capacities (from Chapter 5) should be *different* on different rows. A score in which every human row says *"domain knowledge"* is making the student do the same kind of thinking in five different places. A score in which every human row says *"Claude figures it out"* is delegated work, not Boondoggled work.

A healthy Phase 1 distribution: half the rows Claude, half human; the human rows cover at least two distinct supervisory capacities; the highest-risk row is human. If your score does not look like that, the score is telling you something is wrong with the plan.

## When the Score Reveals a Thin SDD Section

This is the most common diagnostic in early Boondoggling, and the first time it happens it feels like a failure. It is not. It is the score doing its job.

The pattern: you finish the SDD, you start writing the Boondoggle Score, and at some row — usually two or three rows in — you cannot write a clean handoff condition. The condition you write keeps being vague. *"The function works."* *"The data is saved."* *"The page looks right."* You read what you wrote and you can hear, in the words, that you do not know what *works* means for this row.

This is the score telling you a section of the SDD is thin. The handoff condition's vagueness is the visible symptom; the cause is a section of the specification that did not commit to a concrete artifact. *"The function works"* is what you write when *failure modes* was thin. *"The data is saved"* is what you write when *persistence* was thin. *"The page looks right"* is what you write when *UI states* never named the states.

The response is not to write a better handoff condition. The response is to walk back through Gate 3, find the thin section, thicken it, and re-walk Gate 3. Thickening means naming a concrete artifact: a file, a data shape, a state, a failure trigger. *"The data is saved"* becomes *"after every mutation, the full task array is written to `localStorage['tasks']` as JSON; on load, malformed JSON triggers a confirmation dialog and a reset."* Now the handoff condition can be sharp: *"refresh preserves state across 10 toggles; corrupt JSON triggers reset-with-confirm prompt."*

The gate works because the score *cannot lie*. You can write a vague SDD section and convince yourself it is specific. You cannot write a vague handoff condition without hearing the vagueness. The Boondoggle Score is the SDD's lie detector.

A second pattern, less common but worth naming: the score reveals a section is thin because it was *not the right section*. The data model section may not be the load-bearing part of this project — the rubric is, or the input schema is, or the timing constants are. Move it. The SDD is not a fixed template. If your project's load-bearing artifact is not the data model, your SDD's longest section is not the data model.

## The Planning Gate

The planning gate is the moment you decide planning is complete and the first prompt is permitted. It is the single most important decision in the build, and it is the decision most students get wrong — almost always in the same direction. They open the gate early. They open it because the planning is slow, and the execution will be fast, and the asymmetry is uncomfortable.

The gate opens when all four sub-gates are satisfied:

1. **/v0 reads as one sentence, one user, one outcome.** A reader who does not know your project understands what you are building from the sentence alone.
2. **/v1 has a non-empty out-of-scope list with at least three items you actually want.** The list contains the temptations, not just the obvious exclusions.
3. **The SDD has five sections, each with at least one concrete artifact named (file name, data shape, UI state, named failure mode).** No section says *"we will figure out later"*.
4. **The Boondoggle Score's every row has a sharp handoff condition.** No row says *"Claude figures it out"* or *"it works"*.

When all four are true, the gate is open. When any one is false, the gate is closed.

There is a fifth, informal test that the gate is open: you have not thought about Claude for the last ten minutes. You have been thinking about the project — the user, the data, the failure, the state — and Claude has, briefly, disappeared from your mental workspace. That disappearance is the cognitive shift the planning phase exists to cause. The planning gate is the door between *thinking about the tool* and *thinking about the work*. When you have walked through, you know it; the silence of the absent narrator is loud.

There is a temptation, here at the gate, to start prompting "just to see." Resist it. The first prompt is special. It is the moment the build is no longer reversible at zero cost. After the first prompt, there is generated code in your workspace, and the cost of pivoting includes the cost of throwing the code away. Before the first prompt, pivots are free.

The economic case for the gate is the IBM Systems Science Institute defect-cost curve, in which a defect caught in design costs roughly 1×, in implementation roughly 6×, and in production 30–100×.[^ibm] You may have seen the curve in a software engineering textbook; it is the most-cited chart in software economics. The mechanism is the one the chapter has been arguing: the planning gate is the only phase where a wrong decision is cheap. After the gate, every wrong decision compounds against the code already written against it. The independent industrial confirmation is Code Climate's 2023 report, which found that engineering teams spend roughly 26% of their effort reworking code before release, and rework code costs roughly 2.5× more than first-pass code.[^code-climate] Boondoggling's claim is that the same curve applies, and possibly steepens, in agentic AI work — because Claude makes producing wrong code *cheap*, which makes catching it *late* the new default. The gate is the countermeasure.

[^ibm]: The widely circulated curve is the IBM Systems Science Institute figure (1970s); the canonical academic citation is Capers Jones, *Applied Software Measurement*, 3rd ed. (McGraw-Hill, 2008). The original IBM source is not freely available; treat the specific multipliers as order-of-magnitude rather than precise. [verify]

[^code-climate]: Code Climate, *Engineering Insights Report* (2023). Industry-wide instrumentation across hundreds of engineering organizations; rework percentage and cost multiplier are aggregated metrics, not single-study results.

The deeper case is Fred Brooks's, made in "No Silver Bullet" at the IFIP Tenth World Computing Conference in 1986: software's *essential* difficulties — complexity, conformity, changeability, invisibility — are not reduced by faster tools.[^brooks] Faster tools reduce *accidental* difficulty. The planning phase is where you work on the essential difficulties. The execution phase is where you and Claude work on the accidental. A student who skips planning is not working faster; they are working entirely on accident and calling it essence. The gate is Brooks's distinction made operational.

[^brooks]: Frederick P. Brooks Jr., "No Silver Bullet — Essence and Accident in Software Engineering," in *Proceedings of the IFIP Tenth World Computing Conference* (1986); reprinted in *Computer* (April 1987) and in *The Mythical Man-Month*, Anniversary Edition (Addison-Wesley, 1995). Verified PDF at worrydream.com.

There is one more reason the gate matters. The plan/execute split is now the dominant architecture in production agentic systems. Yao and colleagues' ReAct paper (2022) operationalized interleaved planning and acting at the model level.[^react] The 2025 survey of plan-then-execute implementations across LangGraph, CrewAI, and AutoGen confirms the split is no longer a stylistic preference but a structural assumption of production agent systems.[^pte] Claude Code's *plan mode* is the same distinction surfaced as a UI affordance. The Boondoggle Score is the *human-side* of this architecture: the reasoning the student writes down before Claude is permitted to act.

[^react]: Shunyu Yao et al., "ReAct: Synergizing Reasoning and Acting in Language Models," ICLR 2023, arXiv:2210.03629 (October 2022). The paper introduces the interleaved plan-act trace; the Boondoggle Score is the human-readable analog at project scale.

[^pte]: *Architecting Resilient LLM Agents: A Guide to Secure Plan-then-Execute Implementations*, arXiv:2509.08646 (2025). Surveys the plan-then-execute pattern across major agent frameworks; useful as confirmation that the planning/execution split is now an industry default rather than a stylistic choice.

## Start with Codebase Q&A, Not Code Editing

A note for the case you may not be starting fresh.

If your first Boondoggled build is in an existing codebase — a repo a club ships, a project from a previous class, a fork of something on GitHub — the planning sequence has a prequel. Before the /v0 sentence, before the SDD, you spend at least one session asking Claude questions about the code. No edits. No prompts to "fix" or "add" or "refactor." Questions only.

The reason is mechanical: an SDD written against a codebase you do not understand will produce a Boondoggle Score whose handoff conditions are wrong, because the *conditions* will inherit the misunderstandings. It is also genuinely difficult to write a /v0 sentence about a project whose existing shape you cannot describe — the sentence comes out either too small (you scope only the file you happen to have open) or too large (you scope as if from scratch, ignoring what is already there).

The codebase Q&A session is structured. You ask Claude, in order: *What does this project do? How is it organized? What is the data model? Where does state live? What does the user actually do with the running system? Where in the code does that user action enter, and where does its effect leave? What runs at startup? What are the external dependencies and what does each provide?* You do not paste code at Claude — Claude Code can read the code on its own. When you have written down the answers in your own words, the existing project has stopped being a black box, and you can write a /v0 sentence that is honest about what is being added to what.

The Q&A session is the gate before the gate — the precondition for the planning gate when the planning is not greenfield.

## Worked Example: The Task Tracker Plan

What follows is the entire planning document I wrote for the build I will execute in Chapter 12. The /v0 sentence, the /v1 expansion, the abbreviated SDD, and the Boondoggle Score for Phase 1. It fits on roughly one printed page. That is on purpose. A student SDD is not an industrial SDD. The minimum viable spec is the spec that admits no ambiguous handoff — no longer.

---

**`v0.md`**

> A web page that lets me add, check off, and delete tasks for one school day, and remembers them across browser refreshes.

*Gate 1 read-aloud check:* One sentence. The user is *me*. The outcome is *I keep track of one school day's tasks across refreshes*. The *and* in *add, check off, and delete* connects three operations on the same data — one project, not three. Gate 1 open.

---

**`v1.md`**

*In scope*

- Add a task with text only
- Toggle a task between done and not done
- Delete a task
- Persist all tasks across browser refresh on the same device
- Keyboard-only operation if I want it

*Out of scope*

- Multiple days, calendars, due dates, due times
- Sync across devices or browsers
- Login, accounts, user identity
- Tags, categories, priorities, sorting beyond "as entered"
- Mobile-specific layout (it will work on mobile; it is not designed for mobile)
- Editing the text of an existing task (delete and re-add)

*Gate 2 check:* In-scope has five items I will build. Out-of-scope has six items I actually want and will not build. The temptations are on the list. Gate 2 open.

---

**`sdd.md`** *(abbreviated; five sections)*

*Data model.* One `Task` interface, declared in `types.ts`:

```ts
interface Task {
  id: string;       // crypto.randomUUID()
  text: string;     // 1..200 chars, trimmed
  done: boolean;    // default false
}
```

Tasks live in memory as `Task[]`. Order is insertion order; never resorted.

*UI states.* Three: *empty* (no tasks; show a single placeholder line), *populated* (one or more tasks; show the list and the input), *all-done* (all tasks `done: true`; show the list and a quiet "all done for today" message). No loading state — `localStorage` reads are synchronous.

*Persistence.* On every mutation (`addTask`, `toggleTask`, `deleteTask`), serialize the full `Task[]` to `localStorage['tasks-v1']` as JSON. On page load, read `localStorage['tasks-v1']`, attempt `JSON.parse`. On parse failure, show a confirmation dialog (*"Saved tasks are corrupted. Reset?"*); on confirm, clear the key and start empty; on cancel, leave the key untouched and show an empty list for this session.

*Failure modes.* (1) Corrupt JSON in storage — handled above. (2) `localStorage` unavailable (private browsing) — fall back to in-memory only; show a one-line banner: *"Tasks won't persist in this window."* (3) Task text exceeds 200 chars — reject submission, focus stays in input, no error toast.

*Out of scope, restated.* See `v1.md`. Most relevant during execution: no editing of task text, no due dates, no sorting beyond insertion order.

*Gate 3 check:* Five sections. Each names at least one concrete artifact (interface name, state list, storage key, failure trigger). Gate 3 open.

---

**`boondoggle-score-phase-1.md`**

| # | Phase | Step | Labor | Supervisory capacity (if human) | Handoff condition |
|---|---|---|---|---|---|
| 1 | Phase 1 | Define the `Task` data model | Human | Domain knowledge: what fields this app actually needs | Written as one TypeScript interface in `types.ts` before any UI code |
| 2 | Phase 1 | Scaffold the HTML page (head, single root div) | Claude | — | Page loads in browser with empty root; no console errors |
| 3 | Phase 1 | Implement `addTask`, `toggleTask`, `deleteTask` | Claude | — | Three unit tests pass on `Task[]` of length 0, 1, 3 |
| 4 | Phase 1 | Decide visual hierarchy (font, spacing, contrast) | Human | Aesthetic judgment: what this student wants the app to feel like | Three CSS variables (`--font`, `--primary`, `--accent`) named in `style.css` before Claude writes any styles |
| 5 | Phase 1 | Wire `localStorage` save/load with corruption guard | Claude | — | Refresh preserves state across 10 toggles; corrupt JSON triggers reset-with-confirm dialog |

*Gate 4 check:* Five rows. Two human, three Claude. Human rows cover two distinct supervisory capacities (domain knowledge, aesthetic judgment). The highest-risk row (row 1, data model) is human. Every row's handoff condition is testable: a written interface, a loaded page with no console errors, three passing tests, three named CSS variables, a refresh-and-corruption check. No row says *"Claude figures it out."* Gate 4 open.

---

That is the entire planning document. Roughly one printed page. It took me two hours to write, most of which I spent on the out-of-scope list and the failure modes — the two sections students are most tempted to skip and which catch the most bugs. The document is now the contract Chapter 12's build executes against. When Claude proposes that we add tags to the data model "since we're touching it anyway," the answer is on this page. When Claude proposes a loading spinner because *localStorage* is "async," the answer is on this page. The document does not just plan the build. It *defends* the build — against Claude and against me.

Note what the document is not. It is not a paragraph of prose. It is not maximalist. It is the minimum that admits no ambiguous handoff in the planning gate's four sub-gates. The pedagogical case for this minimum-viable form is reinforced by the project-based learning literature: Chen and Wang's 2019 meta-analysis finds *student-led written planning* among the strongest moderators of project quality, and Larmer, Mergendoller, and Boss's *Setting the Standard for Project Based Learning* (2015) names the Project Planning Form a non-negotiable of gold-standard PBL.[^pbl] The discipline is not Boondoggling's invention; it is the through-line of every serious project pedagogy. Boondoggling adds the labor column and the handoff condition that the PBL planning form does not have, because PBL was written before the labor question had a model in it.

[^pbl]: Yan Chen and Jane Wang, "Revisiting the effects of project-based learning on students' academic achievement: A meta-analysis investigating moderators," *Educational Research Review* 26 (2019); John Larmer, John Mergendoller, and Suzie Boss, *Setting the Standard for Project Based Learning* (ASCD / Buck Institute, 2015). The Frontiers in Psychology 2023 PBL synthesis reports an overall effect size of g ≈ 1.11 on academic achievement under high-quality implementation.

## Exercises

1. **(Apply)** Produce a /v0 sentence, a minimum viable SDD, and a Phase 1 Boondoggle Score for the project you will build in Chapter 12. Constraints: the /v0 sentence must be one sentence; the SDD must have all five core sections, each with at least one named concrete artifact; the Score must have at least four rows, with at least one human row whose supervisory capacity is named from Chapter 5's framework. Total length: target one printed page. Time budget: 90 minutes; if you exceed, stop and note where the time went.

2. **(Analyze)** Take the Boondoggle Score in the worked example above (the task tracker, Phase 1, five rows) and identify the two highest-risk handoffs. Write a one-sentence justification for each — the *cost if the handoff condition turns out to be wrong*. Then, for each of the two, write a strengthened handoff condition that would reduce that cost. (Hint: row 1 is one of the two. The second is not row 3.)

3. **(Evaluate)** Read your SDD from Exercise 1. Find the weakest section — the one whose concrete artifacts feel least specific, or whose failure modes you had to invent rather than recognize. Write down why it is the weakest. Then fix it before you begin Chapter 12. The chapter cannot start until the weakest section can stand up to the Score's handoff conditions.

Do all three in order, in one sitting, on the same project. Exercise 1 produces the document; Exercise 2 stress-tests the worked example so Exercise 3 has a reference point; Exercise 3 is the planning gate applied to your own document. When you have done all three, the planning phase of your first Boondoggled build is complete.

## 🕰️ AI Wayback Machine

**Christopher Alexander** (1936–2022). British-Austrian-American architect and design theorist. *A Pattern Language: Towns, Buildings, Construction* (Oxford University Press, 1977), co-authored with Sara Ishikawa and Murray Silverstein, presented 253 design patterns — each one structured in the same three movements: *problem statement, discussion, core solution.* The book physically refuses to present a solution until the problem has been named in standalone form. *The Timeless Way of Building* (Oxford University Press, 1979) is the theoretical companion: design begins with what the inhabitant needs, not with what the builder wants to make. Alexander called the goal *the quality without a name* — the quality of fit between problem and form, not aesthetic preference, not signature style. The SDD is Alexander's structure applied to software: problem first, in writing, before any solution. The /v0 gate is the pattern-language gate. Forty-nine years before this chapter, an architect wrote down the rule the chapter is enforcing.

**Run this:**

> Read *A Pattern Language* (Christopher Alexander, Ishikawa, and Silverstein, 1977) — any single pattern, your choice. Quote the pattern's three movements (problem statement, discussion, core solution) in fewer than 200 words. Then write the equivalent three movements for a single feature of the project you planned in Exercise 1. Where does Alexander's structure refuse to compress? Where does it map cleanly onto the SDD's sections? Where does it suggest something your SDD is missing?

---

The plan is complete. Chapter 12 executes it.
