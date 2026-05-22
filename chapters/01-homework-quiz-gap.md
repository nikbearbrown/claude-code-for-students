# Chapter 1 — The Homework/Quiz Gap: What's Actually Happening

*Students who use AI freely during practice score dramatically lower on unassisted tests — and feel like they learned more, not less.*

---

**Learning outcomes**

1. **(Understand)** Explain why AI-assisted practice can produce the feeling of mastery without the cognitive events that constitute it.
2. **(Analyze)** Distinguish between capability borrowed from the machine and capability built in the learner.
3. **(Evaluate)** Assess your own recent AI use against this distinction.

---

In Bastani et al.'s 2025 randomized field experiment with about a thousand Turkish high school students learning math, the group with unrestricted GPT-4 access scored **48% higher than control during AI-assisted practice** and **17 percentage points lower than control on the unassisted exam**. The exam-score decline was statistically significant — meaning unlikely to be a fluke of who got randomized into which group — at *p* = 0.01, with a moderate-to-large effect size of *Cohen's d* ≈ 0.738 [verify against the PNAS supplementary tables; the 2025 paper has a published correction, PubMed 40833419]. Same students. Same content. The tool that made them look better on the homework made them measurably worse on the test (Bastani et al., 2025).

That is not "AI is bad for learning." That sentence is too big to be useful. The Bastani result is narrower and stranger: the practice that *felt* productive is the practice that failed. The thing my brother D was doing in Chapter 0 — paste in, copy back, move on, get a 100, get a 41 on the quiz — that thing is reproducible in a controlled experiment with a thousand kids and a control group. He is not unusual. He is the average outcome of a specific behavior.

This chapter is about why.

I want to be careful with the numbers, so let me put them in one place before I talk about them. A quick vocabulary note first, because you'll need three pieces of jargon. An **RCT** — randomized controlled trial — is a study where participants are randomly assigned into groups so that, on average, the groups differ only by what the researchers did to them. Random assignment is what makes the comparison fair. A **p-value** is the probability that a difference at least this big would show up by chance if there were no real effect; *p* = 0.01 means you'd see this size of gap by sheer luck about one time in a hundred. And **Cohen's d** is a standardized way to report how big a difference is: the gap between two group averages divided by how spread out the scores are within the groups. Roughly: d = 0.2 is a small effect, 0.5 is medium, 0.8 is large. Bastani's d ≈ 0.738 sits in the moderate-to-large range — the kind of gap that, if it were a drug trial, you'd actually take the drug.

<!-- → [TABLE: Bastani RCT results — two columns: AI-Assisted group vs. Hand-Coding group. Rows: practice score, exam score, score gap, Cohen's d, p-value. No color. Editorial style.] -->

## What Bastani Measured

Hamsa Bastani and her colleagues at Wharton ran a field experiment in a Turkish high school over four practice sessions on a real math curriculum (Bastani et al., 2025). About a thousand students. Three arms, which is researcher-speak for three groups. The Control arm did the practice on paper, the normal way. The GPT Base arm did the same practice with full unrestricted access to GPT-4 — they could ask it anything, including "just give me the answer," and it would. The GPT Tutor arm got a version of GPT-4 that had been prompted to behave more like a tutor: it would scaffold, ask leading questions, refuse to hand over solutions outright.

Then they took the laptops away and gave everyone an unassisted exam on the same material.

Three results matter. First: during practice, the AI-using groups crushed it. GPT Base scored 48% higher than control while the AI was in the room. GPT Tutor scored 127% higher. Both groups, by the metric the homework would record, looked like they had a *much* better week than the control group. If you were a parent looking at the grade book, you would think your kid was learning at a rate the control kids weren't.

Second: on the unassisted exam, the picture inverted. GPT Base — the unrestricted-access group — scored 17 percentage points *below* control. Not 17 percent below in some relative sense. Seventeen full points on the test. The students who had done 48% better during practice scored substantially *worse* than students who had never touched the tool. The exam-score decline was significant at *p* = 0.01 with *Cohen's d* ≈ 0.738 [verify against PNAS supplementary materials; see the 2025 correction at PubMed 40833419]. That is a real effect, in a real classroom, large enough that you would not need statistics to see it. The statistics just rule out the possibility that you are imagining it.

Third — and this is the part that recovers the book's thesis — the GPT Tutor arm did not show the same collapse. They scored about level with control on the exam. Not better; just not worse. The Tutor group did 127% better during practice and ended up *equal* to the kids who had done it on paper. The full-access group did 48% better during practice and ended up *behind* the paper kids.

Read that paragraph again. The two AI groups had identical access to the same model. The only thing that differed was *how the model was instructed to behave* — whether it would hand over answers or insist the student do some of the work. That single design choice was the difference between "fluent practice, intact learning" and "fluent practice, damaged learning." The model was the same. The discipline imposed on the model was not.

This will turn out to be the entire argument of the book in seven words: *the model is fine; the discipline is missing.* I am putting it here because Bastani's result is the cleanest empirical version of it I know.

## What's Happening in the Brain When You Struggle

Skip the neuroscience word-cloud. Here is the part you need.

When you sit with a problem you don't yet know how to solve, your brain does something specific. It pulls up bits of related stuff you do know — half-remembered examples, a function that almost matches, the general shape of how problems like this go — and tries to put them together. Most of the attempts are wrong. You notice they are wrong, throw them out, and try again. Each cycle of *guess → check → revise* is a tiny event of effortful retrieval and generation. The cycle is uncomfortable. It is also the cycle that lays down the durable version of the skill.

Cognitive psychologists have a name for the uncomfortable part: **desirable difficulties** (Bjork & Bjork, 2011). A desirable difficulty is exactly what it sounds like — a difficulty that feels like it is slowing you down, and is actually doing the opposite. The two cleanest examples are the **testing effect** (retrieving something from memory strengthens the memory more than rereading it) and the **generation effect** (producing an answer yourself strengthens it more than reading the same answer written by someone else). Both effects feel worse in the moment and pay off later. Both effects depend on you doing the work that, with Claude open, you can skip.

Robert and Elizabeth Bjork put it cleanly: *short-term performance and long-term learning are not the same thing, and frequently they are negatively correlated* (Bjork & Bjork, 2011). When you make practice easier, you usually make learning worse, because what you removed was the friction the brain was using to encode. The metaphor I like is that the struggle is the *encoding event*. Skip the struggle, skip the encoding. Read correct code instead of writing wrong code; you get the comprehension feeling, you don't get the consolidation.

This is older than LLMs. William James wrote in 1890 that habit is the nervous system's mechanism for converting effortful struggle into automaticity. The thing you do twenty times with attention becomes the thing you do once without it. The mechanism James described in plain English in 1890 is the mechanism cognitive scientists made quantitative in the late twentieth century and the mechanism Bastani measured in a Turkish classroom in 2025. The novelty is the tool that lets you skip the struggle and still produce the artifact. The mechanism is the same one James named.

## The Kosmyna EEG Result

If Bastani measured the gap from the outside — homework score versus exam score — Nataliya Kosmyna's team at the MIT Media Lab measured something close to it from the inside. They ran 54 Boston-area college students through four essay-writing sessions with a 32-channel EEG cap on their heads (Kosmyna et al., 2025). Three groups: brain-only, search-engine, and ChatGPT.

A few words on the apparatus, because the headline number sounds like science fiction. EEG — electroencephalography — measures the electrical activity of the brain through sensors on the scalp. It can't tell you what you're thinking, but it can tell you, in real time, how synchronized different brain regions are with each other. Neuroscientists call this **functional connectivity** — basically, how much different parts of the brain are talking to each other while you do something. More connectivity in the bands associated with effortful thought (the alpha and beta ranges) is, very roughly, the EEG signature of "this person is actually working on the task."

Kosmyna's headline finding: ChatGPT-assisted writers showed **up to a 55% reduction in neural connectivity** compared to brain-only writers (Kosmyna et al., 2025). The "up to" matters — 55% is the maximum reduction across regions, not the average — but the pattern is consistent across measurements. The brain that wrote with ChatGPT was substantially less internally connected than the brain that wrote without it.

Two other findings from the same paper land harder than the connectivity number. First: **83% of ChatGPT users could not accurately quote a sentence from their own essay** minutes after submitting it. Their fingers had typed it, more or less, but the encoding event didn't happen, and the sentence wasn't theirs in the way memory normally makes a thing yours. Second: when researchers had ChatGPT users write a fourth essay *without* the assistant, the reduced connectivity *persisted*. The brain didn't immediately spring back to brain-only patterns when the tool went away. Whatever pattern of engagement they had learned with the assistant in the loop, they kept doing without it.

A caution: this is a preprint. Not yet peer-reviewed. The mechanism — connectivity drop tracking consolidation loss — is suggestive, not proven. Kosmyna's team has not shown that the EEG difference causes the kind of exam-score gap Bastani measured. What they have shown is that, on a brain-activity measure that correlates with engagement during the task, the AI-assisted brain is doing measurably less work. That is a *plausible* mechanism for the Bastani outcome. It is not yet a proven one. I am putting both papers in this chapter because they point the same direction; I am not telling you Kosmyna *explains* Bastani. She might. The study to confirm it has not yet been run.

## The Fluency Trap

Pull the two findings together and you get something I want to name, because it is the thing I am going to keep referring back to.

Bastani gives you the behavioral picture: practice score up, exam score down, gap is large. Kosmyna gives you a plausible internal picture: less neural engagement during the assisted work, less memory of the assisted work after. Bjork gives you the older theory the new data fits inside: easy practice does not produce durable learning; effortful practice does. Stack the three together and the same shape appears in three different measurements.

I'll call it **the fluency trap**.

The trap goes like this. You sit down to do problem two. You ask Claude. Claude gives you a clean solution. You read it, nod, paste it in, move on. The output is fluent — meaning it looks finished, it runs, it satisfies the assignment. *And the act of reading fluent output triggers, in your brain, the warm feeling of comprehension.* You feel like you got it. The feeling is not lying — you really did follow the logic, in the moment, the way you follow a well-explained YouTube video. But the feeling of following is not the same as the act of producing, and your brain encodes the two events very differently. Producing wrong code and fixing it is an encoding event. Reading correct code is not. The first builds the skill. The second simulates the experience of having the skill while leaving the skill itself unbuilt.

The cruel part is that the fluent path *feels better* than the effortful one. It is not just easier; it is more pleasant. The brain reports "I am learning" while the cognitive events that constitute learning are not happening. Bjork and Bjork call this the **illusion of competence** (Bjork & Bjork, 2011). You feel competent because the output in front of you is competent. The output's competence is borrowed from the model. Your competence is not measured by what is on the screen. It is measured by what happens when the screen is closed.

<!-- → [DIAGRAM: The fluency trap — a simple two-path diagram. Path A: struggle → consolidation → durable capability. Path B: delegate → fluent output → no consolidation → atrophy. Editorial style. No color.] -->

The diagram is the simplest way to hold this in your head. Same problem at the top. Same artifact at the bottom — both paths produce a working solution. The middle is different. One path runs through *you*; one path bypasses you. The visible outputs are nearly identical. The invisible outputs — the version of you that exists six weeks later — are not.

## The Three Low-Scoring Patterns

The next study I want to bring in is the Anthropic one, because it is the programming version of Bastani's math result and because it identifies the specific *patterns* that produce the gap.

In January 2026, Jane Hsieh Shen and Alex Tamkin published an RCT with 52 junior Python engineers learning a library called Trio — an asynchronous library none of the participants had used before (Shen & Tamkin, 2026). This is an arXiv preprint, not yet peer-reviewed, which I'm flagging so you can calibrate. The setup was Bastani-shaped: some engineers used Claude during the learning task; some didn't. Afterwards, everyone took a comprehension assessment with no AI. The headline number: **AI-assisted engineers scored about 17% lower on comprehension** than the unaided coders, even though they finished tasks slightly faster. Same pattern as Bastani. Different domain. Different study population. Convergent result.

What makes the Anthropic paper useful beyond replication is that the researchers transcribed the interactions and clustered them. They found that the engineers who scored worst — average comprehension under 40% — fell into three distinct patterns of how they used the assistant. The students who scored best — 65%+ — did not. The patterns are worth memorizing, because once you can name them you can catch yourself doing them.

**1. AI Delegation.** The engineer hands the whole task to Claude, accepts the output, and moves on. No interrogation. No "why did you do it this way?" Just paste and ship. This is the cleanest match for what D was doing in Mr. K's class. It is also the pattern that produced the lowest comprehension scores in the Anthropic study.

**2. Progressive AI Reliance.** The engineer starts the task themselves, hits a hard part, asks the assistant for help, and from that point on the assistant is doing most of the cognitive work. They are not handing the whole thing over up front — that would feel like cheating to them — but once the assistant is in the loop, they never pull it back out. Each subsequent problem starts with "let me just ask Claude." The reliance is *progressive* — it grows session over session, problem over problem, without the engineer noticing.

**3. Iterative AI Debugging.** The engineer writes some code, doesn't understand why it's not working, pastes the error into Claude, accepts whatever fix Claude suggests, runs it again, pastes the next error, and continues. The loop produces working code eventually. It does not produce a person who can debug. The engineer never forms a model of *why* the code is broken — they form a model of *which error messages Claude can fix.*

All three patterns share a feature: at no point does the engineer have to construct the model of the system inside their own head. Claude is doing that for them. Once you can see the three patterns, you can see them in other people's screens, and you can see them in your own. Mostly your own.

The contrast group — the engineers who scored over 65% on the comprehension test — used the assistant differently. The Anthropic researchers called the pattern **conceptual inquiry**. Those engineers asked Claude questions about the library before asking it to write code in the library. They used the model as something between a tutor and a colleague who'd read the docs. They generated their own implementation and then asked Claude to critique it. The output looked like the same Claude window the delegation group was using. The *moves* were different. The same instrument played two ways.

## The Debugging Gap

There is one more piece of the Anthropic result that I want to call out because it lines up with what I see in AP CS.

The largest performance disparity between the AI-assisted engineers and the unaided ones was not on "write code that does X." It was on **diagnostic** questions — the kind that ask *why is this code behaving this way?* or *what would happen if we changed this line?* Production tasks showed a modest gap. Diagnostic tasks showed the gap that made the result publishable (Shen & Tamkin, 2026).

This is the part that, when I read it, made me sit up. Because it explains a thing I have watched happen.

D, in Chapter 0, can produce code that works. He can hand in a 100. What D cannot do — and what the kid in my brother's calculus class cannot do, and what M with the React app cannot do — is *look at code that isn't working and figure out why.* Building, when you ask Claude, is a problem Claude solves for you. Debugging is a problem you have to solve yourself, because debugging requires a model of what the code is supposed to be doing, and Claude doesn't have your model. It only has the code.

This is the cleanest signature of borrowed capability. Anything Claude can do for you, Claude can keep doing for you, and you don't notice that the skill is hollow. Diagnostic tasks make it impossible to hide, because diagnostic tasks require an internal model and an internal model is exactly what delegation doesn't build. You can fake "write me a function." You can't fake "tell me why this function is broken." The asymmetry is the test.

In a way, the AP CS quiz is a diagnostic task. Mr. K isn't asking the kids to *produce* something on the quiz; he's asking them to demonstrate that they have an internal model of what loops and conditionals do. D's homework score and quiz score are the production-versus-diagnostic gap from the Anthropic paper, in miniature, in a Tuesday classroom in our town.

## A Tale of Two Students

Two students in the same AP CS class. They get the same database project. The assignment: design a schema for a small library inventory system. Books, authors, checkouts, due dates.

*This is a composite. I am drawing on what I have watched and what the Bastani and Anthropic papers describe. The specific kids are not specific kids. The pattern is documented.*

Call them Student A and Student B. Same teacher, same starting knowledge, same six weeks of class up to this point. Both submit working schemas. Both get an A. A reasonable observer looking at the gradebook would say they had identical educations that week.

Student A starts the project by opening a notebook. She writes down what she thinks the entities are: books, authors, checkouts. She draws arrows between them. She gets it wrong — at first she has authors as a column on the books table, which makes co-authored books impossible. She notices the problem when she tries to imagine adding a Pratchett-Gaiman collaboration to her draft schema and realizes she'd need two rows for one book. She rewrites it with an authors table and a join table for the many-to-many. She types it into the SQL editor by hand. She runs the migration. The first migration fails because she forgot a foreign key. She reads the error, finds the missing key, fixes it, runs it again. She submits.

Student B opens Claude. He pastes the assignment text in. He asks Claude to generate a schema for a library inventory system. Claude produces a clean schema with a books table, an authors table, a join table, foreign keys, indexes on the columns you'd expect. Student B reads the schema. It looks reasonable. He pastes it into his SQL editor. The migration runs first try because Claude already accounted for the foreign keys. He submits.

Both get an A. The artifacts are roughly equivalent — Claude's schema is probably slightly better, actually, because it includes an `is_deleted` soft-delete column Student A didn't think of.

Six weeks later there is a quiz. The quiz question: *here is a small social-media-style application. Design the schema.*

Student A reaches for her notebook. She writes down entities. Users, posts, comments. She thinks about whether a comment is a post — for now she'll keep them separate. She thinks about likes. A like is a relationship between a user and a post, so it's a join table. She is doing, on a different domain, the same moves she did six weeks ago. The library project did not give her the schema she needed for the quiz. It gave her *the habit of producing schemas*. She produces one.

Student B reads the question. He thinks: I know a schema looks like a bunch of tables with foreign keys between them. He writes down "Users table, Posts table" and stops. He doesn't know whether comments should be a separate table or a column. He doesn't have a model of when to make something a table and when to make it a column. He knows what the *output* of that decision looks like — he saw one six weeks ago — but he never *made* the decision, so he doesn't have it. The notebook in front of him has two words on it when the quiz ends.

Same final grade six weeks ago. Different person now. One can design a database. One can describe what a database looks like. The grade was identical. The cognition was opposite.

The Anthropic researchers would call what Student B did *AI Delegation*. Bjork would call the missing thing the *generation effect*. James would say her nervous system never grooved the pathway, because the pathway wasn't traversed. Bastani would say: this is what the exam measures, and we measured it.

You already knew all this in your bones. The point of the chapter is to give you the numbers.

## Start with Questions Before Code

Here is the practical move, which I am putting at the end of the chapter because the rest of the chapter has earned it.

Before you ask Claude to *build* something in a domain, ask Claude *questions* about the domain. Spend ten minutes — really ten minutes, by the clock — interrogating the assistant about the codebase, the library, the problem space, the API. Ask: *what does this class do? what does this function expect? what are three ways someone could mess this up? what would change if I removed this dependency?* You are not asking Claude to write anything. You are using Claude as the documentation it half-is.

Two things happen when you do this. First, you start to form an internal model of the territory before any code gets written — which is the move the Anthropic conceptual-inquiry group made, and which is exactly what the delegation, progressive-reliance, and iterative-debugging groups skipped. Second, and almost as important, you learn *the boundary*: what Claude can answer accurately and what it confabulates about; what it knows about your codebase and what it pretends to know. Knowing the boundary is the part that doesn't transfer from generic prompt-engineering tips. You only learn it by asking and checking.

The discipline is: *questions before code.* Every time. For ten minutes. Even when you're sure you know what you want, especially when you're sure. The reps you build in the questions phase are the reps that turn up later on the diagnostic question you can't fake. They are the encoding events Bjork was talking about. They are the practice the Bastani control group was getting without realizing it was practice.

This is not the whole conducting discipline. The whole discipline is the rest of the book. But this is the first move, and it is the move you can start doing tomorrow without learning anything else. If you take one behavior away from this chapter, take that one. Open Claude. Don't ask for code. Ask for the shape of the problem. Then put the code on your own keyboard.

## Exercises

**1. (Apply)** Identify three recent AI interactions of your own — homework problems, build sessions, debugging episodes, whatever. For each one, in writing: did you *build* the capability or *borrow* it? Use the test: a week from now, with no AI, on a similar problem, could you reproduce the move? Be honest. The exercise is worthless if you grade yourself on the curve.

**2. (Analyze)** Two assignment transcripts are provided in the companion materials. Each shows a student working through the same algorithms assignment with Claude. For each transcript, identify which Anthropic pattern (AI Delegation, Progressive AI Reliance, Iterative AI Debugging, or Conceptual Inquiry) the student is exhibiting, and quote two specific lines as evidence. Then predict — in one sentence — how each student will do on the unassisted quiz. Defend with reference to a finding from this chapter.

**3. (Evaluate)** Design one rule for your own AI use that would prevent the homework/quiz gap in your next unit of school. The rule has to be specific enough that someone watching your screen could tell whether you followed it. "Use AI less" is not a rule. "Spend ten minutes asking questions before requesting code on every new assignment, and write the first attempt by hand even if Claude could one-shot it" is closer to a rule. Write your rule. Pin it somewhere you'll see it. We'll revisit it in Chapter 14.

## 🕰️ AI Wayback Machine

**William James (1842–1910)** was the founding figure of American psychology and the author, in 1890, of *The Principles of Psychology*, a two-volume textbook so well-written it is still on philosophy syllabi. The chapter that matters for this book is Chapter IV, "Habit." James argued that the nervous system is a plastic material that physically reshapes itself in response to repeated effort: what you do twenty times with attention becomes, eventually, what you can do once without it. Habit, for James, is the mechanism by which struggle now becomes capability later — a 19th-century, jargon-free description of consolidation, eight decades before anyone could measure it with an MRI. He warned against breaking new habits, against half-attention, against acting as if the cost of a single shortcut were paid only once. His advice — *make our nervous system our ally instead of our enemy* — is the advice this chapter is restating in the language of randomized controlled trials. Bastani measured the exam scores of students whose nervous systems were not made allies. James named the mechanism in 1890. The mechanism has not changed (James, 1890).

**Run this:**

```text
In the voice of William James, circa 1890, explain to a
high-school AP CS student why doing the work yourself —
even badly, even slowly — makes a more permanent change
than reading correct code Claude produced. Use the language
of habit and the nervous system. Do not mention computers.
One paragraph.
```

## Bridge

The reader knows the risk. They don't yet know which specific cognitive capacities are at stake — or that Claude is structurally unable to supply them.

---

**Links:** [boondoggling.ai](https://boondoggling.ai) · [irreducibly.xyz](https://irreducibly.xyz)

**References (in-chapter citations):**

- Bastani, H., Bastani, O., Sungu, A., Ge, H., Kabakcı, Ö., & Mariman, R. (2025). Generative AI without guardrails can harm learning: Evidence from high school mathematics. *Proceedings of the National Academy of Sciences*, 122. (See 2025 correction, PubMed 40833419.)
- Bjork, E. L., & Bjork, R. A. (2011). Making things hard on yourself, but in a good way: Creating desirable difficulties to enhance learning. In M. A. Gernsbacher et al. (Eds.), *Psychology and the Real World*. Worth.
- James, W. (1890). *The Principles of Psychology*, Vol. I, Chapter IV: Habit. New York: Henry Holt.
- Kosmyna, N., et al. (2025). Your brain on ChatGPT: Accumulation of cognitive debt when using an AI assistant for essay writing task. *arXiv:2506.08872* (preprint, not peer-reviewed).
- Shen, J. H., & Tamkin, A. (2026). How AI impacts skill formation. *arXiv:2601.20245*, Anthropic (preprint, not peer-reviewed).
