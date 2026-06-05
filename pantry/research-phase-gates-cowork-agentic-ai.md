# Research Note: Phase Gates for Cowork and Agentic AI Pipelines

**Research date:** 2026-06-05  
**Local context:** Gru / Boondoggle Score pasted spec, especially `/v0`, `/claude`, explicit handoff conditions, supervisory capacities, and the rule that no step should run until its prerequisite has a testable exit condition.  
**Relevant books:** `claude`, `claude-agentic-ai`, `claude-code-for-students`, `claude-code-for-teachers`, `claude-cowork`, `claude-for-education-a-practitioners-guide`, `claude-prompt-engineering`

## Core Claim

Cowork and agentic AI pipelines should not become fully automated because a model can complete a sequence of tasks. They should become automated only after each phase has a named gate, an explicit test, a recorded result, and a failure path.

The right mental model is continuous delivery applied to cognition: fast automated execution is valuable only when quality gates, approvals, rollback paths, and evidence logs are built into the flow. In Cowork, the artifact being promoted is not only code. It may be a problem statement, data claim, design decision, prompt, schema, chapter draft, lesson plan, test suite, or deployment candidate.

## Two Customers for Gate Documentation

Phase-gated Cowork systems have two customers:

1. **Agents** that read recipes, skills, manifests, and gate definitions in order to execute work.
2. **Humans** who must understand what the agents do, what each gate proves, where risk remains, and when human judgment is required.

Recipes and gate manifests are primarily agent-facing. They should be precise enough for an agent to execute: entry conditions, allowed tools, verified-data requirements, exit tests, evidence artifacts, failure paths, and promotion rules. But every recipe should begin with a short human-readable executive summary that explains purpose, expected outcome, risk level, and the condition under which the recipe is safe to run.

Skills and the overall Cowork system should be compiled into human-readable documentation. A human operator should be able to understand the available skills, recipes, phase gates, automation levels, approvals, data sources, and rollback model without reading every low-level agent instruction.

The system should treat scripts the same way it treats preferred data. Preferred data is vetted and stored; preferred scripts should also be tested, vetted, stored, and reused. If a suitable script exists in `scripts/`, `SCRIPTS/`, or another approved project script directory, the gate should prefer that script over letting an agent create a new script on the fly.

## Why Phase Gates Matter for Agents

Agentic systems create a compounding-error problem. An agent can make a plausible but wrong assumption in step 1, then use that assumption as the foundation for steps 2 through 20. A traditional CI/CD pipeline catches some of this with tests and quality gates. A Cowork pipeline needs the same discipline earlier in the reasoning chain.

Phase gates convert vague supervision into a testable promotion rule:

- The agent may not move from formulation to planning until the thing being built is named.
- The agent may not move from planning to implementation until dependencies and verified local data are checked.
- The agent may not move from implementation to automation until a small manual or semi-automated run has passed.
- The agent may not move from automation to production-scale execution until tests, review, observability, and rollback are in place.

This matches the Gru / Boondoggle principle: every Claude task needs a handoff condition before the next step begins. A handoff condition is a phase gate in miniature.

## Source-Grounded Findings

Anthropic's agent guidance distinguishes workflows from agents and recommends the simplest reliable system before adding autonomy. For open-ended agents, it emphasizes environmental ground truth, human feedback checkpoints, stopping conditions, and sandboxing because agent errors can compound.

Claude Code best practices emphasize giving Claude verification criteria: tests, builds, screenshots, expected outputs, or other checks it can run. They also recommend subagents for investigation and verification, checkpoints/rewind for recovery, and separate sessions or worktrees for parallel work.

Anthropic's agent-evaluation guidance notes that multi-turn agents are harder to evaluate because they use tools over many turns, modify state, and adapt as they go. Production teams build evals around dimensions such as not breaking things, doing what was requested, quality, regression testing, browser testing, static analysis, and LLM judges calibrated by humans.

Claude's multi-agent guidance notes that explicit verification checkpoints remain valuable when verification requires specialized tools, independent review, or enforceable workflow boundaries.

CI/CD practice provides the operational analogy. Continuous delivery chains build, test, and deployment steps into one release workflow, while continuous deployment requires stronger discipline because there is no final human intervention. GitLab deployment approvals block protected-environment deployments until required approvals are granted. SonarQube-style quality gates block promotion when predefined criteria fail, such as coverage, reliability, security, maintainability, duplication, or unresolved security hotspots.

## Phase Gate Pattern

A phase gate has seven parts:

1. **Entry condition:** What must already be true before this phase starts.
2. **Allowed work:** What the agent may do inside the phase.
3. **Verified data rule:** Which local files, databases, datasets, logs, metadata, or approved sources must be checked before external lookup or model inference.
4. **Verified script rule:** Which tested and stored scripts must be preferred before any ad hoc script is created.
5. **Exit tests:** The explicit checks that must pass.
6. **Evidence artifact:** The file, log, report, diff, screenshot, score, or citation list proving the checks were run.
7. **Approver or verifier:** Human, CI job, read-only subagent, reviewer agent, rubric, or automated test harness.
8. **Failure path:** Stop, ask for input, revise prompt, rerun narrow phase, roll back, or downgrade from automation to manual review.

If a gate has no failure path, it is not a gate. It is theater.

Every gate definition should also include an executive summary for humans and an execution block for agents. The summary answers "what does this gate protect?" The execution block answers "what exactly should the agent do and prove?"

## Recommended Cowork Gates

### Gate 0: Problem Formulation

**Question:** Do we know what is being built?  
**Entry condition:** User has provided a request or project context.  
**Exit test:** A reviewer can read one sentence and name the proposed thing, where it sits, and what it produces.  
**Evidence artifact:** Confirmed one-sentence build statement.  
**Failure path:** Stop intake and ask formulation questions. Do not let the agent turn vague intent into architecture.

Suggested test:

```text
[THING] is a [WHAT] inserted [WHERE] that produces [OUTPUT].
```

### Gate 1: Verified Evidence

**Question:** Has the agent checked trusted local or database-backed data before looking outward?  
**Entry condition:** Problem statement exists.  
**Exit test:** The handoff names the local files, datasets, DB tables, logs, metadata, or approved records checked, or explains why none exist.  
**Evidence artifact:** Source inventory with paths, table names, query summaries, or dataset versions.  
**Failure path:** Search local sources first; only then allow external research.

### Gate 2: Vetted Script Inventory

**Question:** Has the agent checked for existing tested scripts before writing new code?  
**Entry condition:** Problem statement and evidence inventory exist.  
**Exit test:** The handoff names matching scripts checked in `scripts/`, `SCRIPTS/`, or approved script directories, or explains why no stored script fits.  
**Evidence artifact:** Script inventory with paths, purpose, required inputs, and last known verification status when available.  
**Failure path:** Use the vetted script if it fits. Create an ad hoc script only if no suitable stored script exists; if the ad hoc script becomes reusable, review and promote it into the stored script library.

### Gate 3: Plan and Dependency Map

**Question:** Is the sequence safe to execute?  
**Entry condition:** Problem, evidence inventory, and vetted script inventory exist.  
**Exit test:** Every step has dependencies, required context, allowed tools, expected output, and a handoff condition.  
**Evidence artifact:** Boondoggle Score, task plan, or pipeline manifest.  
**Failure path:** Do not implement; repair the missing dependency or test.

### Gate 4: Small Manual Run

**Question:** Does the approach work once under supervision?  
**Entry condition:** Plan is approved.  
**Exit test:** One narrow case passes with logs and visible output.  
**Evidence artifact:** Command output, screenshot, generated file, test result, or reviewer note.  
**Failure path:** Revise prompt, tool choice, schema, or data assumptions before scaling.

### Gate 5: Automated Test Harness

**Question:** Can the pipeline prove its own basic correctness?  
**Entry condition:** Small manual run passed.  
**Exit test:** Unit tests, schema validation, lint/typecheck, citation checks, rubric checks, browser checks, or data-integrity checks run automatically.  
**Evidence artifact:** Test report and stored logs.  
**Failure path:** Hold automation. Add or repair tests before broader execution.

### Gate 6: Independent Review

**Question:** Has a fresh reviewer checked what the builder is likely to miss?  
**Entry condition:** Automated checks pass.  
**Exit test:** Read-only reviewer subagent or human review checks edge cases, security, data grounding, instruction adherence, and over-broad changes.  
**Evidence artifact:** Review report with file paths, findings, and residual risk.  
**Failure path:** Return to implementation or tests, depending on finding type.

### Gate 7: Scaled Dry Run

**Question:** Does the pipeline behave across a representative batch?  
**Entry condition:** Independent review passes.  
**Exit test:** Pipeline runs on a small representative batch with bounded permissions and produces expected outputs, timing, cost, and error rates.  
**Evidence artifact:** Batch report, failure table, and sampled output review.  
**Failure path:** Fix prompt, agent routing, retry logic, rate limits, or data validation before full automation.

### Gate 8: Full Automation Approval

**Question:** Is it safe to remove the human from the normal path?  
**Entry condition:** Scaled dry run passes.  
**Exit test:** Approval criteria are met: tests pass, rollback exists, logs are stored, permissions are scoped, costs are bounded, failure alerts exist, and high-stakes cases still route to humans.  
**Evidence artifact:** Promotion record with approver, date, version, commands, and rollback procedure.  
**Failure path:** Continue as semi-automated, not fully automated.

## Automation Maturity Levels

**Level 0: Manual only**  
The human runs each step and inspects every output.

**Level 1: Assisted**  
Claude drafts or edits, but the human controls phase transitions.

**Level 2: Semi-automated with gates**  
Claude or scripts run phases, but every promotion requires explicit test evidence.

**Level 3: Batch automation with sampled review**  
The pipeline processes many items after passing small-run and dry-run gates. Humans review samples, failures, and high-risk cases.

**Level 4: Fully automated normal path**  
The pipeline runs unattended only after gates, tests, rollback, observability, and escalation paths are proven. Humans still own policy, exceptions, and post-run audits.

## Explicit Test Types for Cowork

- **Formulation tests:** Can a stranger name the thing, insertion point, and output?
- **Local evidence tests:** Did the agent check verified local/database sources first?
- **Schema tests:** Does output validate against a declared structure?
- **Regression tests:** Did existing behavior remain intact?
- **Grounding tests:** Are claims tied to local paths, source links, database rows, or generated artifacts?
- **Rubric tests:** Does the output satisfy a human-authored rubric?
- **Security tests:** Did the pipeline avoid secrets, unsafe commands, injection, and over-broad permissions?
- **Scope tests:** Did changes stay inside the requested files, folders, or concepts?
- **Independence tests:** Did a fresh reviewer or subagent evaluate the output without inheriting the builder's reasoning path?
- **Rollback tests:** Can the system restore the previous state or isolate failed outputs?

## Phase Gate Manifest Template

```yaml
phase: "G3-small-manual-run"
executive_summary:
  purpose: "Prove one representative case works before scaling the pipeline."
  human_risk_note: "Do not promote if the output cannot be traced to verified local evidence."
  safe_to_run_when: "Problem, evidence, and plan gates have passed."
owner: "human-or-orchestrator"
agent_allowed:
  tools:
    - Read
    - Grep
    - Bash(test command only)
  write_scope:
    - "sandbox/output/"
entry_condition:
  - "Problem formulation gate passed"
  - "Verified local evidence inventory exists"
  - "Vetted script inventory exists or no suitable stored script was found"
  - "Plan lists dependencies and handoff conditions"
verified_data_first:
  required: true
  sources:
    - "repo files"
    - "approved datasets"
    - "database tables or exports when relevant"
verified_scripts_first:
  required: true
  preferred_locations:
    - "scripts/"
    - "SCRIPTS/"
    - "approved project script directories"
  fallback: "Create ad hoc script only when no vetted script fits; review for promotion after use."
exit_tests:
  - name: "single-case output generated"
    command_or_check: "Run one representative item"
    pass_condition: "Output exists and matches expected schema"
  - name: "human plausibility audit"
    command_or_check: "Reviewer compares output to source evidence"
    pass_condition: "No unsupported claims or unexplained transformations"
evidence_artifact:
  - "logs/g3-small-run.log"
  - "reports/g3-review.md"
failure_path:
  - "Stop pipeline"
  - "Revise prompt or data contract"
  - "Rerun this phase only"
promotion:
  next_phase: "G4-automated-test-harness"
  approval_required: true
```

## Boondoggle Integration

The Boondoggle Score should include a phase-gate line after every Claude task:

```text
HANDOFF CONDITION:
This step is complete only when [explicit condition] is true and [test/evidence]
has been recorded at [artifact path]. If the condition fails, do not proceed to
Step N+1; return to [specific prior step] with [specific correction].
```

For fully automated pipelines, add a promotion gate:

```text
FULL AUTOMATION GATE:
The pipeline may run unattended only after:
1. one manual run passes,
2. automated tests pass,
3. a scaled dry run passes,
4. rollback is documented,
5. logs and artifacts are retained,
6. high-risk cases route to a human,
7. an approver signs the promotion record.
```

## Best-Practice Rules

1. Start with verified local or database-backed data. External research is a fallback, not the first move.
2. Start with tested, vetted, stored scripts. Ad hoc scripts are a fallback, not the first move.
3. Treat every phase transition as a promotion decision.
4. Make the test cheaper than the mistake.
5. Keep early gates human-readable and later gates machine-runnable.
6. Write recipes primarily for agents, but start each one with a human-readable executive summary.
7. Compile skills and the overall system into human-readable documentation for operators, teachers, reviewers, and maintainers.
8. Do not let a model grade its own work when the stakes are high; use independent review.
9. Prefer narrow, repeatable checks over broad "review this" instructions.
10. Store evidence artifacts so audits can reconstruct what happened.
11. Allow automation to expand only after repeated gated success.
12. Keep a rollback or rewind path for every automated write.
13. Route ambiguous, high-impact, or value-laden decisions to humans.

## Sources

- Anthropic, "Building effective agents" (2024-12-19): https://www.anthropic.com/engineering/building-effective-agents
- Anthropic, "Demystifying evals for AI agents" (2026): https://www.anthropic.com/engineering/demystifying-evals-for-ai-agents
- Claude Code Docs, "Best practices for Claude Code": https://code.claude.com/docs/en/best-practices
- Claude, "How and when to use subagents in Claude Code": https://claude.com/blog/subagents-in-claude-code
- Claude, "When to use multi-agent systems, and when not to": https://claude.com/blog/building-multi-agent-systems-when-and-how-to-use-them
- GitLab Docs, "Deployment approvals": https://docs.gitlab.com/ci/environments/deployment_approvals/
- Atlassian, "Continuous delivery pipeline 101": https://www.atlassian.com/continuous-delivery/principles/pipeline
- Atlassian, "What is continuous deployment?": https://www.atlassian.com/continuous-delivery/software-testing/continuous-deployment
- SonarSource, "Integrating Quality Gates into Your CI/CD Pipeline": https://www.sonarsource.com/resources/library/integrating-quality-gates-ci-cd-pipeline/
