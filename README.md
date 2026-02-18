# claude-skills

Battle-tested skills for Claude. Planning pipelines, multi-persona audits, and more.

## Skills

| Skill | What it does | Status | Download |
|-------|-------------|--------|----------|
| [planning-skill](./planning-skill/) | 6-phase pipeline that transforms project ideas into battle-tested execution plans. Works for software, process design, strategy, content, research, and office automation. | ✅ Ready | [planning-skill.skill](https://github.com/BasicsOnly/claude-skills/releases/download/planning-skill-v1.0/planning-skill.skill) |

## Installation

Each skill folder contains a `SKILL.md` orchestrator and supporting reference files. To install:

1. Copy the skill folder into your Claude skills directory
2. The skill triggers automatically based on the description in its `SKILL.md` frontmatter

Alternatively, use the `.skill` package files where available.

---

## planning-skill

Most AI-assisted projects fail the same way: you describe an idea, the AI jumps straight to building, and the output is a superficial first draft riddled with gaps, missing edge cases, and assumptions nobody validated. You spend more time fixing than you saved.

The planning skill exists to fix this. It forces a structured pipeline between "I have an idea" and "start executing" that catches problems when they're cheap to fix (a 2-minute conversation) instead of expensive to fix (a failed deployment, a derailed process, a wasted sprint).

It works for any project type: software, process redesign, strategic planning, content pipelines, research synthesis, office automation.

### The pipeline

The skill runs 6 phases in sequence. Each phase reads the previous phase's output from disk (preventing context degradation), applies a distinct analytical lens, and writes a refined artifact. The user has explicit approval gates at key transitions.

**Phase 1: Collaborative Brainstorm**
A structured discovery session that stress-tests the idea before any planning begins. The skill asks hard questions one at a time: What problem does this actually solve? Who has it? What evidence supports the assumptions? Where is the scope too broad for a single execution cycle? The output is a concept brief with clear problem definition, scope boundaries, success criteria, and key decisions.

The value here is friction. Most projects skip straight to "how" without nailing "what" and "why." This phase prevents the most expensive mistake in project work: building the wrong thing.

**Phase 2: Detailed Plan**
Converts the validated concept brief into a full project blueprint. Every significant choice includes rationale with named alternatives ("We chose X over Y because Z"). Sections adapt to project type: software projects get file trees and schemas, process projects get stage definitions and SLAs, strategic plans get assumption registers and decision criteria.

The execution sequence is ordered so every milestone produces a testable intermediate state. No forward dependencies: every task's inputs come from a prior task or the prerequisites.

**Phase 3: Refine**
A senior-reviewer pass that reads the plan as an executor would. Finds every vague word ("appropriate," "as needed," "standard"), every missing step, every inconsistent reference. For each finding, proposes exact replacement text ready to apply.

This is where "add relevant validation" becomes "validate email format with RFC 5322 regex, reject duplicates against existing records, return specific error codes per failure type." The gap between those two statements is where execution fails.

**Phase 4: Convert to Execution Steps**
Transforms the human-readable plan into zero-ambiguity instructions. Every step is atomic (one action), self-contained (no assumed context), and verifiable (concrete success criteria). Creation steps include complete inline content: full file contents, complete template text, exact configuration values.

The target executor could be an AI assistant operating autonomously, a team member following the plan independently, or a hybrid. The document works for all three because it assumes zero prior context.

**Phase 5: Hostile Audit**
An adversarial review that simulates execution step-by-step, looking for what breaks. Attack vectors are prioritized by failure frequency: missing prerequisites, integration gaps between steps, silent failures, ambiguous instructions, external dependency fragility, environment assumptions, edge cases, and weak verification.

For high-stakes deliverables (enterprise proposals, F500-facing documents, cross-functional process launches), the skill activates a multi-persona audit panel: 8-12 reviewers with different professional frames (technical, financial, procurement, executive, design, strategy) each reviewing independently before findings are consolidated with consensus metadata.

**Phase 6: Steelman Defense**
Every audit finding gets a genuine defense before being accepted. This prevents over-engineering from theoretical concerns. Each finding is evaluated on probability of actual failure, cost of the fix versus cost of the failure, and whether it addresses a real execution risk or a hypothetical preference.

Findings that survive scrutiny get applied. The final output is a single self-contained document that references no conversation history, no prior artifacts, and no assumed context. The acid test: could someone with zero prior knowledge execute this document successfully on their first attempt?

### What you get

One file: `FINAL_EXECUTION_PLAN.md`. Everything else is intermediate work product. The final document is deployment-ready, whether "deployment" means running a script, launching a process, shipping a document, or executing a strategy.

### When to use it

- You have a project idea and want to maximize first-attempt success rate
- You're handing off execution to someone (or something) that needs precise instructions
- The project is complex enough that gaps in planning will cause expensive rework
- You want structured adversarial review before committing resources
- You need a repeatable planning process across different project types

[Browse the source](./planning-skill/) · [Download planning-skill.skill](https://github.com/BasicsOnly/claude-skills/releases/download/planning-skill-v1.0/planning-skill.skill)

---

## License

[claude-skills Community License](./LICENSE): free for individuals and organizations under $1B annual revenue. Attribution and share-alike required for derivatives. Organizations above $1B revenue must request a commercial license.
