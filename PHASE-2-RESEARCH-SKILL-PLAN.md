---
phase: 02-research-skill-consolidated
type: execute
priority: high
estimated-effort: medium
depends-on: PHASE-1-RESEARCH-COMMAND-PLAN.md
---

<objective>
Create a dedicated research skill that consolidates research patterns into a single source of truth, making research methodology reusable across all skills and commands.

Purpose: Eliminate duplication of research patterns across create-meta-prompts, create-plans, and debug-like-expert. Establish research skill as the authoritative source for research methodology that other components can invoke or reference.

Output:
- skills/research/ directory with full skill structure
- SKILL.md with router pattern (intake → workflows)
- Consolidated references/ from existing skills
- Workflows for each research type
- Updated /research command to invoke skill
- Updated create-meta-prompts and create-plans to reference/invoke skill
</objective>

<execution_context>
This is Phase 2 of 3 for implementing comprehensive research feature. Phase 1 (research command) is complete and provides standalone research capability. This phase consolidates methodology into reusable skill.

Prerequisites: Phase 1 complete (commands/research.md exists)
Success unlocks: Phase 3 (research agent optimization), full skill ecosystem integration
</execution_context>

<context>
**Phase 1 Outputs:**
@commands/research.md - Working research command to be refactored

**Existing Research Patterns (to consolidate):**
@skills/create-meta-prompts/references/research-patterns.md - Comprehensive research patterns
@skills/create-meta-prompts/references/research-pitfalls.md - Known mistakes (already shared)
@skills/create-meta-prompts/references/intelligence-rules.md - Extended thinking triggers
@skills/create-plans/workflows/research-phase.md - Research workflow for planning
@skills/debug-like-expert/references/when-to-research.md - Research strategy for investigation

**Skill Architecture:**
@skills/create-agent-skills/SKILL.md - How to build skills (router pattern)
@skills/create-agent-skills/references/skill-structure.md - Structural requirements
@skills/create-plans/SKILL.md - Example of complex router-based skill

**Commands to Update:**
@commands/research.md - Will wrap research skill
@commands/create-meta-prompt.md - Example of command wrapping skill
</context>

<tasks>

<task type="auto">
  <name>Task 1: Create skill directory structure</name>
  <files>skills/research/</files>
  <action>
Create skills/research/ directory structure following router pattern:

```
skills/research/
├── SKILL.md              # Router + essential principles (<500 lines)
├── workflows/            # Step-by-step procedures for each research type
│   ├── technology-research.md
│   ├── api-research.md
│   ├── best-practices-research.md
│   ├── comparison-research.md
│   ├── feasibility-research.md
│   └── quick-research.md
├── references/           # Consolidated domain knowledge
│   ├── research-patterns.md      # Extracted from create-meta-prompts
│   ├── research-pitfalls.md      # Symlink to shared file
│   ├── quality-controls.md       # Verification checklists, audits
│   ├── output-formats.md         # XML structure, SUMMARY.md specs
│   ├── tool-usage.md             # WebSearch, WebFetch, MCP patterns
│   └── confidence-assessment.md  # How to assess and gate on confidence
└── templates/
    ├── research-output.md        # XML structure template
    └── summary-template.md       # SUMMARY.md template

Why this structure:
- SKILL.md is router only (stays under 500 lines per best practices)
- Workflows are executable procedures Claude follows
- References are knowledge Claude reads for context
- Templates are structures Claude copies and fills
- Matches create-plans pattern (proven effective)
```

Create all directories (contents in subsequent tasks).
  </action>
  <verify>Directory structure exists: skills/research/ with SKILL.md, workflows/, references/, templates/ subdirectories</verify>
  <done>All directories created, structure follows router pattern, matches skills/create-plans/ organization</done>
</task>

<task type="auto">
  <name>Task 2: Create SKILL.md with router pattern</name>
  <files>skills/research/SKILL.md</files>
  <action>
Create SKILL.md following create-agent-skills/references/use-xml-tags.md structure:

**YAML Frontmatter:**
```yaml
---
name: research
description: Perform systematic research with quality controls and verification. Use for technology evaluation, API investigation, best practices research, feasibility analysis, or when other skills need research capabilities. Provides structured outputs with confidence assessment and source verification.
---
```

**Essential Principles (always applies):**
```xml
<essential_principles>

<principle name="systematic_verification">
Research is only as good as its verification. Every claim requires:
- Source URL with publication date
- Cross-reference from multiple authoritative sources
- Explicit confidence level (High/Medium/Low)
- Distinction between verified facts and assumptions
</principle>

<principle name="quality_over_speed">
Thorough research with proper verification beats quick research with gaps.
Apply all quality controls: verification checklists, blind spots review, critical claims audit.
Known pitfalls documented in references/research-pitfalls.md must be avoided.
</principle>

<principle name="incremental_writing">
Write findings to output file incrementally as discovered, not all at end.
This prevents token limit failures and ensures zero lost work.
Initialize skeleton early, append findings progressively, update metadata at completion.
</principle>

<principle name="confidence_gates">
Low-confidence research should not proceed to implementation without user awareness.
- HIGH: Official docs verified, multiple sources, current
- MEDIUM: Some official sources, some inference, mostly current
- LOW: Limited sources, assumptions made, uncertain currency
Gate on LOW/MEDIUM for user decision.
</principle>

<principle name="reusable_by_design">
This skill is designed to be invoked by other skills and commands.
Outputs are structured (XML) for Claude parsing and include SUMMARY.md for human scanning.
Compatible with .prompts/ structure and .planning/ structure.
</principle>

</essential_principles>
```

**Intake (adaptive based on invocation context):**
```xml
<intake>
Detect invocation context:

IF invoked WITH arguments (topic/question provided):
→ Infer research type from keywords, proceed to adaptive_analysis

IF invoked WITHOUT arguments:
→ Use AskUserQuestion:
  - header: "Research type"
  - question: "What type of research do you need?"
  - options:
    - "Technology" - Libraries, frameworks, tools evaluation
    - "API/Service" - External service investigation, endpoints, auth
    - "Best Practices" - Standards, patterns, methodologies
    - "Comparison" - Evaluate multiple options against criteria
    - "Feasibility" - Assess if something is possible/practical
    - "Quick Research" - Fast overview without deep verification

After type selection, gather specifics, then route to appropriate workflow.
</intake>
```

**Routing:**
```xml
<routing>
| Research Type | Workflow |
|---------------|----------|
| "Technology", "tech", "library", "framework", "tool" | workflows/technology-research.md |
| "API", "service", "endpoint", "authentication" | workflows/api-research.md |
| "Best practices", "patterns", "standards", "OWASP" | workflows/best-practices-research.md |
| "Comparison", "vs", "evaluate options", "compare" | workflows/comparison-research.md |
| "Feasibility", "is it possible", "can we", "should we" | workflows/feasibility-research.md |
| "Quick", "fast", "overview", "summary" | workflows/quick-research.md |

**After reading the workflow, follow it exactly.**
</routing>
```

Why this structure:
- Pure XML (no markdown headings in body) per skills/create-agent-skills/references/use-xml-tags.md
- Essential principles encode critical research quality rules
- Intake is adaptive (with/without arguments)
- Routing is clear and keyword-based
- Under 500 lines (router only, details in workflows/references)
  </action>
  <verify>skills/research/SKILL.md exists with valid YAML, pure XML structure, essential principles, intake, routing</verify>
  <done>SKILL.md follows router pattern, under 500 lines, pure XML structure, routing to 6 workflow types</done>
</task>

<task type="auto">
  <name>Task 3: Extract and consolidate research-patterns.md</name>
  <files>skills/research/references/research-patterns.md</files>
  <action>
Extract research patterns from create-meta-prompts and consolidate into authoritative reference:

**Source**: skills/create-meta-prompts/references/research-patterns.md

**Extract and organize**:
1. Overview section (what research patterns are for)
2. Prompt template (structured research prompt format)
3. Research scope patterns (include/exclude, sources, verification checklist)
4. Quality assurance framework (completeness, source verification, blind spots, critical claims)
5. Output structure (XML format with required sections)
6. Incremental output workflow (step-by-step write-as-you-go)
7. Summary requirements (SUMMARY.md structure)
8. Research types (technology, best practices, API, comparison) with examples

**Enhancements**:
- Add cross-references to research-pitfalls.md
- Add references to confidence-assessment.md (created in next task)
- Add tool-usage.md reference for WebSearch/WebFetch/MCP patterns
- Clarify that this is THE authoritative source (single source of truth)

**Important**: This file should be THE reference for research patterns. create-meta-prompts will reference this file instead of duplicating content.

Why consolidate here:
- Single source of truth for research methodology
- Eliminates duplication between create-meta-prompts and create-plans
- Other skills can reference this authoritative source
- Easier to maintain and update patterns
  </action>
  <verify>skills/research/references/research-patterns.md exists, contains consolidated patterns from create-meta-prompts</verify>
  <done>Research patterns consolidated, enhanced with cross-references, clearly marked as authoritative source</done>
</task>

<task type="auto">
  <name>Task 4: Create quality-controls.md reference</name>
  <files>skills/research/references/quality-controls.md</files>
  <action>
Create comprehensive quality controls reference extracted from research-patterns.md:

**Contents**:
1. Verification Checklist Framework
   - How to enumerate known options/scopes
   - Configuration scope verification (user/project/local/environment)
   - Tool-specific variation checking
   - Version and publication date verification

2. Quality Assurance Steps
   - Completeness check (all enumerated items covered)
   - Source verification (official docs cited with URLs)
   - Blind spots review (self-audit: "What might I have missed?")
   - Critical claims audit (verify negative claims)

3. Pre-Submission Checklist
   - Scope coverage verified
   - Claims have source URLs
   - Confidence levels assigned
   - Assumptions distinguished from facts

4. Red Flags to Watch For (from research-pitfalls.md)
   - Zero "not found" results (suspiciously perfect)
   - No confidence indicators (all findings equally certain)
   - Missing URLs ("according to documentation..." without link)
   - Definitive statements without evidence
   - Incomplete enumeration (checklist lists 4, output covers 2)

5. Quality Report Template
   - Sources consulted (with URLs)
   - Claims verified vs assumed
   - Contradictions encountered and resolution
   - Confidence by finding (granular assessment)

Why separate file:
- Quality controls are cross-cutting (apply to all research types)
- Can be referenced by specific workflows as needed
- Clear, focused responsibility (this file = quality, not methodology)
  </action>
  <verify>skills/research/references/quality-controls.md exists with comprehensive quality frameworks</verify>
  <done>Quality controls comprehensive, includes verification checklist, QA steps, red flags, quality report template</done>
</task>

<task type="auto">
  <name>Task 5: Create confidence-assessment.md reference</name>
  <files>skills/research/references/confidence-assessment.md</files>
  <action>
Create confidence assessment guide extracted from create-plans/workflows/research-phase.md and research-patterns.md:

**Contents**:
1. Confidence Level Definitions
   - HIGH: Official docs verified, multiple authoritative sources, current (2024-2025), no contradictions
   - MEDIUM: Some official sources, some inference, mostly current, minor contradictions resolved
   - LOW: Limited sources, significant assumptions, uncertain currency, unresolved contradictions

2. Confidence Assessment Criteria
   - Source authority (official docs > community blog)
   - Source currency (current year > old documentation)
   - Source agreement (cross-referenced > single source)
   - Verification depth (hands-on tested > documentation review)
   - Claim strength (specific examples > general statements)

3. Confidence Gates (from create-plans)
   - LOW confidence → AskUserQuestion:
     - "Dig deeper" - Continue research with expanded scope
     - "Proceed anyway" - Accept uncertainty with documented caveats
     - "Pause" - Stop for user review and guidance
   - MEDIUM confidence → Inline confirmation:
     - "Research complete (medium confidence: [reason]). Sufficient?"
   - HIGH confidence → Proceed:
     - Note confidence level, proceed to next step

4. Open Questions Handling
   - If critical questions unanswered → Present to user
   - Options: "Address first" or "Acknowledge and proceed"
   - Document open questions in research output

5. Confidence by Finding (Granular Assessment)
   - Not all findings have same confidence
   - Assign confidence to individual findings
   - Overall confidence is conservative (lowest critical finding)

Why separate file:
- Confidence assessment is specific domain knowledge
- Used by all workflows for quality gating
- Clear criteria prevent subjective assessment
  </action>
  <verify>skills/research/references/confidence-assessment.md exists with level definitions, criteria, gates, open questions handling</verify>
  <done>Confidence assessment comprehensive, includes definitions, criteria, gates matching create-plans pattern</done>
</task>

<task type="auto">
  <name>Task 6: Create tool-usage.md reference</name>
  <files>skills/research/references/tool-usage.md</files>
  <action>
Create tool usage patterns for research tasks:

**Contents**:
1. WebSearch Patterns
   - Always include {current_year} for currency
   - Multiple queries for thorough coverage
   - Examples: "{topic} best practices {current_year}", "{library} security {current_year}"
   - Avoid single query bias

2. WebFetch Patterns
   - Prefer for official documentation (exact URLs)
   - Always verify publication date in fetched content
   - Cross-reference multiple official sources
   - Check for redirect handling

3. MCP Server Patterns
   - context7: Use for library documentation (resolve-library-id, get-library-docs)
   - Prefer MCP over WebSearch for authoritative library docs
   - Note when MCP unavailable (graceful degradation)

4. Source Citation Requirements
   - Every finding must have source URL
   - Publication/update date required
   - Authority level noted (official/community/blog)
   - Access date for web resources

5. Tool Selection Decision Tree
   - Known official docs URL → WebFetch
   - Library/framework research → MCP (if available), else WebSearch
   - Recent updates/changes → WebSearch with {current_year}
   - Comparison research → WebSearch multiple perspectives
   - Best practices → WebSearch + WebFetch official standards

6. Common Pitfalls (from research-pitfalls.md)
   - "Search for X" vagueness → Provide exact URLs
   - Single source reliance → Require cross-reference
   - Outdated results → Verify publication dates
   - Missing MCP opportunity → Check availability first

Why separate file:
- Tool usage is tactical (how to execute research)
- Research patterns are strategic (what to research)
- Can be updated as tools evolve
  </action>
  <verify>skills/research/references/tool-usage.md exists with patterns for WebSearch, WebFetch, MCP, citations</verify>
  <done>Tool usage comprehensive, includes decision tree, common pitfalls, source citation requirements</done>
</task>

<task type="auto">
  <name>Task 7: Create output-formats.md reference</name>
  <files>skills/research/references/output-formats.md</files>
  <action>
Create output format specifications:

**Contents**:
1. Primary Research Output (XML Structure)
   ```xml
   <research>
     <summary>2-3 paragraph executive summary</summary>
     <findings>
       <finding category="...">
         <title>...</title>
         <detail>...</detail>
         <source>URL with date</source>
         <relevance>Why this matters</relevance>
       </finding>
     </findings>
     <recommendations>
       <recommendation priority="high|medium|low">
         <action>What to do</action>
         <rationale>Why</rationale>
       </recommendation>
     </recommendations>
     <code_examples>
       <example name="...">
         ```language
         code here
         ```
         Source: URL
       </example>
     </code_examples>
     <metadata>
       <confidence level="high|medium|low">Justification</confidence>
       <dependencies>What's needed to act</dependencies>
       <open_questions>What remains uncertain</open_questions>
       <assumptions>What was assumed</assumptions>
       <quality_report>
         <sources_consulted>List URLs</sources_consulted>
         <claims_verified>Key verified findings</claims_verified>
         <claims_assumed>Findings based on inference</claims_assumed>
         <contradictions_encountered>Conflicts and resolution</contradictions_encountered>
       </quality_report>
     </metadata>
   </research>
   ```

2. SUMMARY.md Format
   - **One-liner** - Substantive description (not "research completed")
   - **Key Findings** - 3-5 actionable takeaways
   - **Confidence** - Level with justification
   - **Sources Consulted** - Numbered list of URLs
   - **Decisions Needed** - What requires user input
   - **Blockers** - External impediments
   - **Next Step** - Concrete forward action

3. File Naming Conventions
   - .prompts/ structure: `.prompts/NNN-[topic]-research/[topic]-research.md`
   - .planning/ structure: `.planning/phases/XX-name/FINDINGS.md`
   - Standalone: `research-[topic].md`
   - SUMMARY always: `SUMMARY.md` in same directory

4. Compatibility Requirements
   - XML parseable by Claude (for chaining to plan/implement)
   - SUMMARY.md readable by humans (quick scanning)
   - Works with create-meta-prompts (.prompts/)
   - Works with create-plans (.planning/)
   - Works standalone (current directory)

Why separate file:
- Output formats are specifications (reference material)
- Multiple format contexts need clear documentation
- Can be used by workflows as templates to follow
  </action>
  <verify>skills/research/references/output-formats.md exists with XML structure, SUMMARY format, naming conventions</verify>
  <done>Output formats comprehensive, includes XML structure, SUMMARY template, compatibility with all contexts</done>
</task>

<task type="auto">
  <name>Task 8: Create technology-research.md workflow</name>
  <files>skills/research/workflows/technology-research.md</files>
  <action>
Create technology research workflow (most common type):

**Structure**:
```xml
<required_reading>
Load these references before starting:
- references/research-patterns.md (overall methodology)
- references/quality-controls.md (verification framework)
- references/tool-usage.md (WebSearch, WebFetch, MCP patterns)
- references/output-formats.md (XML structure to produce)
</required_reading>

<process>

<step name="define_scope">
Gather research parameters via AskUserQuestion:
- Technology name/category
- Research focus: security, performance, adoption, vs alternatives, all of above
- Include: Specific aspects to investigate
- Exclude: Out of scope items
- Depth: Quick overview vs thorough investigation
</step>

<step name="create_verification_checklist">
Based on technology type, enumerate what to verify:
- Known libraries/options in category (list explicitly)
- Key comparison dimensions (security, perf, adoption, maintenance)
- Official documentation locations
- Community resources (npm, GitHub, Stack Overflow)
- Recent updates (current year changelogs)
</step>

<step name="initialize_output">
Create output file early with skeleton structure (from output-formats.md).
Use incremental writing throughout research.
</step>

<step name="research_execution">
For each enumerated item in verification checklist:
1. Find official documentation (WebFetch or MCP)
2. Verify current status (active, deprecated, beta)
3. Check security advisories (GitHub, npm audit)
4. Review adoption metrics (npm downloads, GitHub stars)
5. Read recent comparisons (WebSearch with {current_year})
6. Document finding immediately (append to output file)
</step>

<step name="quality_assurance">
Apply quality controls from quality-controls.md:
- Completeness check (all enumerated items covered)
- Source verification (URLs documented)
- Blind spots review ("What did I miss?")
- Critical claims audit (verify negative claims)
</step>

<step name="assess_confidence">
Use confidence-assessment.md criteria to determine overall confidence level.
Apply appropriate gate for LOW/MEDIUM confidence.
</step>

<step name="create_summary">
Write SUMMARY.md with key findings, confidence, next step.
</step>

</process>

<success_criteria>
- All enumerated technologies/libraries researched
- Official docs cited for each
- Security status verified
- Comparison dimensions covered
- Confidence level assessed
- Quality report complete
- SUMMARY.md created
</success_criteria>
```

Why this workflow:
- Most requested research type (libraries, frameworks, tools)
- Demonstrates full research methodology
- Other workflows can reference this as example
  </action>
  <verify>skills/research/workflows/technology-research.md exists with complete process and success criteria</verify>
  <done>Technology research workflow comprehensive, includes all steps, references quality controls, demonstrates incremental writing</done>
</task>

<task type="auto">
  <name>Task 9: Create remaining workflow files</name>
  <files>
skills/research/workflows/api-research.md,
skills/research/workflows/best-practices-research.md,
skills/research/workflows/comparison-research.md,
skills/research/workflows/feasibility-research.md,
skills/research/workflows/quick-research.md
  </files>
  <action>
Create 5 additional workflow files following technology-research.md pattern:

**api-research.md** (API/service investigation):
- Focus: Endpoints, authentication, webhooks, rate limits, SDKs
- Verification: Current API version, deprecation timeline, sandbox availability
- Sources: Official API docs, SDK documentation, status page

**best-practices-research.md** (Standards and patterns):
- Focus: OWASP, official guidelines, industry standards
- Verification: Publication date, authority of source, current relevance
- Sources: Official standards bodies, OWASP, framework docs

**comparison-research.md** (Evaluate options):
- Focus: Define criteria, evaluate each option, synthesize recommendation
- Verification: All options evaluated against ALL criteria
- Sources: Official docs for each option, neutral comparisons

**feasibility-research.md** (Is it possible/practical):
- Focus: Technical constraints, existing solutions, effort estimation
- Verification: Official capabilities documented, limitations verified
- Sources: Official docs, release notes, known issue trackers

**quick-research.md** (Fast overview):
- Focus: High-level summary, skip deep verification
- Verification: Reduced (official sources only, no cross-reference required)
- Note: Confidence usually MEDIUM/LOW, appropriate for quick decisions

Each workflow follows same structure:
- required_reading
- process with steps
- success_criteria specific to research type

Why multiple workflows:
- Each research type has unique focus and verification needs
- Clear routing from SKILL.md based on user request
- Demonstrates skill flexibility and coverage
  </action>
  <verify>All 5 workflow files exist with appropriate focus and verification for each research type</verify>
  <done>5 workflows created, each tailored to research type, all follow same structure pattern</done>
</task>

<task type="auto">
  <name>Task 10: Create template files</name>
  <files>
skills/research/templates/research-output.md,
skills/research/templates/summary-template.md
  </files>
  <action>
Create templates that workflows reference:

**research-output.md** (XML structure to copy):
```xml
<research>
  <summary>
  [2-3 paragraph executive summary - write at END after all findings complete]

  Key insight 1...
  Key insight 2...
  Overall recommendation...
  </summary>

  <findings>
    <!-- Append findings incrementally as you discover them -->
    <finding category="category-name">
      <title>Finding title</title>
      <detail>Detailed explanation with specifics</detail>
      <source>https://exact-url (accessed YYYY-MM-DD)</source>
      <relevance>Why this matters for the research goal</relevance>
    </finding>
  </findings>

  <recommendations>
    <recommendation priority="high">
      <action>Specific action to take</action>
      <rationale>Why this is recommended based on findings</rationale>
    </recommendation>
  </recommendations>

  <code_examples>
    <example name="descriptive-name">
    ```language
    code snippet here
    ```
    Source: https://source-url
    Context: When to use this pattern
    </example>
  </code_examples>

  <metadata>
    <confidence level="high|medium|low">
      Justification for confidence level based on source quality, currency, and verification depth
    </confidence>
    <dependencies>
      What is needed to act on this research (tools, access, decisions)
    </dependencies>
    <open_questions>
      Questions that remain unanswered or need hands-on verification
    </open_questions>
    <assumptions>
      Assumptions made during research that should be validated
    </assumptions>
    <quality_report>
      <sources_consulted>
        1. https://source1 - Official documentation
        2. https://source2 - Release notes
        3. https://source3 - Community analysis
      </sources_consulted>
      <claims_verified>
        - Claim 1: Verified with official docs + multiple sources
        - Claim 2: Verified with official docs
      </claims_verified>
      <claims_assumed>
        - Assumption 1: Based on inference from docs
        - Assumption 2: Based on single source, needs verification
      </claims_assumed>
      <contradictions_encountered>
        - Contradiction 1: Source A says X, Source B says Y. Resolution: Z based on...
      </contradictions_encountered>
    </quality_report>
  </metadata>
</research>
```

**summary-template.md** (SUMMARY.md structure):
```markdown
# [Research Topic] Research Summary

**[Substantive one-liner - key finding or recommendation, NOT "research completed"]**

## Key Findings

- **Finding 1**: [Actionable insight with specifics]
- **Finding 2**: [Actionable insight with specifics]
- **Finding 3**: [Actionable insight with specifics]

## Confidence

**Level**: HIGH / MEDIUM / LOW

**Justification**: [Why this confidence level - source quality, currency, verification depth]

## Sources Consulted

1. [Official documentation] - https://url
2. [Release notes] - https://url
3. [Community analysis] - https://url

## Decisions Needed

- [Decision 1]: [What user needs to decide based on research]
- [Decision 2]: [What user needs to decide based on research]
- OR "None - ready to proceed"

## Blockers

- [External blocker 1]: [What's needed that's outside our control]
- OR "None"

## Next Step

[Concrete, specific next action - "Create implementation plan", "Evaluate option A vs B", "Test hypothesis X"]
```

Why templates:
- Workflows reference these for consistent output structure
- Easier to update format in one place
- Clear example of expected quality
  </action>
  <verify>Both template files exist with complete XML and markdown structures</verify>
  <done>Templates comprehensive, annotated with guidance, match output-formats.md specifications</done>
</task>

<task type="auto">
  <name>Task 11: Update /research command to invoke skill</name>
  <files>commands/research.md</files>
  <action>
Refactor commands/research.md to invoke research skill:

**Replace existing implementation with**:
```markdown
---
description: Perform systematic research with quality controls and verification. Use for technology evaluation, API investigation, best practices research, feasibility analysis.
argument-hint: [research topic or question]
allowed-tools:
  - Skill(research)
---

Invoke the research skill for: $ARGUMENTS
```

**Why this refactor**:
- Command becomes thin wrapper (3 lines of logic)
- All methodology now in skill (single source of truth)
- Command provides CLI interface, skill provides implementation
- Matches pattern: create-meta-prompt.md wraps create-meta-prompts skill

**Backward compatibility**:
- Command interface unchanged (/research [topic])
- Output location/format unchanged
- Quality controls unchanged (now from skill)
- User experience identical (invocation → result)

**Benefits**:
- Eliminates duplication (command had embedded research logic)
- Skill can be invoked by other skills directly
- Easier to maintain (update skill, command gets updates)
- Consistent with other commands (create-plan, create-meta-prompt)
  </action>
  <verify>commands/research.md refactored to Skill invocation, YAML frontmatter preserved</verify>
  <done>Command refactored, thin wrapper around skill, backward compatible, follows wrapper pattern</done>
</task>

<task type="auto">
  <name>Task 12: Update create-meta-prompts to reference research skill</name>
  <files>skills/create-meta-prompts/references/research-patterns.md</files>
  <action>
Update create-meta-prompts/references/research-patterns.md to reference authoritative source:

**Add at top of file**:
```xml
<authoritative_source>
The authoritative research patterns are now maintained in:
@skills/research/references/research-patterns.md

This file is kept for backward compatibility and specific meta-prompting context.
For general research methodology, reference the research skill.

To invoke research capability: Skill("research")
</authoritative_source>
```

**Options for file content**:
1. Keep full content + note (maintains backward compatibility)
2. Replace with redirect + meta-prompt-specific additions
3. Symlink to research skill version (filesystem-level)

**Recommendation**: Option 1 (keep full content + note)
- Zero breaking changes for existing meta-prompts
- Gradual migration path (can deprecate later)
- Meta-prompt-specific patterns can remain

**Why update this way**:
- Non-breaking (existing meta-prompts still work)
- Clear direction (authoritative source identified)
- Enables future consolidation (can remove duplication later)
  </action>
  <verify>create-meta-prompts/references/research-patterns.md updated with note pointing to authoritative source</verify>
  <done>File updated, authoritative source noted, backward compatibility maintained</done>
</task>

<task type="auto">
  <name>Task 13: Update create-plans to reference research skill</name>
  <files>skills/create-plans/workflows/research-phase.md</files>
  <action>
Update create-plans/workflows/research-phase.md to invoke research skill:

**Add option to invoke research skill**:
```xml
<execution_options>

<option name="invoke_research_skill" recommended="true">
Invoke the research skill for phase research:

Skill("research") with context:
- Research type: Inferred from phase (technology, API, best practices, feasibility)
- Scope: Phase unknowns from ROADMAP.md
- Output location: .planning/phases/XX-name/FINDINGS.md
- Integration: Results inform PLAN.md creation

Benefits:
- Full research methodology and quality controls
- Consistent with standalone research
- Single source of truth for patterns
</option>

<option name="embedded_workflow" legacy="true">
Continue using embedded research workflow in create-plans.

Use when:
- Research skill not available
- Custom planning-specific research needs
- Backward compatibility required
</option>

</execution_options>
```

**Update workflow to prefer skill invocation**:
- Check if research skill exists (Glob for skills/research/SKILL.md)
- If exists: Invoke Skill("research")
- If not exists: Use embedded workflow (graceful degradation)

**Why this approach**:
- Non-breaking (embedded workflow still available)
- Prefers consolidation (skill invocation recommended)
- Graceful degradation (works without research skill)
  </action>
  <verify>create-plans/workflows/research-phase.md updated to invoke research skill with fallback</verify>
  <done>Workflow updated, research skill invocation added, backward compatible with fallback</done>
</task>

<task type="checkpoint:decision" gate="blocking">
  <decision>Research skill discoverability and documentation approach</decision>
  <context>
The research skill is now created as reusable infrastructure. We need to decide how to document and communicate its dual purpose:
1. Direct invocation via /research command (user-facing)
2. Programmatic invocation by other skills (infrastructure)

**Options for documentation:**
  </context>
  <options>
    <option id="readme-skill-section">
      <name>Add research skill to Skills section in README.md</name>
      <pros>
- Full visibility of research capability
- Shows skill can be invoked directly: Skill("research")
- Demonstrates infrastructure nature
- Consistent with other skills documentation
      </pros>
      <cons>
- May confuse users ("Do I use /research or Skill('research')?")
- Duplicates information (already have /research in Commands)
- README becomes longer
      </cons>
    </option>
    <option id="readme-command-note">
      <name>Document only /research command, add note about skill</name>
      <pros>
- Simple user-facing documentation (/research is entry point)
- Note mentions "powered by research skill" for developers
- Cleaner README (one entry point documented)
- Users don't need to know implementation details
      </pros>
      <cons>
- Skill less discoverable for skill developers
- May not be obvious that other skills can invoke research
- Infrastructure nature hidden
      </cons>
    </option>
    <option id="both-documented">
      <name>Document both command and skill with clear relationship</name>
      <pros>
- Complete documentation
- Clear for both users and skill developers
- Shows command → skill relationship explicitly
- Enables both use cases
      </pros>
      <cons>
- Potentially redundant
- Need to maintain clarity about when to use which
- Longer documentation
      </cons>
    </option>
  </options>
  <resume-signal>
Select documentation approach: "readme-skill-section", "readme-command-note", or "both-documented"

Recommendation: "readme-command-note" for simplicity, with note: "Powered by the research skill, which can also be invoked directly by other skills using Skill('research')."
  </resume-signal>
</task>

<task type="auto">
  <name>Task 14: Update README.md with research skill documentation</name>
  <files>README.md</files>
  <action>
Based on decision from previous checkpoint, update README.md:

**If "readme-skill-section" selected**:
Add to Skills section:
```markdown
### [Research](./skills/research/)

Systematic research with quality controls and verification. Performs technology evaluation, API investigation, best practices research, and feasibility analysis with structured outputs and confidence assessment.

**Research types:** Technology, API/Service, Best Practices, Comparison, Feasibility, Quick Research

**Quality controls:** Verification checklists, source citation, blind spots review, confidence gates

**Invocation:** `Skill("research")` from other skills, or `/research [topic]` from command line

**Reusable infrastructure:** Consolidates research patterns from create-meta-prompts and create-plans. Single source of truth for research methodology.
```

**If "readme-command-note" selected**:
Update existing /research command entry:
```markdown
- [`/research`](./commands/research.md) - Conduct structured research with quality controls

  Powered by the research skill, which provides systematic verification, source citation, and confidence assessment. Can also be invoked directly by other skills using `Skill("research")` for reusable research capabilities.
```

**If "both-documented" selected**:
Add to both Commands and Skills sections with cross-references.

**Also update**:
- Total counts (27 commands → 27 or 28 depending on additions)
- Skills count (7 → 8)
  </action>
  <verify>README.md updated according to selected documentation approach</verify>
  <done>README.md reflects research skill documentation, counts updated, clear user guidance</done>
</task>

<task type="auto">
  <name>Task 15: Create research skill README.md</name>
  <files>skills/research/README.md</files>
  <action>
Create README.md for research skill (following create-plans/README.md pattern):

**Structure**:
```markdown
# research

**Systematic research with quality controls and verification**

Perform thorough research with proven methodology for technology evaluation, API investigation, best practices research, and feasibility analysis.

## Quick Start

\`\`\`
Skill("research")
\`\`\`

OR via command line:

\`\`\`
/research [topic or question]
\`\`\`

## Research Types

- **Technology Research**: Libraries, frameworks, tools evaluation
- **API/Service Research**: External service investigation, endpoints, authentication
- **Best Practices Research**: Standards, patterns, methodologies
- **Comparison Research**: Evaluate multiple options against criteria
- **Feasibility Research**: Assess if something is possible/practical
- **Quick Research**: Fast overview without deep verification

## Quality Controls

Built-in quality controls prevent common research pitfalls:

- **Verification checklists**: Enumerate all options before researching
- **Source citation**: Every finding has URL and publication date
- **Blind spots review**: "What might I have missed?" self-audit
- **Critical claims audit**: Verify "X is not possible" with official docs
- **Confidence assessment**: HIGH/MEDIUM/LOW with user gates
- **Quality reports**: Distinguish verified facts from assumptions

## Output Structure

**Primary output** (XML for Claude parsing):
- Structured findings with sources
- Recommendations with priority
- Code examples with context
- Metadata with confidence and quality report

**SUMMARY.md** (for human scanning):
- Key findings (3-5 actionable insights)
- Confidence level with justification
- Sources consulted
- Next step

## Integration

**Used by other skills**:
- create-meta-prompts: For research prompts in Claude-to-Claude pipelines
- create-plans: For phase research in project planning
- debug-like-expert: For external knowledge gathering

**Single source of truth**:
Consolidates research patterns from multiple skills. Other skills invoke this skill or reference its patterns.

## Files Reference

**Structure**:
- `SKILL.md` - Router + essential principles
- `workflows/` - Step-by-step procedures for each research type
- `references/` - Domain knowledge (patterns, quality controls, formats)
- `templates/` - Output structures

**Key references**:
- `references/research-patterns.md` - Authoritative research methodology
- `references/quality-controls.md` - Verification and QA frameworks
- `references/research-pitfalls.md` - Known mistakes to avoid
- `references/confidence-assessment.md` - How to assess quality
- `references/tool-usage.md` - WebSearch, WebFetch, MCP patterns

## Success Criteria

Research succeeds when:
- All verification checklist items completed
- Sources are current and authoritative
- Confidence level assessed with justification
- Quality controls applied (no shortcuts)
- Output structured for both Claude and human consumption
- SUMMARY.md created for quick scanning
```

Why README for skill:
- Follows create-plans pattern (complex skills have README)
- Explains invocation, integration, quality controls
- Reference for skill developers
  </action>
  <verify>skills/research/README.md exists with comprehensive documentation</verify>
  <done>README created, follows create-plans pattern, documents invocation and integration</done>
</task>

</tasks>

<verification>
Before declaring Phase 2 complete:

- [ ] skills/research/ directory structure complete (SKILL.md, workflows/, references/, templates/)
- [ ] SKILL.md follows router pattern with pure XML, under 500 lines
- [ ] All 6 workflows created (technology, api, best-practices, comparison, feasibility, quick)
- [ ] All reference files created (research-patterns, quality-controls, confidence-assessment, tool-usage, output-formats)
- [ ] research-pitfalls.md symlinked or referenced (shared across skills)
- [ ] Both templates created (research-output, summary-template)
- [ ] commands/research.md refactored to invoke skill
- [ ] create-meta-prompts updated to reference authoritative source
- [ ] create-plans updated to invoke research skill
- [ ] README.md updated with research documentation
- [ ] skills/research/README.md created
- [ ] Test invocation: Skill("research") routes correctly to workflows
- [ ] Test invocation: /research command works (wraps skill)
- [ ] No duplication of research patterns (single source of truth achieved)
- [ ] Backward compatibility maintained (existing skills still work)
</verification>

<success_criteria>
- Research skill is authoritative source for research methodology
- All research patterns consolidated (no duplication)
- Other skills can invoke research skill or reference its patterns
- /research command wraps skill (thin wrapper)
- Backward compatibility with create-meta-prompts and create-plans
- Documentation clear for both users and skill developers
- Quality controls from research-pitfalls.md embedded
- Foundation established for Phase 3 (research agent optimization)
</success_criteria>

<output>
After Phase 2 completion, create `PHASE-2-SUMMARY.md`:

# Phase 2: Research Skill (Consolidated) - Summary

**Research skill established as single source of truth for research methodology across all skills**

## Accomplishments
- Created skills/research/ with full router pattern structure
- Consolidated research patterns from create-meta-prompts and create-plans
- Built 6 specialized workflows for different research types
- Created comprehensive references (quality controls, confidence assessment, tool usage, output formats)
- Refactored /research command to invoke skill (thin wrapper)
- Updated create-meta-prompts and create-plans to reference/invoke research skill
- Maintained backward compatibility with all existing functionality
- Documented in README.md with clear user guidance

## Files Created/Modified
- `skills/research/SKILL.md` - Router with essential principles, intake, routing
- `skills/research/workflows/` - 6 workflow files (technology, api, best-practices, comparison, feasibility, quick)
- `skills/research/references/` - 5 reference files (patterns, quality-controls, confidence, tool-usage, formats)
- `skills/research/templates/` - 2 templates (research-output, summary-template)
- `skills/research/README.md` - Comprehensive skill documentation
- `commands/research.md` - Refactored to Skill invocation
- `skills/create-meta-prompts/references/research-patterns.md` - Added authoritative source note
- `skills/create-plans/workflows/research-phase.md` - Added research skill invocation option
- `README.md` - Updated with research documentation

## Decisions Made
- Output directory structure: [Decision from Phase 1 checkpoint]
- Documentation approach: [readme-skill-section / readme-command-note / both-documented]
- create-plans integration: Invoke research skill with fallback to embedded workflow
- create-meta-prompts integration: Reference authoritative source, maintain content for compatibility

## Issues Encountered
[Document any issues and resolutions, or "None"]

## Next Step
Ready for Phase 3: Research Agent (Optional) - Create specialized research agent for isolated contexts and advanced optimization.

## Impact
- **Single source of truth**: Research methodology now centralized in research skill
- **Reusability**: create-meta-prompts, create-plans, debug-like-expert can all leverage research skill
- **Maintainability**: Update research patterns once, all skills benefit
- **Discoverability**: Research capability now clearly documented and accessible
- **Quality**: Consolidated quality controls prevent common pitfalls across all research
</output>

<notes>
**Why Phase 2 Critical:**
- Eliminates duplication (DRY principle)
- Establishes clear ownership (research skill owns methodology)
- Enables ecosystem integration (all skills can leverage)
- Simplifies maintenance (single update point)

**Phase 2 → Phase 3 Transition:**
Phase 3 will create a specialized research agent that can be invoked by the research skill for isolated research contexts. This optimization is optional but beneficial for complex research requiring fresh context or specific model selection.

**Backward Compatibility Strategy:**
- No breaking changes to existing commands/skills
- Graceful degradation (fallbacks if research skill unavailable)
- Progressive enhancement (existing skills adopt research skill over time)
- Clear migration path (documented in each updated file)
</notes>
