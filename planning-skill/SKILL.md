---
name: planning-skill
description: >
  A 6-phase pipeline that transforms raw project ideas into deployment-ready execution plans
  for AI assistants or human teams. Use this skill whenever the user wants to plan a project
  before building it, asks for help turning a vague idea into a structured plan, wants to
  create instructions for an AI assistant to execute autonomously, asks to "plan before
  doing," mentions wanting to "get it right the first time," wants a systematic approach
  to building or creating something, or says anything like "I have an idea for an
  app/tool/script/project/process/document/workflow." Also use when the user references
  execution plans, implementation plans, project planning, process design, technical
  blueprints, or workflow optimization. Works for software development, office automation,
  content creation, process refinement, research synthesis, strategic planning, and
  prototyping. Trigger broadly; planning before doing always helps, and the user can
  exit early if they want to skip phases.
---

# Planning Skill

A 6-phase pipeline that transforms project ideas into battle-tested execution plans. Each phase runs with a clean analytical frame, passing structured artifacts through disk-persisted files to prevent context degradation.

The pipeline: Brainstorm (with user) → Detailed Plan → Refine (with user) → Convert to Execution Steps → Hostile Audit → Steelman Defense → Final Plan.

## When to use this skill

Use this skill when the user has a project idea and wants to maximize the probability of successful execution. This applies to both technical and non-technical projects:

- Software development ("I want to build a price comparison tool")
- Office automation and document creation ("I need a system for generating quarterly reports from our data")
- Process design and refinement ("Our onboarding process is broken, help me redesign it")
- Content creation workflows ("I want to build a repeatable pipeline for producing client proposals")
- Research synthesis and notes processing ("I have 200 pages of interview transcripts, help me plan the analysis")
- Strategic planning ("I need a go-to-market plan for entering the Nordic market")
- Prototyping ("I want to prototype a new dashboard layout before we commit engineering resources")

The skill can be entered at any phase. If the user already has a concept brief, skip Phase 1. If they have a detailed plan, skip to Phase 3. Match the entry point to what already exists.

## Phase reference files

Before starting each phase, read the corresponding reference file. Each file contains the complete phase prompt with identity, instructions, examples, output format, and quality gates.

| Phase | File | When to read |
|-------|------|--------------|
| 1. Brainstorm | `references/phase-01-brainstorm.md` | At pipeline start, or when the user presents a new idea |
| 2. Detailed Plan | `references/phase-02-detailed-plan.md` | After Phase 1 gate passes (user confirms concept) |
| 3. Refine | `references/phase-03-refine.md` | After Phase 2 produces detailed_plan.md |
| 4. Convert | `references/phase-04-convert.md` | After Phase 3 gate passes (user approves refined plan) |
| 5. Audit | `references/phase-05-audit.md` | After Phase 4 produces execution_steps.md |
| 6. Steelman | `references/phase-06-steelman.md` | After Phase 5 produces audit_report.md |

Read each reference file at the start of its phase. The files contain detailed instructions, examples with reasoning chains, contrastive examples showing what to avoid, and quality gates to verify before writing each artifact. Follow the phase prompt completely.

## Workflow

### Phase sequence

Execute phases 1 through 6 in strict order. Complete each phase fully and write its output artifact before advancing.

Phases 1 and 3 are interactive: they require user dialogue and explicit user confirmation before the gate opens. Phases 2, 4, 5, and 6 are autonomous: execute them independently, write the artifact, and proceed.

Each phase's output is the input for the next. Skipping ahead produces plans built on incomplete foundations, which compounds into execution failures downstream. This is why the gate system exists: each gate verifies the foundation is solid before the next layer is built on top.

### Artifact management

All artifacts persist to disk in the project workspace. Each phase reads its input artifact from disk and writes its output artifact to disk. This prevents context degradation, which research shows begins at approximately 3,000 tokens of reasoning content.

At the start of each new phase, re-read the required input artifact(s) from disk, even if you generated them earlier in the same session. This ensures you work from the persisted version rather than a degraded in-context representation.

Artifact manifest:

| Phase | Output file | Contents |
|-------|------------|----------|
| 1 | `concept_brief.md` | Problem definition, scope boundaries, success criteria |
| 2 | `detailed_plan.md` | Complete project blueprint |
| 3 | `refined_plan.md` | Plan with all review improvements applied |
| 4 | `execution_steps.md` | Zero-ambiguity imperative instructions |
| 5 | `audit_report.md` | Hostile review findings with severity ratings |
| 6 | `FINAL_EXECUTION_PLAN.md` | Battle-tested plan ready for deployment |
| All | `PROGRESS.md` | Running log updated after every phase |

Write each artifact immediately upon phase completion. Update PROGRESS.md after every phase with: phase completed, key decisions made, open questions surfaced, and any scope changes. This file is the continuity thread that survives context window limitations across the full pipeline.

### Phase gates

Each phase has a completion gate that must be satisfied before proceeding:

1. **Brainstorm**: User explicitly confirms the concept is solidified
2. **Detailed Plan**: Plan covers all relevant dimensions for the project type (see Phase 2). Every significant choice includes rationale.
3. **Refine**: User approves the refined plan as ready for conversion
4. **Convert**: Every instruction is atomic, unambiguous, includes all necessary detail, and has verification criteria
5. **Audit**: Minimum 10 specific findings with severity ratings and concrete fix proposals
6. **Steelman**: Every finding has an accept/reject/modify verdict with reasoning. Final plan is coherent after all changes.

### Status updates

Between phases, provide a 3-4 sentence status update: which phase completed, key decisions from that phase, what comes next, and approximate remaining work.

### Retry protocol

If a phase's output fails its quality gate, re-execute with a specific diagnosis of what fell short. Limit to 2 re-execution attempts per phase. After 2 failures, present the gap to the user and ask for guidance.

### Backtracking

If Phase 3 review reveals the overall approach from Phase 2 is fundamentally wrong (wrong direction, not underspecified), return to Phase 2 with the issue identified. Preserve the concept brief from Phase 1; only the detailed plan needs regeneration.

If Phase 5 audit reveals systemic problems in execution steps (more than 5 critical findings, or a pervasive pattern), return to Phase 3 rather than attempting to patch in Phase 6.

Log all backtracking events and their reasons in PROGRESS.md.

## Examples

### Normal phase transition (software)

**Scenario:** Phase 1 completing, transitioning to Phase 2.

The user confirmed the concept brief: a CLI tool for batch-renaming files using regex, macOS/Linux, local files only. The brief is written to concept_brief.md and PROGRESS.md is updated.

**Status update to user:** "Phase 1 complete. Concept brief written: CLI batch-rename tool with regex support, macOS/Linux, local files only. Moving to Phase 2 to generate the detailed plan. This runs autonomously; I'll present the result when finished."

**Next action:** Read `references/phase-02-detailed-plan.md`, then re-read `concept_brief.md` from disk, then generate the plan as a fresh exercise.

### Normal phase transition (non-technical)

**Scenario:** Phase 1 completing for a process redesign project.

The user confirmed the concept brief: redesigning the client onboarding process for a consulting firm, reducing time-to-first-deliverable from 3 weeks to 1 week, covering intake forms, resource allocation, kickoff meeting structure, and handoff protocols.

**Status update to user:** "Phase 1 complete. Concept brief written: onboarding process redesign targeting 1-week time-to-deliverable. Key constraints: must work with existing CRM, no additional headcount. Moving to Phase 2 for the detailed process design."

### Backtracking decision

**Scenario:** Phase 3 review discovering that Phase 2's approach is wrong.

The detailed plan proposes a REST API server for what the concept brief defines as a CLI tool. This affects the architecture, deployment, and roughly 40% of the plan.

**Status update to user:** "Phase 3 review found a fundamental issue: the plan proposes a REST API server, but the concept brief defines a CLI tool. Rather than patching individual sections, I'm returning to Phase 2 to regenerate with the correct frame. The concept brief remains valid."

**Next action:** Re-read `references/phase-02-detailed-plan.md`, re-read `concept_brief.md`, regenerate the plan.

### Gate failure and retry

**Scenario:** Phase 4 output has 8 steps with placeholder text instead of specific content.

The quality gate requires complete detail in every step. This is a specific, bounded gap.

**Next action:** Re-execute Phase 4 targeting the 8 steps with placeholder content. Each must contain the complete, correct detail derived from the refined plan.

### Skipping Phase 3 (what not to do)

The plan covers all sections and seems comprehensive. Moving directly to Phase 4.

**Why this fails:** Plans that "look good" on first read consistently contain vague specifications, missing steps, and implicit assumptions. These only surface under structured review. Phase 3 exists because first-draft plans fail in execution at high rates. The gate requires explicit user approval, which cannot be inferred.

### Multi-persona audit for high-stakes deliverables

**Scenario:** The pipeline is producing execution steps for a tool that generates enterprise sales collateral (assessment decks, ROI analyses, technical proposals). The deliverable will be presented to F500 C-suite stakeholders and reviewed by their procurement and legal teams.

A single-perspective audit catches execution failures (missing steps, broken references) but misses stakeholder-specific failure modes. A CFO reads a pricing page differently than a CTO reads an architecture diagram. For deliverables where the audience is the risk, Phase 5 scales the audit into a multi-persona review panel.

**Phase 5 adaptation:** Instead of one hostile auditor, spawn independent reviewer agents with distinct professional frames. Each conducts a full, isolated, line-by-line review. Example panel for enterprise sales collateral:

- Internal account lead (does this accurately represent our capabilities?)
- Client business owner, CTO, CFO, Procurement lead, IT Security (each applying their approval criteria)
- Senior strategy consultants (calibrating against the standard these executives see weekly)
- Expert designer (visual hierarchy, information architecture, readability)

Each reviewer writes a standalone finding report with no awareness of other reviewers' work. Independence prevents anchoring bias. After all reviews complete, findings are consolidated: deduplicate (4 reviewers flagging the same ROI gap = 1 finding with strong consensus), resolve conflicts (technical lead wants more detail where designer says page is too dense), and prioritize by reviewer consensus count and approval-chain impact.

**Phase 6 adaptation:** Consolidated findings carry reviewer agreement metadata. The steelman defense weighs this: a finding flagged by 4 of 11 independent reviewers (including 2 approval-chain stakeholders) gets a different defense calculus than a finding flagged by 1 reviewer with no deal authority. The defense still applies genuinely, but the assessment must address why multiple independent professionals converged on the same concern.

**When to use this pattern:** Match panel size to stakes. Internal process documentation gets a standard solo audit. A proposal going to a F500 C-suite gets the full panel. The trigger is the audience, not the content complexity.

## Completion

When Phase 6 finishes, present FINAL_EXECUTION_PLAN.md to the user with:

- Header: "EXECUTION PLAN - READY FOR DEPLOYMENT"
- Summary of the review process: total audit findings, accept/reject/modify breakdown, critical issues resolved
- Instruction that the document can be handed to a team member, loaded into an AI assistant session, or used as a project management backbone

The final deliverable is one document: FINAL_EXECUTION_PLAN.md. Everything else is intermediate work product.

## Design principles

These prompts were designed using empirically validated techniques from prompt engineering research:

- **Structured formatting**: Performance varies up to 40% based on prompt format alone. Each phase uses consistent section structure with clear hierarchies.
- **U-shaped attention positioning**: Identity and objective at the top of each file (high-attention zone), critical constraints reinforced at the bottom (recency zone). Long reference content positioned before the instructions that reference it.
- **High-level thinking guidance**: Phases use "Think deeply about..." rather than prescriptive step sequences. Models produce better results when given latitude to approach problems creatively rather than following rigid prescribed processes.
- **Positive instruction framing**: "Stay in the problem space" rather than "Don't discuss solutions." Negative instructions are unreliable and can prime toward the forbidden behavior.
- **Examples with reasoning chains**: Every phase includes examples that show the thinking process before the conclusion. Models mirror example structure; examples without reasoning produce outputs without reasoning.
- **Contrastive examples**: Every phase includes at least one "what to avoid" example with explanation. Contrastive examples define boundaries more effectively than additional positive examples alone.
- **Calibrated natural language**: No ALL CAPS, no artificial urgency, no threats. Modern models are highly responsive to the system prompt; heavy emphasis causes overtriggering.
- **Motivation for rules**: Key constraints include rationale ("This matters because..."). Models generalize better to edge cases when they understand intent, not just the rule.
- **Specific role identities**: Each phase has a distinct, targeted identity ("hostile auditor," "senior reviewer," "defense attorney") rather than generic roles. Specific roles produce more targeted behavior.
- **Disk-based artifact persistence**: Each phase reads from and writes to disk. This prevents context degradation, which begins at approximately 3,000 tokens of reasoning content. PROGRESS.md provides continuity across long sessions.
- **Re-read-from-disk at every phase boundary**: Works from persisted artifacts, not degraded in-context copies. The disk version is always the authoritative source.

## Critical rules (reinforced)

Three rules that override all other considerations:

1. **Phases 1 and 3 require explicit user confirmation before proceeding.** Inferred approval is not sufficient. Ask and wait.
2. **Re-read input artifacts from disk at the start of every phase.** Context degrades. The disk version is the source of truth.
3. **Every phase writes its output artifact to disk before the next phase begins.** The artifact chain is the backbone of the pipeline. A phase that produces output only in conversation, without persisting it, breaks every subsequent phase.
