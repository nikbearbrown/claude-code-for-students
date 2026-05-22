# Chapter 2 — What You're Actually Good At (And What Claude Is Better At)

*Pattern recognition is Claude's domain. Supervisory intelligence is yours. Knowing which is which is the whole game.*

---

**Learning outcomes**

1. **(Understand)** Distinguish pattern recognition — where Claude is, by any honest measure, superhuman — from supervisory intelligence, where Claude is currently and structurally weak.
2. **(Apply)** Classify a set of real build tasks as Claude work, human work, or the dangerous middle that looks like one and is actually the other.
3. **(Analyze)** Identify the specific supervisory capacity being exercised — or being skipped — at a given step in a build.

---

The function compiled. The tests passed. I almost shipped it.

This was a Saturday afternoon, the project was a small grade-tracking tool I was building for myself, and the function in question was something I had asked Claude to write in about eleven seconds: *take a list of student records, return the top three by GPA, break ties alphabetically by last name.* I have written sorting code before. I could have written this one. I asked Claude because asking Claude is faster than typing, and the function was supposed to be a five-minute step inside a larger thing I actually cared about.

Claude wrote it. It was clean. It used a sort with a tuple key — sort by GPA descending, then by last name ascending — exactly the move I would have used if I'd done it myself. I ran my tests. The three students with the highest GPAs in my test data came back in the right order. I was about to move on to the next step.

I'm not sure why I looked at the test data one more time. I think I noticed that all my test students had unique GPAs and felt a small itch about it. I added one row — two students tied at 3.92, last names *Bell* and *Adams*. I ran it again.

The function returned Bell first, then Adams.

That's not what the assignment said. The assignment said *break ties alphabetically by last name*, which means Adams before Bell. Claude's code didn't sort the tied pair the wrong way on purpose. It sorted it correctly *by the wrong key*. Looking back at the function, I saw what had happened: the tuple key Claude wrote was `(-gpa, last_name)`, which is correct for ascending alphabetical, *except* that Python's sort is stable and the input list was already in some other order, and on my test set with unique GPAs the stability had been doing the work I assumed the sort was doing. With the tie introduced, the stability broke the wrong way. I forget the exact diagnosis I wrote in my notebook; what I remember is the feeling.

The feeling was that the code had been correct *for my test set* and silently wrong *for the real spec*, and the gap between those two things was invisible until I happened to add one more row.

I almost didn't add the row.

That near-miss is this chapter. Not because the bug was hard — it was a sorting bug, the most embarrassing kind. The chapter is the gap between *the code compiled and passed my tests* and *the code did what I actually needed.* Claude could not see that gap. The same statistical machinery that wrote the function was the only machinery available to audit it, and the audit said *looks good.* The gap was in the part Claude does not have access to: the room I was sitting in, the assignment I had written down, the actual definition of "tie-break" that lived in my head and nowhere in the prompt.

This chapter is about that division. Where Claude is better than you. Where you are better than Claude. And — most dangerous — the places that look like the first kind and are actually the second.

<!-- → [TABLE: Division of labor — two columns: Claude does / Human does. Rows: pattern completion, code generation, syntax resolution, test execution (Claude) vs. plausibility auditing, problem formulation, interpretive judgment, tool orchestration, executive integration (Human). No color.] -->

## Pattern Recognition: Where Claude Is Superhuman

I want to start by being honest about what Claude is great at, because the rest of the chapter is going to be about what it isn't, and if I lead with the limitations I'll sound like the kind of adult who tells you the internet is a fad.

Claude is better than you at pattern completion. That sentence is uncomfortable to type as a high school senior, but it is true in a measurable way. Claude has read more code than I will read in my life. It has seen the standard binary search written ten thousand times, the standard merge sort written ten thousand times, the standard React component pattern written ten thousand times. When you ask it to produce one of those, it produces a version that is on average better than the version you would produce on your first try, in a fraction of the time. Brynjolfsson, Li, and Raymond's 2023/2025 study of about five thousand customer-support agents at a Fortune 500 company found that AI assistance raised average productivity by 14%, with the biggest gains — about 34% — going to novices on routine tasks (Brynjolfsson, Li, & Raymond, 2025). The finding has replicated in coding contexts: AI lifts the floor faster than it lifts the ceiling. Pattern work is the floor.

Here is a partial list of what counts as pattern work, in the sense the chapter means.

Boilerplate. The seventeen lines at the top of every Flask app, the React component skeleton, the standard Python `argparse` setup. Claude writes these correctly while you are still deciding what to name the file.

Idiomatic translation. You wrote a thing in Python. You want it in TypeScript. The shape of the answer is determined; Claude does the typing.

Syntax resolution. You forget whether it's `==` or `===`, whether the comma goes inside or outside the bracket, what the exact name of the `defaultdict` import path is. Claude does not forget. Claude has never forgotten.

Standard-library recall. The Python standard library has, depending on how you count, around two hundred modules. You know maybe twenty of them well. Claude knows all of them at the level of "here is the function you want."

First-draft architecture. Asked to sketch a file structure for a small web app — `models/`, `routes/`, `static/`, a `requirements.txt` — Claude produces a reasonable scaffold in seconds, and the scaffold is on average better than the one a first-year student would produce by hand. Not because the student is dumb. Because the student has built three web apps and Claude has read a million.

Test execution and output inspection. Run my test suite, summarize the failures, group them. This is reading work, which is to say pattern work over text, and Claude is faster than you at reading.

Daniel Kahneman, in *Thinking, Fast and Slow* (2011), drew a distinction between two modes of cognition. **System 1** is fast, associative, pattern-completing — the part of your brain that recognizes your friend's face across a crowded room before you consciously look. **System 2** is slow, effortful, rule-checking — the part that does long division. Kahneman's central finding, drilled in over hundreds of pages, is that System 1 *cannot help itself*. It completes the pattern whether or not the pattern is correct. It is confident even when it is wrong, because confidence is what System 1 is *for* — System 1 doesn't know how to be uncertain; uncertainty is System 2's job (Kahneman, 2011).

Claude is not literally System 1; Claude is a transformer running on a server somewhere. But Claude's outputs *arrive in your mind* as System 1 candidates. They are fluent, plausible, completed-feeling. They land before you can audit them. Your only defense is to route them, deliberately, into System 2 before you act on them. The rest of this chapter is about how.

## Supervisory Intelligence: Where Claude Is Structurally Weak

There is a class of work Claude is, currently and structurally, weak at. The word "currently" is doing real work in that sentence; I'll come back to it. The class of work has a name in the broader literature — *supervisory intelligence* — and it breaks down, for this book's purposes, into five capacities. I want to introduce them by name here so you can spot them in the wild. The full formal definitions are Chapter 5's job; this chapter is the gesture.

The first capacity I'll call **plausibility auditing.** When the code comes back, the question is not *does it run* but *does the output match the world I'm building it for.* My GPA-sorting bug was a plausibility-auditing miss. The code ran. The output looked right. The mismatch was between Claude's output and the spec that lived in my head. Claude could not see the spec, and so the audit Claude performed — implicit, internal, the small *yes this seems right* before it printed the function — had nothing to check against. I had the spec. The audit was mine to do and I almost didn't.

The second I'll call **problem formulation.** Before you ask Claude to solve, somebody has to decide what the problem actually is. This sounds trivial until you watch a friend paste a graph problem with negative edge weights into Claude and get back a textbook Dijkstra implementation. Dijkstra is the dominant pattern in the training distribution; Dijkstra is also wrong for negative weights. The code Claude returned was the code for the wrong problem. The formulation step — *this graph has negative edges, so I cannot use Dijkstra* — sat upstream of anything Claude could do, and if I don't do it Claude will not flag it. It will produce confident output for whatever question I asked, which is not necessarily the question I needed to ask.

The third I'll call **interpretive judgment.** Claude returns a function. The function returns `0.7142857142857143`. Does that mean what I need it to mean? Is it a probability? An accuracy? A ratio? A bug — should it have been an integer? Interpretation is the work of mapping an output back to the domain it came from. Claude can describe the number; Claude cannot tell you whether the number is *right for what you're doing*, because what you're doing is the part that lives outside Claude's window.

The fourth I'll call **tool orchestration.** A modern build is rarely one prompt to one model. It is one prompt to Claude Code, which runs a script, which writes a file, which gets committed to git, which triggers a test, which produces a log, which goes back to Claude. The question of *which tool, in which order, on which input* is the orchestration question, and the answer is currently yours. Claude can run a tool if you ask it to. Claude does not, reliably, decide which tool was the right one — and even when it does, it's choosing inside the menu *you* set up. If your menu is wrong, Claude's choice is wrong.

The fifth I'll call **executive integration.** This is the one that's hardest to see, because it doesn't sit at any one step; it sits across all of them. You are building a thing. At step seven you are debugging a subroutine. The question *does this subroutine still fit the design we sketched at step one* is not a question step seven contains. Somebody has to hold the design in their head across the whole build. That somebody is you. Claude has a context window; you have a project. The context window forgets the design when it scrolls off; you, in principle, do not.

Five capacities. Plausibility auditing, problem formulation, interpretive judgment, tool orchestration, executive integration. The chapter will not, again, define them formally — that's Chapter 5. What it will do is argue that they are the work currently sitting on your side of the table, and that the table has two sides for reasons of architecture, not preference.

## Why AI Is Structurally Weak at Supervision

Here is the part I want to be careful about, because it is the part that, if I overstate it, becomes the kind of confident wrong claim this whole book is supposed to teach you to spot.

Claude is structurally weak at supervising its own output because, in the most literal sense, the same weights that produced the output are the only weights available to do the audit. When Claude reads its own function and says *looks good*, that "looks good" is computed by the very statistical machine that just produced the function. The audit is not an independent check; it is the same process running again with slightly different framing. If the production step was confidently wrong, the audit step will, on average, be confidently wrong in the same direction.

This is the empirical signature you see across the literature. Pearce, Ahmad, Tan, Dolan-Gavitt, and Karri (2022), in the paper widely cited as "Asleep at the Keyboard," generated 1,689 programs with GitHub Copilot across scenarios drawn from the MITRE Top 25 Common Weakness Enumeration list — the list of the most dangerous software flaws. Roughly 40% of the generated programs contained the very vulnerability the scenario was designed to elicit (Pearce et al., 2022). The code compiled. The code passed basic tests. The code was exploitable. Copilot did not flag it as exploitable because Copilot wrote it, and "this is exploitable" is not a pattern that overrode "this completes the user's request" in Copilot's weights.

Liu et al.'s 2024 hallucination taxonomy, published in *ACM Transactions on Software Engineering and Methodology*, sampled thousands of code-generation outputs and identified two failure categories that turn out to be the same architectural fact wearing different costumes: *task-requirement-conflicting* outputs (the function does something close to but not exactly what was asked) and *knowledge-conflicting* outputs (the function references a library, a function signature, or a behavior that does not exist) (Liu et al., 2024). In both cases, the model cannot detect the conflict because detecting the conflict would require checking against something the model does not have — your actual requirement, or the actual API of the actual library.

The package-hallucination version of this is particularly clean. A meaningful fraction of imported packages in code Claude writes — different studies put it at roughly one in five for some scenarios — do not exist on PyPI or npm. Claude is not lying. Claude has correctly identified that a package *of approximately that shape and name* would solve the problem, and produced the import statement that *would* import it. The package is hallucinated in the precise sense that the audit step — *does this package exist* — cannot be performed without leaving Claude's window, and Claude has no reliable mechanism for leaving its window unprompted.

The careful version of the claim, then, is this. *Currently*, an LLM whose architecture is statistical completion cannot reliably audit its own output, because audit and production are the same operation. This is not a metaphysical claim about machines forever. It is an empirical claim about Claude in 2026 and the family of models Claude is part of. Retrieval augmentation might shrink some of the gap. Tool use might shrink more. The contested question — and you should know it's contested — is whether the five supervisory capacities are all reducible to better tooling, or whether some of them (problem formulation and executive integration look the most stubborn) are structurally outside what a statistical completer can do. Honest answer: nobody knows. The chapter is teaching you the division of labor as it stands today, which is the division of labor that determines whether your function ships with the GPA bug in it.

## The Solve-Verify Asymmetry

There is a corollary to all of this that I want to draw out because it sets the shape of the whole student/Claude relationship, and once you see it you will not stop seeing it.

Claude's *solve speed* — how quickly it produces a candidate solution — is rising fast. It has been rising fast for three years and there is no public evidence that the rise is slowing. Every model generation gets faster, gets more accurate on standard benchmarks, handles more context. Your verification speed — how quickly you can check whether the candidate solution is correct against the domain you're working in — is approximately constant. You can train it; my AP CS class has measurably improved my verification on Python in a year. But it does not improve at the rate Claude's solve speed improves, and it cannot, because verification is bottlenecked on a human cognitive process Claude is not subject to.

<!-- → [DIAGRAM: The solve-verify asymmetry — simple timeline. Claude's solve speed increasing over time. Human verification capacity stable. The gap widens. The human's job is not to solve faster but to verify better.] -->

The diagram says it more cleanly than I can. Two lines. One rising. One roughly flat. The gap between them is the dangerous middle this chapter is leading up to, and it is widening.

The temptation, when you see those two lines, is to try to catch up — to use Claude to verify Claude, to delegate the audit too. The corollary of the architectural argument I just made is that this does not work. Audit by Claude has the same architectural limitation as production by Claude. You can run a second pass; you can ask a second model; you cannot, with current tools, get an independent check by handing the check to the same kind of system that did the work. The verification has to land on a process that has access to *the world Claude doesn't see* — your spec, your test data, your assignment rubric, your user, your actual graph with its actual edge weights. That process is, currently, you.

So here is the version of the lesson that's worth memorizing: **your job is not to solve faster. Your job is to verify better.** Claude has the solve side. You have the verify side. If you try to compete on the solve side, you lose, and you should — the same way you should lose at long division to a calculator. If you abandon the verify side, the build ships with whatever Claude got wrong, and you will not know until something in the world tells you.

## The Dangerous Middle

I have been gesturing at the dangerous middle for the whole chapter; here is where I'll name it.

There is a third category of task that is the actual point of this chapter, and it is the category most students miss. *Claude work* — pure pattern completion — is honest and safe; you ask, it answers, you move on, the stakes are low. *Human work* — explicit supervisory judgment — you'll usually catch yourself doing, because it feels like work. *The dangerous middle* is the category of tasks that **look** like Claude work and **are** actually human work in disguise.

The sorting function I opened the chapter with was a dangerous-middle task. It looked like boilerplate. It looked like the kind of thing you delegate. It was, on inspection, a task that required a plausibility audit against a spec that lived only in my head.

The SQL-injection failures in the Pearce paper are dangerous-middle tasks. They look like *build me a string and run it as a query.* That is a pattern Claude can complete in its sleep. The correct completion requires that somebody — the human, currently — know that prepared statements exist, that the codebase already uses them, and that string-concatenated SQL is the canonical example of *the pattern Claude completes confidently and you must override.* About 40% of Copilot's completions in those scenarios were vulnerable (Pearce et al., 2022). Not because Copilot is bad. Because the surface task is pattern work and the actual task is judgment.

The off-by-one bug in pagination is a dangerous-middle task. Claude will happily write `for (int i = 0; i <= n; i++) page[i] = ...`. The surrounding code suggested inclusive bounds; Claude completed the inclusive-bounds pattern; the bug is famous and Claude wrote it anyway. The supervisory move is interpretive — reading the loop in the context of the rest of the function, not as a local snippet.

The Dijkstra-on-negative-weights bug is a dangerous-middle task. The student pasted a graph problem; Claude returned the dominant pattern for graph problems. The supervisory move is problem formulation — recognizing the actual structure of the problem before delegating it.

The hallucinated library on a science-fair project is a dangerous-middle task. Claude imported `from quickstats import bootstrap`; no such package exists on PyPI; the code failed on the lab machine. The supervisory move is executive integration — keeping the ground-truth environment in mind across the build, including the boring fact that the lab machine has a fixed Python install.

Notice the shape. In each case the task *presents* as pattern work — write a sort, build a query, write a loop, pick an algorithm, import a library. Each task, on inspection, requires a supervisory capacity that Claude cannot supply because the input Claude needs to supply it lives outside Claude's window. The dangerous middle is not a fixed list of bug categories; it is the entire region where surface-pattern fluency overrides judgment requirement. It is most of the bugs you will ever ship.

The countermove is easier to name than to do. Before you accept a Claude output, *name the supervisory capacity this step needs.* Out loud, in your head, in a comment, anywhere. If you can name it, you'll do it. If you can't name it, you're in the middle and you don't see the middle.

Bondi-Kelly and colleagues (2025), in a 2,784-participant study published in *Harvard Data Science Review*, found that the dominant predictor of whether a person accepted a wrong AI suggestion was not domain knowledge and not task difficulty. It was *attitude toward the tool*. People favorable to automation accepted incorrect suggestions at significantly higher rates than skeptics did. The result is uncomfortable. It means that *how you feel about Claude going into a task* changes whether you'll catch its mistakes. The chapter cannot make you a skeptic. It can ask you to be one in this specific way: when the task feels like pattern work, raise the suspicion that it's middle work in disguise.

## A Build Step, Twice

I want to do the worked example here because the labor split is much easier to see in a transcript than in the abstract.

The build step is the GPA-sorting function from the opening, with the actual spec written out. *Input: a list of student records, each with name (first, last) and gpa. Output: the top three by gpa, descending. Ties broken alphabetically by last name, ascending. If fewer than three students, return all of them.*

**Run one: delegated.** I open Claude, paste the spec, accept the output. The output is the tuple-key sort I described in the opener. The function compiles. I run it on my test set — three students, all different GPAs. It returns the right three in the right order. I move on. Two days later, with real data containing a tie, the function silently returns the wrong order. The bug ships into the rest of my code, where it causes a downstream display to list the wrong student at the top of a "highest GPA" leaderboard. I find the bug only because a friend tells me her sister's name is in the wrong spot.

Same Claude. Same output. The cost is the silent failure that propagated through everything that depended on the function.

**Run two: each capacity exercised, briefly, in order.** I open Claude. Before I paste, I do problem formulation: I look at the spec and notice that *ties* are a thing the spec explicitly mentions, which means my test data needs to include ties or my tests are not testing the spec. I add a tie to my test set before I even ask Claude. I paste the spec. Claude returns the tuple-key sort. I do plausibility auditing: I look at the key — `(-gpa, last_name)` — and ask whether this matches the spec on the tie case. I'm not sure. I run it on the test set, which now has the tie in it. The tied pair comes back in the wrong order. I do interpretive judgment: I read what came back as a domain output (which student is first?), not just as a syntax-correct return value, and I see the mismatch. I do tool orchestration: I don't ask Claude to "fix it" with no further input — I tell Claude the specific failure (`Bell came back before Adams; the spec requires Adams first on a GPA tie`) and ask for a corrected key. Claude returns a version that explicitly handles ties. I run the test again. The tie comes out right. I do executive integration: I add the tie case to my permanent test file, so future versions of the function — including ones I haven't written yet — will be checked against the same case, because the build is going to grow and I will forget this lesson in a week.

Same Claude. Same starting output. Five small moves and the bug doesn't ship.

The difference between the two runs is not effort, exactly. Run two took me maybe four extra minutes. The difference is that run two contained five small acts of supervision and run one contained zero. Each act was small. Each one is a habit. Once they're a habit, the four-minute overhead becomes the way you work, and the silent-failure path becomes a thing that happens to other people.

The lesson, in the format Chapter 1 set up: *the model is fine; the supervision is missing.*

The limit: the supervision moves above are the ones I happened to need on this function. A different function would need a different mix. The point is not to memorize five moves and apply them on every build. The point is to know the five capacities exist, so you can ask the right question at the right step. Chapter 5 will be the formal version of this. For now: the five names and the worked example are enough to start.

## Exercises

1. **(Apply) Sort the build.** Take the next ten things you build, or pull a list from any project you have in flight. Write each task on a line. Beside each, write one of three labels: *Claude work*, *human work*, or *dangerous middle*. The middle column is the one you have to defend. For any item you label "middle," write one sentence naming the supervisory capacity that, if skipped, would let a silent failure through. The exercise is harder than it looks; that's the point.

2. **(Analyze) Read a transcript.** Pull a Claude or Claude Code session you ran in the last week — one with at least eight to ten back-and-forth turns. Go through it slowly. At each Claude response, ask *which supervisory capacity would have caught a mistake here, and did I exercise it?* Mark every step where the answer is "no." You will probably find more than you expect. Submit the annotated transcript with a one-paragraph reflection on the pattern of capacities you skipped.

3. **(Create) Write your labor-separation rule.** Pick one project you are currently working on. Write, in your own words, a labor-separation rule for *this project*: what tasks go to Claude unattended, what tasks go to Claude under supervision (and which capacity supervises them), and what tasks you do without Claude at all. Make the rule short enough to fit on one page. Tape it next to your laptop. In two weeks, revise it based on what you actually did versus what the rule said. Both versions count as the deliverable.

**Links:** [irreducibly.xyz](https://irreducibly.xyz) — the full seven-tier taxonomy for readers who want the complete architecture.

## 🕰️ AI Wayback Machine

**Frederick Winslow Taylor** (1856–1915) is the person who first tried to write down, on paper, which cognitive work belongs to the human and which to the system. *The Principles of Scientific Management*, published in 1911, contains the line that gives the chapter its bones: "the work of every workman is fully planned out by the management at least one day in advance" (Taylor, 1911). Taylor's move was to separate *planning* — judgment, formulation, evaluation — from *execution* — repeatable procedural pattern — and then to study each separately. Sound familiar? It is the move this chapter performs, transposed: separate pattern work from supervisory work, and analyze each.

I want to be honest about Taylor, because he is famous and he is controversial and the controversy is earned. The version of scientific management Taylor implemented in steel mills and on shop floors stripped craft workers of judgment, handed it to a managerial class, and produced both real productivity gains and real dehumanization. The historical record on his selective data and on the human cost of his system is not flattering. The chapter borrows Taylor's *question* — which cognitive work belongs where — while rejecting his answer that the worker's judgment is residue. In this book, supervisory intelligence is not what's left over after the machine takes the good parts. It is the load-bearing capacity. The student, not Claude, is the engineer in Taylor's sense; Claude is the part Taylor would have studied for time and motion.

**Run this:** *"You are Frederick Winslow Taylor in 1911, writing a memo to a 2026 high school student building a project with Claude Code. Using the planning-versus-execution framework from* The Principles of Scientific Management, *but updating it for a worker whose 'machine' is a large language model, write a one-page memo telling the student which work belongs to her and which belongs to Claude. Be specific. Then, in a second page, critique the memo from the perspective of a 2026 reader who has read the historical record on the human cost of Taylorism. What does this division of labor get right? What does it dehumanize?"*

## Bridge

The reader can name the capacities. Chapter 3 explains why school isn't teaching them — and why the student is on their own.
