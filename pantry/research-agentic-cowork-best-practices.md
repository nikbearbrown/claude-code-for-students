# Research Note: Best Practices for Using Claude Agents with Cowork

**Research date:** 2026-06-05  
**Relevant books:** `claude`, `claude-agentic-ai`, `claude-code-for-students`, `claude-code-for-teachers`, `claude-cowork`, `claude-for-education-a-practitioners-guide`, `claude-prompt-engineering`

## Scope

This note summarizes current best practices for using Claude agents, Claude Code subagents, and Cowork-style repository workflows. Here, "Cowork" means a collaborative work loop where an AI agent can inspect files, edit artifacts, run checks, maintain task context, and hand results back to a human or another agent.

The central design principle is simple: agents are most useful when they can act in the work environment, but they are most reliable when the task, tools, memory, permissions, verification checks, and stop conditions are explicit.

Before an agent searches the open web or asks a model to infer facts, it should first look for verified local or database-backed evidence: repository files, approved datasets, internal databases, source exports, logs, indexes, metadata, and other maintained project records. External lookup is a fallback for gaps, freshness checks, or questions the local verified data cannot answer.

The same rule applies to execution. Preferred scripts should be tested, vetted, stored, and reused. If a suitable script exists in `scripts/`, `SCRIPTS/`, or another approved project script directory, the agent should prefer that script over creating a new one on the fly. Ad hoc scripts are a fallback for genuinely new work, and useful ad hoc scripts should be promoted into the vetted script library after review.

## Two Customers

Cowork documentation has two customers:

1. **Agents** that read recipes, skills, repo instructions, manifests, and gate definitions in order to perform work correctly.
2. **Humans** who must understand what those agents are doing, why the workflow is safe, where judgment is required, and how to audit or intervene.

Agentic recipes should be written primarily for agents to execute. They need precise inputs, allowed tools, required reads, step order, tests, handoff conditions, and stop rules. Each recipe should also begin with a short human-readable executive summary so a human can quickly understand purpose, risk, and expected outcome before letting the agent run it.

Skills and the overall Cowork system should be compiled into human-readable documentation. Humans should not need to inspect every operational recipe to understand the architecture, available skills, safety model, phase gates, data sources, and escalation paths.

## Source-Grounded Takeaways

Anthropic distinguishes predictable workflows from more autonomous agents. Workflows follow predefined paths; agents dynamically choose processes and tool use. The practical implication for Cowork is to start with the simplest reliable workflow and move to autonomous agents only when the task needs flexible exploration, many tool calls, or uncertain intermediate steps.

Claude Code guidance emphasizes that agent performance is constrained by context. Every file read, command output, pasted artifact, and correction competes for attention. Good Cowork practice therefore treats context as a scarce resource: clear it between unrelated tasks, compact long sessions, delegate noisy investigations to subagents, and preserve durable project facts in memory rather than in long chat histories.

Claude Code subagents are specialized assistants with separate context windows, specific prompts, tool access, permissions, memory, and optional skills. They are most useful for side investigations, code review, security review, debugging, migration audits, and other tasks that would otherwise fill the main session with search results or logs.

Anthropic's Claude Code best-practice guidance repeatedly returns to verification: give Claude tests, expected output, screenshots, lint/build commands, scripts, or acceptance criteria it can run. Without a check, the agent stops when work looks plausible; with a check, it can iterate against ground truth.

For education, Anthropic frames Claude as a thinking partner rather than an answer machine. In student and teacher Cowork workflows, the agent should scaffold, question, review, and help debug, while preserving the learner's authorship, reflection, and ability to explain the work.

## Practical Best Practices

1. Define a tight task contract.
   State the goal, target files or folders, allowed sources, output format, done criteria, and what the agent must not touch.

2. Check verified local or database data first.
   Start with repository files, approved datasets, internal databases, logs, source exports, metadata, and maintained indexes. Search externally only when verified local data is missing, stale, incomplete, or explicitly insufficient for the question.

3. Prefer vetted stored scripts over ad hoc scripts.
   Check `scripts/`, `SCRIPTS/`, and approved project script directories before writing one-off code. Use existing tested scripts when they fit the task. Create temporary scripts only when no vetted script exists, and consider promoting reusable temporary scripts into the stored library after review.

4. Choose workflow before autonomy.
   Use a direct prompt or fixed workflow for repeatable tasks. Use an autonomous agent when the task needs open-ended exploration, planning, tool use, and adaptation.

5. Use subagents for contained work.
   Delegate codebase investigation, edge-case review, source gathering, test diagnosis, or adversarial review to subagents so the main Cowork session stays focused.

6. Give each agent a role and a boundary.
   A good agent spec names when to use it, what it reads first, which tools it may use, what evidence it must return, and when it should stop.

7. Keep memory concise and operational.
   Put stable project facts in `CLAUDE.md` or repo instructions: build commands, test commands, architecture map, style rules, safety limits, and recurring corrections. Move long procedures into skills or recipe files.

8. Prefer least-privilege permissions.
   Start read-only for research agents. Allow edit tools only for implementers. Scope shell commands to known safe commands when possible. Require human approval for destructive operations, external side effects, sensitive data access, commits, pushes, deployments, grades, or policy decisions.

9. Make verification part of the prompt.
   Tell the agent what to run or compare before declaring completion. For code: tests, lint, typecheck, build, screenshots, sample CLI runs, or diff review. For prose: source checks, citation checks, rubric checks, and consistency checks.

10. Separate exploration, plan, implementation, and review.
   For uncertain or multi-file work, first ask the agent to read and report. Then ask for a plan. Then implement. Then review in a fresh context or with a reviewer subagent.

11. Design documentation for two customers.
   Recipes are primarily executable instructions for agents, with a concise executive summary for humans. Skills and the overall system need compiled human-readable docs that explain what agents do, when to use them, and how to supervise them.

12. Use structured handoffs.
   Each handoff should include objective, files read, files changed, checks run, failures, assumptions, open questions, and next recommended action.

13. Avoid parallel edit collisions.
   Run parallel agents in separate worktrees, separate folders, or read-only review roles. One coordinator should merge decisions.

14. Treat agent outputs as claims until checked.
   Ask for file paths, command outputs, source links, and uncertainty notes. Do not let elegant prose substitute for evidence.

15. Refresh context when the session gets muddy.
   If the agent repeats the same mistake, mixes tasks, or carries failed attempts forward, start a fresh session with a better initial prompt and a short summary of what was learned.

## Cowork Agent Card Template

Use this card before creating a new Cowork agent or subagent.

```yaml
name: short-agent-name
purpose: One sentence describing the work this agent owns.
use_when:
  - The task type is recurring.
  - The work benefits from separate context or specialized review.
inputs:
  - User request or issue
  - Required files, folders, URLs, datasets, or rubrics
tools_allowed:
  - Read/search tools by default
  - Edit tools only if this agent implements changes
  - Specific shell commands only when needed for verification
required_reads:
  - README.md
  - CLAUDE.md or AGENTS.md
  - Verified local datasets, databases, logs, or metadata relevant to the task
  - Existing vetted scripts in scripts/, SCRIPTS/, or approved script directories
  - Relevant chapter, source, or project files
procedure:
  - Restate the objective and constraints.
  - Check verified local or database-backed data before external lookup.
  - Check for an existing vetted script before creating ad hoc code.
  - Inspect only the needed context.
  - Produce a brief plan for non-trivial work.
  - Act in small steps.
  - Verify with the declared checks.
output_contract:
  - Human-readable executive summary
  - Findings or changes
  - Evidence with file paths, commands, or source links
  - Verification results
  - Remaining risks or open questions
stop_conditions:
  - Needed permission is missing.
  - Required source material cannot be found.
  - Verification fails repeatedly for the same reason.
  - The task would require destructive or external action.
```

## Suggested Subagent Skeleton

```markdown
---
name: cowork-reviewer
description: Reviews Cowork agent outputs for correctness, evidence, safety, and missed verification.
tools: Read, Glob, Grep
model: sonnet
---

You are a Cowork review agent. Review the task output against the user's request,
the repository instructions, and the changed files.

Focus on:
- factual correctness
- source and file evidence
- missed tests or checks
- over-broad edits
- security, privacy, or educational integrity risks

Return:
1. blocking issues
2. non-blocking improvements
3. verification performed
4. residual risk
```

## Patterns by Book

`claude`: Introduce agents as Claude systems that combine language, tools, memory, and human feedback. Emphasize when a user should stay in chat versus move into Claude Code or Cowork.

`claude-agentic-ai`: Build the architecture chapter around workflows, agents, orchestrator-worker patterns, evaluator-optimizer loops, tool design, memory, and verification.

`claude-code-for-students`: Teach students to use agents as tutors, reviewers, debuggers, and study partners. Require students to explain decisions, cite what the agent helped with, and verify outputs themselves.

`claude-code-for-teachers`: Focus on assignment design, transparent AI-use policies, classroom agent cards, rubric-based review, and workflows that preserve student authorship.

`claude-cowork`: Make Cowork the operational playbook: task intake, repo scan, agent selection, scoped execution, test/check loop, handoff note, and human checkpoint.

`claude-for-education-a-practitioners-guide`: Connect institutional practice to privacy, access, transparency, academic integrity, student support, faculty training, and responsible deployment.

`claude-prompt-engineering`: Treat prompts as executable specifications for agents. Cover clear roles, context blocks, examples, structured output, verification instructions, stop conditions, and iterative prompt refinement.

## Failure Modes to Teach Explicitly

- Context pollution from too many unrelated tasks in one session.
- Overstuffed memory files that bury important instructions.
- Agents searching externally before checking verified local or database-backed data.
- Agents writing one-off scripts before checking for existing tested scripts.
- Useful temporary scripts never being reviewed, vetted, and stored for reuse.
- Agent-facing recipes with no human-readable executive summary.
- Human-facing system docs that fail to explain available skills, recipes, gates, and escalation paths.
- Agents inventing file state or source claims after insufficient inspection.
- Broad tool access causing accidental edits, external side effects, or unsafe commands.
- Parallel agents editing the same files without coordination.
- "Looks right" completion without tests, citations, or other ground truth.
- Prompt injection through untrusted repository content, web pages, documents, or issue text.
- Educational misuse where the agent replaces the learner's reasoning instead of strengthening it.

## Verification Checklist for a Cowork Run

- The task objective and target scope are explicit.
- Verified local or database-backed data was checked first, or the handoff explains why it was unavailable or insufficient.
- Existing vetted scripts were checked first, or the handoff explains why no suitable stored script existed.
- Agent-facing recipes include an executive summary, and system-level skill/recipe documentation is human-readable.
- The agent read the relevant repo instructions and source files.
- The agent used the smallest adequate tool set.
- A plan exists for multi-step work.
- Changes are limited to the requested area.
- Tests, builds, lint, screenshots, citation checks, or rubric checks were run when relevant.
- The final handoff names files changed, checks run, failures, assumptions, and next steps.
- A human checkpoint exists before destructive, external, sensitive, or high-stakes action.

## Sources

- Anthropic, "Building effective agents" (2024-12-19): https://www.anthropic.com/engineering/building-effective-agents
- Claude Code Docs, "Best practices for Claude Code": https://code.claude.com/docs/en/best-practices
- Claude Code Docs, "Create custom subagents": https://code.claude.com/docs/en/sub-agents
- Claude Code Docs, "How Claude remembers your project": https://code.claude.com/docs/en/memory
- Anthropic Engineering, "Effective context engineering for AI agents" (2025-09-29): https://www.anthropic.com/engineering/effective-context-engineering-for-ai-agents
- Anthropic, "Building agents with the Claude Agent SDK" (2025-09-29): https://claude.com/blog/building-agents-with-the-claude-agent-sdk
- Claude API Docs, "Prompting best practices": https://platform.claude.com/docs/en/build-with-claude/prompt-engineering/claude-prompting-best-practices
- Claude for Education: https://claude.com/solutions/education
