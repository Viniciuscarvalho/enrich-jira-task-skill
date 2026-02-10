# Manual Mode Quick Guide

**No MCP? No problem!** Use this skill without any setup or API integration.

## How Manual Mode Works

Manual Mode enriches Jira tasks using:
- ✅ Industry best practices
- ✅ Proven patterns and templates
- ✅ Domain knowledge for common task types
- ✅ Your input and context
- ✅ Screenshot analysis (if provided)

**No external API calls. No authentication. Just enrichments.**

---

## Quick Start

### Option 1: With Screenshot (Recommended)

1. **Take screenshot** of your Jira task (include title, description, acceptance criteria)

2. **Run skill:**
   ```bash
   /enrich-jira-task
   ```

3. **Provide screenshot** when prompted or paste image

4. **Answer questions** about context (optional but helpful):
   - "Are there similar completed tasks you know of?"
   - "Any specific technical constraints?"
   - "What's the task domain (API, UI, DB, etc.)?"

5. **Get enrichments** exported to markdown file

### Option 2: Without Screenshot

1. **Run skill:**
   ```bash
   /enrich-jira-task
   ```

2. **Provide task details** when prompted:
   ```
   Ticket ID: CRED-237
   Summary: [Component] Text Input (iOS)
   Description: Component for text input following Figma design
   Current ACs:
     - Component meets Figma requirements
     - Component is accessible
   Related: CRED-203 (accessibility requirements)
   ```

3. **Answer context questions**

4. **Get enrichments**

---

## What You'll Get

### 1. Enriched Description
Comprehensive description with:
- Overview (what and why)
- Detailed requirements
- Use cases (3-5 scenarios)
- Edge cases to consider
- Examples
- Out of scope (scope control)

### 2. Acceptance Criteria
Given-When-Then format:
- Minimum 3 criteria
- Maximum 7 criteria
- Covers: happy path, edge cases, non-functional requirements
- Testable and specific

### 3. Subtasks (if needed)
Breakdown for complex tasks:
- Setup/Infrastructure
- Core Implementation
- Integration
- Testing & Validation
- Documentation

Each with:
- Clear goal
- Acceptance criteria
- Effort estimate (S/M/L)
- Dependencies

### 4. Technical Context
- Dependencies (blocks/blocked by)
- Risks & mitigation strategies
- Resources (docs, similar tasks)
- Technical notes

### 5. Test Cases
For each acceptance criterion:
- Preconditions
- Test steps
- Expected results
- Test data
- Priority

---

## Export Options

### Option A: Single Markdown File (Recommended)
```
📄 CRED-237-enrichment-2026-02-10.md
```

**Contains:** All sections in one file, ready to copy/paste

**Best for:** Complete review, sharing with team

---

### Option B: Jira Markup Format
```
Display in Jira Wiki Markup syntax
```

**Contains:** Formatted for direct copy/paste into Jira

**Best for:** Quick application to Jira without conversion

---

### Option C: Separate Files
```
📁 CRED-237-enrichments/
   ├── description.md
   ├── acceptance-criteria.md
   ├── subtasks.md
   ├── technical-context.md
   └── test-cases.md
```

**Contains:** Each section in its own file

**Best for:** Selective application, sharing specific sections

---

## Workflow Example

### Scenario: iOS Component Task

**Step 1: Take Screenshot**
![Jira task showing minimal description and basic ACs]

**Step 2: Run Skill**
```bash
/enrich-jira-task
```

**Step 3: Skill Analyzes**
```
✓ Detected task: CRED-237 - Text Input Component (iOS)
✓ Identified gaps:
  - Minimal description
  - Basic ACs, need expansion
  - No subtasks
  - Missing technical context
  - No test cases
```

**Step 4: Skill Asks Questions**
```
Q: "Do you know of similar completed tasks?"
A: "CRED-221 is the Android version, CRED-203 has accessibility requirements"

Q: "Any specific technical constraints?"
A: "Must support iOS 15+, SwiftUI, and VoiceOver"

Q: "What platform patterns should I follow?"
A: "Apple HIG for text fields"
```

**Step 5: Skill Generates**
```
Generating enrichments...
✓ Enriched description (450 words)
✓ 7 acceptance criteria (Given-When-Then)
✓ 5 subtasks (Setup → Implementation → Accessibility → Features → Testing)
✓ Technical context (3 dependencies, 3 risks, 4 resources)
✓ 8 test cases (coverage: happy path, edge cases, accessibility)
```

**Step 6: Choose Export**
```
How would you like to use these enrichments?
> Export to markdown file ✓
```

**Step 7: Review and Apply**
```
✓ File created: ./jira-enrichments/CRED-237-enrichment.md

Next steps:
1. Review enrichments
2. Copy sections to Jira
3. Create subtasks in Jira
4. Share with team for feedback
```

---

## Tips for Better Results

### Provide Context

**Good:**
```
Task: Text Input Component (iOS)
Domain: UI component library
Platform: iOS 15+, SwiftUI
Related: Android version (CRED-221), Accessibility spec (CRED-203)
Constraints: Must follow Apple HIG, support VoiceOver
Similar patterns: We have Button component with similar structure
```

**Less helpful:**
```
Task: Text Input
```

### Use Screenshots Effectively

**Include in screenshot:**
- Task title and ID
- Current description
- Existing acceptance criteria
- Linked tasks (epic, related tickets)
- Labels and components

**Don't worry about:**
- Comments section
- History
- Workflow transitions

### Answer Questions Thoughtfully

Skill may ask:
- "What similar tasks exist?" → Great chance to ensure consistency
- "Any technical constraints?" → Specify frameworks, versions, policies
- "What's the task domain?" → Helps apply relevant patterns (API vs UI vs DB)

### Iterate if Needed

Not satisfied with first result?
```bash
# Run again with more context
/enrich-jira-task

# Provide additional details:
"This is an authentication API endpoint using JWT tokens.
We use Express.js and already have user model in MongoDB.
Similar endpoint: POST /api/auth/login (see PROJ-100)"
```

---

## Quality Indicators

### High-Quality Enrichment

✅ **Description:**
- Clear overview (what/why)
- Specific requirements (not vague)
- 3+ use cases
- Edge cases identified
- Examples provided

✅ **Acceptance Criteria:**
- 3-7 criteria
- Given-When-Then format
- Testable (specific, measurable)
- Covers happy path + edge cases

✅ **Subtasks:**
- Logical phases
- Clear dependencies
- Realistic effort estimates
- Each has own ACs

✅ **Test Cases:**
- Match acceptance criteria
- Include test data
- Cover edge cases
- Accessibility tests (if UI)

### When to Provide More Context

⚠️ **If enrichments seem:**
- Too generic
- Missing domain-specific details
- Not matching your project patterns

**Solution:** Run again with more specific context about:
- Your tech stack
- Similar completed tasks
- Project conventions
- Specific requirements

---

## Common Use Cases

### Use Case 1: Feature Implementation
```
Input: "Add user authentication"
Context: Backend API, Node.js, JWT tokens
Result:
  - Description with JWT flow
  - 5 ACs (register, login, logout, refresh, security)
  - 4 subtasks (DB model, API routes, middleware, tests)
  - Technical context (security risks, token management)
  - 6 test cases
```

### Use Case 2: Bug Fix
```
Input: "Fix login timeout issue"
Context: Users timing out after 5 minutes, should be 30 minutes
Result:
  - Description with root cause analysis
  - 3 ACs (timeout duration, session renewal, error handling)
  - 3 subtasks (config update, backend fix, testing)
  - Technical context (session management, related config)
  - 4 test cases
```

### Use Case 3: UI Component
```
Input: "Text Input Component (iOS)"
Context: SwiftUI, accessibility required, design in Figma
Result:
  - Description with component specs
  - 7 ACs (visual design, keyboard types, validation, accessibility, etc.)
  - 5 subtasks (structure, accessibility, validation, features, testing)
  - Technical context (dependencies, accessibility requirements, HIG)
  - 8 test cases (including VoiceOver, Dynamic Type)
```

### Use Case 4: Refactoring
```
Input: "Refactor authentication module"
Context: Move from sessions to JWT, maintain backward compatibility
Result:
  - Description with migration strategy
  - 4 ACs (JWT implementation, backward compat, migration path, cleanup)
  - 6 subtasks (new JWT logic, migration, testing, docs, deprecation, cleanup)
  - Technical context (breaking changes, risks, rollback plan)
  - 5 test cases (new flow, backward compat, migration)
```

---

## Comparison: Manual vs Automated Mode

| Feature | Manual Mode | Automated Mode |
|---------|-------------|----------------|
| **Setup Required** | None | MCP Plugin + Auth |
| **Context Source** | Templates + User Input | Jira Search + Confluence |
| **Input Method** | Screenshot or Text | Ticket ID |
| **Apply Changes** | Export to File | Direct Jira Update |
| **Privacy** | Fully Offline | API Calls to Jira |
| **Best For** | Quick enrichments, no API | Frequent use, automation |

**Both modes produce the same quality enrichments!**

---

## Troubleshooting

### "Enrichments too generic"
**Solution:** Provide more context:
- Tech stack details
- Similar task examples
- Project-specific patterns

### "Missing domain-specific details"
**Solution:** Specify domain in context:
- "This is a REST API endpoint"
- "This is a SwiftUI component"
- "This is a database migration"

### "Subtasks don't match our workflow"
**Solution:** Mention your workflow:
- "We separate frontend and backend tasks"
- "We always create testing subtasks"
- "We have a docs-first approach"

### "Test cases seem incomplete"
**Solution:** Specify test requirements:
- "We need unit + integration tests"
- "Must include accessibility tests"
- "Need performance test cases"

### "Want to customize templates"
**Solution:** Edit template files:
```
~/.claude/skills/enrich-jira-task/
  ├── templates/enriched-description.md
  ├── templates/subtask-template.md
  └── templates/test-cases.md
```

---

## FAQ

**Q: Is Manual Mode as good as Automated Mode?**
A: Yes! Both produce comprehensive enrichments. Automated Mode adds context from your actual Jira/Confluence, but Manual Mode uses proven templates and best practices.

**Q: Can I switch from Manual to Automated Mode later?**
A: Yes! Just install and configure the MCP plugin. The skill will automatically detect it and use Automated Mode.

**Q: How long does enrichment take?**
A: Usually 30-60 seconds, regardless of mode.

**Q: Can I use Manual Mode for batch enrichment?**
A: Not yet, but planned for v1.2. Currently one task at a time.

**Q: What if I disagree with the enrichments?**
A: Perfect! The markdown file is fully editable. Adjust as needed before applying to Jira.

**Q: Can I save custom templates?**
A: Yes! Edit the template files in the skill directory. Your changes will be used for future enrichments.

---

## Next Steps

1. **Try it out:**
   ```bash
   /enrich-jira-task
   ```

2. **Provide a screenshot** or task details

3. **Review the enrichments**

4. **Copy to Jira** or save for later

5. **Iterate** if needed with more context

6. **Share feedback** to improve the templates!

---

**Happy enriching!** 🚀

For more details, see:
- `SKILL.md` - Complete workflow documentation
- `README.md` - Full feature overview
- `examples/` - Before/after examples
- `templates/` - Customizable templates
