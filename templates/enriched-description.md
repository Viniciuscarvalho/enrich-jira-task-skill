# Enriched Description Template

Use this template when generating expanded task descriptions. Adapt sections based on task type (feature, bug, refactoring).

---

## For Feature Implementation

```markdown
## Overview
{1-2 sentence summary of what this feature does and why it's valuable}

## Context
{Background information from similar tasks, epic context, or documentation}
This feature is part of {larger initiative/epic} and builds upon {related work}.

## Detailed Requirements

### Core Functionality
- {Specific requirement 1}
- {Specific requirement 2}
- {Specific requirement 3}

### Non-Functional Requirements
- **Performance**: {Response time, throughput, or resource usage targets}
- **Security**: {Security requirements or compliance needs}
- **Scalability**: {How it should scale}
- **Accessibility**: {WCAG compliance or a11y requirements}

## Use Cases

### UC-1: {Primary use case title}
**Actor**: {User role or system}
**Goal**: {What the actor wants to achieve}
**Flow**:
1. {Step 1}
2. {Step 2}
3. {Expected outcome}

**Example**:
{Concrete example of this use case}

### UC-2: {Secondary use case title}
**Actor**: {User role or system}
**Goal**: {What the actor wants to achieve}
**Flow**:
1. {Step 1}
2. {Expected outcome}

### UC-3: {Edge case use case title}
**Actor**: {User role or system}
**Goal**: {What happens in edge condition}
**Flow**:
1. {Edge condition setup}
2. {How system handles it}

## Edge Cases to Consider
- **Edge Case 1**: {Description from documentation or past tasks}
  - Handling: {How to address}
- **Edge Case 2**: {Description from documentation or past tasks}
  - Handling: {How to address}
- **Edge Case 3**: {Error condition}
  - Handling: {How to gracefully handle}

## Examples

### Example 1: {Scenario}
**Input**: {Sample input data}
**Process**: {What happens}
**Output**: {Expected result}

### Example 2: {Scenario}
**Input**: {Sample input data}
**Process**: {What happens}
**Output**: {Expected result}

## Out of Scope
To prevent scope creep, the following are explicitly NOT included in this task:
- {Out of scope item 1}
- {Out of scope item 2}
- {Future enhancement to track separately}

## References
- [Related task: {TASK-ID}]({Jira URL}): {Why relevant}
- [Documentation: {Doc title}]({Confluence URL}): {What to reference}
- [Design: {Design name}]({Figma/Design URL}): {Design reference}
```

---

## For Bug Fixes

```markdown
## Problem Description
{Clear description of the bug from original report}

## Current Behavior (Incorrect)
{Detailed description of what currently happens}

**Symptoms**:
- {Symptom 1}
- {Symptom 2}

## Expected Behavior (Correct)
{Detailed description of what should happen}

## Steps to Reproduce
1. {Precondition setup}
2. {Action 1}
3. {Action 2}
4. {Observe incorrect behavior at step X}

**Reproduction Rate**: {100% / Intermittent / Rare}

## Root Cause Analysis
{Technical explanation of why the bug occurs}
{Extracted from similar bug investigations or current investigation}

**Contributing factors**:
- {Factor 1}
- {Factor 2}

## Proposed Solution
{How to fix the bug}

**Approach**:
{High-level fix strategy based on similar bug fixes}

**Changes Required**:
- {File/component 1}: {What needs to change}
- {File/component 2}: {What needs to change}

## Impact Analysis

### User Impact
- **Severity**: {Critical/High/Medium/Low}
- **Affected users**: {Percentage or user segment}
- **Frequency**: {How often users encounter this}

### System Impact
- **Affected components**: {List components}
- **Data integrity**: {Any data corruption concerns}
- **Performance**: {Any performance impact}

## Workaround (if available)
{Temporary workaround users can use}

**Steps**:
1. {Workaround step 1}
2. {Workaround step 2}

**Limitations**: {What the workaround doesn't address}

## Regression Risk
- **Risk level**: {High/Medium/Low}
- **Areas to test**: {What else might break}
- **Mitigation**: {How to prevent regressions}

## References
- [Similar bug: {BUG-ID}]({Jira URL}): {How it was fixed}
- [Original report: {BUG-ID}]({Jira URL}): {Original bug report}
- [Related documentation]({Confluence URL}): {Relevant context}
```

---

## For Technical Improvements/Refactoring

```markdown
## Motivation
{Why this improvement is needed}

**Current pain points**:
- {Pain point 1}
- {Pain point 2}

**Drivers**:
- {Technical debt reduction}
- {Performance improvement}
- {Maintainability}
- {Scalability}

## Current State
{How it works today}

**Architecture**:
{Current architecture/approach}

**Issues with current approach**:
- {Issue 1}
- {Issue 2}
- {Issue 3}

**Metrics** (baseline):
- {Metric 1}: {Current value}
- {Metric 2}: {Current value}

## Proposed State
{How it should work after improvement}

**New architecture**:
{Proposed architecture/approach}

**Benefits**:
- {Benefit 1: specific and measurable}
- {Benefit 2: specific and measurable}
- {Benefit 3: specific and measurable}

## Technical Approach
{High-level implementation strategy}

**Strategy**: {Incremental / Big bang / Parallel run / Feature flag}

### Changes Required

#### Component A: {Component name}
- **Current**: {Current implementation}
- **Proposed**: {New implementation}
- **Reason**: {Why changing}

#### Component B: {Component name}
- **Current**: {Current implementation}
- **Proposed**: {New implementation}
- **Reason**: {Why changing}

### Migration Strategy
{How to transition from old to new without breaking things}

**Phase 1**: {Initial phase}
- {Step 1}
- {Step 2}

**Phase 2**: {Next phase}
- {Step 1}
- {Step 2}

**Phase 3**: {Final phase}
- {Step 1}
- {Validation}

## Backward Compatibility
{How to maintain compatibility or plan for breaking changes}

**Breaking changes**: {Yes/No}
- {If yes, list breaking changes}

**Deprecation plan**:
- {Timeline for deprecation}
- {Migration guide for users}

## Risks & Mitigation

### Risk 1: {Risk description}
- **Likelihood**: {High/Medium/Low}
- **Impact**: {High/Medium/Low}
- **Mitigation**: {How to reduce risk}

### Risk 2: {Risk description}
- **Likelihood**: {High/Medium/Low}
- **Impact**: {High/Medium/Low}
- **Mitigation**: {How to reduce risk}

## Success Metrics
{How to measure success of the improvement}

- **Metric 1**: {Baseline} → {Target}
- **Metric 2**: {Baseline} → {Target}
- **Metric 3**: {Baseline} → {Target}

**Validation**:
{How to verify metrics improved}

## Rollback Plan
{If improvement causes issues, how to roll back}

1. {Rollback step 1}
2. {Rollback step 2}
3. {Validation step}

## References
- [Architecture documentation]({Confluence URL}): {Relevant architecture}
- [Similar refactoring: {TASK-ID}]({Jira URL}): {How they approached it}
- [Performance benchmarks]({URL}): {Baseline data}
- [Technical RFC/ADR]({URL}): {Decision context}
```

---

## Template Selection Guide

| Task Type | Template to Use | Key Focus |
|-----------|-----------------|-----------|
| New feature (user-facing) | Feature Implementation | Use cases, examples, out of scope |
| API development | Feature Implementation | API spec, error handling, examples |
| Bug fix | Bug Fixes | Root cause, reproduction steps, regression risk |
| Performance improvement | Technical Improvement | Metrics (before/after), benchmarks |
| Refactoring | Technical Improvement | Migration strategy, backward compatibility |
| Security enhancement | Feature Implementation | Security requirements, threat modeling |
| Infrastructure/DevOps | Technical Improvement | Deployment strategy, rollback plan |
| UI/UX improvement | Feature Implementation | Use cases, accessibility, examples |

---

## Adaptation Tips

1. **Extract from context**: Always check similar tasks for project-specific patterns
2. **Remove unused sections**: Don't include sections that don't apply
3. **Add domain-specific sections**: If your domain needs special sections, add them
4. **Maintain consistency**: Use same terminology as existing project tasks
5. **Be concise**: Templates are starting points, not requirements to fill every section

---

## Section Priority

### Must Have (Every task):
- Overview/Problem Description
- Detailed requirements/Current vs Expected behavior
- Examples or reproduction steps

### Should Have (Most tasks):
- Context/Background
- Edge cases or Impact analysis
- References

### Nice to Have (Complex tasks):
- Migration strategy
- Success metrics
- Rollback plan

---

**Usage in skill:**

When generating enriched descriptions:
1. Identify task type (feature/bug/improvement)
2. Select appropriate template
3. Fill in sections based on context gathered from searches
4. Remove sections that don't apply
5. Add project-specific sections if needed
6. Ensure description is specific, actionable, and testable

---

**Last updated:** 2026-02-06
