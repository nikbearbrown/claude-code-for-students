# Chapter 4 — Conducting, Not Prompting: The Core Idea

*Programming as conducting. Claude does what it's superhuman at. You do what only you can.*

---

**Learning outcomes**

1. **(Understand)** Explain the difference between prompting Claude and conducting a build with Claude.
2. **(Apply)** Identify whether a given interaction is prompting or conducting.
3. **(Understand)** Explain what a handoff condition is and why it matters.

---

I want to start by describing a room I have never been in.

It is a concert hall — pick the one in your head — and there is a conductor on a small raised box at the front and there are about ninety people behind him with instruments. The people behind him are extraordinary. They have spent more hours on their instruments than I have spent alive. They can sight-read music I cannot hum. They can play, on demand, exactly the note on exactly the beat with exactly the dynamic the score asks for. If I walked onto that stage and tried to play the cello part, I would be removed from the building before I finished tuning.

The conductor cannot play the cello part either. He is not better than the cellist at the cello. He is not better than the violinist at the violin. He is not, in any local sense, a better musician than any of the ninety people behind him. What he is doing — the entire reason there is a person on a box in the first place — is holding the *piece*. He has the score in his head as a whole, he knows what bar 47 is supposed to feel like against bar 12, he knows that the brass needs to come in *quieter than they want to come in* because the strings are doing something underneath that has to land first. The orchestra is excellent. They will play exactly what they understood him to mean. The gap between what he meant and what they understood is where everything breaks.

This is the chapter, in one paragraph. You are the conductor. Claude is the orchestra. The orchestra is excellent — better than you at the instruments, faster than you at the parts, capable of playing things you could not play in a lifetime of practice. The orchestra cannot hold the piece. You can. The gap between what you meant and what Claude understood is the entire reason this book exists.

<!-- → [DIAGRAM: The conductor/orchestra model. Human = conductor (holds intent, sequences, verifies). Claude = orchestra (executes specifications, returns output). Between them: specification + handoff condition. Editorial style.] -->

The rest of this chapter is the mechanics of conducting. What goes through the air between the conductor and the orchestra. What a specification is, and how it differs from a prompt. What a handoff condition is, and why "looks good" isn't one. Why some of the work has to happen in a sequence the orchestra cannot rearrange. And the name of the failure mode that this chapter is built to prevent — *boondoggling* — and where it comes from.

## Prompt vs Specification

The cleanest way to see the distinction is to look at the same task expressed as each.

A **prompt** is a wish. It is a sentence in natural language addressed to a language model, formulated the way you would address a friend who is good at a thing. *Write me a function that logs users in. Get me the top customers. Sort this list.* It assumes the listener will fill in the rest. It works fine when the listener is a friend — a friend knows your codebase, knows your intent, knows that "users" means the `users` table in `db.sql` and not the `User` class in `auth.py` and not the JSON file in `seed/`. The friend resolves ambiguity by knowing you.

A **specification** is a contract. It is a structured artifact that names the invariants the output must satisfy, the fields the code may touch, the format the output must take, and the boundary the code may not cross. It does not assume the listener knows you. It assumes the listener is competent and amnesiac — capable of executing anything, remembering nothing about your intent that is not on the page. A specification works because the listener does not need to know you. It knows the contract.

This distinction is not new and it is not mine. The software-engineering literature has been making it since at least the 1970s. Frederick Brooks's *The Mythical Man-Month* — a book about why adding programmers to a late software project makes it later — argued in Chapter 4 ("Aristocracy, Democracy, and System Design") that the single most important property of a system is what Brooks called **conceptual integrity**: the system must come from one mind holding the user's mental model whole. Brooks's claim, blunt and famous, is that integrity arrives from a small number of people coordinating intent — not from many people consulting each other. The chief architect writes the contract. The implementers execute against it. The contract is what makes the orchestra play one piece instead of ninety competent solos in adjacent keys (Brooks, 1975).

You are the chief architect. You will always be the chief architect — Claude does not, currently, hold the user's mental model of what you are building, because that model lives in your head and has not been written down. The specification is how it gets written down.

The recent prompt-engineering literature has, independently and without much reference to Brooks, rediscovered the same thing. White and colleagues at Vanderbilt, in a 2023 paper that has been one of the most cited in the field, cataloged sixteen reusable **prompt patterns** for working with large language models (White et al., 2023). The catalog is, structurally, a software-design-pattern book — each pattern has an intent, a context, a structural template, an example, and trade-offs. The argument the paper makes by its existence is that effective prompts are not natural-language requests. They are *specifications with documented invariants.* The same engineering grammar used for software design patterns applies to prompts because, as Liang and colleagues at Carnegie Mellon put the title of their 2024 field study, *Prompts Are Programs Too* (Liang et al., 2024).

The Liang paper is worth one more sentence because it documents the actual failure. The Carnegie Mellon team interviewed and observed working software engineers building prompt-powered systems. The dominant pattern they found was **trial-and-error prompt construction followed by silent acceptance of plausible output.** Engineers tweaked phrasing, ran the prompt, looked at the result, decided it was good enough, and shipped. The failure mode was not bad models. It was the absence of specification discipline — no documented invariants, no explicit output format, no testable success criterion, just *does this look about right.* That is the empirical core of the rest of this chapter.

Here is the operational difference, in the smallest possible example.

**Prompt:** *write me a function that sorts a list of students by GPA.*

**Specification:**

> Write a function `top_students(records, n)` that returns the top `n` records from `records`, a list of dicts with keys `gpa: float`, `last_name: str`, `first_name: str`. Sort by `gpa` descending, then by `last_name` ascending (case-insensitive), then by `first_name` ascending. Sort must be stable. The input list must not be mutated. Return a new list. Do not import sort libraries beyond the stdlib. Do not modify any other file. Handoff condition: `pytest tests/sort.py` passes all five cases including the tie case at GPA 3.92 and the unicode last-name case.

Same task. Both are addressed to the same Claude. The first is one sentence; the second is six. The second one rules out, by construction, the bug that nearly shipped in Chapter 2 — the stable-sort-via-input-order failure that came from having no explicit secondary key. The first one will produce *a* sort function. The second one will produce *the* sort function. Same model. Different artifact.

A working definition you can carry: **a prompt is what you say to Claude before you have decided what you want; a specification is what you say to Claude after.** The decision is the work. The typing is the easy part.

## Handoff Condition

The hardest single move in this chapter — the move most students skip the longest — is the handoff condition.

A **handoff condition** is a specific, testable predicate that must be true before the next step begins. It is what comes after the specification, at the bottom, sometimes in one line, often the most important line on the page. It answers exactly one question: *how will you and Claude both know that the work is done.*

"Looks good" is not a handoff condition. "It compiles" is not a handoff condition. "I read the code and it seemed right" is not a handoff condition. These are *vibes about completion*, and vibes about completion are how boondoggle ships. A handoff condition is a sentence with a verb you can run.

The handoff condition for the sort spec above is: `pytest tests/sort.py` passes all five cases including the tie case at GPA 3.92 and the unicode last-name case. That sentence has a verb (`pytest`), a target (`tests/sort.py`), a success criterion (all five cases pass), and two specific edge cases named so that "all pass" can't quietly mean "all pass except those." You can run that sentence. Claude can run that sentence. If the sentence runs and returns zero, the handoff is real. If it doesn't, you haven't completed the step, no matter how good the code looks.

The reason this matters more than any other piece of mechanics in the chapter is that the handoff condition is what makes the conductor/orchestra metaphor work at all. The conductor in the real orchestra has the same problem — he cannot play all the instruments and check them, he has to know, *as the bar passes*, whether the horns landed where he meant them to. He has a handoff condition. It is the sound in the hall against the sound in his head. It is fast, specific, and falsifiable in real time. If the trombones are flat he stops the rehearsal. He does not, ever, decide that the trombones are flat by *looking at the page they were given* and judging that the page seems right. He listens to the room.

You will listen to the room with a test runner, a type checker, a linter, a one-line script that hits a real endpoint, a manually-written fixture file, a property test, a snapshot diff, or — at minimum — a small reproducible script that runs the new code on real input and prints what it did. None of those is "I read the code." All of those are "the room confirms what I asked for."

A small taxonomy of handoff conditions you will use in this book:

**Test-based.** `pytest tests/foo.py` returns zero. The named test file exists. Specific test cases are named so the condition cannot be satisfied by an empty test file.

**Type-based.** `mypy --strict src/foo.py` returns zero. The type contract is on the page and the type checker enforces it.

**Behavior-based.** Running `python -m foo --input fixtures/bad.csv` produces exactly four lines in `errors.log` and zero unhandled exceptions.

**Boundary-based.** The diff touches only the named files. `git diff --stat` shows changes to `src/auth.py` and `tests/auth.py` only — nothing in `schema.sql`, nothing in `seed/`.

**Performance-based.** `EXPLAIN` on the new query shows index use on `orders.created_at`. The query returns in under 50ms on the seed DB.

Each of these has the same shape: a verb, a target, a success criterion, a falsifiable edge. A handoff condition is the smallest possible unit of verification that you can write *before Claude runs* and check *after Claude runs.* The before/after is what makes it real. If you write the handoff condition after seeing the output, you are not verifying. You are rationalizing.

There is, in the cognitive-science literature, a precise word for what the handoff condition is doing. Edwin Hutchins, in *Cognition in the Wild* (1995), studied the bridge team of a US Navy ship as it navigated through fog and pier traffic. The team did not solve the navigation problem in any one person's head. They solved it by passing structured, verified artifacts between people and tools — a position fix from the bearing-taker to the plotter to the navigator, each handoff with an explicit format and an explicit check. Hutchins's word for what makes this work is **coordination**: distributed cognition only succeeds when the artifacts passed between agents are *specific, structured, and verifiable at the boundary* (Hutchins, 1995). The handoff condition is the chapter's name for one of Hutchins's coordination primitives. It is the predicate at the boundary. Without it, you are not coordinating with Claude. You are gambling with Claude.

## Dependency Order

Some Claude tasks cannot run until a human task is complete. This is not a productivity tip. It is a structural property of the work.

The orchestra cannot start playing a piece that has not been chosen. The conductor cannot choose the piece during the downbeat. There is an order. Some things have to happen in human time, in human silence, before any orchestral time is allowed to begin.

In a Claude Code build, the human tasks that have to run first are these. They are not exhaustive but they are typical:

**Decide what you are actually building.** Not a feature list. A one-paragraph description of the artifact, the user, and the success condition. If you cannot write that paragraph, Claude cannot help you write the code, because the code will be coherent against a paragraph you have not written and that paragraph will be Claude's, not yours. (This is a direct application of Brooks's conceptual integrity: the integrity of the artifact lives in the description of the artifact, and the description is yours.)

**Read what is already there.** If there is a codebase, you read it before Claude touches it. Not all of it. Enough of it that you know the shape, the conventions, the names, the dependencies. If you skip this step, Claude will assume conventions and you will not know whether the assumptions are right.

**Name the boundary.** What files are in scope. What files are explicitly out of scope. What invariants must hold across the change. What database tables and schemas are frozen for this change. The boundary is what makes the work bounded; without it, Claude has the entire codebase as ambient scope, and the entire codebase is too much scope for any single task.

**Write the handoff condition.** Before any code is written. The handoff condition is what makes "done" mean something. If you write it after the fact you have written a justification, not a criterion.

Only after these four are done does Claude get to play. That is the dependency order. Claude can read for you, Claude can summarize for you, Claude can sometimes help you write the description by asking questions — but the decisions in those four bullets are the *Gru Part* (we will name this in a moment) and they precede the Claude work because Claude has no basis to do them on your behalf. The artifact you are building lives in your head. It has to come out of your head and onto the page before Claude can execute against it.

This is why, in the rest of the book, you will see a phrase repeated almost annoyingly: *explore → plan → implement → commit.* That phrase is the dependency order, named. Exploring and planning are the human-task lane. Implementing and committing are where Claude joins the work. Mixing the lanes is the most common source of failure in student builds, because it feels productive to be typing into Claude and it does not feel productive to be reading a codebase quietly. The feeling is wrong. Reading the codebase is the work. Typing into Claude before reading the codebase is the boondoggle.

## The Boondoggling Name

There is a word for the failure mode this chapter is built to prevent. The word is **boondoggling**, and the framework is named after it, and it is worth knowing where the word comes from because the etymology is the lesson.

The word *boondoggle* entered American English in the 1920s out of the Boy Scout movement — a *boondoggle* was a braided leather cord that scouts made at camp. It was a craft. It was, by the standards of the camp, real work — you made the thing with your hands and you had a thing at the end. By the 1930s the word had been borrowed into political use, where it picked up a very specific extra meaning: a *boondoggle* was now a project that *looked* productive — that had hours logged against it, deliverables, photographs, official reports — and that *accomplished nothing real.* The shape of work, the surface of work, with nothing underneath.

The contemporary application is exact. A boondoggled Claude session is one where the surface is right — Claude responded, code appeared, the terminal scrolled, you spent two hours, you have an artifact — and the underneath is wrong. The code does not do what you needed. Or it does what you asked but you asked the wrong thing. Or it works on your sample and silently fails on the real data. Or it works completely and you do not know how, and the next change will break it because you cannot reason about it. The boondoggle is fluent activity with no verified output.

The framework introduced in this book — the conducting framework, the supervisory-capacity framework, the explore/plan/implement/commit workflow — has a working name on the author's site at [boondoggling.ai](https://boondoggling.ai). Use the name. It is short, it is specific, and it gives you a thing to point at when you catch yourself doing it. (Note for honesty: boondoggling.ai is the author's own framework, not a peer-reviewed source. Cite it as the definitional artifact, not as evidence. The empirical weight in the chapter comes from White, Liang, Hutchins, Brooks, and Simon — not from the framework's site.)

The reason naming it matters is the same reason naming "code smell" mattered when Kent Beck first used the phrase, and the same reason naming "tech debt" mattered when Ward Cunningham coined it. Programmers had felt these things for years. They could not point at them. The name made them visible, and visible things can be fixed. *Boondoggling* is a name for a thing every Claude user has felt — the strange exhausted feeling at the end of a session that produced a lot of output and no progress — and could not articulate. The name is the first move in the discipline. After the name, you can see the thing. Before the name, you were inside it.

## The Minion Part and the Gru Part

The work has two lanes, sequenced separately. This book has a name for them, and the name comes from a movie that everyone reading this book has seen.

The **Minion Part** is what Claude is for. It is the typing. It is the boilerplate. It is the syntactic resolution, the idiomatic translation, the pattern-completion, the running of the tests, the searching of the codebase, the drafting of the function once the spec is on the page. The Minion Part is high-volume, fast, error-prone in *specific* ways (hallucinated imports, confident wrong APIs, plausibly named functions that don't exist) and superhuman in *most* ways (Claude has read more Python than you ever will). The Minion Part is what Claude does. It is named after the Minions in the *Despicable Me* movies because the analogy is exact: Minions are competent, fast, plural, eager, and exactly as good as their instructions. Without instructions they wreck things at speed. With instructions they execute things faster than the boss could alone.

The **Gru Part** is what only you can do. It is the deciding. It is the holding of intent across the build. It is the writing of the specification. It is the writing of the handoff condition. It is the reading of the result against the world. It is the noticing that the test set was missing a tie case. It is the choice that one option is more interesting than another. It is the moral and aesthetic weight that the work has to carry, which Claude has no access to because Claude has no stake in the outcome. The Gru Part is what makes the work *yours.* It is the part that, if you delegate it, you have not built the thing — you have approximately watched the thing get built.

Chapter 6 will name this distinction more formally and tie it to a specific operating mode — the **Gru system** — that the book uses for the rest of Act Two. For now, the working version is this: the work has two lanes. The Minion lane and the Gru lane. They run separately. The Gru lane has to be ahead of the Minion lane at every step. If the Minions get ahead — if Claude is doing more deciding than you are — you have stopped conducting. You are now hoping.

A practical sort, before any code is written on any project:

| Gru Part (you) | Minion Part (Claude) |
|---|---|
| Decide what is being built | Generate the boilerplate |
| Read the existing code | Search for occurrences of a pattern |
| Name the boundary (files in/out of scope) | Edit the named files |
| Write the specification | Write the implementation against the spec |
| Write the handoff condition | Run the handoff condition |
| Verify against the world | Report the result |
| Decide if the result is acceptable | Apply the fix once you decide |
| Hold the piece across sessions | Hold no piece across any session |

Look at that table for a moment. Notice which lane has *deciding*. Notice which lane has *typing*. Notice that the lanes are not equal in cognitive load — the Gru lane is lighter in keystrokes and heavier in judgment. That is the trade. The conductor is on a small box, not running between desks with sheet music. Your wrists will be less tired than your head, and that is the correct distribution.

## Explore → Plan → Implement → Commit

The workflow has four stages. They run in order. The order is not negotiable, and the third stage — Implement — is the only one in which Claude gets to write code.

**Explore.** You and Claude both read. You read the assignment, the existing code, the relevant docs. Claude, on your instruction, reads the codebase, summarizes structure, identifies the files relevant to the change. *Nothing is written yet.* The point is to update your mental model of what is already there. The most common failure of a Claude Code session is that the student skips this stage. Claude then writes code against an assumed convention, and the assumption is the bug.

**Plan.** Before any code is written, you and Claude produce a *plan*: a short written artifact that says what will change, in which files, in what order, with what handoff condition. Modern Claude Code has a UI affordance for this — **plan mode** — that institutionalizes the gate: it forbids Claude from writing or editing until you approve the plan. Plan mode is, structurally, an admission by the tool itself that Explore → Plan has to precede Implement. The tool agrees with this chapter. Use the mode.

**Implement.** Claude writes the code, against the plan, against the specification, inside the named boundary. This is the Minion lane. It is fast. It produces an artifact. The artifact is then checked against the handoff condition.

**Commit.** Once the handoff condition is satisfied — actually satisfied, run the verb, see the success criterion, check the edges — the work is committed to version control with a message describing what changed and why. The commit is the punctuation. It is what makes the work *count* as a step, distinguishable from the next step. Without commits the build becomes one long blur in which it is impossible to know what worked and what did not.

The order is Explore → Plan → Implement → Commit. The first two are mostly Gru. The third is mostly Minion. The fourth is Gru again, because deciding what counts as a unit of work is a decision and decisions are yours. Reversing the order — Implement before Plan, Plan before Explore — is the central error of self-taught Claude use. It feels faster. It is slower, because every error introduced in the wrong stage compounds in the next.

There is a fifth, optional stage worth naming for honesty: **Reflect.** After commit, you write down, in a sentence or two, what you learned about the codebase, the tool, or yourself, that you did not know before the session. Reflect is what makes the session *cumulative* — it is the part that builds capability in you, not just in the repository. We will return to it in Chapter 13. For now: at minimum, do not skip Commit, and do not run them out of order.

## Worked Example: One Build Step, Done Twice

Seth is back to walk through one build step done twice. Same Claude. Same model. Same minute. Two completely different artifacts.

The step: I am building a small grade-tracking tool for myself. (It is the one from Chapter 2.) Tonight I am adding a login function so I can demo the tool to a friend without sharing my data. I have not done auth before. I am domain-shallow on authentication, which is exactly the kind of domain where the chapter's warnings apply hardest — fluent-sounding wrong output, no internal basis to evaluate it.

I am going to ask Claude twice. Once badly. Once well.

**Attempt 1: the prompt.**

I open Claude Code. I type:

> write me a login function

Claude responds in about six seconds. It writes a function called `login(username, password)`. It hashes the password with `md5`. It compares against a global dict called `USERS`. It returns `True` on match, `False` otherwise. It does not invalidate sessions because there are no sessions. It does not handle empty input — `login("", "")` returns `False` only if `""` is not a key in `USERS`, which is a reassuring coincidence and not a guarantee. The code is twelve lines, clean, syntactically perfect, and approximately the worst possible login function a real codebase has ever shipped.

I am a high school senior who has not done auth before. I read the function. It is short. It uses `hashlib`, which I have heard of. It returns the right type. I run it on a test username, it returns `True`. I am about to commit it.

If I do, I ship a function that:

- hashes with **MD5**, an algorithm that has been considered cryptographically broken for password storage for over a decade,
- stores users in a global dict that does not persist, so every restart wipes my user base silently,
- accepts empty strings as a username because there is no input validation,
- has no rate-limiting, so a brute-force attempt against the dict is bounded only by the speed of MD5, which is fast,
- has no session token, so "logged in" is a transient property of one function call, which makes the function useless for the demo I intended.

Every one of those failures is invisible to me from inside the code. I do not, as a sixteen-year-old, know that bcrypt or argon2 are the current acceptable hashes. I do not know that `hashlib.md5` *exists in Python's standard library and works*, which makes its presence in the code feel correct. The output passes the only audit I am currently equipped to perform, which is *does this read like a login function.* It reads like one. It is not one. It is a boondoggle wearing a login function's clothes.

This is the failure the chapter is built to prevent. Same Claude. Cooperative, fast, syntactically immaculate. Wrong in every way that matters, in a domain where I cannot see the wrongness from inside the prompt.

**Attempt 2: the specification.**

I close the first attempt. I do not commit it. I open a new conversation. I take ten minutes to do the Gru part first.

What am I actually building? A login function for a single-developer tool that authenticates against a SQLite database. Two users at most. Demo use only. Not production. But — and this matters — I want the function to be correct under the assumption that it might be reused, because Claude will use my code as context for the next thing, and if this code is bad I will be inheriting the badness.

I read the existing code. There are three files: `app.py`, `db.py`, `schema.sql`. The `schema.sql` defines a `users` table with columns `username TEXT UNIQUE`, `password_hash TEXT`, `last_login TEXT`. I notice — this is the kind of thing I would miss without reading the codebase — that there is *already* a hash column named `password_hash`, which means past-me had a plan about how passwords would be stored and I should respect it. I also notice that `db.py` already has a parameterized query helper and that I should use it, not write a new one.

I name the boundary: the change touches `app.py` (to add the function) and `tests/auth.py` (to add the tests). It does not touch `schema.sql` (the schema is frozen for this change) and does not touch `db.py` (the helpers exist; I use them).

I write the handoff condition before writing the spec. The condition: `pytest tests/auth.py` passes all five named cases — valid credentials, wrong password, nonexistent user, empty-string username, and a SQL-injection-style payload in the username field. The condition is concrete enough that I can run it and Claude can run it and we will both know what "done" means.

Now I write the specification.

> In `app.py`, add a function `login(username: str, password: str) -> dict`. The function authenticates against the `users` table in the existing SQLite DB using the helpers in `db.py`. Password verification uses `bcrypt.checkpw` against the `password_hash` column. Empty or null username/password returns `{"ok": False, "error": "empty_credentials"}` without touching the DB. Nonexistent or wrong-password returns `{"ok": False, "error": "invalid_credentials"}` — the two cases return the same error string to avoid leaking which one failed. On success, return `{"ok": True, "token": <hex>, "exp": <iso8601 timestamp 24h from now>}` where the token is `secrets.token_hex(32)` and the timestamp is generated with `datetime.now(timezone.utc) + timedelta(hours=24)`. Do not modify `schema.sql` or `db.py`. Do not add new dependencies beyond `bcrypt`. All SQL uses the parameterized helper in `db.py`. Handoff condition: `pytest tests/auth.py` passes all five named cases — valid, wrong-password, nonexistent, empty-string, and the injection payload `' OR 1=1; --` in the username field. Tests should also confirm that the two failure cases produce identical error strings.

That is eight sentences. It took me maybe four minutes to write, once I had done the reading. I send it to Claude.

Claude responds in about twelve seconds. The function it produces is roughly thirty lines. It imports `bcrypt`, `secrets`, `datetime`, `timezone`, `timedelta`. It uses the helper in `db.py`. It handles all five cases. It uses identical error strings for the two failure branches. Crucially, it asks me one clarifying question before generating — *should the token be stored anywhere, or just returned to the caller?* — which I would not have thought to ask in the first attempt because the first attempt had no concept of token state.

I run the tests. Four pass. One fails — the injection test passes the wrong value through because I had written the test slightly wrong. I fix the test. Five pass. The handoff condition is now true, in the verb-and-result sense. I commit.

Notice the second attempt did not produce *magic* code. It produced *correct* code, which is harder than magic. The difference is not that Claude got smarter between the two attempts. Same Claude, same minute, same model. The difference is that I — the conductor — held the piece, named the boundary, wrote the contract, and supplied the handoff condition. Claude played the parts.

Notice also that the second attempt took longer. Maybe twenty minutes start to finish, against six seconds for the first attempt. The first attempt was faster and produced a boondoggle. The second attempt was slower and produced a thing I would put my name on. That ratio — slow at the start, real at the end, versus fast at the start, hollow at the end — is the trade this entire book is built to make visible.

The lesson, in one sentence: *the prompt is what you wish for; the specification is what you contract for; the handoff condition is how you know the contract was kept.*

The limit, in one sentence: *this works for builds where you can write down what success looks like; for exploration — figuring out what you think — the prompt-as-conversation frame is fine, and trying to specify too early is its own kind of failure.* The chapter is calibrated for *building things that have to work*, not for *figuring out what you think.* Tell the difference and pick the right frame.

## Exercises

1. **(Apply)** Take a prompt you have used in the past week — a real one, from your own chat history, in any tool. Rewrite it as a specification using the format introduced here: invariants, field names or other concrete identifiers, output format, what not to touch, and a handoff condition that names a specific verifiable predicate. Run both the original prompt and the new specification in the same model. Save both outputs. Write three sentences comparing them.

2. **(Apply)** Pick a Claude task in a current project you have not yet started or not yet completed. Write a handoff condition for it. The condition must include a verb you can run, a target file or fixture, a success criterion, and at least one named edge case that the success criterion explicitly covers. The condition must fit on one line of plain English.

3. **(Analyze)** Given the Claude transcript provided by your instructor (or, if self-studying, one from your own recent history that ended badly), identify the point at which the handoff condition was missing. Name the specific verb that should have been runnable at that point and was not. Name what broke as a result. One paragraph; specific examples only.

**Links:** [boondoggling.ai](https://boondoggling.ai)

## 🕰️ AI Wayback Machine

**Herbert Simon (1916–2001).** American polymath; Nobel laureate in economics (1978), Turing Award (1975). Simon's most consequential idea is **bounded rationality**: the human decision-maker does not optimize across all possibilities because optimization across all possibilities exceeds cognitive capacity. Instead, the human *satisfices* — chooses a path that is *good enough* against criteria the human can hold in mind at once (Simon, 1957, *Models of Man*; see also Simon, *Administrative Behavior*, 1947). Simon's claim was not that humans are stupid. His claim was that humans are *finite*, and the way finite agents do important work is by designing *systems that extend the bound* — checklists, organizations, division of labor, written procedures, tools. The bound does not go away; the system absorbs the part of the work the human cannot hold.

The conducting framework is Simon applied to a new instrument. You are bounded. You cannot hold an entire build in your head, evaluate every line of generated code, and remember every assumption across a four-hour session. Claude is bounded differently — limitless on the typing, blind on the deciding. The specification + handoff condition is the system that absorbs the part of the work neither of you can hold alone. Boondoggling is what happens when the system is absent and the two bounded agents pretend they aren't. Simon would have called this *the failure to design for the bound* — and he would have been right.

**Run this:** *Apply Simon's bounded rationality to a 16-year-old using Claude Code: where is the bound, what does satisficing look like in practice, and how does a specification + handoff condition encode that compromise?*

---

The reader has the metaphor and the basic mechanics. Chapter 5 names the five things the human must never delegate.
