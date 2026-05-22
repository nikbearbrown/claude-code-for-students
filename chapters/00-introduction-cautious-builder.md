# Chapter 0 — Introduction: The Cautious Builder

*Meet Seth. He noticed something his friends didn't.*

---

**Learning outcomes**

1. **(Remember)** Name the difference between borrowing capability and building it.
2. **(Understand)** Explain why a high homework grade with a low quiz grade is a signal, not a coincidence.
3. **(Understand)** Describe what "conducting" Claude Code means vs. letting it run.

---

The first time I really paid attention, it was a Tuesday and we had thirty-five minutes left in AP CS.

Mr. K had given us a problem set — six functions, the kind where you're supposed to think about loop invariants and edge cases, the kind he tells us are practice for the quiz on Friday. I was on problem two. The kid two seats over — I'll just call him D, because everyone in this book except me is going to stay nameless — was already on problem six. Not because he's smarter than me. He isn't. He's about my speed when we do whiteboard work. But on the laptop, with Claude open in a side tab, D moves like he's flipping pancakes. Thirty seconds per problem. Sometimes less.

I watched him do problem four. He pasted the prompt from the assignment into Claude, copied the response into his editor, ran it, saw it pass the test cases, and moved on. He didn't read the code. I'm not exaggerating to make a point — he genuinely did not read it. His eyes went from the green checkmark to the next problem statement. The function he had just "written" scrolled off the top of his screen and he never looked at it again.

He turned in the assignment that night. He got a 100.

Two weeks later we had the quiz. Closed laptops. Mr. K stood at the front of the room and watched us. The quiz had three problems. They were *easier* than the homework — that was the whole point, Mr. K told us afterward, he calibrates the quiz to be a notch below the practice set so that anyone who actually did the practice should sail through. I sailed through. D didn't. D stared at problem one for about twelve minutes. I could see his pencil hovering. He wrote a `for` loop, crossed it out, wrote a `while` loop, crossed it out, and then sat very still until the bell rang.

He got a 41.

Here's the thing that bothered me, and the thing this whole book is going to be about: D wasn't cheating. Not in any way our school's honor code recognizes. Claude was an allowed tool on that homework — Mr. K said so. D used the allowed tool. He got the grade the tool helped him get. The tool was *good at coding*. D was *good at using the tool*. And then on the day the tool wasn't in the room, D was not good at coding.

I want to be precise about how that felt to watch, because the feeling is the whole reason I started paying attention. When I saw D's score on the quiz, the obvious explanation was that he must not have studied. Except he had — I knew he had, because we'd been in the same group chat the night before and he'd been the one explaining the rubric to other kids. He had spent more time on AP CS than I had that week. He had done the practice. He had done it on a screen with Claude open, but he had *done it*. He'd put in the hours. The hours just didn't accrue.

That's a strange sentence to write down. *The hours didn't accrue.* You spend the time, you produce the output, you get the practice rep — and somehow when you reach for the skill later it isn't there. It's like running on a treadmill where the floor moves backward at exactly your pace, and then noticing, an hour in, that you haven't actually gone anywhere. Sweat is real. Distance is zero.

I didn't have a word for it yet. I just knew something was off.

## What I Noticed

I started watching. Not in a creepy way — I just started paying attention to a pattern I had been ignoring for a year. It was everywhere once I looked.

There was M, who built a whole React app for the science fair and could not, when I asked her, explain why she used `useEffect` instead of `useMemo` on one particular hook. There was J, who got a near-perfect grade on the AP CS Principles Create Task and then bombed a midterm question that asked him to *describe* what his own code did. There was a kid in my brother's grade who turned in a calculus project with proofs in it, beautiful proofs, and then could not differentiate $x^2$ on the board.

The pattern wasn't "AI made people dumb." That's a dumb pattern. The pattern was more specific and more interesting:

> Performance on the assisted task went up. Performance on the *unassisted* task — same skill, same content, just no tool — went down.

It wasn't even that the unassisted score went down compared to the assisted one. That would be obvious, and not very interesting; of course you do worse without a tool than with it. The thing that bothered me was that the unassisted score for kids who used AI heavily was *lower than the unassisted score of kids who hadn't used AI at all*. The tool didn't just fail to teach them. It looked like it was *unteaching* them.

I want to be careful here, because I'm a high-school student and I'm not running studies. But the studies exist. A team at Wharton ran a field experiment in 2025 with about a thousand Turkish high-school students learning math with GPT-4 in the room; the kids with unrestricted access scored *worse* on their unassisted exam than kids who had never used the tool, even though they had crushed the assisted practice (Bastani et al., 2025). Researchers at Anthropic ran a randomized trial with engineers learning a new Python library and found a similar gap (Shen & Tamkin, 2026). I'll get into the actual numbers in Chapter 1 — they're sharper than you'd guess. For now I'm only flagging that what I noticed in Mr. K's class is the same thing the researchers were noticing in their experiments. I'm not making this up. Neither, it turns out, is D.

## Borrowing vs Building

Here is the cleanest way I've found to say what's happening.

When D pastes the problem into Claude and copies the answer back, he is *borrowing* a capability. The capability — solving the problem — exists in the room. It happens. Code gets written, tests pass, the homework gets a grade. But the capability lives in Claude. D is renting it for the thirty seconds it takes to copy and paste.

When I do problem two by hand, slowly, getting it wrong twice before I get it right, I am *building* a capability. It's slower. It's more annoying. The grade is the same — or sometimes worse, because I was on problem two when D was on six. But two weeks later, when the laptops close, the capability is in *me*. It rode home in my head.

Borrowing and building feel the same in the moment. That's the part that messes with you. When the answer is on your screen, you *can read it*. You can follow it. You can nod along. Your brain is producing the warm familiar sensation of "yes, I get it." And that sensation is — and this is the part I had to learn the hard way — almost entirely uncorrelated with whether you could reproduce the same thing tomorrow without help. Reading code is not writing code. Watching someone shoot free throws is not shooting free throws. You already knew this about basketball. You probably did not know it about programming, because programming *looks* like reading, and reading *feels* like learning.

There's a researcher named Yiran Fan who has a phrase for what's happening on the inside when students offload too much to the model: she calls it "metacognitive laziness" (Fan et al., 2024). The idea is that the work of *monitoring your own thinking* — am I getting this? do I see why it works? what would I do if the test cases changed? — is precisely the work students stop doing when the model is in the loop. The output is fine. The monitoring is gone. And the monitoring is the part that turns into a brain you can take to a quiz.

So here is the line I'm going to be drawing for the rest of this book. It is a line between two things that *look identical from the outside* and are *opposite on the inside*:

- **Borrowing.** Claude produces the artifact. You watch. The artifact passes. You move on. Your future self is no different than your past self.
- **Building.** Claude produces *something*, but you are the one deciding what to ask, what to keep, what to reject, what to verify. The artifact passes *because you understood it well enough to defend it*. Your future self is different than your past self.

Same screen. Same grade, often. Opposite outcome.

<!-- → [DIAGRAM: Seth's arc from observer to practitioner — simple two-point timeline showing "watches friends" → "builds the discipline". Minimal. Editorial style. No color.] -->

## What Conducting Means

The word I'm going to use for the thing I'm trying to learn — the thing this book is trying to teach — is *conducting*.

I don't mean it as a metaphor for being bossy. I mean it the way you mean it when you talk about an orchestra. A conductor doesn't play any of the instruments. A conductor isn't faster than the violinists or louder than the brass. The conductor's job is to know what the piece is supposed to sound like, decide which section comes in when, hear when something is off, and stop the rehearsal until it's right. The conductor is the *only* person in the room who isn't producing sound. Everyone else is producing sound. The conductor is producing the *shape* of the sound.

Claude Code — and I'm going to introduce Claude Code properly in Chapter 2, so don't worry if you've never seen it; for now just picture Claude with hands, the version that lives in a terminal and edits real files in a real project — Claude Code can produce sound. Lots of sound. Files, tests, refactors, commits, whole apps. It plays many instruments at once. What it cannot do, and what nobody has figured out how to make it do reliably, is decide what the piece is supposed to sound like, decide which section comes in when, hear when something is off, and stop the rehearsal until it's right.

That part is the human's job. It is the only part that is the human's job. And it is the part that, once you learn it, you keep — even when you walk away from the laptop. *Especially* when you walk away from the laptop. Because that part is not borrowed; it is built.

The whole book is going to be about how to do that part on purpose. There's a name for one specific version of the failure mode — what happens when you stop conducting and just let the orchestra play — but I'm not going to tell you that name yet, because the name is in Chapter 4 and you'd cheat yourself out of figuring out the feeling first. There's also a set of capacities — five of them, roughly — that researchers and practitioners are starting to formalize as the *human-only* parts of the loop, and those come in Chapter 5. For now, the only word you need is *conducting*, and the only image you need is the conductor on the podium, who is the most important person in the room and is not making a sound.

Anthropic's own engineering team has written, in their public guidance, that the human's job in agentic coding workflows is to "set the goal, verify the result, and intervene in the middle when it matters" (Anthropic, 2026). That's a definition of conducting written by the people who built the orchestra. They agree with me. Or I agree with them. Probably them with me — they thought of it first.

I want to flag something subtle here so you don't miss it. *Conducting is work.* It is harder than letting the orchestra play, not easier. When D pasted problem four into Claude and copied the answer back, he was doing less work than I was on problem two. He was getting more output for less effort, by every visible metric. The cost was invisible, because the cost was the practice rep he didn't take. You only see the bill when the laptops close. Most school doesn't make the laptops close; some school does; the AP exam definitely does; and life, eventually, mostly does. The cost is real, but it shows up on a delay. That delay is what makes this whole thing hard to notice in the moment, and it is also why I needed to write down what I saw. If the cost arrived on time, I wouldn't need a book. D would just know.

## How To Read This Book (Seth's voice and yours)

A note about how this book works, because it's a little weird.

I'm Seth. I'm a high-school student. I'm going to be telling this book in my voice through Chapter 3, and then again in Chapters 11 and 12, and then handing the wheel to you for Chapters 13 and 14. In the chapters in between, my dad — Nik — takes over the narration, because those chapters are about frameworks (the conducting discipline, the supervisory capacities, the planning method, the build sessions) and my dad teaches that stuff for a living. I don't. I'm the one who lived it. He's the one who can explain it. That's the deal.

So when you read me, you're reading what I noticed and what I tried. When you read him, you're reading what I noticed *with the structure underneath it spelled out*. The voice will shift. You'll feel it. That's on purpose. Don't worry about which one is "the textbook" voice — they're both the textbook. The book is the conversation between them.

Two more things while we're here.

**This is not an AI ethics book.** I am not going to tell you whether you should use AI. You are going to use AI. Your school is going to use AI. Your future job is going to use AI. The question is not whether. The question is how, and the question of how is a *capability* question, not an ethics question. If you came here for the morality lecture about cheating, I am the wrong narrator and this is the wrong book.

**This is also not a prompt-engineering book.** I am not going to teach you ten tricks to get better outputs from Claude. There are a thousand of those online and most of them will be wrong in six months anyway. What I'm going to teach you — what my dad is going to teach you in the middle chapters — is *the part of the work that doesn't change* when the model changes. The conducting part. The part that's in you, not in the tool.

Read the chapter. Do the exercises. Especially do the exercises — they are short on purpose. The book is laid out so that by Chapter 14 you'll have built something real, in public, with Claude Code, and you'll be able to point at the file and say *I did that*, and mean it. Not because Claude didn't help — Claude helped enormously — but because you can defend every line.

That's the whole game. That's the line I'm trying to draw, the line between borrowing and building. D didn't draw it. I'm still learning to. Let's draw it together.

## Exercises

**1. (Remember)** Before reading Chapter 1: write down three things you have built with AI in the last month. For each: could you explain every decision in it to someone who has never seen the code?

**2. (Understand)** What is the difference between using a calculator and learning arithmetic? Write one paragraph. Keep it. You will return to it in Chapter 14.

**3. (Understand)** Name one thing Seth noticed that you have also noticed in your own class. One sentence.

## 🕰️ AI Wayback Machine

**Norbert Wiener (1894–1964)** was a mathematician at MIT who, after watching anti-aircraft gunners try to predict where a plane would be by the time the shell got there, became convinced that the interesting thing about a tool was not what the tool did but what happened in the *loop* between the tool and the person using it. He named this study *cybernetics*, from the Greek for *steersman* — the person on the boat who doesn't row but decides where the boat goes (Wiener, 1948/1961). Two years later he wrote a book for non-mathematicians called *The Human Use of Human Beings* in which he argued that automation would be morally neutral as a technology and morally consequential as a *use*; the question worth asking, he said, is not what the machine can do, but what the machine does to the person who uses it (Wiener, 1950/1954). That is exactly the question this chapter is asking about D and Claude. Wiener died in 1964. He never saw a laptop. He saw the shape of this argument anyway.

**Run this:**

```text
Pretend you are Norbert Wiener in 1950. I am a high-school
student with Claude Code on my laptop. In one paragraph, what
should I watch for in my own use of it? Don't tell me whether
to use it. Tell me what to notice.
```

## Bridge

The feeling Seth has is real. Chapter 1 gives it a name, a number, and a neurobiological mechanism.

---

**Links:** [boondoggling.ai](https://boondoggling.ai) · [irreducibly.xyz](https://irreducibly.xyz)
