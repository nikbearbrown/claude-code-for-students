# Chapter 12 — Running the Build: Claude Tasks and Human Tasks

*The score is the plan. Now you execute it — one step at a time, with explicit handoff conditions between every step.*

---

**Learning outcomes**

1. **(Apply)** Execute a Boondoggle Score build sequence, completing Claude tasks and human tasks in dependency order.
2. **(Apply)** Apply each of the five supervisory capacities at the steps requiring them.
3. **(Analyze)** Identify when a build is going off-script and stop before it breaks.

---

## Opening: Inside the Loop

It is Saturday morning, and the Boondoggle Score for Phase 1 of the task tracker is on my left monitor. Five rows. Two mine, three Claude's. Every row has a sharp handoff condition. The plan is finished, and it is the most important document in the room.

On the right monitor, Claude Code's prompt is blinking inside the project directory. The directory has one file: `types.ts`, which I wrote by hand in row 1 because row 1's labor column says *human*. The interface is twelve lines. I read it twice. Row 1 passes. I move to row 2.

Row 2 says *scaffold the HTML page; handoff condition: page loads with empty root, no console errors.* I paste a prompt. Claude produces an `index.html`. I open it in the browser. Page loads. Console clean. Empty root div. Row 2 passes.

Row 3 is where the morning gets interesting. *Implement `addTask`, `toggleTask`, `deleteTask`. Handoff condition: three unit tests pass on a `Task[]` of length 0, 1, 3.* I paste a prompt. Claude produces three functions and three tests. I run the tests. All three pass. I read the diff. Something is off. `deleteTask` is removing the task with `tasks.splice(index, 1)` instead of `tasks.filter(t => t.id !== id)`. Splice mutates. Filter returns a new array. My SDD did not explicitly say *immutable updates only* — the handoff condition I wrote down (*three tests pass*) does not catch it. The tests pass with `splice`. They also pass with `filter`. The tests are blind to the thing I am suddenly seeing.

This is the moment the chapter is about. I do not have a test that has failed. I have an output that *passes* the handoff condition I wrote and still fails the condition I *should* have written. The tests-pass path is fast. The reject-and-respecify path is slow. The reject-and-respecify path is the chapter.

I press Esc twice. The conversation rewinds to before the row 3 prompt. The files Claude wrote are gone. I rewrite the prompt with one added sentence: *Use only immutable updates — return new arrays from `toggleTask` and `deleteTask`; do not mutate the input.* I paste. Claude produces three functions and three tests. The diff now uses `filter` and `map`. The tests pass. Row 3 passes.

That is what running a build feels like. Not pressing *accept all changes* on each output — evaluating, at every row, against the condition I wrote and the conditions I should have written; rejecting the ones that fail either test; tightening the spec when the spec was thin. The build is not a series of approvals. It is a series of *audits*, performed by a person who can stop the line.

<!-- → [DIAGRAM: The build loop — Specification → Claude executes → Handoff condition check → [Pass: next step] / [Fail: /rewind and respecify]. Loop annotation showing supervisory capacity at the check step. Editorial style.] -->

The diagram above is the entire chapter in one image. One loop. Four nodes. Two branches at the check. The accent on the failure branch is the part of the chapter most students need to practice — because the failure branch is the one that requires a keystroke (Esc-Esc, or `/rewind`) instead of a habit (typing the next correction).

The rest of this chapter is the loop, slowed down and named. First the mechanics of running Claude rows (the Minion Part), then the mechanics of running human rows (the Gru Part), then four named failure modes and what to do about each one, then one piece of standing infrastructure (a verification mechanism Claude can run against itself), and then the full, prompt-by-prompt build session for Phase 1 of the task tracker.

## Running the Minion Part

The Minion Part is every row where the *Labor* column says Claude. In Phase 1, that is rows 2, 3, and 5. Your job during a Claude row has four parts, in order.

**Read the row.** Out loud, if you are alone. The row contains a step, a handoff condition, and — implicitly — a scope. Row 3 says *implement `addTask`, `toggleTask`, `deleteTask`*. It does not say *implement a `TaskList` component* or *add styling*. The prompt has to honor what the row is silent about as much as what it says.

**Write the prompt from the row.** Chapter 8 made the case that a prompt is a specification. The specification is already in the row; the prompt assembles row, relevant SDD section, and acceptance criteria into one paragraph. The temptation is to skip the row and prompt from the SDD directly. Resist. The row exists so the prompt is bounded; skip the row, lose the bound.

**Paste, then read what Claude produces.** Not run it. *Read it*. The diff, the new files, the function bodies. You are looking for two things: does the output do the row, and does it do anything *other* than the row. Both are failure conditions. Doing-the-row failure gets a tighter prompt. Doing-more-than-the-row gets a `/rewind` and a scope fence.

**Run the handoff condition.** Not "look at it and decide it is fine." *Run* it. If the condition is *three tests pass*, run the tests. If it is *refresh preserves state across 10 toggles*, toggle ten things and refresh. The condition was written down so that evaluation does not require a judgment call.

Two operational notes. Keep prompts at row granularity, not SDD granularity — a prompt covering three rows is a prompt whose output you cannot evaluate row by row. And paste the relevant SDD section as context, even when you think Claude can infer it. Twelve lines of interface in the prompt costs a breath. Letting Claude infer the wrong shape costs the row.[^claude-docs]

[^claude-docs]: Anthropic's Claude Code documentation makes the same point in operational form: "give the model enough context to do the job, including relevant code, errors, and constraints," and "be specific about what you want as output." The principle survives any future change to the prompt API; the specific syntactic affordances (`/clear`, `/rewind`) are flagged in Chapter 11 of this book as high aging-risk. Anthropic, *Claude Code Documentation* — Best Practices, code.claude.com/docs (accessed May 2026).

## Running the Gru Part

The Gru Part is every row where *Labor* says Human. In Phase 1, that is rows 1 and 4. Human rows are not the easy rows; they are the rows whose output everything else inherits from, which is why they sit at the top of the score.

Each human row has a *supervisory capacity* named in the table. That naming is the binding instruction for how to run the row.

**Row 1: Define the `Task` data model. Supervisory capacity: domain knowledge — what fields this app actually needs.** I sit with a blank `types.ts` and think about what a task in *this* app is. Not what tasks are in general. Not what Todoist or Trello use. What this specific app, for this specific user, in this specific scope, needs. The capacity is **[PF] Problem Formulation** from Chapter 5: deciding what the thing *is*, in writing, before Claude sees it. Row 1's labor is not typing; it is committing to a shape. Once committed, the typing is twelve lines.

The handoff condition says *written as one TypeScript interface in `types.ts` before any UI code*. Binary. Either the file exists and contains the interface, or it does not. The condition does not say "the right interface" because *right* is not testable; what is testable is existence and uniqueness. Sharpness does not require omniscience; it requires honesty about what can be checked.

**Row 4: Decide visual hierarchy. Supervisory capacity: aesthetic judgment.** This is the row most students want to give to Claude — because they think they cannot do it, or because Claude can do it faster, or because they have no opinion. The cost of giving it to Claude is that the app ends up looking like every other Claude-generated app: soft shadows, rounded corners, a gradient. Visual language inherited, not chosen.

What I do in row 4 is name three CSS variables. *`--font`* is Inter — loaded already, reads cleanly at small sizes. *`--primary`* is `#1a1a1a` — high-contrast, document-like. *`--accent`* is `#d4a017`, a warm gold — the *done* state gets weight without being green. Three lines. Twenty minutes of decision. The handoff condition is *three variables named before Claude writes any styles*; the *before* is the constraint Claude inherits. After row 4, Claude does not pick colors. Claude picks within the colors row 4 already chose.

The two rows are short in lines of code and large in supervisory load. That is the signature of a well-placed human row. If a human row feels like typing, the labor is mislabeled.

## When Claude Fails a Handoff Condition

The condition fails. Tests don't pass, the page doesn't load, the refresh doesn't preserve state. What now.

The wrong move — the move every student makes the first time, and most make the first ten times — is *forward correction*. Read the failing output, see what is wrong, paste a new prompt: *the toggle function isn't working on an empty array, please fix it.* The new prompt does not replace the old one. It is *appended* to a conversation that already contains the failed attempt and the misunderstanding that produced it. Claude's next output is conditioned on both: the original spec and the wrong code produced against it. The next output usually fixes the named symptom and introduces a new one, because the underlying misunderstanding has not been touched.

The right move is `/rewind`. In Claude Code as of May 2026, that is either the `/rewind` command or the keystroke Esc-Esc, both of which restore conversation *and* code to the state before your last prompt.[^rewind] You are not rolling back the symptom. You are rolling back the cause — the prompt that produced the misunderstanding — and re-specifying. It is the keystroke equivalent of pulling Toyota's andon cord: stop the line, fix the cause, restart.[^andon]

[^rewind]: Anthropic, *Claude Code Documentation* — `/rewind` and conversation checkpoints. The `/rewind` command and the Esc-Esc keystroke are current as of May 2026 and were introduced in the late-2024 Claude Code update; the underlying principle (back up to before the misunderstanding) is older than the command and will outlast any specific syntax. Treat as high aging-risk per Chapter 11. Cross-verified against ClaudeLog session-management notes (accessed March 2026).

[^andon]: Jeffrey K. Liker, *The Toyota Way: 14 Management Principles from the World's Greatest Manufacturer*, 2nd ed. (McGraw-Hill, 2020), Principle 5 — *jidoka*, "build a culture of stopping to fix problems, to get quality right the first time." In mature Toyota plants the andon cord is pulled roughly one thousand times per shift; the rate is the *system working*, not failing.

The re-specification is the part that takes thought. The new prompt is not the old prompt plus a complaint. It is the old prompt with the misunderstanding written *out* of it. In the opening, the misunderstanding was that I had not said *immutable*. The new prompt added the sentence that closed the ambiguity — longer than the old prompt by exactly the missing constraint — and ran against a clean context because the rewind erased the previous attempt.

A heuristic: the new prompt should be *strictly more specified* than the old one in at least one named dimension. If they are the same length, you have appended a correction rather than tightened a spec. Read the diff between the two prompts. If the diff is a complaint sentence, wrong diff. If the diff is a constraint sentence, right diff.

The whole loop — read failed output, identify missing constraint, `/rewind`, paste re-specified prompt, evaluate — is under five minutes once practiced. The first time it takes thirty, because the temptation to forward-correct is enormous and the rewind feels expensive. It is not. It is the cheapest move in the build. Forward correction is what feels cheap and is actually expensive, because each correction adds to the context, and context past a point is where quality goes to die.[^context-rot]

[^context-rot]: Kexun Liu et al., "Context Length Alone Hurts LLM Performance Despite Perfect Retrieval," *Findings of EMNLP* (2025), arXiv:2510.05381. The paper shows systematic degradation of LLM output quality as input context grows, even when retrieval is perfect — across five frontier models and three task categories (math, QA, coding). The Chroma 2025 "context rot" report on eighteen frontier models reaches the same conclusion. Forward correction is *adding tokens* to the context that produced the misunderstanding; `/rewind` is removing them. The empirical case for the keystroke is in this literature.

## When Claude Passes but Feels Wrong

This is the failure mode I almost missed in row 3. The handoff condition passed. Tests ran green. The output should have shipped. Something in the diff felt wrong, and the wrong feeling was correct.

The capacity Chapter 5 named is **[PA] Plausibility Auditing**: hearing the wrong note before verification confirms it. There is no checklist for *the feeling that something is off*. There is only the discipline of taking the feeling seriously when it appears.

The discipline has two halves. The first is *permission*. Most students, when a Claude output passes the condition and feels wrong, override the feeling. The override is rational-sounding: *the tests passed, who am I to second-guess.* The override is the failure mode. Plausibility auditing is, by definition, the capacity to register wrongness *before* a test catches it; if you only trust feelings backed by failed tests, you have no plausibility auditing — only post-hoc justification.

The second half is *investigation*. A wrong-feeling output you cannot articulate is not yet a wrong output; it is a hypothesis. The investigation converts the hypothesis to a confirmed problem or a dismissed one. In row 3, the investigation was: *what specifically am I seeing?* Splice instead of filter. *Why does that bother me?* It mutates the input array, and mutating shared state in event handlers is a bug class I do not want. *What handoff condition would have caught it?* A test that calls the function twice and checks the original array is unchanged.

The whole investigation took under two minutes. It produced a confirmed problem, a sharper handoff condition, and a tighter prompt. Plausibility auditing did not require expertise I did not have. It required taking the feeling seriously long enough to articulate it.

The honest empirical case is that Claude Code makes producing-wrong-code *cheap*. Single-pass generation against an underspecified prompt is a five-second operation. The economics of wrong code have inverted: production rate now exceeds inspection rate, which means the only thing standing between a build and the wrong-code-shipped failure mode is the human's pre-test sense that something is off. This is exactly the problem Deming named in 1986 — when inspection is the only gate, defective work reaches inspection too fast for inspection to handle, and quality degrades.[^deming-inspection] The plausibility audit is in-loop quality control: it stops the defect before it reaches the inspector.

[^deming-inspection]: W. Edwards Deming, *Out of the Crisis* (MIT Center for Advanced Engineering Study, 1986), Point 3: "Cease dependence on inspection to achieve quality. Eliminate the need for inspection on a mass basis by building quality into the product in the first place." The Wayback Machine entry below develops the connection at length.

What plausibility auditing does *not* mean: second-guessing every Claude output until you have personally verified each line. That makes Claude useless. It means cultivating the specific skill of registering and investigating the *wrong note* — the moment when something compiled, tested, and presentable nevertheless reads off. Most outputs do not produce that feeling. The ones that do are the ones to investigate.

## The Scope Creep Moment

Halfway through Phase 1, Claude produced an `index.html` with a `<style>` block in the head containing default styles. The row had asked for *head, single root div*. The row had not asked for styles. Row 4 — *Decide visual hierarchy* — had not yet run. Claude had quietly executed half of row 4 inside row 2.

This is scope creep, and it is the most common Claude failure on a Boondoggled build that has otherwise gone well — because Claude is helpful, and helpful means doing more than the row asks. The output reads as a gift: *I also added X while I was here*. The gift is a bug. It moves work from a row whose supervisory capacity has been named to one whose capacity is silent. In this case, aesthetic judgment migrated from a human row to a Claude row, executed before the human row's input had been written.

The response is `/rewind` plus an explicit scope fence: *Do not add styles. Row 4 handles styles. Limit output to head, root div, minimal HTML to load.* I paste. Claude produces the file. The `<style>` block is gone. Row 2 now does the row and nothing else.

A second pattern, less obvious but more dangerous: Claude proposes a *refactor* during a feature implementation. *I noticed `addTask` could be cleaner if we refactored the index lookup into a helper. While I was at it, I made that change.* The refactor may be correct. The problem is not its quality; the problem is that a change not on the score has been smuggled into a row whose handoff condition does not test for it. The build now contains a change you did not plan and cannot evaluate.

The response is the same: `/rewind`, scope fence, re-prompt. *Implement the named functions only. Do not refactor surrounding code. Any refactor proposal goes in a top-of-file comment labeled `// REFACTOR PROPOSAL` and is otherwise untouched.* The comment turns the smuggled refactor into a *suggestion* — a separate row, evaluated separately, later.

The discipline is harsh and the harshness is the point. *While I was here* is the phrase that ends Boondoggled builds.

## When to Stop and Reformulate vs Push Through

Not every failure is a `/rewind`. Some failures signal that the row itself is wrong — the spec it inherits is thin, the handoff condition does not test for what matters, or the row was attempting work that should have been split.

The diagnostic is the *two-rewind rule*. The first rewind is normal. The second rewind on the same row for the same kind of failure is information: the problem is no longer in Claude's output but in your spec. Stop. Open the SDD and the Boondoggle Score side by side. Ask: *is the handoff condition a test for what I want, or for what I could write down?* Ask: *is the SDD section this row inherits from thin?* Usually one answer is yes, and the fix is in the document, not in the next prompt.

After a third failed rewind, stop the build. The plan is broken; more prompts against a broken plan produce code you will throw away. Walk back through Gate 3 from Chapter 11, thicken the thin section, rewrite the row, restart from the last passing row.

The opposite failure is also real. Some outputs *almost* pass — the row is done, but one cosmetic thing needs fixing. *Push through* is right when the fix is faster than re-prompting, does not require re-running the handoff condition, and is small enough not to lose. Editing a variable name. Tightening a comment. These are editorial fixes — the human side of the labor column for the row's last five percent.

The line between push-through and rewind is whether the fix changes *meaning* or only *surface*. Variable names and comments are surface. Algorithmic choices, data shapes, mutation vs immutability, error handling — those are meaning. Surface, push through; meaning, rewind. When in doubt, rewind: it is reversible, and forward-correction is not.

## Give Claude a Way to Verify Its Own Work

So far this chapter has put the verification load entirely on the human. The handoff condition is checked, as written, by you. One piece of standing infrastructure changes this picture, and you should build it before any Claude row that warrants it.

The infrastructure is a verification mechanism Claude can run *against itself*. In Phase 1 of the task tracker, that is `task-fns.test.ts` and `npm test`. In a Python project it might be `pytest` and `ruff`. In a static site project, `playwright check` against three named states. The form does not matter. What matters is that there exists a single bash command Claude can run whose exit code is the handoff condition's pass/fail signal.

Once that mechanism exists, the prompt for a Claude row changes shape. Not *implement the functions, here are the requirements*. Instead: *implement the functions; run `npm test` after each iteration; iterate until all tests pass*. Claude is now in a feedback loop with a verification mechanism, and the loop closes without the human in the middle. This is *not* delegation — the human still wrote the tests; the tests *are* the handoff condition; the human still audits the final output against the SDD. What has changed is that Claude is no longer producing single-shot output against a spec; Claude is *iterating against a feedback mechanism the human installed*.

The empirical case is that iterative LLM codegen against a verification mechanism beats single-pass generation. Lahiri and colleagues' 2024 TICODER study found test-driven interactive code generation improved pass@1 by roughly 46% within five user interactions, with diminishing returns past three to four iterations — which is also the empirical basis for the two-rewind rule above.[^ticoder]

[^ticoder]: Sandeep Lahiri et al., "LLM-Based Test-Driven Interactive Code Generation: User Study and Empirical Evaluation," *IEEE Transactions on Software Engineering* (2024), arXiv:2404.10100. TICODER demonstrates that LLM coding agents improve substantially when iterating against a test signal, with the improvement curve flattening at three to four iterations — motivating both the in-loop verification mechanism in this section and the two-rewind heuristic above.

The discipline reorders the build. The verification mechanism is *row zero* — the row before any Claude row that needs it. In the task tracker, row 0 was *write `task-fns.test.ts` with the three unit tests*. I wrote it by hand. The tests were the handoff condition for row 3 *before row 3 ran*. The row 3 prompt included *the tests in `task-fns.test.ts` must pass*. Claude implemented the functions, ran the tests, iterated, reported all three passing. I read the diff and ran the tests independently. Row 3 closed in one prompt because the test was the spec and the spec ran on every iteration.

This is the most important operational move in the chapter. The tests are not your final inspection; they are Claude's *handoff condition*. You write the test, Claude works against it, you audit. The labor stack inverts the way Chapter 5 argued it should: human at top, defining *correct*; Claude in the middle, implementing; verification mechanism at the bottom, providing the binary signal that closes the loop. This is Deming's *quality built in, not inspected in*. The test is the built-in quality. The audit is what remains for the human after the build has been compelled to be quality-built.

## /clear Between Unrelated Tasks

The last operational move is `/clear`. As of May 2026 it wipes the conversation history while preserving the code on disk.[^clear] After `/clear`, Claude has no memory of previous prompts, outputs, or corrections. The project files still exist; the conversation does not.

[^clear]: Anthropic, *Claude Code Documentation* — `/clear` and context management. Behavior verified May 2026. The command is current syntax; the principle (reset context between unrelated tasks, especially after repeated corrections) is older than the command and will outlast it. Treat as high aging-risk per Chapter 11.

The use is twofold. First, *between unrelated rows*. If row 5 wires `localStorage` and row 6 is a Phase 2 feature, the history from row 5 — the failed first attempt, the test output, the corrected version — is irrelevant to row 6. Worse than irrelevant: it is *active interference*. The context-rot literature is unambiguous: output quality degrades monotonically with context length, even when retrieval is perfect, across every model tested.[^context-rot-clear] The row 5 conversation is making row 6's output worse. `/clear` between rows is hygiene.

[^context-rot-clear]: See Liu et al. (EMNLP Findings, 2025), arXiv:2510.05381, and the Chroma 2025 context-rot report on eighteen frontier models. Context length is itself a degradation factor, independent of retrieval quality. Walter A. Shewhart anticipated the principle in *Statistical Method from the Viewpoint of Quality Control* (Graduate School of the Department of Agriculture, 1939; Dover reissue 1986, ed. Deming) — an in-control process requires removing accumulated noise between cycles. The conversation is the process; `/clear` removes the noise.

Second use: *after repeated correction on the same issue*. The two-rewind rule says that after two rewinds on the same row, the spec is broken. The corollary: the reformulated spec should be pasted into a *cleared* conversation, not the polluted one. The polluted conversation contains two failed attempts and Claude's reasoning about why; the next attempt inside it inherits that wrong-attempt history. `/clear` first. Then paste the rewritten spec.

There is an emotional component worth naming. `/clear` feels expensive. The conversation feels like progress; clearing it feels like losing progress. But the conversation is not the progress — the code on disk is. The conversation is infrastructure that has accumulated rot. Clearing it restores Claude to a clean baseline.

If you have corrected Claude more than twice on the same issue, the context is polluted. `/clear` and rewrite the spec from scratch with the misunderstanding written out. That is the chapter's last operational rule.

## Worked Example: Phase 1 of the Task Tracker, in Real Time

What follows is the full build session for Phase 1 of the task tracker planned in Chapter 11. Five rows, plus a row 0 for the verification mechanism. One rejection, one rewind, one re-specification. The prompts are the prompts I actually pasted; Claude's outputs are the outputs I actually got, rendered or paraphrased inline.

### Row 0 — Write the verification mechanism (Human, [PF])

Before any Claude row, I wrote `task-fns.test.ts` by hand — the spec for row 3 expressed as code. Three tests, one per function.

```ts
// task-fns.test.ts
import { describe, it, expect } from "vitest";
import { addTask, toggleTask, deleteTask } from "./task-fns";
import type { Task } from "./types";

describe("task-fns", () => {
  it("addTask adds a task to an empty array", () => {
    const result = addTask([], "buy milk");
    expect(result.length).toBe(1);
    expect(result[0].text).toBe("buy milk");
    expect(result[0].done).toBe(false);
  });

  it("toggleTask flips done on a single task without mutating input", () => {
    const input: Task[] = [{ id: "a", text: "x", done: false }];
    const result = toggleTask(input, "a");
    expect(result[0].done).toBe(true);
    expect(input[0].done).toBe(false); // immutability check
  });

  it("deleteTask removes a task without mutating input", () => {
    const input: Task[] = [
      { id: "a", text: "x", done: false },
      { id: "b", text: "y", done: true },
      { id: "c", text: "z", done: false },
    ];
    const result = deleteTask(input, "b");
    expect(result.length).toBe(2);
    expect(result.find(t => t.id === "b")).toBeUndefined();
    expect(input.length).toBe(3); // immutability check
  });
});
```

*Handoff:* `npm test` runs and reports three failures with "module not found" (because `task-fns.ts` does not yet exist).

*Evaluation:* Ran. Three failures, correct error. Row 0 passes. Row 3's handoff condition is now mechanized — Claude runs against this.

### Row 1 — Define the data model (Human, [PF])

Written by hand into `types.ts`:

```ts
// types.ts
export interface Task {
  id: string;       // crypto.randomUUID()
  text: string;     // 1..200 chars, trimmed
  done: boolean;    // default false
}
```

*Handoff:* file exists; one interface; field types match the SDD. *Evaluation:* passes.

### Row 2 — Scaffold the HTML page (Claude)

**My prompt** (pasted into Claude Code):

> Create `index.html` for a one-page task tracker app. Requirements:
>
> - Head section: title "Today's Tasks", meta charset, viewport meta tag, link to `style.css`, link to a module script `main.ts`.
> - Body: a single empty `<div id="root">` and nothing else.
> - No inline styles. No `<style>` block. Row 4 of the build plan handles all styling; do not add styles here.
> - The page must load in a browser with no console errors.

**Claude's output** (the file Claude wrote):

```html
<!doctype html>
<html lang="en">
  <head>
    <meta charset="utf-8" />
    <meta name="viewport" content="width=device-width, initial-scale=1" />
    <title>Today's Tasks</title>
    <link rel="stylesheet" href="style.css" />
    <script type="module" src="main.ts"></script>
  </head>
  <body>
    <div id="root"></div>
  </body>
</html>
```

*Handoff:* page loads with empty root, no console errors.

*Evaluation:* Opened in Vite dev server. Loads clean. No `<style>` block (the scope fence held). Row 2 passes. Capacity engaged: **[TO] Tool Orchestration** — the prompt was bounded so the output stayed on the row.

### Row 3 — Implement the three task functions (Claude) — REJECTED, then accepted

**My first prompt:**

> Implement `task-fns.ts` exporting three pure functions for managing a `Task[]`:
>
> - `addTask(tasks: Task[], text: string): Task[]` — appends a new `Task` with `id` from `crypto.randomUUID()`, trimmed `text`, `done: false`.
> - `toggleTask(tasks: Task[], id: string): Task[]` — returns a new array with the task whose `id` matches having `done` flipped.
> - `deleteTask(tasks: Task[], id: string): Task[]` — returns a new array with the task whose `id` matches removed.
>
> Imports `Task` from `./types`. Run `npm test` after writing; the three tests in `task-fns.test.ts` must pass.

**Claude's first output** (paraphrased — the functions Claude wrote):

```ts
// task-fns.ts (first attempt)
import type { Task } from "./types";

export function addTask(tasks: Task[], text: string): Task[] {
  tasks.push({
    id: crypto.randomUUID(),
    text: text.trim(),
    done: false,
  });
  return tasks;
}

export function toggleTask(tasks: Task[], id: string): Task[] {
  const i = tasks.findIndex(t => t.id === id);
  if (i >= 0) tasks[i].done = !tasks[i].done;
  return tasks;
}

export function deleteTask(tasks: Task[], id: string): Task[] {
  const i = tasks.findIndex(t => t.id === id);
  if (i >= 0) tasks.splice(i, 1);
  return tasks;
}
```

Claude ran `npm test` and reported: *2 failed, 1 passed.* The two immutability tests failed; the addTask basic test passed. Claude proposed a fix: *I'll switch to immutable updates.*

*Evaluation:* output mutates input arrays. Two tests fail. Even if Claude's proposed forward correction were to work, the conversation now contains the wrong implementation as its starting point. Capacity engaged: **[PA] Plausibility Auditing** — the diff read wrong; the tests confirmed the read.

**My response:** Esc-Esc. The conversation rewound to before the first prompt. The `task-fns.ts` file Claude wrote was discarded.

**My second prompt** (the re-specification):

> Implement `task-fns.ts` exporting three pure functions for managing a `Task[]`.
>
> **Immutability requirement:** none of these functions may mutate their input. All return new arrays. Use spread, `map`, and `filter`. Do not use `push`, `splice`, or in-place property assignment.
>
> Signatures:
>
> - `addTask(tasks: Task[], text: string): Task[]` — returns a new array with a new task appended. New task has `id` from `crypto.randomUUID()`, trimmed `text`, `done: false`.
> - `toggleTask(tasks: Task[], id: string): Task[]` — returns a new array where the task with matching `id` has `done` flipped. Other tasks unchanged.
> - `deleteTask(tasks: Task[], id: string): Task[]` — returns a new array without the task whose `id` matches.
>
> Imports `Task` from `./types`. Run `npm test` after writing; all three tests in `task-fns.test.ts` must pass, including the immutability assertions on the input arrays.

**Claude's second output:**

```ts
// task-fns.ts (second attempt)
import type { Task } from "./types";

export function addTask(tasks: Task[], text: string): Task[] {
  return [
    ...tasks,
    { id: crypto.randomUUID(), text: text.trim(), done: false },
  ];
}

export function toggleTask(tasks: Task[], id: string): Task[] {
  return tasks.map(t => (t.id === id ? { ...t, done: !t.done } : t));
}

export function deleteTask(tasks: Task[], id: string): Task[] {
  return tasks.filter(t => t.id !== id);
}
```

Claude ran `npm test` and reported: *3 passed.*

*Evaluation:* spread, map, filter — no mutation. All three tests pass, including immutability assertions. Output does the row and only the row. Row 3 passes on the second attempt.

### Row 4 — Decide visual hierarchy (Human, [IJ])

Written by hand into `style.css`:

```css
:root {
  --font: "Inter", system-ui, sans-serif;
  --primary: #1a1a1a;
  --accent: #d4a017;
}
```

*Handoff:* three variables named before Claude writes any styles. *Evaluation:* three variables present, no other rules. Row 4 passes. Capacity engaged: **[IJ] Interpretive Judgment**.

### Row 5 — Wire localStorage with corruption guard (Claude)

**My prompt:**

> Add `localStorage` persistence to the task tracker in `main.ts`. Requirements:
>
> - Storage key: `tasks-v1`.
> - On every mutation (when calling `addTask`, `toggleTask`, or `deleteTask`), write `JSON.stringify(tasks)` to `localStorage["tasks-v1"]`.
> - On page load, read `localStorage["tasks-v1"]`. Attempt `JSON.parse`. On parse failure, show a confirmation dialog with the text "Saved tasks are corrupted. Reset?" — on confirm, clear the key and start empty; on cancel, leave the key untouched and start with an empty in-memory array for this session.
> - On `localStorage` unavailable (private browsing throws on access), fall back to in-memory only and append a single line to the root div: "Tasks won't persist in this window."
> - Use the existing pure functions in `task-fns.ts`. Do not modify them. Do not add new functions to that file.
> - Add a test in `task-fns.test.ts`? No — persistence is integration, not unit. I will verify by hand.

**Claude's output** (paraphrased — the relevant logic in `main.ts`):

```ts
// main.ts (relevant excerpt)
import { addTask, toggleTask, deleteTask } from "./task-fns";
import type { Task } from "./types";

const KEY = "tasks-v1";
const root = document.getElementById("root")!;

function loadTasks(): Task[] {
  try {
    const raw = localStorage.getItem(KEY);
    if (raw === null) return [];
    try {
      return JSON.parse(raw) as Task[];
    } catch {
      const reset = confirm("Saved tasks are corrupted. Reset?");
      if (reset) {
        localStorage.removeItem(KEY);
        return [];
      }
      return [];
    }
  } catch {
    const banner = document.createElement("p");
    banner.textContent = "Tasks won't persist in this window.";
    root.prepend(banner);
    return [];
  }
}

function save(tasks: Task[]) {
  try {
    localStorage.setItem(KEY, JSON.stringify(tasks));
  } catch { /* private mode — silent */ }
}

// ... render logic, event handlers calling addTask/toggle/delete, then save(...)
```

*Handoff:* refresh preserves state across 10 toggles; corrupt JSON triggers reset-with-confirm.

*Evaluation:* Added five tasks, toggled each twice, refreshed. State preserved. Edited `localStorage["tasks-v1"]` to `{not json` in DevTools. Refreshed. Dialog appeared. Cancel dismissed it; refresh re-triggered (key untouched). Confirmed reset; key cleared; refresh stable. Row 5 passes. Capacity engaged: **[EI] Executive Integration** — does this row, combined with rows 1–4, hold together as the system the SDD specified.

### Phase 1 closes

All rows pass. One rewind, one re-specification, one accepted output per Claude row. The capacities engaged across the build were **[PF]** at rows 0 and 1, **[TO]** at row 2, **[PA]** at row 3, **[IJ]** at row 4, **[EI]** at row 5 — all five Chapter 5 capacities, in five rows, in roughly two hours.

What I have at the end of Phase 1 is a small, ugly, working task tracker. Five files. About a hundred and twenty lines of code. Every line of which I either wrote or evaluated against a written handoff condition. There is no decision in the project Claude made without me either writing the specification for it or rejecting its first attempt. The artifact is small. The capability behind the artifact is the chapter.

## Exercises

1. **(Apply)** Execute the first phase of the Boondoggle Score you wrote at the end of Chapter 11. Document each handoff evaluation: for each row, write down the handoff condition, the supervisory capacity engaged, and the pass/fail outcome. Submit the evaluation log alongside the code. The log is the deliverable, not the code. If the log has fewer rows than the score, the build is not done.

2. **(Analyze)** At least one Claude output in your build will require revision. Pick the rejection that was hardest to make — the one where the temptation to forward-correct was strongest, or the one where the wrong-feeling output passed its handoff condition and you almost shipped. Document (a) the failed output, (b) the missing constraint in the original prompt, (c) the re-specified prompt, (d) the accepted output. The four items together are a *reformulation case study*. They are also, in aggregate across the class, the most useful artifact your cohort will produce — a corpus of named misunderstandings and the constraints that closed them.

3. **(Evaluate)** At the end of Phase 1, write one paragraph (200–400 words) answering: what did you learn that you didn't know at planning? What would you change in the SDD if you were starting over? Be specific. *I learned that I underspecified persistence* is not specific. *I learned that my persistence section did not name what should happen when a user has open the page in two tabs and toggles in one, and that this is a real problem because I do have it open in two tabs sometimes* is specific. The post-build learning paragraph is the document Chapter 14 will return to. Write it now while the build is still warm.

---

## 🕰️ AI Wayback Machine

**W. Edwards Deming** (1900–1993). American statistician and management consultant. Deming spent the 1950s in postwar Japan, where his lectures on statistical quality control to Japanese engineers and executives became the philosophical foundation of what the West later called the Japanese economic miracle. His book *Out of the Crisis* (MIT Center for Advanced Engineering Study, 1986) reorganized those lectures into the *14 Points for Management* and what he eventually called the *PDSA cycle* — Plan, Do, Study, Act — a four-step iterative process for improving any system that produces an output and a way to measure it. (Deming inherited the cycle from Walter Shewhart's 1939 *Statistical Method from the Viewpoint of Quality Control* and renamed Shewhart's *Check* to *Study* to emphasize learning over inspection.) Point 3 of the 14 Points reads: *"Cease dependence on inspection to achieve quality. Eliminate the need for inspection on a mass basis by building quality into the product in the first place."* The handoff condition is exactly this principle, applied to a Claude Code build sequence: quality is the test that lives between two rows, not the audit that happens at the end. The Boondoggle Score sequences verification into every step because every step is the only step where verification is cheap.

**Run this:**

> Read Deming's Point 3 from *Out of the Crisis* (full text available via the Deming Institute, deming.org). Quote the point verbatim in fewer than 30 words. Then translate it from manufacturing language into a one-sentence operational rule for running a Claude Code build. Then look at the Boondoggle Score from your Chapter 11 exercise: identify the row whose handoff condition is the weakest — the row most likely to require post-build inspection rather than in-loop verification. Rewrite the handoff condition so that it builds quality in, in Deming's sense, instead of inspecting it in. Show both versions.

---

The build is done when it passes the handoff conditions. Chapter 13 defines what "done" actually means.
