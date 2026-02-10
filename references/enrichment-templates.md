# Enrichment Templates Reference

This document provides templates and patterns for generating high-quality task enrichments. Use these as starting points and adapt based on context from similar tasks and documentation.

## Table of Contents
1. [Description Templates](#description-templates)
2. [Acceptance Criteria Patterns](#acceptance-criteria-patterns)
3. [Subtask Breakdown Templates](#subtask-breakdown-templates)
4. [Technical Context Patterns](#technical-context-patterns)
5. [Test Case Templates](#test-case-templates)

---

## Description Templates

### Template 1: Feature Implementation

```markdown
## Overview
Implement {feature name} to enable {user capability} and {business value}.

## Context
{Background from similar tasks or epic}
This relates to {larger initiative} and builds upon {previous work}.

## Detailed Requirements

### Core Functionality
- {Requirement 1: specific, measurable}
- {Requirement 2: specific, measurable}
- {Requirement 3: specific, measurable}

### Non-Functional Requirements
- **Performance**: {metric and target}
- **Security**: {security requirements}
- **Scalability**: {scalability considerations}

## Use Cases

### UC-1: {Primary use case title}
**Actor**: {User role}
**Goal**: {What user wants to accomplish}
**Flow**:
1. {Step 1}
2. {Step 2}
3. {Expected outcome}

### UC-2: {Secondary use case title}
**Actor**: {User role}
**Goal**: {What user wants to accomplish}
**Flow**:
1. {Step 1}
2. {Expected outcome}

### UC-3: {Edge case use case title}
**Actor**: {User role}
**Goal**: {What user wants to accomplish}
**Flow**:
1. {Step 1 - edge condition}
2. {How system should handle}

## Edge Cases to Consider
- {Edge case 1 from documentation or past tasks}
- {Edge case 2 from documentation or past tasks}
- {Edge case 3 - error condition}

## Examples
{Concrete examples from similar tasks or documentation}

**Example 1**:
Input: {sample input}
Expected output: {sample output}

**Example 2**:
Input: {sample input}
Expected output: {sample output}

## Out of Scope
{Explicitly exclude features to prevent scope creep}
- {Out of scope item 1}
- {Out of scope item 2}

## References
- {Link to related task}
- {Link to Confluence documentation}
- {Link to design mockups if applicable}
```

**When to use:** New feature development, user-facing functionality

---

### Template 2: Bug Fix

```markdown
## Problem Description
{Clear description of the bug from reporter or similar bugs}

## Current Behavior
{What currently happens - wrong behavior}

## Expected Behavior
{What should happen - correct behavior}

## Steps to Reproduce
1. {Step 1}
2. {Step 2}
3. {Observe incorrect behavior}

## Root Cause (if identified)
{Technical explanation of why bug occurs}
{From similar bug resolutions or investigation}

## Proposed Solution
{How to fix the bug}
{Approach based on similar bug fixes}

## Impact Analysis
- **Severity**: {Critical/High/Medium/Low}
- **Affected users**: {Who is impacted}
- **Workaround**: {Temporary workaround if available}

## Edge Cases Affected
- {Related edge case 1}
- {Related edge case 2}

## Regression Risk
- **Risk level**: {High/Medium/Low}
- **Areas to test**: {What else might be affected}
- **Mitigation**: {How to prevent regressions}

## References
- {Link to similar bug fix}
- {Link to original bug report}
- {Link to related documentation}
```

**When to use:** Bug fixes, defect resolution

---

### Template 3: Technical Improvement/Refactoring

```markdown
## Motivation
{Why this improvement is needed}
{Current pain points or technical debt}

## Current State
{How it works now}
{What makes it suboptimal}

## Proposed State
{How it should work}
{Benefits of new approach}

## Technical Approach
{High-level implementation strategy}
{Based on similar refactoring work}

### Changes Required
- **Component A**: {What changes}
- **Component B**: {What changes}
- **Component C**: {What changes}

### Migration Strategy
{How to transition from old to new}
1. {Step 1}
2. {Step 2}
3. {Validation step}

## Benefits
- {Benefit 1: specific and measurable}
- {Benefit 2: specific and measurable}
- {Benefit 3: specific and measurable}

## Risks
- **Risk**: {Potential issue}
  - **Mitigation**: {How to address}

## Backward Compatibility
{How to maintain compatibility or plan for breaking changes}

## Success Metrics
- {Metric 1}: {Current value → Target value}
- {Metric 2}: {Current value → Target value}

## References
- {Link to architecture documentation}
- {Link to similar refactoring}
- {Link to performance benchmarks}
```

**When to use:** Refactoring, performance improvements, technical debt

---

## Acceptance Criteria Patterns

### Pattern 1: Given-When-Then (Standard)

```markdown
### AC-{N}: {Criterion title}

**Given** {precondition or initial state}
**When** {action performed or event triggered}
**Then** {expected outcome - specific and measurable}

**Validation**: {How to verify this criterion is met}
```

**Example:**
```markdown
### AC-1: User login with valid credentials

**Given** user has a registered account with email "user@example.com"
**When** user submits login form with correct password
**Then** user is authenticated and redirected to dashboard within 2 seconds

**Validation**: Manual test with test account + automated integration test
```

---

### Pattern 2: Functional Requirement

```markdown
### AC-{N}: {Functional capability}

**Requirement**: {What the system must do}

**Acceptance**:
- ✓ {Specific acceptance point 1}
- ✓ {Specific acceptance point 2}
- ✓ {Specific acceptance point 3}

**Validation**: {How to test}
```

**Example:**
```markdown
### AC-2: Password strength validation

**Requirement**: System enforces strong password requirements

**Acceptance**:
- ✓ Minimum 12 characters length
- ✓ Contains at least 1 uppercase, 1 lowercase, 1 number, 1 special char
- ✓ Shows real-time feedback as user types
- ✓ Prevents weak passwords (e.g., "password123", "qwerty")

**Validation**: Unit tests for validation logic + manual UI test
```

---

### Pattern 3: Non-Functional Requirement

```markdown
### AC-{N}: {Non-functional aspect}

**Metric**: {What to measure}
**Target**: {Acceptable threshold}
**Measured**: {How and when to measure}

**Acceptance**:
{Pass criteria}

**Validation**: {How to verify}
```

**Example:**
```markdown
### AC-3: API response time

**Metric**: P95 response time for authentication endpoint
**Target**: ≤ 200ms under normal load (100 req/sec)
**Measured**: Load testing with artillery.io

**Acceptance**:
95% of requests complete within 200ms when tested with 100 concurrent users for 5 minutes

**Validation**: Load test results from CI pipeline
```

---

### Pattern 4: Error Handling

```markdown
### AC-{N}: {Error scenario}

**Given** {error condition}
**When** {action attempted}
**Then** {how system gracefully handles error}

**Error details**:
- Error message: "{User-friendly message}"
- HTTP status: {Status code if API}
- Logging: {What gets logged}

**Validation**: {How to test error condition}
```

**Example:**
```markdown
### AC-4: Invalid credentials handling

**Given** user provides incorrect password
**When** user submits login form
**Then** system displays error without revealing whether email exists

**Error details**:
- Error message: "Invalid email or password. Please try again."
- HTTP status: 401 Unauthorized
- Logging: Failed login attempt logged with email hash (not plain text)
- Rate limiting: After 5 failed attempts, account locked for 15 minutes

**Validation**: Test with intentionally wrong credentials
```

---

### Pattern 5: Data Validation

```markdown
### AC-{N}: {Data validation}

**Input**: {What data is being validated}
**Rules**: {Validation rules}
**Valid examples**: {Pass examples}
**Invalid examples**: {Fail examples}

**On success**: {What happens with valid data}
**On failure**: {What happens with invalid data}

**Validation**: {How to test}
```

**Example:**
```markdown
### AC-5: Email address validation

**Input**: User email field
**Rules**:
- Valid email format (RFC 5322 compliant)
- Maximum 254 characters
- Not from disposable email providers

**Valid examples**:
- user@example.com ✓
- firstname.lastname@company.co.uk ✓

**Invalid examples**:
- invalid-email ✗ (no @ symbol)
- user@tempmail.com ✗ (disposable provider)
- toolong@verylongdomainname... ✗ (exceeds 254 chars)

**On success**: Email stored and verification sent
**On failure**: Inline error message displayed below field

**Validation**: Unit tests with valid/invalid examples
```

---

## Subtask Breakdown Templates

### Template 1: Full-Stack Feature

When breaking down a feature that touches frontend, backend, and database:

```markdown
**Subtask 1**: Database schema and migrations
**Type**: Sub-task
**Description**:
Create database schema for {feature} including tables, indexes, and constraints.

**Deliverables**:
- Migration scripts for {tables}
- Rollback scripts
- Schema documentation

**Acceptance Criteria**:
- AC-1: Migration runs successfully on clean database
- AC-2: Rollback restores previous state
- AC-3: Indexes created for expected query patterns

**Estimated effort**: Small (1-2 days)

---

**Subtask 2**: Backend API implementation
**Type**: Sub-task
**Description**:
Implement REST API endpoints for {feature} with full CRUD operations.

**Deliverables**:
- API endpoints: POST /api/{resource}, GET /api/{resource}/:id, etc.
- Request/response validation
- API documentation (OpenAPI spec)

**Acceptance Criteria**:
- AC-1: Endpoints return correct status codes
- AC-2: Request validation rejects invalid payloads
- AC-3: OpenAPI spec generated and up-to-date

**Dependencies**:
- Blocked by: Subtask 1 (database schema)

**Estimated effort**: Medium (3-5 days)

---

**Subtask 3**: Frontend UI components
**Type**: Sub-task
**Description**:
Build UI components for {feature} using {framework}.

**Deliverables**:
- React components: {ComponentA}, {ComponentB}
- Form validation and error handling
- Responsive design (mobile + desktop)

**Acceptance Criteria**:
- AC-1: Components render correctly on all screen sizes
- AC-2: Form validation matches backend rules
- AC-3: Accessibility (WCAG 2.1 AA compliant)

**Dependencies**:
- Blocked by: Subtask 2 (API endpoints)

**Estimated effort**: Medium (3-4 days)

---

**Subtask 4**: Integration and end-to-end testing
**Type**: Sub-task
**Description**:
Write integration tests and E2E tests for {feature} workflow.

**Deliverables**:
- Integration tests for API layer
- E2E tests for user workflows (Cypress/Playwright)
- Test data fixtures

**Acceptance Criteria**:
- AC-1: All happy path scenarios covered
- AC-2: Error handling scenarios tested
- AC-3: Tests run in CI pipeline

**Dependencies**:
- Blocked by: Subtask 3 (frontend components)

**Estimated effort**: Small (2-3 days)

---

**Subtask 5**: Documentation and deployment
**Type**: Sub-task
**Description**:
Update documentation and deploy {feature} to staging.

**Deliverables**:
- User documentation
- Deployment runbook
- Monitoring/alerting setup

**Acceptance Criteria**:
- AC-1: Feature flag configured for gradual rollout
- AC-2: Metrics dashboard created
- AC-3: User docs published to help center

**Dependencies**:
- Blocked by: Subtask 4 (testing complete)

**Estimated effort**: Small (1-2 days)
```

**Total effort**: ~12-16 days across 5 subtasks

---

### Template 2: API Development

When task is API-focused:

```markdown
**Subtask 1**: API design and specification
**Subtask 2**: Data models and repository layer
**Subtask 3**: Business logic and service layer
**Subtask 4**: API controllers and routing
**Subtask 5**: API tests and documentation
```

---

### Template 3: Infrastructure/DevOps

When task involves infrastructure:

```markdown
**Subtask 1**: Infrastructure as Code (Terraform/CloudFormation)
**Subtask 2**: CI/CD pipeline updates
**Subtask 3**: Monitoring and alerting setup
**Subtask 4**: Security and access control
**Subtask 5**: Documentation and runbooks
```

---

## Technical Context Patterns

### Pattern 1: Dependencies

```markdown
## Dependencies

### Blocks (cannot start until this completes)
- **{TASK-ID}**: {Task title}
  - Reason: {Why blocking}

### Blocked By (must complete first)
- **{TASK-ID}**: {Task title}
  - Reason: {Why dependency}
  - Status: {Current status}
  - Expected completion: {Date if known}

### Related (contextually related)
- **{TASK-ID}**: {Task title}
  - Relationship: {How related}
```

**Example:**
```markdown
## Dependencies

### Blocks
- **PROJ-150**: User profile page redesign
  - Reason: Profile page will use new authentication system

### Blocked By
- **PROJ-120**: Database migration to v2
  - Reason: New auth tables not in v1 schema
  - Status: In Progress (80% complete)
  - Expected completion: 2026-02-10

### Related
- **PROJ-100**: OAuth provider integration
  - Relationship: Similar authentication implementation (reference example)
```

---

### Pattern 2: Risks and Mitigations

```markdown
## Risks & Considerations

### Risk-{N}: {Risk title}
**Likelihood**: {High/Medium/Low}
**Impact**: {High/Medium/Low}
**Description**: {What could go wrong}

**Mitigation**:
- {Action 1 to reduce risk}
- {Action 2 to reduce risk}

**Contingency**:
{Backup plan if risk materializes}
```

**Example:**
```markdown
## Risks & Considerations

### Risk-1: Password reset flow vulnerable to timing attacks
**Likelihood**: Medium
**Impact**: High (security vulnerability)
**Description**: Timing differences in email validation could reveal which emails are registered

**Mitigation**:
- Use constant-time string comparison for email checks
- Add artificial delay to all password reset responses (200ms)
- Rate limit password reset requests (3 per hour per IP)

**Contingency**:
If exploited, immediately deploy mitigation and rotate affected user credentials

### Risk-2: JWT token secret rotation not implemented
**Likelihood**: Low (manual process)
**Impact**: High (if secret compromised, all sessions invalid)
**Description**: No automated secret rotation mechanism

**Mitigation**:
- Track as separate task: PROJ-125
- Document manual rotation procedure
- Set calendar reminder for quarterly rotation

**Contingency**:
Manual emergency rotation procedure documented in runbook
```

---

### Pattern 3: Resources and References

```markdown
## Resources

### Documentation
- [{Doc title}]({Confluence URL}): {Brief description}
- [{Doc title}]({Confluence URL}): {Brief description}

### Similar Implementations
- [{TASK-ID}]({Jira URL}): {What's similar}
- [{TASK-ID}]({Jira URL}): {What's similar}

### External References
- [{Library/Tool name}]({URL}): {Official docs}
- [{Article/Guide}]({URL}): {Relevant content}

### Code References
- `{file path}`: {What to reference}
- `{repository}`: {Existing implementation to learn from}
```

**Example:**
```markdown
## Resources

### Documentation
- [Authentication Architecture v2.0](https://confluence.example.com/auth-v2): System architecture and design decisions
- [Security Best Practices](https://confluence.example.com/security): Company security standards

### Similar Implementations
- [PROJ-100](https://jira.example.com/PROJ-100): OAuth integration (reference for token handling)
- [PROJ-85](https://jira.example.com/PROJ-85): Password reset flow (similar implementation)

### External References
- [bcrypt Documentation](https://github.com/kelektiv/node.bcrypt.js): Password hashing library
- [OWASP Authentication Cheat Sheet](https://cheatsheetseries.owasp.org/cheatsheets/Authentication_Cheat_Sheet.html): Security guidelines
- [JWT Best Practices](https://datatracker.ietf.org/doc/html/rfc8725): RFC 8725

### Code References
- `src/auth/oauth-provider.ts`: Existing OAuth implementation to extend
- `backend-common` repo: Shared authentication utilities
```

---

### Pattern 4: Technical Notes

```markdown
## Technical Notes

### Architecture Constraints
- {Constraint 1 from documentation}
- {Constraint 2 from system design}

### Performance Considerations
- {Performance requirement 1}
- {Performance requirement 2}

### Security Requirements
- {Security requirement 1}
- {Security requirement 2}

### Compatibility Notes
- {Backward compatibility concern}
- {Browser/platform support requirement}

### Configuration
- {Environment variable needed}
- {Feature flag to add}
```

**Example:**
```markdown
## Technical Notes

### Architecture Constraints
- Must use existing Redis instance for session storage (no new infrastructure)
- Authentication microservice deployed separately from main app (separate deployment pipeline)

### Performance Considerations
- Password hashing must not block event loop (use worker threads)
- Login endpoint P95 latency target: <200ms
- Support 1000 concurrent login requests

### Security Requirements
- Passwords hashed with bcrypt cost factor 12 (company standard)
- Failed login attempts logged to security audit system
- HTTPS required (no HTTP fallback)
- CORS configuration for auth endpoints: whitelist only

### Compatibility Notes
- Must support browsers: Chrome/Firefox/Safari (last 2 versions), Edge (latest)
- API backward compatible with v1 auth (parallel support for 6 months)

### Configuration
- Environment variables: `JWT_SECRET`, `JWT_EXPIRY`, `SESSION_REDIS_URL`
- Feature flag: `auth_v2_enabled` (gradual rollout)
```

---

## Test Case Templates

### Template 1: Happy Path Test

```markdown
### TC-{N}: {Test scenario - happy path}

**Objective**: Verify {main functionality} works correctly under normal conditions

**Preconditions**:
- {Setup requirement 1}
- {Setup requirement 2}

**Test Steps**:
1. {Action step 1}
2. {Action step 2}
3. {Verification step}

**Expected Result**:
{Specific expected outcome}

**Test Data**:
- Input: {Sample input data}
- Expected output: {Sample output data}

**Priority**: High
**Type**: {Manual/Automated}
```

**Example:**
```markdown
### TC-1: Successful user login with valid credentials

**Objective**: Verify user can log in with correct email and password

**Preconditions**:
- Test user account exists: email "testuser@example.com", password "SecurePass123!"
- User account is active (not locked or suspended)

**Test Steps**:
1. Navigate to login page
2. Enter email "testuser@example.com"
3. Enter password "SecurePass123!"
4. Click "Login" button
5. Verify redirect to dashboard

**Expected Result**:
- Login successful within 2 seconds
- User redirected to /dashboard
- Welcome message displays: "Welcome back, Test User"
- JWT token stored in secure HTTP-only cookie

**Test Data**:
- Email: testuser@example.com
- Password: SecurePass123!
- Expected user ID: 12345

**Priority**: High
**Type**: Automated (E2E test)
```

---

### Template 2: Edge Case Test

```markdown
### TC-{N}: {Edge case scenario}

**Objective**: Verify system handles {edge condition} correctly

**Preconditions**:
- {Setup for edge condition}

**Test Steps**:
1. {Create edge condition}
2. {Perform action}
3. {Verify behavior}

**Expected Result**:
{How system should handle edge case}

**Priority**: Medium
**Type**: {Manual/Automated}
```

**Example:**
```markdown
### TC-2: Login with maximum length email address

**Objective**: Verify system handles 254-character email correctly (RFC 5322 max)

**Preconditions**:
- Test account created with 254-character email
- Email: "very.long.email.address.that.is.exactly.254.characters.long@subdomain.example-domain.com..."

**Test Steps**:
1. Navigate to login page
2. Enter 254-character email address
3. Enter correct password
4. Submit login form

**Expected Result**:
- Email field accepts full 254 characters (no truncation)
- Login succeeds
- Email displayed correctly in UI (with ellipsis if needed)

**Priority**: Medium
**Type**: Automated (Integration test)
```

---

### Template 3: Error Handling Test

```markdown
### TC-{N}: {Error scenario}

**Objective**: Verify system gracefully handles {error condition}

**Preconditions**:
- {Setup for error condition}

**Test Steps**:
1. {Create error condition}
2. {Perform action that triggers error}
3. {Verify error handling}

**Expected Result**:
- {User-friendly error message}
- {System state remains stable}
- {Error logged appropriately}

**Priority**: {High if common error, Medium otherwise}
**Type**: {Manual/Automated}
```

**Example:**
```markdown
### TC-3: Login with incorrect password

**Objective**: Verify system handles authentication failure securely

**Preconditions**:
- Test user account exists: testuser@example.com
- Account not locked (< 5 failed attempts)

**Test Steps**:
1. Navigate to login page
2. Enter valid email "testuser@example.com"
3. Enter incorrect password "WrongPassword123"
4. Submit login form
5. Verify error handling

**Expected Result**:
- HTTP 401 Unauthorized response
- Error message: "Invalid email or password. Please try again."
  (Does NOT reveal which field is incorrect)
- Failed attempt counter incremented
- Error logged: "Failed login attempt for email hash: abc123..."
  (Email hashed in logs, not plain text)
- No redirect (stays on login page)

**Priority**: High
**Type**: Automated (Integration test)
```

---

### Template 4: Performance Test

```markdown
### TC-{N}: {Performance scenario}

**Objective**: Verify {functionality} meets performance requirements

**Preconditions**:
- {Load conditions}

**Test Configuration**:
- Load: {Concurrent users / requests per second}
- Duration: {Test duration}
- Tool: {Load testing tool}

**Test Steps**:
1. {Setup load test}
2. {Execute load}
3. {Measure metrics}

**Expected Result**:
- {Metric 1}: {Target value}
- {Metric 2}: {Target value}

**Acceptance Threshold**:
{Pass/fail criteria}

**Priority**: {High for critical paths, Medium otherwise}
**Type**: Automated (Performance test)
```

**Example:**
```markdown
### TC-4: Login endpoint performance under load

**Objective**: Verify authentication endpoint meets P95 latency target under normal load

**Preconditions**:
- Staging environment with production-like database (10K user accounts)
- Load balancer and caching configured

**Test Configuration**:
- Load: 100 requests/second
- Duration: 5 minutes
- Tool: artillery.io
- Scenario: 50% valid logins, 50% invalid (realistic mix)

**Test Steps**:
1. Configure artillery with 100 virtual users
2. Run test scenario for 5 minutes
3. Collect metrics: P50, P95, P99 latency, error rate

**Expected Result**:
- P50 latency: ≤ 100ms
- P95 latency: ≤ 200ms
- P99 latency: ≤ 500ms
- Error rate: < 0.1% (excluding intentional auth failures)
- CPU usage: < 70%
- Memory usage: stable (no leaks)

**Acceptance Threshold**:
P95 < 200ms AND error rate < 0.1% = PASS

**Priority**: High
**Type**: Automated (CI pipeline performance test)
```

---

### Template 5: Security Test

```markdown
### TC-{N}: {Security scenario}

**Objective**: Verify protection against {security threat}

**Threat Model**: {OWASP category or CVE}

**Preconditions**:
- {Security test environment}

**Attack Simulation**:
1. {Step 1 of attack}
2. {Step 2 of attack}
3. {Expected defense}

**Expected Result**:
{How system defends}

**Security Requirements**:
- {Requirement 1}
- {Requirement 2}

**Priority**: High
**Type**: {Manual/Automated}
```

**Example:**
```markdown
### TC-5: Protection against brute force attacks

**Objective**: Verify rate limiting prevents credential stuffing attacks

**Threat Model**: OWASP A07:2021 – Identification and Authentication Failures

**Preconditions**:
- Test user account exists
- Rate limiter configured (5 attempts per 15 minutes per IP)

**Attack Simulation**:
1. Attempt login with incorrect password
2. Repeat 5 times in quick succession
3. Attempt 6th login (should be blocked)
4. Wait 15 minutes
5. Attempt login again (should work)

**Expected Result**:
- After 5 failed attempts: HTTP 429 Too Many Requests
- Error message: "Too many login attempts. Try again in 15 minutes."
- Account NOT locked (IP-based rate limit, not account lock)
- After 15 minutes: Rate limit resets, login succeeds

**Security Requirements**:
- Rate limit applied per IP address
- Failed attempts logged to security audit
- No enumeration: Same response for valid/invalid emails

**Priority**: High
**Type**: Automated (Security test suite)
```

---

## Adaptation Guidelines

### When to Use Each Template

**Description Templates**:
- Feature Implementation → New user-facing features
- Bug Fix → Defects and issues
- Technical Improvement → Refactoring, performance, tech debt

**Acceptance Criteria Patterns**:
- Given-When-Then → Most common, use as default
- Functional Requirement → Multiple related acceptance points
- Non-Functional → Performance, security, scalability
- Error Handling → All error scenarios
- Data Validation → Input validation, data rules

**Subtask Templates**:
- Full-Stack Feature → Touches UI, API, DB
- API Development → Backend-focused
- Infrastructure/DevOps → Ops, deployment, infrastructure

**Test Case Templates**:
- Happy Path → Core functionality
- Edge Case → Boundary conditions
- Error Handling → Error scenarios
- Performance → Load/stress testing
- Security → Threat mitigation

### Customization Tips

1. **Extract from context**: Always prefer patterns from similar tasks over generic templates
2. **Combine templates**: Mix elements from multiple templates as needed
3. **Scale to complexity**: Simple tasks need shorter templates
4. **Maintain consistency**: Use same format as existing project tasks
5. **Be specific**: Replace all `{placeholders}` with actual values

---

**Last updated:** 2026-02-06
