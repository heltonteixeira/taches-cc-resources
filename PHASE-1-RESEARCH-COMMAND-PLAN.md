---
phase: 01-research-command-mvp
type: execute
priority: high
estimated-effort: small
---

<objective>
Create a standalone `/research` command that provides immediate, discoverable access to research capabilities.

Purpose: Make research functionality accessible without requiring users to understand create-meta-prompts or create-plans workflows. This MVP establishes the foundation for consolidated research patterns.

Output:
- `/research` command in commands/research.md
- Working integration with existing .prompts/ structure
- Documentation in README.md
</objective>

<execution_context>
This is Phase 1 of 3 for implementing a comprehensive research feature. This phase focuses on immediate user value with minimal effort by creating a command wrapper that leverages existing research patterns.

Success unlocks: Phase 2 (research skill consolidation) and Phase 3 (research agent specialization).
</execution_context>

<context>
**Existing Research Capabilities:**
@skills/create-meta-prompts/references/research-patterns.md - Proven research patterns with quality controls
@skills/create-meta-prompts/references/research-pitfalls.md - Known mistakes and prevention strategies
@skills/create-meta-prompts/SKILL.md - Meta-prompting workflow that includes research

**Command Architecture:**
@commands/create-prompt.md - Example of standalone command with intake and prompt generation
@commands/create-meta-prompt.md - Example of command wrapping a skill
@commands/create-plan.md - Example of command with arguments

**Project Documentation:**
@README.md - Will need update to include /research command
</context>

<tasks>

<task type="auto">
  <name>Task 1: Create research command file with YAML frontmatter</name>
  <files>commands/research.md</files>
  <action>
Create commands/research.md with:
- YAML frontmatter: description, argument-hint, allowed-tools
- Description: "Perform targeted research with systematic verification and quality controls. Use for technology evaluation, API investigation, best practices research, and feasibility analysis."
- Argument-hint: "[research topic or question]"
- Allowed-tools: [Read, Write, Glob, WebSearch, WebFetch, Grep, AskUserQuestion, Task]
- Include access to MCP tools (mcp__*) for library documentation

Why WebSearch and WebFetch are critical: Research requires external knowledge gathering. Why Task tool: May spawn general-purpose agent for complex research. Why AskUserQuestion: For research type clarification and confidence gates.
  </action>
  <verify>File exists at commands/research.md with valid YAML frontmatter</verify>
  <done>YAML frontmatter valid, description clear and includes "when to use", tools appropriate for research tasks</done>
</task>

<task type="auto">
  <name>Task 2: Implement research command intake and routing</name>
  <files>commands/research.md</files>
  <action>
Add intake logic that:
1. Checks if $ARGUMENTS provided (topic/question)
2. If empty, uses AskUserQuestion to determine research type:
   - "Technology Research" - Libraries, frameworks, tools evaluation
   - "API/Service Research" - External service investigation
   - "Best Practices Research" - Standards, patterns, methodologies
   - "Comparison Research" - Evaluate multiple options
   - "Feasibility Research" - Assess if something is possible/practical
3. After type selection, gather specifics via AskUserQuestion:
   - Research scope (what to include/exclude)
   - Depth needed (quick overview vs thorough investigation)
   - Output preference (markdown report vs structured XML)
4. Decision gate: "Ready to proceed with research?"

Why this intake: Matches research-patterns.md types. Structured intake ensures quality controls are applied appropriately.
  </action>
  <verify>Command has clear intake flow with AskUserQuestion for type and scope clarification</verify>
  <done>Intake asks appropriate questions for each research type, includes decision gate before proceeding</done>
</task>

<task type="auto">
  <name>Task 3: Implement quality controls and verification checklists</name>
  <files>commands/research.md</files>
  <action>
Embed quality controls from research-patterns.md:

1. Verification Checklist (adapts based on research type):
   - Enumerate known options/approaches explicitly
   - Verify negative claims with official docs
   - Cross-reference multiple authoritative sources
   - Check publication dates (prefer current year)
   - Document tool/environment-specific variations

2. Research Quality Assurance:
   - Completeness check (all enumerated items documented)
   - Source verification (official docs cited)
   - Blind spots review ("What might I have missed?")
   - Critical claims audit (verify "X is not possible" statements)

3. Pre-submission checklist:
   - All scope coverage verified
   - Claims have source URLs
   - Confidence levels assigned with justification
   - Assumptions distinguished from verified facts

Why these controls: Prevent common research pitfalls documented in research-pitfalls.md (configuration scope assumptions, deprecated vs current confusion, single-source verification).
  </action>
  <verify>Command includes verification checklist, quality assurance steps, and pre-submission checklist embedded in generated research prompt</verify>
  <done>Quality controls match research-patterns.md, adapted for command context (not meta-prompt), prevent known pitfalls</done>
</task>

<task type="auto">
  <name>Task 4: Implement incremental output writing</name>
  <files>commands/research.md</files>
  <action>
Add incremental writing instructions to prevent token limit failures:

1. Initialize research output file early with skeleton structure
2. Write findings incrementally as discovered (not all at end)
3. Use Edit tool to append findings one at a time
4. Update metadata section at completion

Structure to initialize:
```xml
<research>
  <summary>[Will complete at end]</summary>
  <findings></findings>
  <recommendations></recommendations>
  <code_examples></code_examples>
  <metadata>
    <quality_report></quality_report>
  </metadata>
</research>
```

Why incremental: Prevents losing work if token limit hit during research. Proven effective in create-meta-prompts.
  </action>
  <verify>Command instructs to create output file early and append findings incrementally</verify>
  <done>Incremental writing pattern matches research-patterns.md workflow, includes skeleton structure initialization</done>
</task>

<task type="auto">
  <name>Task 5: Implement output structure and SUMMARY.md creation</name>
  <files>commands/research.md</files>
  <action>
Define research output structure:

1. Primary output: `research-[topic].md` or `.prompts/NNN-[topic]-research/[topic]-research.md`
   - Check for .prompts/ directory existence
   - If exists: use numbered structure (compatible with create-meta-prompts)
   - If not exists: create standalone research-[topic].md in current directory

2. XML structure with required sections:
   - `<summary>` - 2-3 paragraph executive summary
   - `<findings>` - Individual findings with category, detail, source, relevance
   - `<recommendations>` - Actionable recommendations with priority
   - `<code_examples>` - Relevant patterns and snippets
   - `<metadata>` - Confidence, dependencies, open_questions, assumptions, quality_report

3. SUMMARY.md creation (for quick scanning):
   - **One-liner** - Substantive description (not "research completed")
   - **Key Findings** - 3-5 actionable takeaways
   - **Confidence** - High/Medium/Low with justification
   - **Sources Consulted** - List of URLs
   - **Decisions Needed** - What requires user input
   - **Next Step** - Concrete forward action

Why this structure: XML for Claude parsing, SUMMARY.md for human scanning. Compatible with existing .prompts/ if it exists.
  </action>
  <verify>Command defines clear output structure with XML for research output and markdown for SUMMARY.md</verify>
  <done>Output structure matches research-patterns.md, adaptable to .prompts/ or standalone, SUMMARY.md has all required sections</done>
</task>

<task type="auto">
  <name>Task 6: Implement confidence assessment and gates</name>
  <files>commands/research.md</files>
  <action>
Add confidence assessment and decision gates:

1. After research completion, assess confidence level:
   - HIGH: Official docs verified, multiple sources, current information
   - MEDIUM: Some official sources, some inference, mostly current
   - LOW: Limited sources, assumptions made, uncertain currency

2. Confidence gates (from create-plans/workflows/research-phase.md):
   - If LOW confidence: Present options to user
     - "Dig deeper" - Continue research before concluding
     - "Proceed anyway" - Accept uncertainty with documented caveats
     - "Pause" - Stop for user review
   - If MEDIUM confidence: Inline confirmation
     - "Research complete (medium confidence: [reason]). Sufficient for next step?"
   - If HIGH confidence: Proceed, just note confidence level

3. Open questions gate:
   - If critical questions remain unanswered, present to user
   - Options: "Address questions first" or "Acknowledge and proceed"

Why confidence gates: Prevents low-quality research from being used as basis for decisions. Matches create-plans pattern.
  </action>
  <verify>Command includes confidence assessment logic and appropriate gates for LOW/MEDIUM confidence</verify>
  <done>Confidence levels defined, gates match create-plans pattern, user has appropriate control over uncertainty</done>
</task>

<task type="checkpoint:decision" gate="blocking">
  <decision>Output directory structure for research command</decision>
  <context>
The /research command needs to decide where to save research outputs. Two approaches are viable:

**Current state**:
- create-meta-prompts uses .prompts/NNN-topic-purpose/ structure
- Standalone research might not need full .prompts/ infrastructure

**Decision needed**: Where should /research save outputs?
  </context>
  <options>
    <option id="prompts-structure">
      <name>Use .prompts/ structure (compatible mode)</name>
      <pros>
- Full compatibility with create-meta-prompts outputs
- Research can be chained into plan → implement workflows
- Consistent numbering and organization
- Future-proof for Phase 2/3 integration
      </pros>
      <cons>
- Creates .prompts/ directory if doesn't exist (may confuse users not using meta-prompts)
- Slightly more complex file paths
- Requires numbering logic
      </cons>
    </option>
    <option id="standalone">
      <name>Standalone files in current directory</name>
      <pros>
- Simple and direct (research-[topic].md)
- No directory creation needed
- Clear for users who just want research results
- Easier to find and read
      </pros>
      <cons>
- Not compatible with create-meta-prompts workflows
- Can't chain to /run-prompt easily
- Less organized if user does multiple research tasks
- May need migration to .prompts/ later for Phase 2
      </cons>
    </option>
    <option id="adaptive">
      <name>Adaptive (check for .prompts/, use it if exists, else standalone)</name>
      <pros>
- Best of both worlds
- Compatible with existing workflows when .prompts/ exists
- Simple standalone when .prompts/ doesn't exist
- No assumptions about user's workflow
      </pros>
      <cons>
- Slightly more complex logic
- Different output locations depending on context
- Users need to understand why location varies
      </cons>
    </option>
  </options>
  <resume-signal>
Select output structure: "prompts-structure", "standalone", or "adaptive"

Recommendation: "adaptive" provides compatibility without forcing .prompts/ on users who don't need it.
  </resume-signal>
</task>

<task type="auto">
  <name>Task 7: Add research-specific tool usage guidance</name>
  <files>commands/research.md</files>
  <action>
Include tool usage patterns for research tasks:

1. WebSearch usage:
   - Always include {current_year} in queries for currency
   - Multiple search queries for thorough coverage
   - Examples: "{topic} best practices {current_year}", "{library} vs {alternative} {current_year}"

2. WebFetch usage:
   - Prefer for official documentation (exact URLs)
   - Always verify publication dates
   - Cross-reference multiple official sources

3. MCP tool usage (if available):
   - Use mcp__context7__resolve-library-id for library research
   - Use mcp__context7__get-library-docs for current API patterns
   - Prefer MCP over web search for authoritative library docs

4. Source citation requirements:
   - Every finding must have source URL
   - Publication/update date required
   - Authority level noted (official vs community)

Why explicit tool guidance: Ensures consistent research quality, prevents "search for X" vagueness pitfall from research-pitfalls.md.
  </action>
  <verify>Command includes specific tool usage patterns for WebSearch, WebFetch, and MCP</verify>
  <done>Tool usage guidance comprehensive, includes source citation requirements, prevents common pitfalls</done>
</task>

<task type="auto">
  <name>Task 8: Update README.md to document /research command</name>
  <files>README.md</files>
  <action>
Add /research command to Commands section in README.md:

**Location**: After the "Meta-Prompting" subsection, add new "Research" subsection

**Content**:
```markdown
### Research

Perform targeted research with systematic verification and quality controls.

- [`/research`](./commands/research.md) - Conduct structured research with quality controls
```

**Full description for commands section**:
Under Commands > Research, describe:
- What: Targeted research with systematic verification
- When to use: Technology evaluation, API investigation, best practices, feasibility analysis
- Output: Structured research.md with findings, recommendations, confidence levels
- Quality controls: Verification checklists, source citation, blind spots review
- Related: Works standalone or with create-meta-prompts for research → plan → implement workflows

Why prominent placement: Research is a fundamental capability that should be easily discoverable. Placement after meta-prompting shows relationship while maintaining independence.
  </action>
  <verify>README.md contains /research command in Commands section with clear description</verify>
  <done>README.md updated, /research documented clearly, relationship to other commands explained</done>
</task>

<task type="auto">
  <name>Task 9: Create example research workflow in command documentation</name>
  <files>commands/research.md</files>
  <action>
Add example workflow to command documentation showing:

1. Simple invocation:
```
/research Best JWT authentication libraries for Node.js
```

2. Interactive flow:
- Type selection → "Technology Research"
- Scope questions → Include: security, performance, maintenance. Exclude: implementation details
- Depth → "Thorough investigation"
- Decision gate → "Proceed"

3. Research execution:
- Verification checklist shown
- Quality controls applied
- Incremental writing demonstrated
- Sources documented

4. Output example:
- research-jwt-libraries.md created
- SUMMARY.md shows key findings
- Confidence level: HIGH
- Actionable recommendation provided

Why include example: Helps users understand expected workflow and output quality. Demonstrates value proposition clearly.
  </action>
  <verify>Command documentation includes complete example workflow from invocation to output</verify>
  <done>Example workflow comprehensive, shows full flow, demonstrates quality controls in action</done>
</task>

</tasks>

<verification>
Before declaring Phase 1 complete:

- [ ] commands/research.md exists with valid YAML frontmatter
- [ ] Command has intake flow with research type selection
- [ ] Quality controls embedded (verification checklist, quality assurance, pre-submission)
- [ ] Incremental writing pattern implemented
- [ ] Output structure defined (XML + SUMMARY.md)
- [ ] Confidence gates implemented (LOW/MEDIUM/HIGH)
- [ ] Tool usage guidance included (WebSearch, WebFetch, MCP)
- [ ] README.md updated with /research command
- [ ] Example workflow documented
- [ ] Test invocation: `/research Test topic` produces appropriate prompts
- [ ] Output compatible with existing .prompts/ structure (if selected)
- [ ] No errors or undefined references
</verification>

<success_criteria>
- /research command functional and documented
- Research quality matches create-meta-prompts standards
- Output compatible with existing .prompts/ structure
- User can invoke /research and get high-quality research results
- Quality controls prevent common pitfalls (from research-pitfalls.md)
- Confidence assessment provides appropriate user gates
- Command discoverable via README.md
- Foundation established for Phase 2 (skill consolidation)
</success_criteria>

<output>
After Phase 1 completion, create `PHASE-1-SUMMARY.md`:

# Phase 1: Research Command (MVP) - Summary

**Standalone /research command delivers immediate research capabilities**

## Accomplishments
- Created /research command with comprehensive intake and routing
- Embedded quality controls from proven research-patterns.md
- Implemented incremental writing to prevent token failures
- Added confidence gates for research quality assurance
- Documented in README.md for discoverability
- Compatible with existing .prompts/ structure

## Files Created/Modified
- `commands/research.md` - New research command with full implementation
- `README.md` - Updated Commands section with Research subsection

## Decisions Made
- Output directory structure: [prompts-structure/standalone/adaptive] chosen
- Quality controls: Full research-patterns.md verification embedded
- Confidence gates: LOW/MEDIUM/HIGH with appropriate user decision points

## Issues Encountered
[Document any issues and resolutions, or "None"]

## Next Step
Ready for Phase 2: Research Skill (Consolidated) - Extract research patterns into reusable skill that /research and other skills can invoke.
</output>

<notes>
**Why Phase 1 First:**
- Immediate user value (discoverability)
- Minimal effort (command wrapper)
- Validates demand for standalone research
- Tests quality controls in simplified context
- Foundation for Phase 2 consolidation

**Phase 1 → Phase 2 Transition:**
Phase 2 will extract the research logic from this command into a dedicated skill, then update /research to invoke that skill. This consolidation makes research patterns reusable by create-meta-prompts, create-plans, and debug-like-expert.

**Integration Points:**
- Compatible with .prompts/ structure (create-meta-prompts)
- Quality controls can be referenced by create-plans
- Tool usage patterns inform Phase 3 agent optimization
</notes>
