# Enrich Jira Task Skill

Transform incomplete Jira tasks into well-defined, actionable work items with comprehensive enrichments. **Works with or without MCP integration!**

## Quick Start

```bash
# Manual Mode (no setup required!)
/enrich-jira-task
# Then provide a screenshot or task details

# Automated Mode (with MCP)
/enrich-jira-task PROJ-123
```

## What It Does

This skill generates comprehensive enrichments for your Jira tasks:

1. 📋 **Gathers task information** (screenshot, manual input, or auto-fetch from Jira)
2. 🔍 **Analyzes context** (similar tasks, docs, or best practices)
3. 🧠 **Generates enrichments**:
   - Expanded descriptions with examples and use cases
   - Acceptance criteria in Given-When-Then format
   - Subtasks for complex work breakdown
   - Technical context (dependencies, risks, resources)
   - Test cases aligned with acceptance criteria
4. 📊 **Presents enrichments** for your review
5. 💾 **Delivers results** (export to file OR apply directly to Jira)

## Two Modes: Choose What Works for You

### 🔧 Manual Mode (Default - No Setup Required!)

**Perfect for:**
- Quick enrichments without Jira API setup
- Privacy-conscious workflows
- Reviewing before applying to Jira
- Teams without MCP access

**How it works:**
1. Provide task info (screenshot or paste details)
2. Get comprehensive enrichments based on best practices
3. Export to markdown file or copy formatted text
4. Manually apply to Jira as needed

**No installation, no API keys, no problem!**

### 🚀 Automated Mode (Optional - With MCP)

**Perfect for:**
- Frequent task enrichment workflows
- Automatic context from similar tasks
- Direct Jira updates
- Full Confluence integration

**How it works:**
1. Automatically fetches task from Jira
2. Searches similar tasks and documentation
3. Generates enrichments with real context
4. Can apply changes directly to Jira

**Setup required:** Atlassian MCP Plugin (see below)

## Setup (Optional - Only for Automated Mode)

### Install Atlassian MCP Plugin

1. **Check if already installed**:
   ```bash
   claude-code plugins list
   ```

2. **Install if needed**:
   ```bash
   claude-code plugins install atlassian
   ```

3. **Authenticate with Atlassian**:
   Follow the OAuth setup flow when prompted

4. **Test connection**:
   ```bash
   /enrich-jira-task YOURPROJECT-1
   ```

**If MCP setup fails or you prefer not to use it:** The skill automatically falls back to Manual Mode. No problem!

## Usage Examples

### Example 1: Minimal Task → Fully Enriched

**Before:**
```
PROJ-123: Add user authentication

Description: (empty)
```

**After running `/enrich-jira-task PROJ-123`:**
```
PROJ-123: Add user authentication

Description:
## Overview
Implement user authentication system to secure application access
and enable personalized user experiences.

## Context
Based on authentication patterns used in PROJ-100 (OAuth integration)
and architecture documented in "Security Architecture v2.0" Confluence page.

## Detailed Requirements
- Support email/password authentication
- Implement JWT token-based session management
- Add password reset functionality
- Support OAuth providers (Google, GitHub)

## Use Cases
- UC-1: User registers with email/password
- UC-2: User logs in with existing credentials
- UC-3: User resets forgotten password

## Acceptance Criteria
AC-1: User registration
Given user provides valid email and password
When registration form is submitted
Then account is created and confirmation email sent

AC-2: Secure password storage
Given user password is provided
When stored in database
Then password is hashed with bcrypt (cost factor 12)

AC-3: JWT token generation
Given user successfully authenticates
When login is complete
Then valid JWT token with 24h expiry is returned

## Technical Context
Dependencies:
- Database schema migrations (PROJ-122) must be complete
- Email service integration (PROJ-120) required for confirmations

Risks:
- Password reset flow vulnerable to timing attacks (mitigation: use constant-time comparison)
- JWT secret rotation not yet implemented (track in PROJ-125)

Resources:
- [Security Architecture v2.0](link-to-confluence)
- [Similar implementation: PROJ-100](link-to-jira)

Subtasks created:
- PROJ-124: Setup authentication database schema
- PROJ-125: Implement JWT token generation/validation
- PROJ-126: Build password reset flow
- PROJ-127: Add OAuth provider integration
- PROJ-128: Write authentication tests
```

### Example 2: Complex Task → Broken into Subtasks

**Before:**
```
PROJ-456: Implement notification system

Description: Users need notifications
```

**After:**
The skill creates 5 subtasks:
- PROJ-457: Design notification data model
- PROJ-458: Build notification API endpoints
- PROJ-459: Implement real-time notification delivery (WebSocket)
- PROJ-460: Create notification UI components
- PROJ-461: Add notification preferences management

Each subtask has specific ACs and technical context.

## Interactive Workflow

When you run the skill, you'll be guided through:

### 1. Analysis Phase
```
🔍 Analyzing PROJ-123...
✓ Task fetched from Jira
✓ Found 3 similar tasks: PROJ-100, PROJ-105, PROJ-110
✓ Found 2 relevant Confluence docs
✓ Epic context loaded: PROJ-50
```

### 2. Preview Phase
```
📋 Current Task: PROJ-123
---
[Shows original task state]

✨ Proposed Enrichments
---
[Shows all generated enrichments with sources]
```

### 3. Approval Phase
```
🎯 What would you like to do?

1. Apply all enrichments (Recommended)
   ✓ Updates task description
   ✓ Adds acceptance criteria
   ✓ Creates 5 subtasks
   ✓ Adds technical context comment

2. Apply selectively
   Choose which parts to apply

3. Add as comment only
   Non-invasive, just adds enrichments as comment

4. Review and edit first
   See editable version before applying

5. Cancel
```

### 4. Application Phase
```
✨ Applying enrichments...
✓ Description updated (+350 words)
✓ Created subtask PROJ-124
✓ Created subtask PROJ-125
✓ Created subtask PROJ-126
✓ Added technical context comment
```

### 5. Validation Phase
```
✅ Enrichment Complete!

View updated task: https://your-jira.atlassian.net/browse/PROJ-123

Subtasks:
- PROJ-124: Setup authentication database schema [View]
- PROJ-125: Implement JWT token generation [View]
- PROJ-126: Build password reset flow [View]
```

## When to Use This Skill

### Perfect for:
- ✅ Tasks with **minimal or vague descriptions**
- ✅ Tasks **missing acceptance criteria**
- ✅ **Complex tasks** that need breakdown into subtasks
- ✅ Tasks needing **technical context** from related work
- ✅ **Ensuring consistency** with established project patterns

### Not ideal for:
- ❌ Already well-defined tasks with clear ACs
- ❌ Simple tasks (< 1 day of work)
- ❌ Tasks with complete, detailed descriptions

## What Gets Generated

### 1. Enriched Description
- **Overview**: Concise summary of what and why
- **Context**: Background from similar tasks and documentation
- **Detailed requirements**: Specific, actionable requirements
- **Use cases**: Primary, secondary, and edge case scenarios
- **Examples**: Concrete examples from past work
- **Out of scope**: Explicit exclusions to prevent scope creep

### 2. Acceptance Criteria (3-7 criteria)
- **Format**: Given-When-Then
- **Coverage**: Happy path, error cases, non-functional requirements
- **Testable**: Specific and measurable
- **Source**: Extracted from similar tasks and documentation

### 3. Subtasks (when needed)
- **Created when**: Task description > 200 words or multiple implementation phases
- **Structure**: Setup → Implementation → Integration → Testing
- **Each has**: Title, description, ACs, effort estimate
- **Pattern**: Copied from similar task breakdowns

### 4. Technical Context
- **Dependencies**: Tasks that block or are blocked by this one
- **Risks**: Identified from past tasks and documentation
- **Resources**: Links to relevant docs and similar tasks
- **Technical notes**: Architecture constraints, performance considerations

### 5. Test Cases
- **Derived from**: Acceptance criteria (minimum 1 TC per AC)
- **Includes**: Preconditions, steps, expected results, test data
- **Coverage**: Happy path, edge cases, error handling

## Customization

### Search Scope
The skill searches:
- **Same project** for similar tasks
- **Same epic** for related work
- **Confluence spaces**: Project space, Engineering, Product
- **Time range**: Last 90 days for most queries

### Enrichment Thresholds
- **Min acceptance criteria**: 3
- **Max acceptance criteria**: 7
- **Subtask creation threshold**: 200 words in enriched description
- **Similar tasks analyzed**: Up to 10
- **Confluence docs analyzed**: Up to 10

## Advanced Usage

### Focus on Specific Enrichment Types (Future)

```bash
# Focus on acceptance criteria generation
/enrich PROJ-123 --focus=acceptance-criteria

# Focus on subtask breakdown
/enrich PROJ-123 --focus=subtasks

# Focus on technical context only
/enrich PROJ-123 --focus=context
```

*Note: Focus mode not yet implemented in v1.0*

### Batch Enrichment (Future)

```bash
# Enrich all tasks in an epic
/enrich --epic PROJ-50

# Enrich all unrefined tasks in backlog
/enrich --jql "project=PROJ AND description is EMPTY"
```

*Note: Batch mode planned for v2.0*

## Troubleshooting

### "CloudId not found"
**Cause:** Atlassian MCP plugin not authenticated

**Solution:**
```bash
# Check authentication
claude-code plugins list

# Re-authenticate if needed
claude-code plugins install atlassian
```

### "Task not found"
**Cause:** Ticket ID doesn't exist or you don't have access

**Solution:**
1. Verify ticket ID in Jira browser
2. Check you have read permissions on the project
3. Ensure you're using the correct format: `PROJECTKEY-123`

### "Search returns no results"
**Cause:** No similar tasks or keywords too specific

**Solution:**
- The skill will still generate enrichments based on best practices
- Try enriching a related epic task first to establish patterns
- Check if Confluence documentation exists for your domain

### "Subtask creation fails"
**Cause:** Project settings don't allow subtasks

**Solution:**
- Skill automatically falls back to adding subtask suggestions in a comment
- You can manually create subtasks from the suggestions
- Contact Jira admin to enable subtasks for your project

### "Description update fails"
**Cause:** Task field locked by workflow or permissions

**Solution:**
- Skill falls back to adding enrichments as a comment
- You can copy from comment and paste manually
- Check with Jira admin about field edit permissions

## Integration with Other Skills

### Works well with:
- **`/product-manager`**: Uses same acceptance criteria patterns (Given-When-Then)
- **`/creating-pr`**: Enrich task first, then create PR with richer context
- **`/jira-to-pr-workflow`**: Enrich task before starting implementation workflow

### Workflow Example:
```bash
# 1. Enrich the task
/enrich PROJ-123

# 2. Implement the feature (future integration)
/jira-to-pr-workflow PROJ-123

# 3. Task already has ACs and context for implementation!
```

## Privacy & Data

### What this skill accesses:
- ✅ Jira task data (title, description, metadata)
- ✅ Related tasks in same project/epic
- ✅ Confluence documentation (if accessible)
- ✅ Your Atlassian user info (for cloudId)

### What this skill does NOT:
- ❌ Access private tasks you don't have permissions for
- ❌ Modify tasks without explicit approval
- ❌ Send data outside your Atlassian instance
- ❌ Store task data beyond the current session

### Data retention:
- Enrichments are generated in real-time
- No task data is stored locally (future learning mode may change this with opt-in)
- Search results are used only for current enrichment session

## FAQ

**Q: Will this overwrite my existing task description?**
A: No! By default, enrichments are added below existing content. You can also choose "Add as comment only" to avoid any description changes.

**Q: Can I edit the enrichments before applying?**
A: Yes! Choose "Review and edit first" when prompted, and you'll get an editable version.

**Q: What if I don't have Confluence access?**
A: The skill will work fine with just Jira. It'll find context from similar tasks and use best practices for enrichment.

**Q: How does it know what acceptance criteria to write?**
A: It analyzes similar completed tasks in your project, looks for patterns, and adapts them to your task. If no similar tasks exist, it uses product management best practices.

**Q: Will it create duplicate subtasks?**
A: No. It checks for existing subtasks first and won't create duplicates.

**Q: Can I customize the templates?**
A: Not yet in v1.0, but future versions will support project-specific template customization.

**Q: Does it work with Jira Server or Data Center?**
A: It's designed for Jira Cloud via Atlassian MCP plugin. Check plugin documentation for on-premise support.

## Examples

See the `examples/` directory for full before/after examples:

- **`before-after-1.md`**: Minimal task → Comprehensive enrichment
- **`before-after-2.md`**: Complex epic-level task → Subtask breakdown

## Contributing

Found a bug or have a suggestion?

1. Check existing issues in the Claude Code repository
2. Create a detailed issue with:
   - Jira task example (anonymized)
   - Expected vs actual enrichment
   - Your Jira/Confluence setup details

## Version History

- **v1.0** (2026-02-06): Initial release
  - 7-phase guided workflow
  - Jira + Confluence search
  - Description, ACs, subtasks, context, tests generation
  - User confirmation before all changes

## Roadmap

- **v1.1**: Learning mode (save successful patterns per project)
- **v2.0**: Batch mode (enrich multiple tasks at once)
- **v2.1**: Custom templates per project
- **v3.0**: AI-powered similarity detection and auto-refinement

## License

Part of Claude Code Skills collection.

## Support

For help:
- Run `/help` in Claude Code
- Check troubleshooting section above
- File issues at https://github.com/anthropics/claude-code/issues

---

**Happy enriching! 🚀**
