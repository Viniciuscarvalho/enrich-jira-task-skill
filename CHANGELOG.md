# Changelog - Enrich Jira Task Skill

## [v1.1] - 2026-02-10

### 🎯 Major Changes: MCP Now Optional!

**Breaking Change:** MCP integration is now **optional** instead of required.

### ✨ New Features

#### Dual-Mode Operation
- **Manual Mode (Default)**: Works without any MCP setup
  - Accepts screenshots or manual input
  - Uses best practices and templates
  - Exports to markdown files
  - No API keys or authentication required

- **Automated Mode (Optional)**: Enhanced with MCP integration
  - Auto-fetches from Jira
  - Searches similar tasks and docs
  - Can apply changes directly to Jira
  - Requires Atlassian MCP Plugin

#### Automatic Mode Detection
- Skill automatically detects if MCP is available
- Seamlessly falls back to Manual Mode if MCP not configured
- No user intervention needed for mode switching

#### Enhanced Export Options

**Manual Mode exports:**
- Single comprehensive markdown file
- Jira-compatible markup format
- Plain markdown for general use
- Separate files per section (description, ACs, subtasks, tests)

**Automated Mode exports:**
- Direct Jira updates (description, subtasks, comments)
- Markdown file backup
- Selective application (choose which parts to apply)

### 🔄 Modified Phases

#### Phase 0: Initialization
- Now detects MCP availability first
- Falls back to user input if MCP unavailable
- Supports screenshot analysis in Manual Mode

#### Phase 1: Contextualization
- **Automated**: Searches Jira/Confluence (unchanged)
- **Manual**: Uses templates, best practices, and user-provided context

#### Phase 3: User Presentation
- Different options based on mode
- Manual Mode focuses on export formats
- Automated Mode offers Jira integration

#### Phase 4: Application/Export
- Automated: Apply to Jira (as before)
- Manual: Export to files (new)

#### Phase 5: Confirmation
- Shows different summaries based on mode
- Manual Mode: File paths and next steps
- Automated Mode: Jira links and changes

### 📝 Documentation Updates

#### SKILL.md
- Added "Operating Modes" section
- Updated all phases for dual-mode support
- Enhanced troubleshooting for both modes
- Added workflow diagrams for each mode

#### README.md
- Emphasized "No setup required" for Manual Mode
- Clarified MCP as optional enhancement
- Updated quick start examples
- Added mode comparison

### 🐛 Bug Fixes
- N/A (new feature release)

### 🔧 Improvements

#### User Experience
- No more failed attempts to connect to MCP
- Clear communication about which mode is active
- Graceful degradation when MCP unavailable
- Better error messages and fallbacks

#### Flexibility
- Works offline (Manual Mode)
- Privacy-friendly (no external API calls in Manual Mode)
- Can be used without Jira access
- Export formats for different workflows

### ⚠️ Breaking Changes

**None for existing Automated Mode users:**
- If MCP is configured, workflow is identical to v1.0
- Automated Mode behavior unchanged

**For new users:**
- Skill now defaults to Manual Mode if MCP not detected
- This is a feature, not a bug!

### 📋 Migration Guide

**No migration needed!**

- **If you have MCP configured:** Skill works exactly as before (Automated Mode)
- **If you don't have MCP:** Skill now works for you too (Manual Mode)
- **If MCP stops working:** Skill automatically falls back to Manual Mode

### 🎯 Usage Examples

#### Manual Mode (New)
```bash
/enrich-jira-task
# Provide screenshot or task details when prompted
# Get enrichments exported to markdown file
```

#### Automated Mode (Existing)
```bash
/enrich-jira-task PROJ-123
# If MCP configured, auto-fetches from Jira
# Can apply changes directly to Jira
```

### 🔮 Future Enhancements

Planned for v1.2:
- Image OCR for better screenshot parsing
- Batch enrichment (multiple tasks at once)
- Custom template configurations per project
- Integration with other project management tools

### 📊 Statistics

- **Files modified:** 2 (SKILL.md, README.md)
- **New modes:** 1 (Manual Mode)
- **Lines added:** ~400
- **Lines modified:** ~200
- **Backward compatibility:** 100%

---

## [v1.0] - 2026-02-06

### Initial Release

- 7-phase workflow
- Jira + Confluence integration via MCP
- Automatic context gathering
- Enrichment generation (descriptions, ACs, subtasks, context, tests)
- User approval before changes
- Direct Jira updates

---

**Version History:**
- v1.1 (2026-02-10): MCP now optional, added Manual Mode
- v1.0 (2026-02-06): Initial release with MCP integration
