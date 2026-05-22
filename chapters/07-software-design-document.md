# Chapter 7 — The Software Design Document: Building the Mission Before the Build

*The SDD is not bureaucracy. It is the decision that prevents you from making the same decision twice under deadline pressure.*

---

**Learning outcomes**

1. **(Understand)** Explain why a Software Design Document is a prerequisite for a productive Boondoggle Score.
2. **(Apply)** Complete the core sections of an SDD for a student-scale project.
3. **(Analyze)** Identify the sections of an SDD most likely to reveal a problem formulation gap.

---

## Opening: Three Hours In

Seth opens his laptop on a Saturday morning. He has a `/v0` sentence from Gru — *a local task tracker that lets one AP CS student see today's lab steps in order without a login* — and he has, in his head, what he believes is a clear picture of what he is about to build. He opens Claude Code. He starts.

Three hours later, he stops.

Not because anything has crashed. The code compiles. The page loads. He can add a task. He can mark it done. There is even a stub for a calendar view, which Claude proposed in passing and which Seth accepted in passing, and which is now three files deep into the project. The thing on the screen is, by most fair measures, *a task tracker*. It is also, Seth realizes, not the one he set out to build.

The user model has slipped. Somewhere around the second hour, Claude began treating the user as plural — "users can log in," "users can share lists" — and Seth, deep in a function and not paying full attention, accepted the change without noticing. The data model has slipped along with it; what was supposed to be a single JSON blob in `localStorage` is now sketched as a server-backed `users` table with a foreign-key relationship to `tasks`, because at some point Claude said *"you'll probably want a real backend for this"* and Seth, who has built three things this semester and is starting to suspect a real backend is a real skill, said yes.

The decision Seth needed to make was at the *beginning*, not the middle. He needed to write down, in advance, what the system was and — more importantly — what it was *not*. He didn't. So Claude made the decisions instead, one quiet drift at a time, each one defensible in isolation, each one wrong against the project Seth was actually trying to build.

This chapter is about the document Seth didn't write.

<!-- → [DIAGRAM: The SDD as decision record — five sections as labeled boxes in sequence: Problem Statement → Architecture Principles → Core User Flows → User Needs → Component List. Arrows showing dependency. Editorial style.] -->

The diagram above is the chapter's spine. Five sections, in order, each constraining the next. The Problem Statement is the load-bearing sentence the rest of the document is built on. The Architecture Principles are the decisions that can collide with future asks — and that, when they collide, *win*. The Core User Flows are the verbs the user performs in order. The User Needs are the outcomes — *testable* outcomes, not features. The Component List is what gets built. A small back-arrow from Component List to Problem Statement is the consistency check: if the components you've named can't deliver the problem statement, something upstream is wrong.

The rest of the chapter walks the five sections, names what makes each one strong or weak, builds a complete minimum viable SDD inline so you see what one actually looks like, and then turns the document over to you as the artifact your next Claude session is going to read before it writes a line of code.

## What an SDD Is and Is Not

A **Software Design Document** is a written record of the design decisions a project depends on. That is the entire definition. It is not a project plan. It is not a feature list. It is not a Gantt chart. It is not a pitch deck. It is not a thing you write once for a teacher and then forget. It is a record — durable, version-controlled, re-readable — of the decisions that, if changed, would change the system.

The distinction matters because students arrive at the SDD with one of three wrong mental models, and the wrong model determines whether the document does any work.

The first wrong model is **SDD as paperwork**. The student picture: a Word doc no one reads, written to satisfy a rubric. Under this model, the SDD is a tax. You pay it, you submit it, you forget it. The document never opens again. This model is the one most students inherit from previous classes, and it is the one the rest of this chapter is built to refute.

The second wrong model is **SDD as feature list**. Under this model the SDD is a catalog: "users can log in, users can add tasks, users can mark done, users can share, users can export." This is the failure mode that turns SDDs into wish-lists. The list is unconstrained — every entry is *a thing the system could do* — and the document has no way to refuse anything. The architecture-principles section, if it exists at all, reads like a tagline ("clean code, fast, easy to use"). Six weeks later the build is twice the size it should be, half of it abandoned, the other half half-finished. The list never said *no* to anything.

The third wrong model is **SDD as project plan**. Under this model the document is a schedule: week one we do X, week two we do Y. Schedules are useful but they are not what the SDD is for. The SDD answers *what we are building and what cannot change*; the schedule answers *when we are building it*. They are different documents addressing different questions. Conflating them produces an SDD that ages out of relevance the first time the schedule slips, which it always does.

The right model — the one this book uses, and the one the rest of this chapter operationalizes — is **SDD as decision record**. The document captures the decisions that, if reversed, would force a redesign. Not every decision. Not the small ones. The ones whose reversal cascades. The font of the page is not in the SDD. The choice of `localStorage` over a backend is. The choice of single-user over multi-user is. The choice of "no login" as a hard constraint is. These three decisions, written down, are what would have prevented Seth's three-hour drift. Two of them — single-user, no login — were in his head when he started. None of them were in writing where Claude could read them.

The intellectual lineage of "SDD as decision record" is short and worth naming, because the lineage is what justifies the practice against the student suspicion that the document is bureaucracy.

The IEEE 1016 standard (*IEEE Standard for Information Technology — Systems Design — Software Design Descriptions*, current edition 2009) defines an SDD as a document that "describes the design entities, their relationships, and the design decisions" of a system. The standard is format- and methodology-agnostic. Critically, it does not prescribe length. A one-page SDD that meets the standard is as conformant as a hundred-page SDD, provided the design entities, their relationships, and the decisions are all documented. The student version of IEEE 1016 is the five-section minimum this chapter develops. The standard is the warrant; the minimum is the artifact you actually write.

The lightweight tradition the student SDD descends from is the **arc42** template (Starke and Hruschka, 2005–present), open-source under Creative Commons, available in Markdown and AsciiDoc, designed explicitly for teams who would not write a hundred-page IEEE doc but would write twelve sections of one page each. arc42's design move is to *fix the section list* so the student does not have to invent the document's structure. This chapter's five-section minimum is the arc42 move, cut further: five sections, not twelve, sized for a project a student can finish in a weekend.

The recent-five-years descendant of arc42, and the artifact that has gone from niche-blog to default practice in industry between 2017 and the present, is the **Architecture Decision Record** (Nygard 2011; Fowler 2017). An ADR is *one decision, one page, one filename*, stored in `/doc/adr/` alongside the code. The SDD in this chapter is, in effect, the *first* ADR of a project — the meta-decision that wraps the project's other ADRs. Students who go on to internships will find ADRs everywhere; the SDD is the on-ramp.

The thing the SDD is *not*, finally, is a document optimized solely for human reading. As of 2026, structured design documents have a second reader — the model. Paste your SDD into Claude's prompt at the top of a session and it becomes the context window's load-bearing prefix. Without that prefix, Claude redesigns your system every Saturday. The SDD is currently the cheapest tool in the toolbox for fixing that. Whether the format that's best for humans is the same as the format that's best for an LLM is an open question, and it's worth flagging — currently the answer appears to be *yes, one document serves both readers*, but this may change as model context handling matures.

## The Five Sections That Matter at Student Scale

There are five sections. They go in order. Each constrains the next.

### Problem Statement

The Problem Statement is the `/v0` sentence, lifted from Gru and placed at the top of the SDD. It says *what the system does, for whom, and what makes it count as done*. It is one sentence. It is the load-bearing claim. Every other section of the SDD is downstream of it; if it changes, the rest of the document changes with it.

A weak Problem Statement is unspecific: "An app for productivity." It names no user. It names no done-condition. It cannot be refuted by any future feature, because every conceivable feature is "for productivity." It is not actually a constraint. A strong Problem Statement is specific enough that you can name features that *violate* it: "A local-only task list for one AP CS student to see today's class assignments without a login." The phrase *local-only* refuses a backend. The phrase *one* refuses multi-user. The phrase *without a login* refuses authentication. The phrase *today's class assignments* refuses a calendar view. Each refusal will matter later. None of them are visible in the weak version.

### Architecture Principles

The Architecture Principles are the design decisions that constrain everything downstream and that, under pressure, *win*. Three is the right number at student scale; fewer than three is usually a sign you haven't thought it through, more than five is usually a sign you've started writing taglines.

A principle is not a value. "Clean code" is a value; it is not a principle, because nothing in the project ever defends a decision by saying "actually, we want our code to be dirty here." A principle has to be able to *lose* to another principle under pressure. If two principles cannot collide under any realistic decision, at least one of them is a slogan.

This is the **Principle Collision Test**, and it is the single most useful diagnostic in this chapter. Take any two of your Architecture Principles. Construct a realistic situation in which they would tell you to do different things. If you can, both are real principles. If you can't, one of them is a tagline, and you should rewrite it until the collision is visible.

Worked: in Seth's task tracker, *local-only storage* and *sync across devices* would collide. Under that pressure, the SDD makes the decision in advance: local-only wins. Sync loses. This is documented; it is not re-litigated three hours into a build at 11 p.m. The Principle Collision Test is, in software-engineering vocabulary, an *information hiding* discipline in the sense Parnas (1972) intended: each principle hides a design decision that *would otherwise be made implicitly, differently, by different parts of the system*. Parnas's contribution was to argue that the right criterion for decomposing a system into modules was not the system's process flow but the *decisions each module was concealing*. Architecture Principles, written down, are the project-level version of that move.

### Core User Flows

The Core User Flows are the verbs the user performs in order. They are not features. They are not screens. They are the path through the system, from arrival to outcome, as the user experiences it.

A weak Core User Flow is exhaustive: "users can add, edit, delete, archive, share, export, search, filter, tag." This is a feature catalog dressed as a flow. A strong Core User Flow names *the one path that matters*, in order, with as few steps as the work allows: "add task → mark done → see today's list. Three steps, no detours." The strong version is testable; you can sit a real student down and watch whether the three steps produce the outcome. The weak version cannot be tested because it does not commit to any particular path.

The discipline here is restraint. You will be tempted to list every possible flow. Resist. Most projects have exactly one core flow that, if it works, the project works, and if it doesn't, nothing else matters. Find it. Write it. Trim the rest to one line of *secondary* flows the system supports, if any, and stop there.

### User Needs

The User Needs section is the outcomes the user must get, written as *testable conditions*. This is the section where most student SDDs collapse, because the temptation to write feature descriptions is overwhelming and the discipline to write outcomes is unfamiliar.

A weak User Need is a feature: "should have a calendar view." A weak User Need is also a value: "should be fast and easy." A strong User Need is a testable outcome: "today's tasks render in under 2 seconds on a Chromebook." It names a condition, an environment, and a threshold. It is decidable — you can sit at a Chromebook with a stopwatch and the question is answered. It is also negative-answerable: if the render takes three seconds, the need has *failed*, and you have the diagnostic on the spot.

A second strong User Need: "no data is lost when the tab is closed." Testable: close the tab, reopen, verify. A third: "the page is usable with one hand on a phone-sized screen." Testable: pick up your phone, hold it, try.

Three strong User Needs is the right shape for a student SDD. They are the conditions under which the project is *good*. They are also, not coincidentally, the conditions a Boondoggle Score (Chapter 6) can be measured against. Without them, the Boondoggle Score is measuring effort against a moving target.

### Component List

The Component List is the buildable parts of the system, named at the right level of granularity. It is the *last* section of the SDD, not the first, because every other section constrains what belongs on this list.

A weak Component List is generic: "frontend, backend, database." These are not components; they are architectures-in-the-large. A strong Component List names the specific pieces this specific project needs: "input form, localStorage wrapper, list renderer, today-filter. No backend." Note the *no backend* — the Component List can name things the system is *not* going to have, and at student scale this is often where the document earns its keep, because it forecloses temptations that will arise mid-build.

The Component List is also where the SDD's consistency check fires. Walk back up the document: does this Component List deliver this Problem Statement, under these Architecture Principles, supporting these Core User Flows, satisfying these User Needs? If you cannot answer yes to all four, something is wrong, and the answer is to revise *upward* — change the components, or, more often, discover that the upstream sections were over-promising. The back-arrow in the diagram is doing this work.

<!-- → [TABLE: Minimum viable SDD — section name, one-sentence description, what a weak version looks like, what a strong version looks like. Five rows.] -->

## Why /v0 Is the SDD's Foundation

The SDD has five sections, but it has one load-bearing claim, and that claim is the Problem Statement. Everything else in the document is downstream. If the Problem Statement is wrong — if it is unspecific, or two-systems-disguised-as-one, or unanchored to a real user — the rest of the document inherits the wrongness, and no amount of careful work on Architecture Principles or User Needs will fix it.

This is why `/v0` from Chapter 6 is not a separate exercise from the SDD; it is the *first section* of the SDD, treated separately because it bears so much weight that it deserves its own gate. When Gru refuses to advance you past `/v0` until the sentence passes, Gru is refusing to let you start an SDD on top of a sentence that won't hold the weight.

The structural test for whether a Problem Statement can hold the weight is the same test from Chapter 6, restated here in the SDD's vocabulary:

1. *Does the sentence name a single system?* If there is an `and` connecting two things the system does, the sentence is two Problem Statements; pick one.
2. *Does the sentence name the user — specifically?* "One AP CS student" passes; "students" does not. The named user is what makes the User Needs section testable, because you can ask whether *this* user can do *this* thing under *these* conditions.
3. *Does the sentence name the done-condition?* "Without a login, in under two seconds, on a Chromebook" is a done-condition; "easy to use" is not.

The three tests are checkable in under a minute. Run them. If your sentence fails any one of them, you are not yet at `/v0`, and the SDD cannot begin.

The deeper reason `/v0` is the foundation is that *every other section of the SDD is a refinement of the Problem Statement, not an addition to it.* Architecture Principles refine the *constraints* the problem statement implies. Core User Flows refine the *user path* the problem statement promises. User Needs refine the *done-condition* the problem statement names. Component List refines the *system* the problem statement defines. The document is not five independent sections; it is one sentence elaborated five ways. If the sentence is wrong, the elaborations are wrong, and the document is wrong from the top.

## The Minimum Viable SDD

How much SDD is enough?

The honest answer is *we currently do not know empirically what the floor is at high-school scale*. The 2024 student-quality studies (e.g., the 172-student arXiv study on OOP project quality) measure code quality as a function of process discipline but do not isolate SDD section count as a variable. The industrial evidence — Info-Tech's roughly 50% rework rate attributable to requirements issues, the NASA Mars Climate Orbiter case where unwritten interface assumptions cost $327.6 million — establishes that *some* SDD is much better than *no* SDD, but does not tell us how many sections are enough.

This chapter takes a defensible position: at student scale, five sections is the minimum that does the work, and the five are the ones listed above. The position is defensible because each of the five does work that *cannot be done by another section*. Cut any one and the SDD loses a function:

- Cut **Problem Statement** and the document floats, anchored to nothing.
- Cut **Architecture Principles** and the document cannot refuse anything; every feature is in scope.
- Cut **Core User Flows** and the document does not describe a path through the system, only a pile of capabilities.
- Cut **User Needs** and the document cannot be tested; "done" becomes a feeling.
- Cut **Component List** and the document does not connect to the code; nothing on disk has to match anything in the doc.

For a 200-line script, five sections may feel like overkill. It is not; the five sections will fit on one page. The shape that is overkill at this scale is a thirty-page IEEE 1016 conformant document with appendices. Don't write that. Write the five sections, one page total, and *use* the document — paste it into Claude at the top of every session, reread it before each build, revise it deliberately when a decision changes.

For a capstone project (Chapter 14), the five sections may not be enough; a data model section and an interface specification section may be necessary additions. That is a discussion for Chapter 14. At Chapter 7's scale, five.

## What the SDD Unlocks

The SDD is not its own reward. It is a *prerequisite* for two things the rest of the book depends on.

The first is a **trustworthy Boondoggle Score**. The Boondoggle Score (Chapter 6) measures the ratio of effort spent on the right work to effort spent on misformulated work. The score requires a fixed point — a *what we are building* — to measure against. Without the SDD, the fixed point is in your head, and your head changes between Saturdays. Every Claude session can quietly redefine the problem, and the Boondoggle Score ends up measuring effort against a moving target, which is to say, measuring nothing useful. With the SDD, the fixed point is on disk. The score can be measured. Drift becomes visible — *this commit moved away from the SDD's Architecture Principles, here is the diff* — and correctable.

The second thing the SDD unlocks is **prompt prefix discipline**, which is the topic of Chapter 8. A Claude prompt is more trustworthy when it begins with the SDD, because the SDD constrains the model's solution space. Without the SDD as prefix, Claude is free to redesign your system in every prompt; with it, Claude has to argue against the document, which it will rarely do unprompted. The SDD is currently the cheapest way to keep Claude consistent across sessions. It is also the document you will revise *when* you decide to redesign — which is the point. Redesign is fine; redesign you didn't notice is not.

There is a third thing the SDD unlocks, and it is the one that matters in the long run. The act of writing the SDD is the act of *deciding in advance*. It is the discipline of carrying the system in your head clearly enough to put it on paper. This is the cognitive work the book's thesis is built on — the work that, if delegated to Claude, leaves you with output but no capability. The student who writes the SDD builds the capability. The student who skips it polishes output. By the end of the semester, the difference between the two students is not visible in any single commit; it is visible in the *next* project, when the first student writes the next SDD in twenty minutes and the second student stares at a blank page.

## Worked Example: The Lab Step Tracker

Here is a complete minimum viable SDD for a real student project. Seth's project, after the three-hour rebuild. The system is a lab step tracker — a tool an AP CS student uses during a programming lab to track which step they are on, what they've finished, and what's blocking them. The document below is what would have prevented the drift in this chapter's opening, and it is what Seth wrote on the second Saturday after the lost first one.

The document fits on one page. Read it as written; the prose around it is afterward.

---

### SDD: Lab Step Tracker

**Problem Statement.** A local-only task list for one AP CS student to see today's lab steps in order, mark them done, and know what's blocking the next step, without a login and without leaving the browser tab.

**Architecture Principles.**
1. *Local-only storage.* All data lives in `localStorage`. No server, no backend, no auth. If `localStorage` is unavailable, the app fails visibly with one sentence of explanation.
2. *Single user.* The app assumes one person is using one browser. No accounts, no multi-device sync, no sharing.
3. *Fail visibly.* All errors render to the page as readable text. Nothing is logged silently. If the app cannot save, the user sees that on the screen within one second.

**Core User Flows.** Open the page → add today's lab steps as a list → mark each step done as it completes → optionally tag a step as "blocked" with one line of text → close the tab. The next morning, open the page; yesterday's steps are gone (intentionally — the system shows *today's* steps, not history); add today's.

**User Needs.**
1. The list renders in under 2 seconds on a Chromebook with 4 GB of RAM, measured from page load to first interactive input.
2. No data is lost during a single day's session; closing and reopening the tab within the same day restores the list to the state it was in when the tab was closed.
3. The page is usable with one hand on a phone-sized screen (375px wide); all buttons are at least 44 pixels tall and reachable by thumb.

**Component List.** Input form (one text field, one add button); `localStorage` wrapper (read, write, clear-on-new-day); list renderer (renders steps with done/blocked states); today-filter (clears yesterday's data on first open after midnight); error banner (renders to the top of the page on any caught exception). No backend. No router. No build step beyond a single HTML file with inline CSS and JavaScript.

---

Read it as a whole. Notice what's there and what's not.

What's there: every decision in the document is *decidable*. Each Architecture Principle could collide with a tempting future feature — *sync across devices*, *user accounts*, *silent retry on save failure* — and the document tells you which one wins. Each User Need is a stopwatch-and-Chromebook test; no ambiguity. The Component List names five components, each at the right scale for a student build, and explicitly refuses three things (no backend, no router, no build step) that Claude would otherwise quietly propose.

What's not there: a schedule, a font choice, a list of stretch features, an explanation of *why* this is useful (which belongs in a pitch, not an SDD), a glossary, an appendix, a section for Seth's grade. The document is one page, dense, decisional, re-readable in ninety seconds.

Now run the Principle Collision Test on the Architecture Principles. *Local-only storage* vs. *single user* — do these ever collide? Not really; they reinforce each other. *Local-only* vs. *fail visibly* — yes, they collide: if `localStorage` is full, *fail visibly* says show the user, but *local-only* says don't fall back to a server. The collision tells the user *we can't save this; please clear some space*, which is exactly the right behavior. *Single user* vs. *fail visibly* — yes, if the same tab opens twice with overlapping writes. The collision tells the user *another tab is using this data; we won't proceed*. Each pair produces a defensible decision the document has already made. The principles are real, not slogans.

Now run the back-arrow consistency check. Can these five components, under these three principles, supporting this Core User Flow, deliver this Problem Statement? Walk it: open the page (input form + list renderer) → add today's lab steps (input form + `localStorage` wrapper) → mark each done (list renderer + `localStorage` wrapper) → tag blocked (input form + list renderer) → close the tab (browser does this for free) → next morning today-filter clears yesterday's data on open. The Problem Statement delivers. The User Needs are testable against this build. The document is consistent.

This SDD is what Seth pastes into Claude at the top of his next session. It is what Claude reads before writing the first line of code. Claude will not propose `users.email` as a column. Claude will not propose a calendar view. Claude will not propose authentication. The SDD has refused them in writing, and the model is constrained by what's in front of it. The eleven minutes Seth spent writing the SDD are, again, the most expensive eleven minutes of the build, and the most cost-saving.

## Exercises

1. **(Apply)** Write the Problem Statement section of an SDD for a project you actually want to build this semester. The sentence must pass the three `/v0` tests: a single system (no `and`), a named user (specifically, not generically), and a done-condition (testable, not aspirational). Submit the sentence and the three test answers — one line each, yes or no with one-sentence justification.

2. **(Apply)** Write three Architecture Principles for the same project. Then run the **Principle Collision Test**: for each pair of principles, construct a realistic decision in which the two would tell you to do different things, and name which principle wins. If any pair cannot collide, rewrite the weaker member until it can. Submit the three principles, the three pairwise collisions, and the three resolutions.

3. **(Analyze)** Below is the User Needs section from a draft SDD a classmate wrote. Identify which entries are feature descriptions and which are testable outcomes. Rewrite the feature descriptions as outcomes. Note: at least one of these may be unfixable — i.e., the underlying need cannot be made testable as written, and the right move is to ask the SDD's author what they actually meant.

   > **User Needs.**
   > 1. The app should have a clean, modern interface.
   > 2. Users can export their data as CSV.
   > 3. Performance should be good even on older devices.
   > 4. The app should be easy to use for first-time users.
   > 5. Data is never lost if the browser crashes mid-session.

   For each, label it *feature*, *outcome*, or *unfixable*. For each *feature*, write a one-sentence rewrite as an outcome (or, if unfixable, write the question you would ask the author to make it fixable).

## 🕰️ AI Wayback Machine

**Edsger Dijkstra** (1930–2002), Dutch computer scientist, made one claim that the SDD is the descendant of: *program correctness is a property of design, not of testing*. In "Notes on Structured Programming" (EWD249, 1970/1972) and more fully in *A Discipline of Programming* (1976), Dijkstra argued that programs should be *derived* from specifications — that the act of programming is the construction of a proof of correctness, and code is the residue, the last step. His most-quoted line — *"Program testing can be used to show the presence of bugs, but never to show their absence"* — is a structural argument: if correctness is built in at design time, testing confirms; if it is not, testing cannot fix what was decided wrongly upstream. The SDD is the student-scale form of this stance. The document is where the project's correctness as a *design property* is recorded — before any code, before any test, before Claude is asked to produce anything. The five-section minimum is not Dijkstra's proof calculus; it is the student version of his discipline: decide what the system is, in writing, before the system exists in code.

**Run this:** Paste this prompt into Claude. *"Read EWD249, section 1, from the E.W. Dijkstra Archive at UT Austin (cs.utexas.edu/~EWD/). In one sentence, restate Dijkstra's claim about why testing cannot establish correctness. Then apply that claim to the SDD in Chapter 7 of Claude Code for Students: which section of the SDD does the work that testing cannot do, and why?"*

---

The reader can build an SDD. Chapter 8 teaches them to write Claude prompts that are specifications, not requests.
