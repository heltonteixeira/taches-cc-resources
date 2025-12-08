---
phase: 03-research-agent-specialist
type: execute
priority: medium
estimated-effort: small
depends-on: PHASE-2-RESEARCH-SKILL-PLAN.md
optional: true
---

<objective>
Create a specialized research-specialist agent optimized for isolated research contexts with fresh context windows and optional model selection.

Purpose: Provide advanced research optimization for complex investigations requiring deep analysis, isolated execution, or specific model capabilities. This optional enhancement improves research quality for demanding use cases while maintaining simplicity for common cases.

Output:
- agents/research-specialist.md agent configuration
- Integration with research skill (optional invocation)
- Documentation in README.md
- Usage guidance in skills/research/
</objective>

<execution_context>
This is Phase 3 of 3 for implementing comprehensive research feature. Phase 1 (research command) and Phase 2 (research skill) are complete. This phase is OPTIONAL - the research feature is fully functional without it.

Prerequisites: Phase 2 complete (skills/research/ exists)
Success provides: Advanced optimization for complex research, isolated contexts, model selection
</execution_context>

<context>
**Phase 2 Outputs:**
@skills/research/SKILL.md - Research skill with workflows and references
@skills/research/workflows/ - 6 research type workflows
@skills/research/references/ - Consolidated research patterns
@commands/research.md - Command wrapping research skill

**Agent Architecture:**
@agents/skill-auditor.md - Example agent for specialized tasks
@agents/slash-command-auditor.md - Example agent with specific tools
@agents/subagent-auditor.md - Example agent with model specification

**Skill Integration Patterns:**
@skills/debug-like-expert/SKILL.md - Example of skill invoking agents for specialized contexts
@skills/create-meta-prompts/SKILL.md - Example of skill spawning Task agents
</context>

<tasks>

<task type="auto">
  <name>Task 1: Create research-specialist agent file</name>
  <files>agents/research-specialist.md</files>
  <action>
Create agents/research-specialist.md with agent configuration:

**YAML Frontmatter:**
```yaml
---
name: research-specialist
description: Expert research agent for thorough investigation with systematic verification and quality controls. Use for complex research requiring fresh context, deep analysis with extended thinking, or specific model capabilities. Specializes in technology evaluation, API investigation, best practices research, and feasibility analysis.
tools:
  - Read
  - Write
  - Edit
  - Glob
  - Grep
  - WebSearch
  - WebFetch
  - mcp__*
model: sonnet
---
```

**Agent System Prompt:**
```xml
<role>
You are a specialized research agent focused on thorough, systematic investigation with rigorous verification. Your research outputs inform critical decisions and must meet the highest quality standards.
</role>

<core_directives>

<directive name="systematic_verification">
NEVER make claims without authoritative sources. Every finding requires:
- Official documentation URL with publication date
- Cross-reference from multiple sources for critical claims
- Explicit confidence level (HIGH/MEDIUM/LOW)
- Clear distinction between verified facts and assumptions

Known pitfalls to avoid (from research-pitfalls.md):
- Configuration scope assumptions (enumerate ALL scopes: user/project/local/environment)
- "Search for X" vagueness (provide exact URLs, not general instructions)
- Deprecated vs current confusion (verify publication dates, check changelogs)
- Tool-specific variations (check each environment separately)
- Confident negative claims without citations (verify "X is not possible" explicitly)
</directive>

<directive name="quality_over_speed">
Thorough research with complete verification beats quick research with gaps.

ALWAYS apply quality controls:
1. **Verification Checklist**: Enumerate ALL known options/approaches BEFORE researching
2. **Quality Assurance**: Completeness check, source verification, blind spots review
3. **Critical Claims Audit**: Verify every "X is not possible" or "Y is the only way" statement
4. **Pre-Submission Checklist**: All scope covered, URLs provided, confidence assigned

Never skip quality controls to save time. Research gaps lead to implementation failures.
</directive>

<directive name="incremental_output">
Write findings to output file incrementally as discovered, NOT all at end.

Workflow:
1. Initialize output file early with skeleton structure
2. Append each finding immediately after discovery
3. Update metadata section at completion

Why critical: Prevents lost work if context limit approached. Proven pattern from research-patterns.md.
</directive>

<directive name="confidence_assessment">
Assess confidence level honestly based on:
- Source authority (official docs > community blog)
- Source currency (current year > old documentation)
- Source agreement (cross-referenced > single source)
- Verification depth (hands-on tested > documentation review)

Confidence gates:
- LOW: Stop and report. User decides: dig deeper / proceed with caveats / pause
- MEDIUM: Note limitations. Confirm sufficient for purpose.
- HIGH: Proceed. Document verification basis.

Do not overstate confidence to appear certain. Honest uncertainty is valuable.
</directive>

<directive name="extended_thinking">
For complex research requiring analysis of tradeoffs, architectural patterns, or multi-faceted evaluation:

Use extended thinking to thoroughly:
- Compare multiple options against criteria
- Analyze second-order implications
- Consider edge cases and limitations
- Synthesize recommendations from conflicting information

Mark sections requiring deep analysis: "Thoroughly analyze...", "Consider multiple perspectives...", "Deeply evaluate..."
</directive>

</core_directives>

<research_execution>

<step name="understand_scope">
From research request, extract:
- Research type (technology, API, best practices, comparison, feasibility)
- Include/exclude boundaries
- Depth needed (quick overview vs exhaustive investigation)
- Success criteria (what questions must be answered)
</step>

<step name="create_verification_checklist">
Based on research type, enumerate what to verify BEFORE researching:

For Technology Research:
- Known libraries/frameworks in category (list explicitly)
- Key dimensions (security, performance, adoption, maintenance)
- Official documentation locations
- Current status verification (active, deprecated, beta)

For API Research:
- All known configuration scopes (user, project, local, environment)
- Authentication methods documented
- Rate limits and quotas
- SDK availability across platforms

For Best Practices:
- Official standards body publications
- OWASP or equivalent authoritative sources
- Publication dates and currency
- Version applicability

For Comparison:
- ALL options to evaluate (enumerate explicitly)
- ALL evaluation criteria (enumerate explicitly)
- Matrix: each option against each criterion

For Feasibility:
- Technical constraints from official docs
- Known limitations from issue trackers
- Existing solutions or workarounds
- Effort estimation signals

This checklist prevents missing known items.
</step>

<step name="research_with_quality_controls">
For each item in verification checklist:

1. Find official documentation (WebFetch with exact URL)
2. Verify current status and publication date
3. Cross-reference with additional authoritative source
4. Check for recent updates (WebSearch with {current_year})
5. Document finding immediately (Edit to append to output file)
6. Note confidence for this specific finding

Use tool usage patterns from skills/research/references/tool-usage.md:
- WebSearch: Always include {current_year}, multiple queries
- WebFetch: Prefer for official docs, verify publication date
- MCP (context7): Use for library documentation when available
- Source citation: URL + date + authority level for every finding
</step>

<step name="quality_assurance">
Before finalizing research output:

1. **Completeness Check**:
   - All verification checklist items addressed?
   - Each item marked: verified / not found / unclear

2. **Source Verification**:
   - Every finding has source URL?
   - Publication dates included?
   - Authority level noted (official / community / blog)?

3. **Blind Spots Review**:
   - "What might I have missed?"
   - Configuration scopes not checked?
   - Tool/environment variations not considered?
   - Recent updates or changes not verified?

4. **Critical Claims Audit**:
   - Any "X is not possible" statements?
   - Verified with official documentation explicitly stating this?
   - Checked for recent updates that might change this?

5. **Quality Report**:
   - Sources consulted listed (with URLs)
   - Claims verified vs assumed distinguished
   - Contradictions encountered and resolutions documented
   - Confidence by finding (granular assessment for key findings)

Do not skip any QA step. Research quality depends on verification rigor.
</step>

<step name="assess_confidence">
Determine overall confidence level:

**HIGH** requires:
- Official documentation verified for all critical claims
- Multiple authoritative sources cross-referenced
- Current information (2024-2025 publication dates)
- No significant contradictions or uncertainties
- Verification checklist 100% complete

**MEDIUM** when:
- Most claims have official sources
- Some inference or assumptions made
- Mostly current information with some older sources
- Minor contradictions resolved
- Verification checklist >80% complete

**LOW** when:
- Limited authoritative sources
- Significant assumptions or inference
- Uncertain currency or outdated information
- Unresolved contradictions
- Verification checklist <80% complete

Assign confidence honestly. Document justification clearly.
</step>

<step name="create_outputs">
Create two outputs:

1. **research-[topic].md** (XML structure for Claude parsing):
   - All findings with sources, detail, relevance
   - Recommendations with priority and rationale
   - Code examples with context
   - Full metadata with confidence and quality report
   - Follow template: skills/research/templates/research-output.md

2. **SUMMARY.md** (markdown for human scanning):
   - Substantive one-liner (NOT "research completed")
   - 3-5 key actionable findings
   - Confidence level with justification
   - Sources consulted (numbered list with URLs)
   - Decisions needed or "None"
   - Next step (concrete action)
   - Follow template: skills/research/templates/summary-template.md
</step>

</research_execution>

<success_criteria>
Research complete when:
- Verification checklist 100% addressed (verified, not found, or unclear)
- All findings have source URLs with dates
- Quality assurance steps completed (completeness, sources, blind spots, critical claims)
- Confidence level assessed with clear justification
- Quality report distinguishes verified from assumed claims
- Both outputs created (research.md + SUMMARY.md)
- SUMMARY.md substantive (specific findings, not generic)
</success_criteria>

<constraints>
- NEVER make claims without authoritative source URLs
- NEVER skip quality controls to save time
- NEVER overstate confidence (honest uncertainty is valuable)
- ALWAYS enumerate known items before researching (prevents missing things)
- ALWAYS write incrementally (prevents lost work)
- ALWAYS apply blind spots review ("What did I miss?")
</constraints>
```

**Why this system prompt**:
- Embeds essential research principles directly in agent context
- References research-pitfalls.md mistakes explicitly
- Includes full research execution workflow
- Optimized for research tasks (not general-purpose)
- Model selection (sonnet for balance, can upgrade to opus for complex research)

**Tools rationale**:
- Read/Write/Edit: File operations for output creation
- Glob/Grep: Codebase exploration when researching existing implementations
- WebSearch/WebFetch: External knowledge gathering (critical for research)
- mcp__*: All MCP servers for library docs, API references, etc.
- No Bash: Research doesn't need command execution
- No Task: Research agent IS the task (doesn't spawn sub-agents)
  </action>
  <verify>agents/research-specialist.md exists with YAML frontmatter, comprehensive system prompt, research workflow</verify>
  <done>Agent file created, system prompt optimized for research, tools appropriate, model specified</done>
</task>

<task type="auto">
  <name>Task 2: Integrate agent with research skill</name>
  <files>skills/research/SKILL.md</files>
  <action>
Add agent invocation option to research skill:

**Update essential_principles with agent principle**:
```xml
<principle name="agent_optimization">
For complex research requiring:
- Fresh context (avoid context pollution from prior work)
- Extended thinking (deep analysis of tradeoffs, implications)
- Model selection (opus for complex, sonnet for standard)
- Isolation (research separate from implementation context)

The research skill can invoke research-specialist agent via Task tool.

Agent invocation is OPTIONAL. Most research executes directly in skill context.
Use agent when:
- Research complexity HIGH (multi-faceted comparison, feasibility analysis)
- Context usage MEDIUM-HIGH (need fresh window)
- Extended thinking beneficial (architectural tradeoffs, implications)
</principle>
```

**Add agent invocation logic to workflows**:
In each workflow file, add decision point:

```xml
<agent_invocation_decision>
Assess research complexity and context usage:

IF complexity HIGH OR context >40% used:
  → Invoke research-specialist agent via Task tool
  → Benefits: Fresh context, extended thinking, isolation
  → Agent executes full research workflow independently
  → Returns structured output to skill

ELSE:
  → Execute research directly in current context
  → Benefits: Simpler, faster, no agent overhead
  → Suitable for most research tasks

Complexity indicators:
- Multiple options to compare (>3)
- Deep architectural analysis needed
- Tradeoffs require extended thinking
- Feasibility involves multiple constraints
</agent_invocation_decision>
```

**Why optional invocation**:
- Most research doesn't need agent overhead
- Agent valuable for complex cases (fresh context, extended thinking)
- Graceful degradation (if agent unavailable, execute directly)
- User doesn't need to know about agent (skill decides)
  </action>
  <verify>skills/research/SKILL.md updated with agent principle and invocation logic pattern</verify>
  <done>Skill updated, agent invocation optional, decision criteria clear, graceful degradation supported</done>
</task>

<task type="auto">
  <name>Task 3: Add agent invocation to feasibility-research workflow</name>
  <files>skills/research/workflows/feasibility-research.md</files>
  <action>
Feasibility research often benefits from agent invocation (complexity HIGH). Update workflow:

**Add after scope definition step**:
```xml
<step name="assess_complexity_and_invoke_agent">
Feasibility research typically involves:
- Multiple technical constraints
- Tradeoffs between approaches
- Second-order implications
- Uncertainty requiring deep analysis

**Recommended: Invoke research-specialist agent**

Check if agent available:
- Glob for agents/research-specialist.md
- If exists: Invoke via Task tool with subagent_type="research-specialist"
- If not exists: Continue with direct execution (graceful degradation)

Task invocation:
```
Use Task tool:
- subagent_type: "research-specialist"
- description: "Feasibility research: [topic]"
- prompt: Include full research scope, verification checklist, output requirements
- model: "sonnet" (or "opus" for very complex feasibility analysis)
```

Agent executes independently:
- Fresh context window (no pollution from prior work)
- Extended thinking for tradeoff analysis
- Full quality controls applied
- Returns research.md + SUMMARY.md

</step>

<step name="validate_agent_output">
If agent invoked, validate output:
- Both files created (research.md, SUMMARY.md)
- Quality report present and complete
- Confidence level assessed
- Verification checklist addressed

If validation fails:
- Note gaps in output
- Supplement with direct research if needed
- Document in final SUMMARY.md

If agent not available or invocation skipped:
- Continue with direct execution using steps below
- Apply same quality controls
- Produce same output structure
</step>
```

**Why feasibility workflow first**:
- Most likely to benefit from agent (high complexity)
- Demonstrates agent value clearly
- Other workflows can follow pattern if needed
  </action>
  <verify>feasibility-research.md updated with agent invocation step and validation</verify>
  <done>Workflow enhanced, agent invocation recommended for feasibility, validation included, graceful degradation supported</done>
</task>

<task type="auto">
  <name>Task 4: Add agent invocation to comparison-research workflow</name>
  <files>skills/research/workflows/comparison-research.md</files>
  <action>
Comparison research (especially >3 options) benefits from agent invocation. Update workflow:

**Add complexity assessment**:
```xml
<step name="assess_comparison_complexity">
Count options and criteria:
- Options: [number of alternatives to compare]
- Criteria: [number of evaluation dimensions]
- Matrix size: options × criteria

**If matrix size >9 (e.g., 3 options × 3 criteria): Consider agent invocation**

Large comparison matrices benefit from:
- Fresh context (prevent matrix getting lost in conversation)
- Extended thinking (thorough analysis of each cell)
- Isolated execution (comparison separate from implementation)

Decision:
- Matrix >15: Strongly recommend agent
- Matrix 9-15: Consider agent if context >40%
- Matrix <9: Direct execution sufficient

If invoking agent:
- Use Task tool with subagent_type="research-specialist"
- Include: All options, all criteria, evaluation methodology
- Model: "sonnet" for standard, "opus" for complex tradeoff analysis
</step>
```

**Why comparison workflow**:
- Second most likely to benefit (matrix complexity)
- Clear decision criteria (matrix size)
- Demonstrates value for structured analysis
  </action>
  <verify>comparison-research.md updated with agent invocation based on matrix complexity</verify>
  <done>Workflow enhanced, agent recommended for large comparisons, complexity criteria clear</done>
</task>

<task type="checkpoint:decision" gate="blocking">
  <decision>Agent availability and graceful degradation strategy</decision>
  <context>
The research-specialist agent is optional. We need to decide how to handle cases where the agent is unavailable (e.g., older Claude Code versions, environments where agent file doesn't exist).

**Current approach**: Each workflow checks for agent availability and falls back to direct execution.

**Question**: Should we add additional safeguards or user communication?
  </context>
  <options>
    <option id="silent-fallback">
      <name>Silent fallback (current approach)</name>
      <pros>
- Simple and automatic
- User doesn't need to know about agent
- Works in all environments
- No user interruption
      </pros>
      <cons>
- User doesn't know optimization unavailable
- No visibility into agent vs direct execution
- Can't distinguish outputs (agent vs direct)
      </cons>
    </option>
    <option id="notify-fallback">
      <name>Notify user when falling back</name>
      <pros>
- User aware of optimization unavailable
- Can install agent if they want optimization
- Transparency about execution mode
- Helpful for debugging/troubleshooting
      </pros>
      <cons>
- Additional user communication (complexity)
- May confuse users who don't care about internals
- Interrupts workflow
      </cons>
    </option>
    <option id="metadata-only">
      <name>Silent fallback + note in SUMMARY.md</name>
      <pros>
- No interruption during research
- Transparency after completion
- User can see execution mode in summary
- Simple to implement
      </pros>
      <cons>
- User only learns after research complete
- Can't proactively install agent before research
      </cons>
    </option>
  </options>
  <resume-signal>
Select fallback strategy: "silent-fallback", "notify-fallback", or "metadata-only"

Recommendation: "metadata-only" provides transparency without interruption. Add to SUMMARY.md:
"Execution mode: [research-specialist agent / direct execution]"
  </resume-signal>
</task>

<task type="auto">
  <name>Task 5: Implement chosen fallback strategy</name>
  <files>skills/research/templates/summary-template.md</files>
  <action>
Based on checkpoint decision, implement fallback strategy:

**If "silent-fallback"**: No changes needed (already implemented in workflows)

**If "notify-fallback"**: Add to workflows that invoke agent:
```xml
<agent_availability_check>
Check for agent: Glob pattern agents/research-specialist.md

If NOT found:
  Use AskUserQuestion (non-blocking):
  - header: "Research Optimization"
  - question: "Research-specialist agent not available. Complex research benefits from agent optimization (fresh context, extended thinking). Continue with direct execution?"
  - options:
    - "Continue" - Proceed with direct execution
    - "Install agent" - Guide to installing agent (copy from agents/ directory)

If found:
  Proceed with agent invocation (no user interruption)
```

**If "metadata-only"** (recommended): Update SUMMARY.md template:
```markdown
## Research Execution

**Mode**: [Research-specialist agent / Direct execution]
**Context**: [Why this mode was chosen - complexity assessment]

[If direct execution used when agent would help]:
**Note**: Research complexity suggested agent invocation, but research-specialist agent not available. Consider installing agent (agents/research-specialist.md) for optimization of complex research.
```

Apply chosen strategy consistently across all workflows that consider agent invocation.
  </action>
  <verify>Fallback strategy implemented according to checkpoint decision</verify>
  <done>Strategy implemented, user communication appropriate for choice, transparency ensured</done>
</task>

<task type="auto">
  <name>Task 6: Update README.md agents section</name>
  <files>README.md</files>
  <action>
Add research-specialist agent to Agents section in README.md:

**Current Agents section**:
```markdown
**[Agents](#agents)** (3 total) - Specialized subagents for validation and quality
- **skill-auditor**: Reviews skills for best practices compliance
- **slash-command-auditor**: Reviews commands for proper structure
- **subagent-auditor**: Reviews agent configurations for effectiveness
```

**Update to**:
```markdown
**[Agents](#agents)** (4 total) - Specialized subagents for validation, quality, and research
- **skill-auditor**: Reviews skills for best practices compliance
- **slash-command-auditor**: Reviews commands for proper structure
- **subagent-auditor**: Reviews agent configurations for effectiveness
- **research-specialist**: Expert research agent with systematic verification (optional optimization)
```

**In detailed Agents section, add**:
```markdown
### [research-specialist](./agents/research-specialist.md)

Expert research agent for thorough investigation with systematic verification and quality controls. Provides optimization for complex research requiring fresh context, extended thinking, or specific model capabilities.

**When used**: Automatically invoked by research skill for complex feasibility analysis or large comparison matrices. Optional optimization - research works without agent.

**Specializations**:
- Fresh context window (isolated execution)
- Extended thinking for deep analysis
- Model selection (sonnet/opus based on complexity)
- Embedded quality controls and verification checklists

**Integration**: Invoked by research skill via Task tool when complexity or context usage indicates benefit.
```

Why document in agents section:
- Shows advanced optimization available
- Explains optional nature clearly
- Demonstrates automatic invocation by skill
  </action>
  <verify>README.md updated with research-specialist agent in Agents section</verify>
  <done>README updated, agent documented, optional nature clear, integration explained</done>
</task>

<task type="auto">
  <name>Task 7: Update research skill README with agent information</name>
  <files>skills/research/README.md</files>
  <action>
Add agent optimization section to skills/research/README.md:

**Add new section after "Quality Controls"**:
```markdown
## Agent Optimization (Optional)

For complex research, the skill can invoke the **research-specialist agent** for optimization:

**Benefits of agent invocation**:
- **Fresh context**: Isolated execution prevents context pollution
- **Extended thinking**: Deep analysis of tradeoffs and implications
- **Model selection**: Sonnet for standard, Opus for complex analysis
- **Isolation**: Research separate from implementation context

**When agent is invoked** (automatic):
- Feasibility research (high complexity, multiple constraints)
- Comparison research with large matrices (>9 options × criteria)
- Context usage >40% (benefits from fresh window)
- Extended thinking needed (architectural tradeoffs)

**Graceful degradation**:
If research-specialist agent unavailable, research executes directly in skill context with same quality controls. Agent is optimization, not requirement.

**Installation** (optional):
Agent file: `agents/research-specialist.md`
Install by copying to your `.claude/agents/` directory or project `agents/` directory.
```

**Update Integration section**:
```markdown
## Integration

**Used by other skills**:
- create-meta-prompts: For research prompts in Claude-to-Claude pipelines
- create-plans: For phase research in project planning
- debug-like-expert: For external knowledge gathering

**Uses other agents** (optional):
- research-specialist: For complex research requiring fresh context or extended thinking

**Single source of truth**:
Consolidates research patterns from multiple skills. Other skills invoke this skill or reference its patterns.
```

Why document in skill README:
- Users understand advanced optimization available
- Clear about optional nature (no requirement)
- Explains when/why agent invoked
  </action>
  <verify>skills/research/README.md updated with agent optimization section and integration details</verify>
  <done>Skill README updated, agent benefits clear, optional nature emphasized, installation instructions provided</done>
</task>

<task type="auto">
  <name>Task 8: Create agent usage examples</name>
  <files>skills/research/references/agent-usage.md</files>
  <action>
Create reference document with agent usage examples:

**Contents**:
```markdown
# Agent Usage Patterns

This reference documents when and how the research skill invokes the research-specialist agent.

## Invocation Decision

Agent invocation is decided by the research skill based on complexity assessment.

**Automatic invocation for**:
1. **Feasibility research** (recommended for all feasibility)
   - Multiple technical constraints
   - Tradeoff analysis needed
   - Second-order implications
   - Uncertainty requiring deep analysis

2. **Large comparison matrices** (matrix size >9)
   - 4+ options to compare
   - 3+ evaluation criteria
   - Thorough analysis of each option × criterion cell

3. **High context usage** (>40% consumed)
   - Benefits from fresh context window
   - Prevents context pollution
   - Maintains analysis quality

4. **Extended thinking needed**
   - Architectural tradeoffs
   - Multi-faceted evaluation
   - Complex synthesis

**Direct execution for**:
- Simple technology research (library evaluation)
- Quick research (overview only)
- API research (straightforward documentation review)
- Best practices research (standard OWASP/official guidelines)
- Small comparison matrices (<9 cells)
- Low context usage (<40%)

## Invocation Workflow

When skill decides to invoke agent:

```xml
<agent_invocation>
<check_availability>
Glob for agents/research-specialist.md
- If found: Proceed with invocation
- If not found: Fall back to direct execution
</check_availability>

<invoke_task>
Use Task tool:
- subagent_type: "research-specialist"
- description: "[Research type]: [topic]"
- prompt: |
    Research request: [full scope]

    Verification checklist:
    [enumerated items to verify]

    Output requirements:
    - research-[topic].md (XML structure)
    - SUMMARY.md (markdown summary)

    Apply full quality controls from research-specialist system prompt.
- model: [Inferred from complexity]
  - "sonnet": Standard research, most feasibility
  - "opus": Very complex feasibility, large comparisons
</invoke_task>

<receive_results>
Agent returns:
- research-[topic].md with full findings
- SUMMARY.md with key insights
- Quality report embedded in metadata

Validate output:
- Both files exist
- Quality report complete
- Confidence assessed
- Verification checklist addressed
</receive_results>
</agent_invocation>
```

## Example: Feasibility Research

**Request**: "Research feasibility of implementing real-time collaboration in our app"

**Complexity assessment**:
- Multiple constraints: WebSocket support, state synchronization, conflict resolution
- Tradeoffs: Operational complexity vs user experience
- Extended thinking needed: Architecture implications, scaling concerns
- Verdict: HIGH complexity → Invoke agent

**Agent execution**:
1. Spawn research-specialist agent (fresh context)
2. Agent enumerates verification checklist:
   - WebSocket libraries available
   - State sync patterns (CRDT, OT, etc.)
   - Conflict resolution approaches
   - Infrastructure requirements
   - Operational complexity
3. Agent researches each item systematically
4. Agent applies quality controls
5. Agent assesses confidence
6. Agent returns structured output

**Output**:
- research-collaboration-feasibility.md (detailed findings)
- SUMMARY.md: "Real-time collaboration feasible with Yjs CRDT library + WebSocket infrastructure. MEDIUM confidence - requires proof-of-concept to validate scaling assumptions."

## Example: Comparison Research (Small)

**Request**: "Compare Jest vs Vitest for our test suite"

**Complexity assessment**:
- 2 options × 3 criteria (performance, DX, migration) = 6 cells
- Verdict: LOW complexity → Direct execution

**Direct execution**:
1. Skill executes comparison in current context
2. Enumerates verification checklist (same process)
3. Applies quality controls (same standards)
4. Produces structured output (same format)

**No agent needed**: Simple comparison, small matrix, direct execution sufficient.

## Benefits vs Costs

**Agent benefits**:
- ✅ Fresh context (no pollution)
- ✅ Extended thinking (deep analysis)
- ✅ Isolation (research separate)
- ✅ Model selection (opus for complex)

**Agent costs**:
- ⏱️ Slight overhead (agent spawn)
- 🔧 Requires agent file installed
- 📊 Overkill for simple research

**Decision**: Use agent when benefits outweigh costs (complex research), skip when costs exceed benefits (simple research).
```

Why create this reference:
- Documents decision logic clearly
- Provides examples for both invocation and direct execution
- Helps skill developers understand when agent is valuable
  </action>
  <verify>skills/research/references/agent-usage.md created with invocation patterns, examples, benefits/costs</verify>
  <done>Reference comprehensive, includes decision logic, examples, tradeoff analysis</done>
</task>

<task type="auto">
  <name>Task 9: Add agent to skill files index</name>
  <files>skills/research/SKILL.md</files>
  <action>
Update SKILL.md reference index to include agent documentation:

**Add to end of SKILL.md**:
```xml
<reference_index>
All in `references/`:

**Methodology:** research-patterns.md (authoritative source)
**Quality:** quality-controls.md, research-pitfalls.md
**Assessment:** confidence-assessment.md
**Tools:** tool-usage.md
**Output:** output-formats.md
**Agent:** agent-usage.md (when agent invoked, benefits vs costs)

All in `workflows/`:

**Research types:** technology-research.md, api-research.md, best-practices-research.md, comparison-research.md, feasibility-research.md, quick-research.md

All in `templates/`:

**Structures:** research-output.md, summary-template.md

Related agents:

**research-specialist** (optional): agents/research-specialist.md - Invoked for complex research requiring fresh context or extended thinking
</reference_index>
```

Why add to index:
- Makes agent-usage.md discoverable
- Shows relationship between skill and agent
- Complete documentation reference
  </action>
  <verify>skills/research/SKILL.md updated with complete reference index including agent</verify>
  <done>Reference index comprehensive, agent documented, relationships clear</done>
</task>

</tasks>

<verification>
Before declaring Phase 3 complete:

- [ ] agents/research-specialist.md created with YAML, system prompt, tools, model
- [ ] Agent system prompt includes research workflow, quality controls, success criteria
- [ ] skills/research/SKILL.md updated with agent principle and invocation logic
- [ ] feasibility-research.md updated with agent invocation step
- [ ] comparison-research.md updated with complexity-based agent invocation
- [ ] Fallback strategy implemented (based on checkpoint decision)
- [ ] summary-template.md updated if metadata-only fallback chosen
- [ ] README.md updated with research-specialist in Agents section
- [ ] skills/research/README.md updated with agent optimization section
- [ ] agent-usage.md created with invocation patterns and examples
- [ ] skills/research/SKILL.md reference index updated
- [ ] Test: Agent file valid (YAML parseable, tools appropriate)
- [ ] Test: Skill can check for agent availability (Glob pattern works)
- [ ] Test: Graceful degradation (skill works without agent)
- [ ] Documentation clear about optional nature (agent is optimization, not requirement)
</verification>

<success_criteria>
- research-specialist agent created and documented
- Agent optimized for research (system prompt, tools, model)
- Integration with research skill complete (invocation logic, fallback)
- Agent invoked automatically for complex research (feasibility, large comparisons)
- Graceful degradation when agent unavailable (direct execution)
- Documentation clear about benefits and optional nature
- No breaking changes (research works with or without agent)
- User transparency (execution mode noted in outputs)
</success_criteria>

<output>
After Phase 3 completion, create `PHASE-3-SUMMARY.md`:

# Phase 3: Research Agent (Optional) - Summary

**Research-specialist agent provides advanced optimization for complex research**

## Accomplishments
- Created research-specialist agent with research-optimized system prompt
- Embedded full research workflow in agent (systematic verification, quality controls)
- Integrated agent with research skill (automatic invocation for complex cases)
- Added agent invocation to feasibility and comparison workflows
- Implemented graceful degradation (works without agent)
- Updated documentation (README, skill README, reference guides)
- Created agent-usage.md with invocation patterns and examples
- Maintained optional nature (agent is optimization, not requirement)

## Files Created/Modified
- `agents/research-specialist.md` - Agent configuration with optimized system prompt
- `skills/research/SKILL.md` - Added agent principle and reference index
- `skills/research/workflows/feasibility-research.md` - Agent invocation step added
- `skills/research/workflows/comparison-research.md` - Complexity-based agent invocation
- `skills/research/templates/summary-template.md` - [If metadata-only: execution mode field]
- `skills/research/references/agent-usage.md` - Invocation patterns and examples
- `skills/research/README.md` - Agent optimization section
- `README.md` - research-specialist added to Agents section

## Decisions Made
- Agent availability strategy: [silent-fallback / notify-fallback / metadata-only]
- Model selection: sonnet default, opus for very complex analysis
- Invocation triggers: Feasibility (all), Comparison (matrix >9), Context >40%
- Graceful degradation: Direct execution when agent unavailable

## Issues Encountered
[Document any issues and resolutions, or "None"]

## Next Step
Phase 3 complete. All three phases of research feature implementation planned:
- ✅ Phase 1: Research command (MVP) - Immediate discoverability
- ✅ Phase 2: Research skill (Consolidated) - Single source of truth
- ✅ Phase 3: Research agent (Optional) - Advanced optimization

Ready for implementation when approved.

## Impact
- **Optimization**: Complex research benefits from fresh context and extended thinking
- **Automatic**: Skill decides when agent valuable (user doesn't need to know)
- **Optional**: Research fully functional without agent (graceful degradation)
- **Transparency**: Execution mode documented in outputs
- **Quality**: Agent system prompt embeds full research methodology
- **Flexibility**: Model selection based on complexity (sonnet/opus)
</output>

<notes>
**Why Phase 3 Optional:**
- Research is fully functional without agent (Phase 1 + 2 sufficient)
- Agent provides optimization for advanced use cases
- Not all users need complex research capabilities
- Graceful degradation ensures no dependency

**Agent Value Proposition:**
- Fresh context prevents pollution (critical for large research)
- Extended thinking for tradeoff analysis (feasibility, comparisons)
- Model selection (opus for very complex analysis)
- Isolation (research separate from implementation)

**When to Skip Phase 3:**
- Users primarily do simple research (library evaluation)
- Context management not an issue (small projects)
- Extended thinking not needed (straightforward decisions)
- Prefer simplicity over optimization

**When to Implement Phase 3:**
- Users do complex feasibility analysis regularly
- Large comparison matrices common (many options/criteria)
- Context pollution becomes issue (large conversations)
- Extended thinking valuable (architectural decisions)

**Implementation Flexibility:**
Can implement Phase 1 + 2 first, gather user feedback, then decide if Phase 3 valuable for actual usage patterns.
</notes>
