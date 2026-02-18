# claude-skills

```
+------------------------------------------------------+
|  ====================================================|
|                                                      |
|    CLAUDE : : SKILLS                                 |
|                                                      |
|    sys.init ................................. OK      |
|    loading skills ........................... 1       |
|    planning-skill v1.0 ............... [ACTIVE]      |
|    ??? .............................. [PENDING]       |
|    ??? .............................. [PENDING]       |
|                                                      |
|  ====================================================|
+------------------------------------------------------+
```

Skills for Claude that work.

## Skills

| Skill | What it does | Status | Download |
|-------|-------------|--------|----------|
| [planning-skill](./planning-skill/) | 6-phase pipeline: idea to execution plan. Software, processes, strategy, content, research. | ✅ Ready | [planning-skill.skill](https://github.com/BasicsOnly/claude-skills/releases/download/planning-skill-v1.0/planning-skill.skill) |

## Installation

Copy the skill folder into your Claude skills directory. It triggers automatically based on the frontmatter in `SKILL.md`.

Or download the `.skill` package from the release link above.

---

## planning-skill

```
+------------------------------------------------------+
|  ====================================================|
|                                                      |
|    PLANNING : : SKILL                                |
|                                                      |
|    [1] brainstorm ......... define the problem       |
|    [2] plan ............... blueprint it             |
|    [3] refine ............. kill the vagueness       |
|    [4] convert ............ step-by-step             |
|    [5] audit .............. break it on paper        |
|    [6] defend ............. keep what survives       |
|                                                      |
|    output: FINAL_EXECUTION_PLAN.md                   |
|                                                      |
|  ====================================================|
+------------------------------------------------------+
```

You describe a project idea to Claude. Claude starts building immediately. The output has gaps, missed edge cases, and assumptions nobody checked. You spend the next hour patching things that should have been caught before the first line was written.

This skill sits between the idea and the building. Six phases, each one narrowing the space for error, each one reading the previous phase's output fresh from disk so nothing degrades over a long session.

It works for code. It also works for process redesign, strategic plans, content workflows, research synthesis, and office automation. The phases adapt their depth and focus to whatever you're planning.

### How it works

**Phase 1: Brainstorm.**
You talk through the idea. The skill pushes back, one question at a time. What problem is this solving? Who actually has that problem? What evidence supports the scope you're proposing? It keeps going until the problem, scope, constraints, and success criteria are pinned down in a concept brief. Most projects go wrong here (building the wrong thing, or the right thing at the wrong scale), so this phase is deliberately slow.

**Phase 2: Plan.**
The concept brief becomes a full blueprint. Every major decision names what was considered and why the chosen option won. Software projects get file trees and schemas. Process projects get stage owners and SLAs. Strategy work gets assumption registers and decision criteria. The plan is sequenced so each milestone can be tested on its own. No task depends on something built later.

**Phase 3: Refine.**
A reviewer pass that reads the plan the way an executor would: someone who has to turn these words into action with zero follow-up questions. Every vague word gets flagged. "Handle errors gracefully" becomes retry logic with specific counts, delays, and fallback behavior. "Follow standard onboarding procedures" becomes 12 named steps with owners and deadlines. Each finding includes the exact replacement text, ready to paste in.

**Phase 4: Convert.**
The reviewed plan becomes step-by-step instructions. One action per step. Every file to be created includes its full contents inline. Every reference includes the literal value. Every step has a verification check so the executor knows whether it worked. The instructions assume the executor knows nothing about the project's history. An AI assistant, a teammate, or a mix of both can follow the same document.

**Phase 5: Audit.**
A hostile pass that walks through the instructions and looks for what breaks. Missing prerequisites that will crash step 1. Data that one step produces in a format the next step doesn't expect. Silent failures that look like success. Environment assumptions that hold on your machine but not on the target.

For high-stakes work (enterprise proposals, documents going to a F500 C-suite, cross-team process launches), the skill spins up a multi-persona review panel. Eight to twelve simulated reviewers with different professional backgrounds (technical, financial, procurement, executive, design, independent strategy) each review independently. Findings are consolidated with data on how many reviewers flagged the same issue and whether any of them sit in the approval chain.

**Phase 6: Defend.**
Every audit finding gets argued against before it's accepted. The question for each one: is this a real failure that will actually happen during execution, or a theoretical concern that adds complexity without reducing risk? Findings that survive get applied. The rest get documented and set aside.

The output is one file. It references no conversation history, no earlier drafts, no context outside itself. Someone picking it up cold should be able to execute it on the first attempt.

### Why you'd use it

The short version: you've tried asking Claude to plan something and the plan had holes. Steps were missing. Details were vague. It worked for the happy path and fell apart on the first edge case.

This skill exists because planning is where AI assistants are weakest and where the cost of getting it wrong compounds fastest. A bad plan produces bad code, a bad process, a bad document. Catching a missing database migration step in Phase 3 costs two minutes of conversation. Catching it in production costs an afternoon.

Use it when the project is complex enough that you wouldn't hand it to a new hire without written instructions. Use it when you're handing off to someone (human or AI) who won't have you available to answer "what did you mean by this?" Use it when the cost of a failed first attempt is high enough to justify 30 minutes of structured planning.

[Browse the source](./planning-skill/) · [Download planning-skill.skill](https://github.com/BasicsOnly/claude-skills/releases/download/planning-skill-v1.0/planning-skill.skill)

---

## License

[claude-skills Community License](./LICENSE): free for individuals and organizations under $1B annual revenue. Attribution and share-alike required for derivatives. Organizations above $1B revenue must request a commercial license.
