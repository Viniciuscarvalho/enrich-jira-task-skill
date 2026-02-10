# Search Patterns Reference

This document contains battle-tested JQL (Jira Query Language) and CQL (Confluence Query Language) patterns for finding relevant context during task enrichment.

## Table of Contents
1. [JQL Patterns](#jql-patterns)
2. [CQL Patterns](#cql-patterns)
3. [Keyword Extraction](#keyword-extraction)
4. [Search Strategy](#search-strategy)

---

## JQL Patterns

### Pattern 1: Similar Completed Tasks

**Purpose:** Find tasks similar to current task that have been successfully completed

**Query:**
```jql
project = "{PROJECT_KEY}" AND
(summary ~ "{keyword1}" OR summary ~ "{keyword2}" OR description ~ "{keywords}") AND
type = "{SAME_ISSUE_TYPE}" AND
status IN (Done, Closed, Resolved) AND
resolution = Done
ORDER BY updated DESC
```

**Variables:**
- `{PROJECT_KEY}`: Current task's project (e.g., "PROJ")
- `{keyword1}, {keyword2}`: Extracted from task title/description
- `{SAME_ISSUE_TYPE}`: Match issue type (Story, Task, Bug, etc.)

**Example:**
```jql
project = "AUTH" AND
(summary ~ "authentication" OR summary ~ "login" OR description ~ "OAuth") AND
type = "Story" AND
status IN (Done, Closed, Resolved) AND
resolution = Done
ORDER BY updated DESC
```

**Why this works:**
- Same project = consistent patterns and architecture
- Same issue type = similar complexity and structure
- Completed status = proven successful implementations
- Recent updates = most relevant to current practices

**Typical results:** 5-15 tasks

---

### Pattern 2: Tasks in Same Epic

**Purpose:** Understand epic context and see related work

**Query:**
```jql
"Epic Link" = "{EPIC_KEY}" AND
status IN (Done, "In Progress", "To Do")
ORDER BY status ASC, created DESC
```

**Variables:**
- `{EPIC_KEY}`: Parent epic of current task (e.g., "PROJ-50")

**Example:**
```jql
"Epic Link" = "AUTH-50" AND
status IN (Done, "In Progress", "To Do")
ORDER BY status ASC, created DESC
```

**Why this works:**
- Shows completed work in epic (patterns to follow)
- Shows pending work (avoid duplicates, find dependencies)
- Status ordering puts completed work first (best examples)

**Typical results:** 3-20 tasks

**Note:** If task has no epic, skip this search.

---

### Pattern 3: Recent Tasks by Component

**Purpose:** Find recent work in the same system component

**Query:**
```jql
project = "{PROJECT}" AND
component = "{COMPONENT}" AND
created >= -90d AND
status IN (Done, Closed, Resolved)
ORDER BY created DESC
```

**Variables:**
- `{COMPONENT}`: Component assigned to task (e.g., "API", "Frontend")

**Example:**
```jql
project = "AUTH" AND
component = "API" AND
created >= -90d AND
status IN (Done, Closed, Resolved)
ORDER BY created DESC
```

**Why this works:**
- Component-specific patterns (e.g., API tasks have swagger docs)
- Recent = reflects current architecture
- 90 days = good balance of recency vs. volume

**Typical results:** 10-30 tasks

**Note:** Only use if task has component assigned.

---

### Pattern 4: Tasks with Similar Labels

**Purpose:** Find tasks with similar categorization

**Query:**
```jql
project = "{PROJECT}" AND
labels IN ("{LABEL1}", "{LABEL2}") AND
status IN (Done, Closed) AND
created >= -180d
ORDER BY updated DESC
```

**Variables:**
- `{LABEL1}, {LABEL2}`: Labels from current task

**Example:**
```jql
project = "AUTH" AND
labels IN ("security", "authentication") AND
status IN (Done, Closed) AND
created >= -180d
ORDER BY updated DESC
```

**Why this works:**
- Labels indicate topic/domain similarity
- Longer time range (180d) since labels are more selective

**Typical results:** 5-20 tasks

**Note:** Only use if task has meaningful labels (skip generic labels like "backend").

---

### Pattern 5: Failed or Reopened Tasks (Risk Analysis)

**Purpose:** Learn from past failures and edge cases

**Query:**
```jql
project = "{PROJECT}" AND
(summary ~ "{keywords}" OR description ~ "{keywords}") AND
(status changed TO "Reopened" OR resolution = "Cannot Reproduce" OR resolution = "Won't Fix") AND
updated >= -180d
ORDER BY updated DESC
```

**Example:**
```jql
project = "AUTH" AND
(summary ~ "authentication" OR description ~ "login") AND
(status changed TO "Reopened" OR resolution = "Cannot Reproduce") AND
updated >= -180d
ORDER BY updated DESC
```

**Why this works:**
- Identifies known pitfalls and edge cases
- Reopened tasks reveal missed requirements
- Failed tasks show what doesn't work

**Typical results:** 2-10 tasks

**Use in enrichment:**
- Extract risks from comments
- Add "edge cases to consider" section
- Include mitigation strategies in technical context

---

### Pattern 6: Tasks by Same Assignee (Personal Patterns)

**Purpose:** Leverage assignee's own past work patterns

**Query:**
```jql
project = "{PROJECT}" AND
assignee = currentUser() AND
type = "{SAME_TYPE}" AND
status IN (Done, Closed) AND
created >= -90d
ORDER BY updated DESC
```

**Example:**
```jql
project = "AUTH" AND
assignee = currentUser() AND
type = "Story" AND
status IN (Done, Closed) AND
created >= -90d
ORDER BY updated DESC
```

**Why this works:**
- People have consistent work patterns
- Personal style in writing ACs and descriptions
- Familiar with their own past context

**Typical results:** 5-15 tasks

**Note:** Only use if current task has assignee.

---

## CQL Patterns

### Pattern 1: Documentation by Keyword

**Purpose:** Find relevant technical documentation

**Query:**
```cql
text ~ "{keyword1} {keyword2}" AND
type = page AND
space IN ("{PROJECT_SPACE}", "Engineering", "Product")
ORDER BY lastModified DESC
```

**Variables:**
- `{keyword1}, {keyword2}`: Domain terms from task
- `{PROJECT_SPACE}`: Project's Confluence space

**Example:**
```cql
text ~ "authentication OAuth JWT" AND
type = page AND
space IN ("AUTH", "Engineering", "Product")
ORDER BY lastModified DESC
```

**Why this works:**
- Text search matches body content (not just title)
- Multiple spaces cover different doc types (specs, architecture, guides)
- Recent modifications indicate maintained docs

**Typical results:** 5-20 pages

---

### Pattern 2: Architecture Documentation

**Purpose:** Find high-level architecture and design docs

**Query:**
```cql
(title ~ "architecture" OR title ~ "design" OR title ~ "technical spec" OR label = "architecture") AND
type = page AND
space IN ("{SPACE1}", "{SPACE2}")
ORDER BY lastModified DESC
```

**Example:**
```cql
(title ~ "architecture" OR title ~ "design" OR label = "architecture") AND
type = page AND
space IN ("Engineering", "AUTH")
ORDER BY lastModified DESC
```

**Why this works:**
- Title and label filtering for architecture docs
- Broader than keyword search (finds all arch docs)

**Typical results:** 3-10 pages

**Use in enrichment:**
- Add to "Technical Context" → Resources
- Extract architectural constraints
- Identify system dependencies

---

### Pattern 3: API Documentation

**Purpose:** Find API specs and endpoint documentation

**Query:**
```cql
(title ~ "API" OR title ~ "endpoint" OR title ~ "swagger" OR label IN ("api", "rest")) AND
type = page
ORDER BY lastModified DESC
```

**Example:**
```cql
(title ~ "Auth API" OR title ~ "endpoint" OR label = "api") AND
type = page
ORDER BY lastModified DESC
```

**Why this works:**
- Targets API-specific documentation
- Includes swagger/OpenAPI specs

**Typical results:** 5-15 pages

**Note:** Only use if task involves API work.

---

### Pattern 4: User Documentation (Edge Cases)

**Purpose:** Find user-facing docs that reveal edge cases and use cases

**Query:**
```cql
(title ~ "{feature}" OR text ~ "{feature}") AND
type = page AND
space = "Product" AND
label IN ("user-guide", "manual", "documentation")
ORDER BY lastModified DESC
```

**Example:**
```cql
(title ~ "login" OR text ~ "authentication") AND
type = page AND
space = "Product" AND
label IN ("user-guide", "manual")
ORDER BY lastModified DESC
```

**Why this works:**
- User docs describe actual use cases
- Often include edge cases and error scenarios
- Product perspective complements technical view

**Typical results:** 2-10 pages

---

### Pattern 5: Meeting Notes and Decisions

**Purpose:** Find decision context and discussions

**Query:**
```cql
(title ~ "meeting" OR title ~ "decision" OR label = "meeting-notes") AND
text ~ "{keywords}" AND
created >= -90d
ORDER BY created DESC
```

**Example:**
```cql
(title ~ "meeting" OR label = "meeting-notes") AND
text ~ "authentication OAuth" AND
created >= -90d
ORDER BY created DESC
```

**Why this works:**
- Captures decision context (the "why")
- Recent meetings (90d) most relevant
- Reveals requirements and constraints

**Typical results:** 3-10 pages

**Use in enrichment:**
- Add to "Context" section
- Extract decision rationale
- Identify stakeholder concerns

---

## Keyword Extraction

### From Task Title

**Rules:**
1. Extract noun phrases: "user authentication" → ["user", "authentication"]
2. Extract action verbs: "implement login" → ["implement", "login"]
3. Remove stopwords: "the", "a", "is", "to", "for"
4. Include compound terms: "OAuth integration" → keep together

**Example:**
- Title: "Implement OAuth authentication for user login"
- Keywords: ["OAuth", "authentication", "user", "login", "implement"]

### From Task Description

**Rules:**
1. Extract domain-specific terms (capitalize, technical)
2. Extract proper nouns (product names, technologies)
3. Limit to top 5-7 keywords by frequency
4. Include synonyms: "auth" → also search "authentication"

**Example:**
- Description: "Users need to authenticate via OAuth. Support Google and GitHub providers."
- Keywords: ["OAuth", "authenticate", "Google", "GitHub", "providers"]

### Keyword Expansion

**Synonyms to add:**
- "auth" → ["authentication", "authorize", "login"]
- "user" → ["account", "customer"]
- "API" → ["endpoint", "REST", "service"]

**Technology mapping:**
- "OAuth" → also search "JWT", "SSO"
- "database" → also search specific DB (Postgres, MongoDB)

---

## Search Strategy

### Phase 1: Broad Discovery

**Goal:** Get overview of available context

**Approach:**
1. Run Pattern 1 (similar tasks) with broad keywords
2. Run Pattern 2 (same epic) if epic exists
3. Run CQL Pattern 1 (documentation) with broad keywords

**Result threshold:** Need at least 5 total results to proceed
- If < 5 results → Broaden keywords, search across more projects
- If > 50 results → Add filters (time range, status)

### Phase 2: Targeted Deep Dive

**Goal:** Get specific, high-quality examples

**Approach:**
1. From Phase 1 results, identify most relevant 3-5 items
2. For each, fetch full details (description, comments, subtasks)
3. If items have labels/components → Run Pattern 4 or 3

**Result threshold:** Need at least 2 high-quality examples
- High quality = has detailed description + acceptance criteria + comments

### Phase 3: Risk and Edge Case Discovery

**Goal:** Find what could go wrong

**Approach:**
1. Run Pattern 5 (failed/reopened tasks)
2. Run CQL Pattern 4 (user docs for edge cases)

**Result threshold:** Optional, proceed even if 0 results

### Parallel Execution

**For performance:** Execute Phase 1 searches in parallel
- JQL Pattern 1 + JQL Pattern 2 + CQL Pattern 1
- All use same keywords
- Aggregate results after all complete

**Error handling:**
- If one search fails → Continue with others
- If all searches fail → Fall back to best practices templates

---

## Search Optimization

### When Searches Return Too Much (>50 results)

**Solutions:**
1. Add time filter: `created >= -60d`
2. Add status filter: Only `Done` (exclude `Closed`)
3. Narrow keywords: Use exact phrases `summary ~ "\"OAuth integration\""`
4. Limit results: `LIMIT 10` at end of JQL

### When Searches Return Too Little (<5 results)

**Solutions:**
1. Broaden keywords: "authentication" → "auth*" (wildcard)
2. Search across projects: `project IN (PROJ1, PROJ2, PROJ3)`
3. Extend time range: `-180d` or remove time filter
4. Try Rovo search (cross-system): `mcp__plugin_atlassian_atlassian__search()`

### When Searches Return Irrelevant Results

**Solutions:**
1. Add negative keywords: `AND summary !~ "test" AND summary !~ "spike"`
2. Filter by resolution: `AND resolution = Done` (exclude "Won't Fix")
3. Add component filter: `AND component = "Backend"`
4. Require multiple keywords: Use AND instead of OR

---

## Example Multi-Search Workflow

**Task:** PROJ-123 "Implement user authentication"

### Step 1: Extract Keywords
- From title: ["implement", "user", "authentication"]
- Expanded: ["auth", "authentication", "login", "user", "account"]

### Step 2: Execute Parallel Searches

**Search 1 (JQL - Similar Tasks):**
```jql
project = "PROJ" AND
(summary ~ "auth*" OR summary ~ "login" OR description ~ "authentication") AND
type = "Story" AND
status IN (Done, Closed) AND
resolution = Done
ORDER BY updated DESC
```
**Result:** 8 tasks found

**Search 2 (JQL - Same Epic):**
(Skipped - task has no epic)

**Search 3 (CQL - Documentation):**
```cql
text ~ "authentication login OAuth" AND
type = page AND
space IN ("PROJ", "Engineering")
ORDER BY lastModified DESC
```
**Result:** 12 pages found

### Step 3: Analyze Top Results

**From 8 similar tasks:**
- 3 have detailed ACs → Extract patterns
- 2 have subtasks → Extract breakdown approach
- 5 have comments with edge cases → Extract risks

**From 12 docs:**
- 1 architecture doc → Extract technical constraints
- 2 API specs → Extract endpoint patterns
- 1 security guideline → Extract security requirements

### Step 4: Generate Enrichments

**Description:** Combine patterns from 3 best task examples + arch doc
**ACs:** Adapt from 3 tasks with detailed ACs
**Subtasks:** Follow breakdown from 2 tasks with subtasks
**Technical context:** Dependencies from arch doc, risks from comments

---

## Common JQL Functions

### Time-based
- `created >= -90d` - Last 90 days
- `updated >= startOfWeek()` - This week
- `resolved > startOfMonth(-1)` - Last month

### User-based
- `assignee = currentUser()` - My tasks
- `reporter in membersOf("developers")` - Team's tasks

### Text search
- `summary ~ "auth*"` - Wildcard
- `description ~ "\"exact phrase\""` - Exact match
- `text ~ "anywhere"` - Search all text fields

### Status & Resolution
- `status WAS "In Progress"` - Was ever in progress
- `status CHANGED TO "Done" DURING (startOfWeek(), endOfWeek())` - Changed this week
- `resolution = Done` - Successfully completed

---

## Common CQL Functions

### Space filtering
- `space = "ENG"` - Exact space
- `space IN ("ENG", "PROD")` - Multiple spaces

### Content type
- `type = page` - Pages only
- `type = blogpost` - Blog posts only

### Labels
- `label = "architecture"` - Exact label
- `label IN ("api", "rest")` - Multiple labels

### Time-based
- `created >= "2025-01-01"` - Absolute date
- `lastModified >= -30d` - Relative date

---

## Best Practices

### DO:
- ✅ Use broad keywords first, narrow if too many results
- ✅ Always order by most recent (`ORDER BY updated DESC`)
- ✅ Filter completed work (`status = Done`) for examples
- ✅ Search multiple spaces in Confluence
- ✅ Use wildcards for technology terms (`OAuth*` catches OAuth2)

### DON'T:
- ❌ Search for >6 keywords at once (too specific)
- ❌ Use only exact matches (miss relevant results)
- ❌ Ignore error handling (searches can fail)
- ❌ Search across all projects by default (slow, irrelevant)
- ❌ Rely on single search (use multiple patterns)

---

## Troubleshooting

### "No results found"
1. Check keywords are relevant (not too specific)
2. Remove time filters (`created >= -90d`)
3. Search parent epic or project level
4. Try Confluence instead of Jira (or vice versa)

### "Search timeout"
1. Reduce result limit (`maxResults: 10`)
2. Add time filter to reduce scope
3. Narrow to single project
4. Try one search at a time instead of parallel

### "Results not relevant"
1. Add more specific keywords
2. Filter by component or label
3. Require completed status (`resolution = Done`)
4. Use exact phrase matching (`"user authentication"`)

---

**Last updated:** 2026-02-06
