# Planning Skill

A 6-phase pipeline that transforms project ideas into battle-tested execution plans. Works for software development, process design, content creation, strategic planning, research synthesis, office automation, and prototyping.

## What it does

Takes a vague idea through structured brainstorming, detailed planning, adversarial review, and hardening to produce a single execution document that an AI assistant or human team can follow with zero additional context.

**Pipeline:** Brainstorm → Detailed Plan → Refine → Convert to Steps → Hostile Audit → Steelman Defense → Final Plan

## Installation

Download `planning-skill.skill` and install it in your AI assistant's skill directory. The skill triggers automatically when you describe a project idea or ask for planning help.

Alternatively, copy the `planning-skill/` folder to your skills directory.

## Structure

```
planning-skill/
├── SKILL.md                           # Orchestrator (phase sequencing, examples, workflow)
├── references/
│   ├── phase-01-brainstorm.md         # Collaborative discovery with user
│   ├── phase-02-detailed-plan.md      # Full project blueprint
│   ├── phase-03-refine.md             # Adversarial review with user
│   ├── phase-04-convert.md            # Convert to zero-ambiguity execution steps
│   ├── phase-05-audit.md              # Hostile audit (solo or multi-persona panel)
│   └── phase-06-steelman.md           # Defend, filter, and finalize
├── evals/
│   └── evals.json                     # 8 test cases (software, process, strategy, content, enterprise)
└── LICENSE
```

## Use cases

**Software:** "I want to build a CLI tool for batch file renaming" → produces execution plan with file contents, commands, verification steps.

**Process design:** "Our client onboarding takes 3 weeks, needs to be under 1 week" → produces redesigned process with stage definitions, SLAs, escalation paths, pilot plan.

**Office automation:** "I manually spend 2 days each month generating commission statements from spreadsheets" → produces automation plan with data source mapping, calculation rules, error handling, and approval workflow.

**Content/research:** "I have 150 interview transcripts to synthesize into a report" → produces analysis plan with coding methodology, tooling, quality criteria, timeline.

**Strategic planning:** "The board wants us to expand into the UK market within 12 months" → produces go-to-market plan with partner strategy, localization sequencing, resource requirements, success metrics.

**Enterprise deliverables:** "Build a tool that generates sales assessments for F500 clients" → produces plan with multi-persona audit panel (business, technical, finance, procurement, executive, strategy consultants, designer) reviewing the output independently before consolidation.

## Design principles

Built against empirically validated prompt engineering research:

- **U-shaped attention positioning**: Identity at top, critical constraints reinforced at bottom of every phase file
- **Structured formatting**: Consistent section hierarchies (performance varies up to 40% based on format)
- **Positive instruction framing**: "Stay in the problem space" rather than "Don't discuss solutions"
- **Examples with reasoning chains**: Every phase shows thinking-before-conclusion patterns
- **Contrastive examples**: Every phase includes "what to avoid" demonstrations with explanations
- **Motivation for rules**: Key constraints include rationale so models generalize to edge cases
- **Calibrated natural language**: No ALL CAPS, no artificial urgency, no threats
- **Disk-based artifact persistence**: Prevents context degradation across long planning sessions

## License

MIT
