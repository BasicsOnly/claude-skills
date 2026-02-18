# Phase 4: Conversion to Execution Steps

Read this file at the start of Phase 4. Follow these instructions completely.

## Identity

You are converting a human-readable plan into execution-ready instructions. The target executor may be an AI assistant operating autonomously, a team member following the plan independently, or a hybrid (human managing AI-executed subtasks). The executor will receive this document with zero prior context about the project's history or reasoning.

Every instruction must be self-contained, atomic, and verifiable. The executor has strong general capability but zero project context. Given precise specifications, they produce correct output on the first attempt. Given vague directives, they make inconsistent choices that compound into failures. This is why precision matters: the quality of the execution steps directly determines whether the project succeeds.

Produce comprehensive, production-grade instructions. Include complete details, full specifications, and exhaustive edge case handling.

## Input

Re-read `refined_plan.md` from disk. Work from the persisted version, even if you generated it earlier in this session.

## Conversion principles

Think deeply about how each section of the refined plan translates into concrete actions. Many plan statements that seem clear in context become ambiguous when stripped of surrounding narrative. Your job is to resolve every ambiguity into a specific instruction.

### Self-contained steps
The executor knows only what this document tells them. Include only "what" and "how" content. When a step requires understanding project intent to handle an edge case correctly, embed that reasoning as a decision rule within the step itself rather than relying on the executor's judgment.

### Atomic actions
Each numbered step performs exactly one action. "Create the directory structure and configure the environment" becomes two steps. A step is atomic when it can succeed or fail as a discrete unit with no partial completion. This matters for failure diagnosis: when a compound step fails, the executor cannot determine which part failed or whether the first part completed correctly.

### Explicit references
Every reference is fully specified. For software: complete file paths from project root. For processes: full role title, system name, and access method. For content: exact source document name, section, and location. "Update the config" becomes "Open file at `project-root/config/settings.json` and set the `api_base_url` value to `https://api.example.com/v2`."

### Complete inline content
Creation steps include the complete content or specification inline. For software: every line of code, every config value. For documents: full outline with section descriptions and word targets. For processes: complete template text, form fields, email drafts. For strategy: complete framework structure, evaluation criteria, scoring rubrics.

Include the complete literal value for every reference. If the instructions say "[add your key here]," the executor has no key. Either specify the literal value, reference a named environment variable with setup instructions, or include a prerequisite step that acquires it.

### Cross-step references
When a value defined in an early step is used later, define it once with a clear label and reference that label in subsequent steps. Include the literal value inline at each reference point so the executor can verify consistency without scrolling back. This prevents the most common integration failure: a value changed in one step but not updated in the 4 later steps that reference it.

Pattern: "This step uses the DATABASE_URL configured in Step 3 (postgresql://localhost:5432/myapp)." Or for processes: "This step uses the intake questionnaire template created in Step 2 (12-field form, Appendix A)."

### Failure handling
For every action that can fail, specify what the executor does on failure:
1. How to identify the failure (expected output vs actual)
2. Recovery action (retry, alternative approach, fallback)
3. Whether to continue with documented limitations, or halt with a diagnostic message

### Verification gates
After every milestone, include a verification step confirming the milestone succeeded. Verification is concrete: a command to run with expected output, a checklist of observable conditions, or a deliverable to inspect against defined criteria.

Weak: "Confirm the setup is complete." Strong: "Run `psql $DATABASE_URL -c 'SELECT count(*) FROM products'`. Expected: 0." Or: "Open the intake form and verify all 12 fields render correctly, the submit button sends to the correct endpoint, and the confirmation email triggers within 30 seconds."

## Step format

```
STEP [N]: [Brief action description]

Action: [Single imperative sentence]

Detail:
[Complete specification. For code: full file content. For documents: full outline/template.
For processes: complete procedure with decision criteria.]

On failure:
[Specific recovery for this step.]

Verify:
[Concrete check with expected result.]
```

Group into milestones, each ending with an integration verification.

## Examples

### Good step (software: file creation)
*Thinking: This creates the database schema. I need the complete SQL, not a description of what it should contain.*

```
STEP 3: Create the database schema file

Action: Create file at src/db/schema.sql

Detail:
CREATE TABLE IF NOT EXISTS products (
  id SERIAL PRIMARY KEY,
  name VARCHAR(255) NOT NULL,
  category VARCHAR(100),
  unit VARCHAR(50) DEFAULT 'each',
  created_at TIMESTAMP DEFAULT NOW()
);

CREATE TABLE IF NOT EXISTS prices (
  id SERIAL PRIMARY KEY,
  product_id INT REFERENCES products(id) ON DELETE CASCADE,
  retailer VARCHAR(100) NOT NULL,
  price_cents INT NOT NULL,
  captured_at TIMESTAMP DEFAULT NOW(),
  CONSTRAINT positive_price CHECK (price_cents >= 0)
);

CREATE INDEX idx_prices_product ON prices(product_id);
CREATE INDEX idx_prices_retailer ON prices(retailer);

On failure: Verify src/db/ directory exists (Step 1). Create with mkdir -p if missing.

Verify: Run `grep -c "CREATE TABLE" src/db/schema.sql`. Expected: 2.
```

### Good step (process: template creation)
*Thinking: The intake questionnaire is referenced in 4 later steps. I need the complete form specification, not a description of it.*

```
STEP 2: Create the client intake questionnaire

Action: Create intake form in the project management tool as a reusable template.

Detail:
Form title: "New Client Intake - [Client Name]"
Fields (all required unless noted):
1. Company legal name (text)
2. Primary contact name (text)
3. Primary contact email (email)
4. Company size - employees (dropdown: 1-50, 51-200, 201-1000, 1000+)
5. Industry (dropdown: Financial Services, Healthcare, Manufacturing, Technology, Other)
6. Current document processing volume - pages per month (number)
7. Primary document types (multi-select: Contracts, Invoices, Regulatory Filings, Technical Specs, Other)
8. Existing systems for document management (text, optional)
9. Compliance requirements (multi-select: GDPR, HIPAA, SOC2, ISO 27001, None, Other)
10. Target go-live date (date)
11. Budget range (dropdown: Under $50k, $50-150k, $150-500k, $500k+, TBD)
12. Decision maker name and title (text)

Assign to: Account Lead
Due date: Client + 5 business days (configured via automation rule)
Auto-reminder: Day 3

On failure: If template creation fails in the PM tool, create as a shared form (Google Forms or equivalent) with identical fields and link it in the project channel.

Verify: Open the template. Confirm all 12 fields are present, required flags are correct, and the auto-reminder rule is active.
```

### Good step (document/content creation)
*Thinking: The research report outline is referenced in later analysis steps. I need the complete structure with section descriptions and word targets, not a general instruction to "create an outline."*

```
STEP 4: Create the research report template

Action: Create file at deliverables/research-report-template.md

Detail:
# Customer Pain Point Analysis: Q3 2025

## Executive Summary (300-400 words)
[Synthesized findings: top 5 pain points ranked by frequency, severity assessment,
strategic implications for Q3 roadmap. Written last, after all analysis is complete.]

## Methodology (200-250 words)
- Sample: [N] interviews conducted [date range]
- Participant profile: [role distribution, company size distribution]
- Analysis method: Thematic coding with intercoder reliability check
- Coding framework: [reference codebook from Step 3]

## Finding 1: [Pain Point Name] (500-600 words)
- Frequency: Mentioned by [N] of [Total] participants ([%])
- Severity: [Critical / High / Medium] based on impact scoring rubric
- Description: [2-3 paragraph synthesis of the theme]
- Representative quotes: [3 quotes selected per criteria in Step 6]
- Product implications: [Specific feature/fix recommendations]

[Repeat Finding sections 2-5 with identical structure]

## Cross-cutting themes (300-400 words)
[Patterns that span multiple pain points. Interactions between findings.]

## Recommendations (400-500 words)
[Prioritized list tied to specific findings. Each recommendation includes:
effort estimate, expected impact, and dependency on other recommendations.]

## Appendix
- Full codebook with definitions
- Interview participant list (anonymized)
- Frequency table: all codes by count

Target total length: 4,000-5,000 words excluding appendix.

On failure: If the template structure conflicts with the team's existing report format,
adapt section names to match but preserve the word targets and structural requirements.

Verify: Template contains all 7 sections (exec summary, methodology, 5 findings,
cross-cutting, recommendations, appendix). Each finding section specifies frequency,
severity, description, quotes, and implications.
```

### Good milestone verification
*Thinking: Individual step verification confirms pieces in isolation. Milestone verification confirms they integrate correctly.*

```
MILESTONE 2 VERIFICATION: Database Layer

1. Run `psql $DATABASE_URL -c "\dt"` (expected: lists products and prices)
2. Run `psql $DATABASE_URL -c "\d products"` (expected: id, name, category, unit, created_at)
3. Run `node src/db/test-connection.js` (expected: "Connected. Tables: products, prices")

If check 1 fails: return to Step 5, verify schema ran against correct database.
If check 3 fails: verify DATABASE_URL matches Step 4's connection string.
```

### Bad instruction (what to avoid)
"STEP 5: Set up the API routes. Create the necessary route files. Include proper error handling and input validation."

Why bad: No paths, no content, "necessary" is undefined, "proper" is not a specification. The executor would need to invent the entire API layer, making different choices every time. This is why execution results are inconsistent: the instructions forced the executor to design, not just execute.

## Output artifact

Write `execution_steps.md` with:
1. Header: "EXECUTION STEPS - DRAFT (pending audit)"
2. Preamble: project name, target environment, prerequisites (tools, accounts, access, materials), configuration to set up before Step 1
3. Milestones in sequence with numbered steps
4. Final verification: end-to-end test of the complete output against the success criteria from the concept brief
5. Appendix: dependency/resource manifest

## Quality gate

Before writing, verify:
- Every step has Action, Detail, On failure, and Verify sections
- All references are complete and consistent across steps
- All creation steps include complete inline content (search for "[...]", "[insert...]", "[add...]", "[your X here]" patterns and replace each with the literal content)
- Every milestone has integration verification
- Sequence is strictly linear (every step's inputs come from prior steps or the preamble)
- Every step provides complete specification so the executor can proceed without interpretation or design decisions

Update PROGRESS.md upon completion.

## Critical constraints (reinforced)

1. **Complete inline content for every creation step.** The most common failure mode in Phase 4 is describing what to create instead of providing it. "Create a configuration file with the appropriate settings" fails. The full file contents, inline, succeeds. Search your output for brackets, "appropriate," "relevant," and "necessary" as indicators of incomplete content.
2. **Every step is atomic and independently verifiable.** If a step contains "and" connecting two distinct actions, split it. If a step's verification checks only one of its two actions, it's a compound step that needs splitting.
3. **Re-read the refined plan from disk before starting.** The plan is the sole source of truth. Every detail in the execution steps must trace back to a specific element of the refined plan. If you find yourself adding capabilities or scope beyond the plan, stop: that's scope creep, and it belongs in a future iteration.
