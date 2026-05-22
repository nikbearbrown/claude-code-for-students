<!--
    00-frontmatter.md
    FRONT MATTER — everything that appears before Chapter 1.

    This file contains four sections in order:
      1. Copyright page
      2. Dedication (optional — delete if not using)
      3. Preface

    Do not number these sections. They use roman numerals in print
    and appear before the body in the compiled EPUB.
-->

# Claude Code for Students

**Nik Bear Brown** with **Seth Brown**

---

## Copyright

Copyright © 2026 Nik Bear Brown. All rights reserved.

Published by Bear Brown, LLC.

No part of this publication may be reproduced, distributed, or transmitted
in any form or by any means without the prior written permission of the
publisher, except in the case of brief quotations in critical reviews and
certain other noncommercial uses permitted by copyright law.

ISBN: [INSERT ISBN]

---

## Dedication

<!-- Optional. Delete this section if not using. -->

*[For — ]*

---

## Preface

<!-- The preface is written in the author's voice.
     It answers three questions:
       - Why does this book exist? (the gap it fills)
       - Why now? (what changed that makes this urgent)
       - Why you? (what credentials or experience qualify you to write it)
     It is NOT a summary of the book — that belongs in the Introduction.
     Typical length: 2–5 pages. -->

This book exists because my son came home from AP Computer Science one afternoon and described — without prompting, without any framework yet to describe it — a pattern I had been trying to name for two years.

He had watched a friend ace a problem set by pasting prompts into Claude, accepting whatever came back, and never reading the code. Two weeks later the laptops closed for an in-class quiz and the friend froze. Seth had used Claude on the same problem set. He had passed the same quiz. He had spent more time on the homework, not less. He had done something different *inside the loop* that the other student had not done, and the difference, two weeks later, was the entire grade.

I teach AI engineering at Northeastern University. I have spent the last decade watching graduate students, engineers, and researchers adapt to one wave of capability after another — search, then deep learning, then large language models, now agentic coding tools that write whole applications. The pattern Seth described in AP CS is the same pattern I was watching in my own seminars and at companies that had adopted Claude Code. It has a shape. It is replicable. It produces excellent output and atrophying practitioners on a delay long enough that almost nobody connects the cause to the effect.

That is the gap this book fills. There is excellent documentation for Claude Code — Anthropic publishes it, the developer community improves it daily. There is a growing literature on AI in education that mostly treats students as plagiarism risks to manage. There is, as far as I can tell, no book yet that treats a high-school or early-college student as a *builder to equip* and gives them an operational discipline they can apply on Monday morning to a real Claude Code session and protect their own capability while shipping real work.

This is that book. The discipline has a name — *conducting* — and an operational form. Chapter by chapter the book teaches the discipline against real Claude Code builds: planning, specification, the handoff conditions where things go silently wrong, verification, and finally a full build the reader conducts end to end.

A note on voice. The book is co-written. My son **Seth Brown** is the co-author. Seth is a high-school senior in Troy, Missouri, and a self-taught game developer across five engines. He has shipped a co-op horror survival game in Godot 4 (*Haunt & Harvest*, migrated system-by-system from Unreal Engine), a Roblox/Luau horror game with a cinematic intro and modular networked architecture (*Midnight Fuel*), a mobile arcade game on Google Play with AdMob integration (*Bubble Pop*), and a Next.js platform where he writes weekly on the design of horror mechanics and player psychology (*Zebonastic*).

The deeper credential is operational. Seth and I co-built two production AI agents that operationalize the discipline this book teaches — *Walker*, a Unity refactoring conductor that enforces the five-phase audit-restructure-CLAUDE.md-refactor-verify model and names the five supervisory capacities by initial; and *Zelda*, a senior game-design-documentation consultant with a 34-command library, a phase-gate enforcement layer, and a seven-failure-mode audit pass. Both are public at [humanitarians.ai/tools](https://www.humanitarians.ai/tools/). The books teach the abstraction. Walker and Zelda are the worked artifacts that prove the abstraction holds up under production pressure. A high-school co-author who has co-architected two agents at this level of rigor is a different co-author than one who simply observed a pattern.

The narrative chapters are in Seth's voice. The framework chapters are in mine. The voice shifts on purpose. A framework written only by an adult about how students should behave has one shape. A framework developed by a practitioner working on real software and real AI tooling, with an adult helping articulate the structure, has a different shape — and a different authority.

The book does not cover Claude Code's full API surface or the deep internals of how the model reasons. It does not argue whether students should use AI; they will, the question is how. It does not pretend the discipline is complete; the final chapters say exactly what would change our minds.

If you are a student opening this book, you do not need to take its claims on faith. The discipline is empirical. Try it on one build. Compare the result, and compare what you can do without the tool a month later, against the version of yourself who would have produced the same artifact with no discipline at all. The difference is what the book is for.

— Nik Bear Brown and Seth Brown
Boston, Massachusetts and Troy, Missouri
May 2026
