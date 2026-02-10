---
name: enrich-jira-task
description: Guided workflow to enrich incomplete Jira tasks with acceptance criteria, subtasks, technical context, and test cases - works with or without MCP integration
allowed-tools: Read, Write, Grep, Glob, AskUserQuestion, mcp__plugin_atlassian_atlassian__*
aliases: [enrich]
---

# Enrich Jira Task Skill

## Overview

This skill implements a **guided workflow** to transform incomplete or vague Jira tasks into well-defined, actionable work items. It generates:

- **Expanded descriptions** with examples and use cases
- **Acceptance criteria** in Given-When-Then format
- **Subtasks** for complex work breakdown
- **Technical context** (dependencies, risks, resources)
- **Test cases** aligned with acceptance criteria

**When to use this skill:**
- Task has minimal or vague description
- Missing acceptance criteria or success criteria
- Complex task that needs breakdown into subtasks
- Need to add technical context from related work
- Want to ensure consistency with project patterns

**Key principle:** Always get user confirmation before presenting enrichments. Show comprehensive analysis and let user choose how to apply.

## Operating Modes

### 🔧 Manual Mode (Default)
**No MCP integration required** - Works with screenshots and manual input

**How it works:**
1. User provides Jira task info (screenshot, ticket ID, or description)
2. Skill analyzes the content and generates enrichments based on:
   - Best practices and patterns
   - Similar domain knowledge
   - Established templates
3. Skill presents enrichments as formatted markdown
4. User manually copies to Jira or exports to file

**Advantages:**
- ✅ No setup required
- ✅ Works without Jira API access
- ✅ Privacy-friendly (no external connections)
- ✅ Great for review and refinement

### 🚀 Automated Mode (Optional)
**With Atlassian MCP Plugin** - Full integration with Jira/Confluence

**How it works:**
1. Skill fetches task details automatically from Jira
2. Searches for similar tasks and documentation
3. Generates enrichments based on actual context
4. Can apply changes directly to Jira (with approval)

**Advantages:**
- ✅ Automatic context gathering
- ✅ Direct Jira updates
- ✅ Search similar tasks for patterns
- ✅ Link to related documentation

**Setup required:**
- Atlassian MCP Plugin configured
- Jira API access credentials
- (Optional) Confluence access

## Usage

```bash
# Manual Mode (no MCP needed)
/enrich-jira-task
# Then provide screenshot or paste task details

# Manual Mode with context
/enrich-jira-task PROJ-123
# Provide screenshot and any relevant context manually

# Automated Mode (requires MCP)
# Plugin will be auto-detected if available
/enrich-jira-task PROJ-123
```

## Workflow (Adaptive based on mode)

### Phase 0: Initialization & Mode Detection

**Goal:** Detect available tools and gather task information

**Steps:**

1. **Detect MCP Availability** (automatic)
   ```
   Try: ListMcpResourcesTool(server="plugin_atlassian_atlassian")
   If successful → Automated Mode
   If fails → Manual Mode
   ```

2. **Gather Task Information**

   **Automated Mode:**
   ```
   a. Get Cloud ID:
      mcp__plugin_atlassian_atlassian__getAccessibleAtlassianResources()

   b. Fetch Task Details:
      mcp__plugin_atlassian_atlassian__getJiraIssue(cloudId, ticketId)
      Fields: summary, description, issuetype, project, status, parent
   ```

   **Manual Mode:**
   ```
   a. Check if user provided screenshot → Analyze image

   b. If no screenshot, ask user:
      - Task ID and summary
      - Current description (if any)
      - Acceptance criteria (if any)
      - Related tasks or epic (if known)
      - Any other relevant context

   c. Parse provided information
   ```

3. **Parse and Analyze** (same for both modes)
   - Extract: project key, issue type, current description, epic (if any)
   - Identify gaps:
     * Missing/minimal description → Need expansion
     * No acceptance criteria → Generate ACs
     * Complex scope → Need subtasks
     * No technical context → Add dependencies/risks
     * Missing test guidance → Generate test cases

4. **Extract Keywords**
   - From title: noun phrases and action verbs
   - From description: domain terms
   - For context gathering in next phase

**Error handling:**
- **Automated Mode**: If MCP fails → Fall back to Manual Mode
- **Manual Mode**: If insufficient info → Ask user for more details
- Always inform user which mode is being used

---

### Phase 1: Contextualization

**Goal:** Gather relevant context to inform enrichments

#### Automated Mode: Search Jira & Confluence

Execute **3 searches simultaneously** to maximize context:

**Search 1: Similar Completed Tasks**
```
mcp__plugin_atlassian_atlassian__searchJiraIssuesUsingJql(
  cloudId,
  jql: "project = '{PROJECT}' AND (summary ~ '{keywords}')
        AND type = '{TYPE}' AND status IN (Done, Closed)",
  fields: ["summary", "description", "status", "issuetype"],
  maxResults: 10
)
```
Extract: Description patterns, AC formats, subtask approaches

**Search 2: Tasks in Same Epic**
```
mcp__plugin_atlassian_atlassian__searchJiraIssuesUsingJql(
  cloudId,
  jql: "\"Epic Link\" = '{EPIC_KEY}'",
  maxResults: 10
)
```
Extract: Epic context, dependencies, task patterns

**Search 3: Confluence Documentation**
```
mcp__plugin_atlassian_atlassian__searchConfluenceUsingCql(
  cloudId,
  cql: "text ~ '{keywords}' AND type = page",
  limit: 10
)
```
Extract: Architecture, API specs, edge cases, limitations

#### Manual Mode: Use Available Context

**Context sources:**
1. **User-provided information**
   - Related task IDs mentioned
   - Documentation links shared
   - Similar tasks described

2. **Domain knowledge & best practices**
   - Industry-standard patterns for the task type
   - Common acceptance criteria for similar features
   - Typical subtask breakdowns
   - Known risks and edge cases

3. **Template library** (from skill references)
   - Enrichment templates by task type
   - Acceptance criteria patterns
   - Subtask breakdown strategies
   - Test case templates

4. **Visual analysis** (if screenshot provided)
   - Existing acceptance criteria structure
   - Linked tasks and dependencies
   - Current description format
   - Project naming conventions

**User interaction:**
- Ask user: "Do you know of similar completed tasks I should reference?"
- Ask user: "Are there specific technical docs or specs for this feature?"
- Inform user: "I'll use best practices and standard patterns for [task type]"

**Fallback strategy:**
- If minimal context available → Use comprehensive templates
- Focus on general best practices
- Ask clarifying questions about domain-specific requirements

---

### Phase 2: Analysis & Synthesis

**Goal:** Generate enrichments based on collected context

#### 2.1 Enriched Description

**Format:**
```markdown
## Overview
[1-2 sentences: what and why]

## Context
[Background from related tasks/docs]

## Detailed Requirements
[Specific requirements extracted/inferred from similar tasks]

## Use Cases
- **UC-1**: [Primary use case]
- **UC-2**: [Secondary use case]

## Edge Cases
- [Edge case 1 from documentation]
- [Edge case 2 from past issues]

## Examples
[Concrete examples from similar tasks or docs]
```

**Sources to integrate:**
- Current description (preserve existing content)
- Similar task descriptions (adapt patterns)
- Confluence docs (add technical details)
- Epic context (align with larger goals)

#### 2.2 Acceptance Criteria

**Format:** Given-When-Then (adapt from /product-manager patterns)

```markdown
## Acceptance Criteria

### AC-1: [Core functionality]
**Given** [initial state]
**When** [action performed]
**Then** [expected outcome]

### AC-2: [Edge case handling]
**Given** [edge condition]
**When** [action performed]
**Then** [expected behavior]

### AC-3: [Non-functional requirement]
**Given** [performance/security context]
**When** [measured/tested]
**Then** [meets threshold]
```

**Guidelines:**
- Minimum 3 ACs, maximum 7
- Cover: happy path, error cases, non-functional requirements
- Make testable (specific, measurable)
- Extract from: similar tasks' ACs, documentation, implicit requirements

#### 2.3 Subtasks Breakdown

**When to create subtasks:**
- Task description > 200 words after enrichment
- Multiple distinct implementation phases
- Different skill sets required (frontend/backend/testing)

**Subtask template:**
```markdown
**Subtask**: [Action verb] + [specific component]
**Type**: Sub-task
**Description**: [Brief, focused description]
**Acceptance Criteria**:
  - AC-1: [Specific to this subtask]
**Estimated effort**: [S/M/L relative to parent]
```

**Breakdown strategy:**
1. **Setup/Infrastructure** (if needed)
2. **Core Implementation** (main logic)
3. **Integration** (with other systems)
4. **Testing & Validation**
5. **Documentation** (if substantial)

**Extract from:**
- Similar task subtasks (copy proven patterns)
- Epic task breakdown (maintain consistency)
- Technical docs (implementation phases)

#### 2.4 Technical Context

```markdown
## Dependencies
- **Blocks**: [Tasks that can't start until this completes]
- **Blocked by**: [Tasks that must complete first]
- **Related**: [Contextually related tasks]

## Risks & Considerations
- **Risk-1**: [Identified risk from past tasks]
  - Mitigation: [How to address]
- **Risk-2**: [Technical limitation from docs]
  - Mitigation: [Workaround approach]

## Resources
- [Link to Confluence spec]
- [Link to similar task PROJ-100]
- [Link to API documentation]

## Technical Notes
- [Architecture constraints from docs]
- [Performance considerations from past work]
- [Security requirements]
```

**Extract from:**
- Related tasks (dependency patterns)
- Documentation (known risks, limitations)
- Epic context (architectural constraints)

#### 2.5 Test Cases

**Format:**
```markdown
## Test Cases

### TC-1: Happy Path
**Preconditions**: [Setup needed]
**Steps**:
  1. [Action]
  2. [Action]
**Expected Result**: [Outcome]
**Test Data**: [Sample data]

### TC-2: Edge Case - [Scenario]
**Preconditions**: [Edge condition setup]
**Steps**: [...]
**Expected Result**: [How system should handle]

### TC-3: Error Handling
**Preconditions**: [Invalid state]
**Steps**: [...]
**Expected Result**: [Graceful error]
```

**Derive from:**
- Acceptance criteria (1 TC per AC minimum)
- Edge cases in documentation
- Error scenarios from past bugs

---

### Phase 3: User Presentation (CRITICAL)

**Goal:** Present comprehensive enrichments to user

**Display format:**

```markdown
# 📋 Current Task: {TICKET-ID}

## Original State
**Summary**: {original summary}
**Description**:
{original description or "[Empty]"}

**Gaps Identified**:
- ❌ No acceptance criteria
- ❌ Minimal description
- ⚠️  Complex scope (needs subtasks)
- ❌ Missing technical context

---

# ✨ Proposed Enrichments

## 1. Enriched Description
{Generated description - show in full}

**Source context**:
[Automated Mode]: Based on 3 similar tasks: PROJ-100, PROJ-105, PROJ-110
[Manual Mode]: Based on best practices for [task type] and domain knowledge

---

## 2. Acceptance Criteria
{Generated ACs}

---

## 3. Suggested Subtasks
1. **{TITLE-1}** - Setup [Effort: S]
2. **{TITLE-2}** - Implementation [Effort: M]
3. **{TITLE-3}** - Testing [Effort: S]

---

## 4. Technical Context
{Dependencies, risks, resources}

---

## 5. Test Cases
{Generated test cases}

---

# 🎯 Next Steps
```

**Ask user via AskUserQuestion (mode-dependent):**

**Automated Mode (MCP available):**
```
Question: "How would you like to apply these enrichments?"
Options:
  1. "Apply all to Jira automatically" (Recommended)
     - Updates task description
     - Creates subtasks
     - Adds comment with technical context

  2. "Apply selectively to Jira"
     - Let me choose which parts to apply

  3. "Export to markdown file"
     - Save enrichments to file for manual review

  4. "Show as formatted text only"
     - Just display, I'll copy manually
```

**Manual Mode (no MCP):**
```
Question: "How would you like to use these enrichments?"
Options:
  1. "Export to markdown file" (Recommended)
     - Save to file for easy copying to Jira

  2. "Show as formatted Jira markup"
     - Display in Jira-compatible format for copy/paste

  3. "Show as plain markdown"
     - Display as standard markdown

  4. "Create separate files for each section"
     - Description, ACs, Subtasks, Tests as separate files
```

---

### Phase 4: Application / Export

**Goal:** Deliver enrichments in user's preferred format

#### Automated Mode - Apply to Jira

**Option A: Update Task Description**
```
mcp__plugin_atlassian_atlassian__editJiraIssue(
  cloudId,
  issueIdOrKey: ticketId,
  fields: {
    description: { [ADF format enriched description] }
  }
)
```
Note: Preserve existing content, append enrichments

**Option B: Create Subtasks**
```
For each subtask:
  mcp__plugin_atlassian_atlassian__createJiraIssue(
    cloudId, projectKey, issueTypeName: "Sub-task",
    summary, description, additional_fields: { parent: ticketId }
  )
```

**Option C: Add Comment**
```
mcp__plugin_atlassian_atlassian__addCommentToJiraIssue(
  cloudId, ticketId,
  commentBody: [formatted enrichments]
)
```

#### Manual Mode - Export Options

**Option A: Export to Markdown File**
```
Use Write tool to create:
  {TICKET-ID}-enrichment-{timestamp}.md

Structure:
  # Task Enrichment: {TICKET-ID}

  ## Enriched Description
  [content]

  ## Acceptance Criteria
  [content]

  ## Suggested Subtasks
  [content]

  ## Technical Context
  [content]

  ## Test Cases
  [content]

  ---
  Generated: {date}
  Mode: Manual
```

**Option B: Format as Jira Markup**
Display in Jira Wiki Markup format for easy copy/paste into Jira

**Option C: Plain Markdown Display**
Show formatted markdown in chat for user to copy

**Option D: Separate Section Files**
```
Create individual files:
  {TICKET-ID}-description.md
  {TICKET-ID}-acceptance-criteria.md
  {TICKET-ID}-subtasks.md
  {TICKET-ID}-test-cases.md
  {TICKET-ID}-technical-context.md
```

**File export location:**
```
Default: ./jira-enrichments/
User can specify custom path via question
```

---

### Phase 5: Confirmation

**Goal:** Confirm completion and provide next steps

#### Automated Mode - Jira Updated

**Fetch and verify:**
```
mcp__plugin_atlassian_atlassian__getJiraIssue(cloudId, ticketId)
```

**Display summary:**
```markdown
# ✅ Enrichment Complete!

## Changes Applied to Jira
- ✓ Description updated (+{X} words)
- ✓ {N} subtasks created: {TICKET-ID-1}, {TICKET-ID-2}, {TICKET-ID-3}
- ✓ Technical context added as comment

## Updated Task
**{TICKET-ID}**: {title}

**Subtasks**:
- [{SUBTASK-1-ID}] {title} - [View]({jira-url})
- [{SUBTASK-2-ID}] {title} - [View]({jira-url})

## Quick Links
- [View updated task]({jira-url}/{TICKET-ID})

## Context Used
- Similar tasks analyzed: {count}
- Confluence docs referenced: {count}
```

#### Manual Mode - Files Created

**Display summary:**
```markdown
# ✅ Enrichment Complete!

## Files Created
- ✓ {file-path}/CRED-237-enrichment.md
  (Complete enrichment with all sections)

## What's Included
- Enriched description (+{X} words)
- {N} acceptance criteria (Given-When-Then format)
- {M} suggested subtasks with effort estimates
- Technical context (dependencies, risks, resources)
- {P} test cases

## Next Steps
1. Review the generated enrichments
2. Copy sections to Jira as needed
3. Create subtasks manually in Jira (or copy titles)
4. Adjust based on your team's specific needs

## File Location
📁 {absolute-path-to-file}
```

**Optional: Show preview**
```markdown
## Preview (first 300 chars of each section)

### Description
{preview...}

### Acceptance Criteria
{preview...}

### Subtasks
{preview...}
```

---

### Phase 6: Learning (Optional - Future Enhancement)

**Goal:** Improve future enrichments

**What to save:**
- Successful enrichment patterns
- User-approved AC templates
- Effective search keywords
- Project-specific patterns

**Storage location:**
```
~/.claude/skills/enrich-jira-task/learned/
  {PROJECT-KEY}-patterns.json
```

**Future use:**
- Pre-populate with project patterns
- Suggest project-specific templates
- Improve keyword extraction

**Note:** This phase is not yet implemented. For V1, rely on reference templates.

---

## Enrichment Best Practices

### Writing Acceptance Criteria

**Good AC characteristics:**
- ✅ **Testable**: Can verify pass/fail
- ✅ **Specific**: No ambiguous terms
- ✅ **Independent**: Not dependent on other ACs
- ✅ **Valuable**: Directly relates to user/business value

**Example - Bad AC:**
```
AC-1: The feature should work well
```
❌ Not testable, not specific

**Example - Good AC:**
```
AC-1: Search response time
**Given** search index with 10,000 products
**When** user submits search query
**Then** results display within 500ms
```
✅ Testable, specific, measurable

### Breaking Down into Subtasks

**When to create subtasks:**
- Original task > 3 days effort
- Multiple technical domains (UI + API + DB)
- Distinct phases (research → prototype → implement → test)

**When NOT to create subtasks:**
- Simple tasks (< 1 day)
- Tightly coupled work that can't be parallelized
- Already well-defined with clear ACs

**Subtask sizing:**
- Aim for 0.5-2 days per subtask
- Each should be independently deployable (if possible)
- Clear handoff points between subtasks

### Search Strategy Tips

**Effective keyword selection:**
- Include: domain terms, action verbs, component names
- Exclude: stopwords, overly generic terms
- Use OR for synonyms: `(authentication OR login OR auth)`

**When search returns little:**
- Broaden to parent epic or project level
- Search across all projects: `project IN (PROJ1, PROJ2)`
- Use Confluence exclusively: specs often more detailed than tasks

**When search returns too much:**
- Add time filter: `created >= -90d`
- Filter by specific component: `component = "API"`
- Limit to same assignee: `assignee = currentUser()`

### Technical Context Gathering

**Dependencies - Where to find:**
- Task links in Jira (blocks/is blocked by)
- Epic description (high-level dependencies)
- Confluence architecture docs (system dependencies)

**Risks - Common sources:**
- Comments on similar tasks (past issues)
- Confluence pages tagged "risks" or "limitations"
- Failed/reopened tasks (what went wrong)

**Resources - What to link:**
- API documentation (if task involves APIs)
- Design specs (if UI work)
- Architecture Decision Records (if architectural changes)
- Similar completed tasks (as reference)

---

## Search Patterns Reference

### JQL Query Templates

**Similar tasks by keyword:**
```jql
project = "{PROJECT}" AND
(summary ~ "{keyword}" OR description ~ "{keyword}") AND
type = "{ISSUE_TYPE}" AND
status IN (Done, Closed)
ORDER BY updated DESC
```

**Tasks in same epic:**
```jql
"Epic Link" = "{EPIC_KEY}"
ORDER BY status ASC, created DESC
```

**Recent tasks by component:**
```jql
project = "{PROJECT}" AND
component = "{COMPONENT}" AND
created >= -90d
ORDER BY created DESC
```

**Failed/reopened tasks (for risk analysis):**
```jql
project = "{PROJECT}" AND
(status changed TO "Reopened" OR status changed TO "Failed") AND
updated >= -180d
ORDER BY updated DESC
```

### CQL Query Templates

**Documentation by keyword:**
```cql
text ~ "{keyword}" AND
type = page AND
space = "{SPACE}"
ORDER BY lastModified DESC
```

**Architecture docs:**
```cql
(title ~ "architecture" OR title ~ "design" OR label = "architecture") AND
type = page AND
space IN ("{SPACE1}", "{SPACE2}")
ORDER BY lastModified DESC
```

**API specifications:**
```cql
(title ~ "API" OR title ~ "endpoint" OR label = "api") AND
type = page
ORDER BY lastModified DESC
```

---

## Templates

### Enriched Description Template

```markdown
## Overview
{1-2 sentence summary of what and why}

## Context
{Background from similar tasks, epic, or documentation}

## Detailed Requirements
{Specific, actionable requirements extracted from analysis}

## Use Cases
- **UC-1**: {Primary scenario}
- **UC-2**: {Secondary scenario}
- **UC-3**: {Edge case scenario}

## Edge Cases to Consider
- {Edge case from documentation}
- {Edge case from past similar tasks}

## Examples
{Concrete examples from context}

## Out of Scope
{Explicitly exclude to prevent scope creep}
```

### Acceptance Criteria Template

```markdown
## Acceptance Criteria

### AC-{N}: {Criterion title}
**Given** {precondition/initial state}
**When** {action or trigger}
**Then** {expected outcome - specific and measurable}

**Validation**: {How to test this}
```

### Subtask Template

```markdown
**Summary**: [Action verb] {specific component/feature}

**Description**:
## Goal
{What this subtask accomplishes}

## Acceptance Criteria
- AC-1: {Specific to this subtask}
- AC-2: {Another criterion}

## Dependencies
- Blocked by: {Parent task or other subtask}
- Blocks: {Subsequent work}

## Resources
- {Relevant documentation}
- {Related code/components}
```

### Test Case Template

```markdown
### TC-{N}: {Test scenario name}

**Objective**: {What aspect is being tested}

**Preconditions**:
- {Setup requirement 1}
- {Setup requirement 2}

**Test Steps**:
1. {Action}
2. {Action}
3. {Verification step}

**Expected Result**:
{Specific expected outcome}

**Test Data**:
- Input: {Sample input}
- Expected output: {Sample output}

**Priority**: {High/Medium/Low}
```

---

## Examples

See `examples/` directory for full before/after examples:

- **before-after-1.md**: Task with minimal description → Fully enriched
- **before-after-2.md**: Epic-level task → Broken down into subtasks

**Common patterns demonstrated:**
- How to expand vague requirements
- How to derive ACs from context
- How to identify subtask boundaries
- How to add technical context without over-engineering

---

## Troubleshooting

### Mode Detection

**Issue: "MCP not detected, using Manual Mode"**
**Cause:** Atlassian MCP plugin not installed or not running
**Solution:** This is expected if you don't have MCP set up. Manual Mode works great!

**Issue: "MCP available but authentication failed"**
**Cause:** MCP plugin not authenticated with Jira
**Solution:**
  - Check MCP plugin configuration
  - Verify Jira API credentials
  - Fall back to Manual Mode if needed

### Automated Mode Issues

**Issue: "Task not found"**
**Cause:** Wrong ticket ID or no access
**Solution:** Verify ticket ID in browser, check permissions

**Issue: "Search returns no results"**
**Cause:** Keywords too specific or project restrictions
**Solution:** Skill will fall back to templates and best practices

**Issue: "Subtask creation fails"**
**Cause:** Project doesn't allow subtasks
**Solution:** Skill will offer to export subtasks to file instead

### Manual Mode Issues

**Issue: "Screenshot not clear enough"**
**Solution:** Provide text summary of task details instead

**Issue: "Insufficient context for enrichment"**
**Solution:** Skill will ask clarifying questions about:
  - Task domain (authentication, UI, API, etc.)
  - Known similar tasks or patterns
  - Specific requirements or constraints

### General Issues

**Issue: "Enrichments seem generic"**
**Cause:** Limited context available (especially in Manual Mode)
**Solution:**
  - Provide more details about similar tasks
  - Share links to documentation
  - Describe domain-specific requirements
  - Use Automated Mode for better context gathering

**Issue: "Want to customize templates"**
**Solution:** Edit template files in:
  - `references/enrichment-templates.md`
  - `templates/enriched-description.md`
  - `templates/subtask-template.md`

---

## Integration with Other Skills

### /product-manager
- **Use for**: Acceptance criteria patterns (Given-When-Then)
- **Use for**: Requirement structure (FR-XXX, NFR-XXX)
- **Don't use for**: Full PRD generation (too heavy for task enrichment)

### /creating-pr
- **Potential integration**: Link enriched tasks to PR workflow
- **Future**: Auto-populate PR description from task ACs

### /jira-to-pr-workflow
- **Overlap**: Both work with Jira tasks
- **Difference**: This skill enriches tasks before implementation
- **Workflow**: Use /enrich first, then /jira-to-pr for implementation

---

## Skill Configuration

**Default settings:**
```yaml
max_similar_tasks: 10
max_confluence_docs: 10
min_acceptance_criteria: 3
max_acceptance_criteria: 7
default_apply_mode: "ask_user"  # Never auto-apply
include_test_cases: true
create_subtasks_threshold: 200  # words in enriched description
```

**Customization:** Future versions will support project-specific config files.

---

## Workflow Summary

### Automated Mode (with MCP)
```
┌─────────────────────────┐
│ Phase 0: Detect MCP     │  ✓ MCP available
│         Fetch from Jira │  → Get task details automatically
└────────┬────────────────┘
         │
         ▼
┌─────────────────────────┐
│ Phase 1: Search Context │  Search similar tasks + Epic + Docs
└────────┬────────────────┘
         │
         ▼
┌─────────────────────────┐
│ Phase 2: Generate       │  Create enrichments based on context
└────────┬────────────────┘
         │
         ▼
┌─────────────────────────┐
│ Phase 3: Present        │  Show enrichments, ask how to apply
└────────┬────────────────┘
         │
         ▼
┌─────────────────────────┐
│ Phase 4: Apply to Jira  │  Update description, create subtasks
└────────┬────────────────┘
         │
         ▼
┌─────────────────────────┐
│ Phase 5: Confirm        │  Show Jira links, verify success
└─────────────────────────┘
```

### Manual Mode (no MCP)
```
┌─────────────────────────┐
│ Phase 0: Detect Mode    │  ✗ MCP not available
│         Get from User   │  → Ask for screenshot or details
└────────┬────────────────┘
         │
         ▼
┌─────────────────────────┐
│ Phase 1: Use Templates  │  Apply best practices & patterns
│         Ask Questions   │  → Gather context from user
└────────┬────────────────┘
         │
         ▼
┌─────────────────────────┐
│ Phase 2: Generate       │  Create enrichments based on templates
└────────┬────────────────┘
         │
         ▼
┌─────────────────────────┐
│ Phase 3: Present        │  Show enrichments, ask export format
└────────┬────────────────┘
         │
         ▼
┌─────────────────────────┐
│ Phase 4: Export Files   │  Create markdown/Jira markup files
└────────┬────────────────┘
         │
         ▼
┌─────────────────────────┐
│ Phase 5: Confirm        │  Show file paths, next steps
└─────────────────────────┘
```

**Key principles:**
- Automatic mode detection (try MCP first, fall back to Manual)
- Manual mode is fully functional without any external dependencies
- Always present enrichments before applying
- User chooses how to use the enrichments

---

## Version History

- **v1.0**: Initial release
  - 7-phase workflow
  - Jira + Confluence search
  - Description, ACs, subtasks, context, tests
  - User confirmation before changes

**Roadmap:**
- **v1.1**: Learning mode (save successful patterns)
- **v2.0**: Batch mode (multiple tasks)
- **v2.1**: Custom templates per project
- **v3.0**: AI-powered similarity detection

---

**Skill created:** 2026-02-06
**Last updated:** 2026-02-06
**Maintainer:** Claude Code Skills
