# Enrich Jira Task Skill - File Index

Quick navigation for all skill files.

## Core Files

### SKILL.md
**Main skill prompt** - Dual-mode workflow (Manual + Automated), enrichment patterns
- Overview and operating modes
- Complete workflow (Phase 0-5)
- Mode detection and fallback
- Enrichment best practices
- Search patterns reference
- Templates reference
- Troubleshooting for both modes

### README.md
**User documentation** - Quick start, mode comparison, setup (optional)
- Quick start guide (both modes)
- Two modes: Manual (default) vs Automated (optional)
- What it does (features)
- Setup instructions (optional, for Automated Mode only)
- Usage examples
- FAQ

### MANUAL-MODE-GUIDE.md ⭐ NEW
**Complete guide for Manual Mode** - No MCP required!
- How Manual Mode works
- Quick start with/without screenshots
- What enrichments you get
- Export options (markdown, Jira markup, separate files)
- Workflow examples
- Tips for better results
- Quality indicators
- Common use cases
- Troubleshooting
- FAQ

### CHANGELOG.md ⭐ NEW
**Version history and migration guide**
- v1.1: MCP now optional, Manual Mode added
- v1.0: Initial release
- Breaking changes (none!)
- Migration guide
- Feature comparison

## Reference Documentation

### references/search-patterns.md
**JQL and CQL patterns** for finding context
- JQL patterns (similar tasks, epic context, component-based, failed tasks)
- CQL patterns (documentation, architecture, API specs)
- Keyword extraction strategies
- Search optimization tips

### references/enrichment-templates.md
**Templates for all enrichment types**
- Description templates (feature, bug fix, refactoring)
- Acceptance criteria patterns (5 formats)
- Subtask breakdown templates
- Technical context patterns
- Test case templates

### references/acceptance-criteria-guide.md
**Complete guide to writing ACs**
- What are acceptance criteria
- Given-When-Then format
- INVEST principles
- Common patterns
- Anti-patterns to avoid
- Examples by domain

## Templates

### templates/enriched-description.md
**Description templates by task type**
- Feature implementation template
- Bug fix template
- Technical improvement/refactoring template
- Template selection guide

### templates/subtask-template.md
**Subtask breakdown templates**
- Generic subtask template
- Full-stack breakdown (5 subtasks: DB → API → UI → Tests → Docs)
- API-only breakdown
- Infrastructure/DevOps breakdown
- Naming conventions

### templates/test-cases.md
**Test case templates**
- Standard test case template
- Happy path tests
- Validation/edge case tests
- Error handling tests
- Performance tests
- Security tests
- Integration tests
- Accessibility tests

## Examples

### examples/before-after-1.md
**Example: Minimal task → Fully enriched**
- Task: AUTH-123 "Add user authentication"
- Before: Empty description, no ACs
- After: Comprehensive description, 7 ACs, 5 subtasks, technical context
- Shows: Context gathering, pattern extraction, enrichment synthesis

### examples/before-after-2.md
**Example: Complex epic → Subtask breakdown**
- Task: NOTIF-200 "Implement notification system"
- Before: Vague requirements, no breakdown
- After: Detailed architecture, 7 ACs, 10 subtasks with dependencies
- Shows: Epic-level planning, risk analysis, dependency mapping

## File Relationships

```
SKILL.md (main prompt)
    │
    ├──→ references/
    │       ├── search-patterns.md (JQL/CQL queries)
    │       ├── enrichment-templates.md (all enrichment types)
    │       └── acceptance-criteria-guide.md (AC writing guide)
    │
    ├──→ templates/
    │       ├── enriched-description.md (description formats)
    │       ├── subtask-template.md (subtask formats)
    │       └── test-cases.md (test case formats)
    │
    └──→ examples/
            ├── before-after-1.md (simple task enrichment)
            └── before-after-2.md (epic breakdown)
```

## Usage Flow

### Manual Mode (Default)

1. **User runs**: `/enrich-jira-task`

2. **Skill detects**: No MCP → Manual Mode

3. **User provides**: Screenshot or task details

4. **Skill reads**:
   - `SKILL.md` (Manual Mode workflow)
   - `references/enrichment-templates.md` (templates to apply)
   - `references/acceptance-criteria-guide.md` (AC patterns)

5. **Skill executes**:
   - Phase 0: Get task info from user
   - Phase 1: Use templates and best practices
   - Phase 2: Generate enrichments
   - Phase 3: Present to user
   - Phase 4: Export to markdown file
   - Phase 5: Confirm file created

6. **User gets**:
   - Markdown file with all enrichments
   - Ready to copy/paste to Jira

### Automated Mode (Optional)

1. **User runs**: `/enrich-jira-task PROJ-123`

2. **Skill detects**: MCP available → Automated Mode

3. **Skill reads**:
   - `SKILL.md` (Automated Mode workflow)
   - `references/search-patterns.md` (JQL/CQL queries)
   - `references/enrichment-templates.md` (enrichment formats)

4. **Skill executes**:
   - Phase 0: Fetch task from Jira (MCP)
   - Phase 1: Search context (JQL/CQL from search-patterns.md)
   - Phase 2: Generate enrichments (templates + context)
   - Phase 3: Present to user
   - Phase 4: Apply to Jira (if approved)
   - Phase 5: Confirm changes in Jira

5. **User sees**:
   - Interactive workflow
   - Changes applied to Jira
   - Links to updated tasks

## Quick Links by Task

### "I want to get started quickly (no setup)"
→ Read `MANUAL-MODE-GUIDE.md` ⭐

### "I want to understand the skill"
→ Read `README.md`

### "I want to know what changed in v1.1"
→ Read `CHANGELOG.md` ⭐

### "I don't have MCP setup"
→ Read `MANUAL-MODE-GUIDE.md` - works without MCP! ⭐

### "I want to customize search patterns" (Automated Mode)
→ Edit `references/search-patterns.md`

### "I want to modify enrichment templates" (Both modes)
→ Edit `references/enrichment-templates.md`

### "I want to see example results"
→ Read `examples/before-after-1.md` or `examples/before-after-2.md`

### "I want to improve AC generation"
→ Edit `references/acceptance-criteria-guide.md`

### "I want to change the workflow"
→ Edit `SKILL.md` (Phases 0-5)

## File Sizes (Approximate)

- `SKILL.md`: ~800 lines (comprehensive workflow)
- `README.md`: ~500 lines (user documentation)
- `references/search-patterns.md`: ~450 lines (JQL/CQL patterns)
- `references/enrichment-templates.md`: ~600 lines (all templates)
- `references/acceptance-criteria-guide.md`: ~550 lines (AC guide)
- `templates/enriched-description.md`: ~350 lines (description templates)
- `templates/subtask-template.md`: ~450 lines (subtask templates)
- `templates/test-cases.md`: ~550 lines (test templates)
- `examples/before-after-1.md`: ~500 lines (full example)
- `examples/before-after-2.md`: ~550 lines (full example)

**Total**: ~5,300 lines of comprehensive skill documentation

---

**Last updated**: 2026-02-06
**Skill version**: 1.0
