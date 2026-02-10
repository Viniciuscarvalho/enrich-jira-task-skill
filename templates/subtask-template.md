# Subtask Template

Use these templates when breaking down complex tasks into manageable subtasks.

---

## Generic Subtask Template

```markdown
**Summary**: [Action verb] [specific component/feature]

**Description**:

## Goal
{What this subtask accomplishes within the parent task}

## Scope
{Specific boundaries of this subtask}

## Deliverables
- {Deliverable 1: concrete output}
- {Deliverable 2: concrete output}
- {Deliverable 3: concrete output}

## Acceptance Criteria

### AC-1: {Core requirement}
**Given** {precondition}
**When** {action}
**Then** {expected outcome}

### AC-2: {Quality requirement}
**Given** {precondition}
**When** {verification performed}
**Then** {quality threshold met}

## Dependencies

### Blocked By
- {TASK-ID}: {Why this must complete first}

### Blocks
- {TASK-ID}: {Why this blocks that task}

## Technical Notes
- {Note 1: implementation guidance}
- {Note 2: constraints or considerations}

## Resources
- [Link to documentation]({URL})
- [Link to related code]({URL})
- [Link to design]({URL})

**Estimated Effort**: {Small (1-2d) / Medium (3-5d) / Large (5-8d)}
```

---

## Full-Stack Breakdown Templates

### Subtask 1: Database Schema / Data Model

```markdown
**Summary**: Create database schema for {feature}

**Description**:

## Goal
Design and implement database tables, indexes, and constraints to support {feature}.

## Scope
Database layer only - no business logic or API endpoints.

## Deliverables
- Migration scripts (up and down)
- Database schema documentation
- ER diagram (if complex)
- Seed data for testing

## Database Objects
- **Tables**: {table1}, {table2}
- **Indexes**: {index1}, {index2}
- **Constraints**: {constraint1}, {constraint2}
- **Foreign keys**: {fk1}, {fk2}

## Schema Design

### Table: {table_name}
```sql
CREATE TABLE {table_name} (
  id BIGSERIAL PRIMARY KEY,
  {column1} VARCHAR(255) NOT NULL,
  {column2} INTEGER,
  created_at TIMESTAMP DEFAULT NOW(),
  updated_at TIMESTAMP DEFAULT NOW()
);
```

## Acceptance Criteria

### AC-1: Migration succeeds
**Given** clean database
**When** migration script runs
**Then** all tables created with correct schema
And indexes created for expected query patterns

### AC-2: Rollback works
**Given** migration has been applied
**When** rollback script runs
**Then** database returns to previous state
And no data loss occurs

### AC-3: Constraints enforced
**Given** table with constraints
**When** invalid data inserted
**Then** database rejects with appropriate error

## Dependencies
- Blocked by: None (first subtask)
- Blocks: {TASK-ID} (API implementation needs schema)

## Technical Notes
- Use PostgreSQL 14+ features
- Follow project naming conventions: snake_case
- Add indexes for foreign keys
- Include audit columns (created_at, updated_at)

## Resources
- [Database conventions]({Confluence URL})
- [Similar schema: {table_name}]({GitHub URL})

**Estimated Effort**: Small (1-2 days)
```

---

### Subtask 2: Backend API Implementation

```markdown
**Summary**: Implement {feature} API endpoints

**Description**:

## Goal
Build REST API endpoints for {feature} with full CRUD operations and validation.

## Scope
API layer only - assumes database schema exists.

## Deliverables
- API endpoints: POST, GET, PUT, DELETE /api/{resource}
- Request/response validation
- Error handling
- API documentation (OpenAPI/Swagger spec)
- Unit tests for business logic

## API Specification

### POST /api/{resource}
**Purpose**: Create new {resource}

**Request**:
```json
{
  "{field1}": "value",
  "{field2}": "value"
}
```

**Response** (201 Created):
```json
{
  "id": 12345,
  "{field1}": "value",
  "created_at": "2026-02-06T10:00:00Z"
}
```

**Errors**:
- 400: Invalid request (validation error)
- 401: Unauthorized
- 409: Conflict (duplicate resource)

### GET /api/{resource}/:id
**Purpose**: Retrieve {resource} by ID

**Response** (200 OK):
```json
{
  "id": 12345,
  "{field1}": "value",
  "{field2}": "value"
}
```

**Errors**:
- 404: Not found

## Acceptance Criteria

### AC-1: Endpoints return correct status codes
**Given** API is authenticated
**When** requests made to endpoints
**Then** status codes match API spec (201, 200, 400, 404, etc.)

### AC-2: Request validation
**Given** invalid request payload
**When** POST /api/{resource} with missing required field
**Then** 400 Bad Request with validation error details

### AC-3: OpenAPI spec generated
**Given** API implementation complete
**When** OpenAPI spec generated
**Then** spec matches actual endpoint behavior
And all request/response schemas documented

## Dependencies
- Blocked by: {TASK-ID} (database schema must exist)
- Blocks: {TASK-ID} (frontend needs API)

## Technical Notes
- Use {framework} (Express, FastAPI, etc.)
- Follow REST conventions
- Implement pagination for list endpoints (limit/offset)
- Add rate limiting (100 req/min per user)
- Log all requests (structured logging)

## Resources
- [API conventions]({Confluence URL})
- [Similar endpoint: {example}]({GitHub URL})
- [Authentication middleware]({GitHub URL})

**Estimated Effort**: Medium (3-5 days)
```

---

### Subtask 3: Frontend UI Components

```markdown
**Summary**: Build {feature} UI components

**Description**:

## Goal
Implement user interface for {feature} using {React/Vue/Angular}.

## Scope
Frontend components only - assumes API endpoints exist.

## Deliverables
- UI components: {ComponentA}, {ComponentB}, {ComponentC}
- Form validation (client-side)
- Error handling and user feedback
- Responsive design (mobile + desktop)
- Component tests (Jest/React Testing Library)

## Components

### Component: {ComponentName}
**Purpose**: {What it does}

**Props**:
- `{prop1}`: {type} - {description}
- `{prop2}`: {type} - {description}

**State**:
- `{state1}`: {type} - {description}

**API calls**:
- Fetch: GET /api/{resource}
- Submit: POST /api/{resource}

## Acceptance Criteria

### AC-1: Responsive design
**Given** user accesses on different devices
**When** component renders
**Then** layout adapts correctly to mobile, tablet, desktop
And all features accessible on all screen sizes

### AC-2: Form validation
**Given** user fills out form
**When** invalid data entered
**Then** validation errors display in real-time
And form cannot be submitted until valid

### AC-3: Accessibility
**Given** user with screen reader
**When** navigating component
**Then** all interactive elements have ARIA labels
And keyboard navigation works (tab, enter, esc)
And meets WCAG 2.1 AA standards

## Dependencies
- Blocked by: {TASK-ID} (API endpoints must exist)
- Blocks: {TASK-ID} (integration testing needs UI)

## Technical Notes
- Use {design system} components where possible
- Match Figma designs: {link}
- Implement loading states (spinners)
- Implement error states (error messages)
- Add optimistic UI updates for better UX

## Resources
- [Design mockups]({Figma URL})
- [Component library]({Storybook URL})
- [Similar component: {example}]({GitHub URL})

**Estimated Effort**: Medium (3-4 days)
```

---

### Subtask 4: Integration & E2E Testing

```markdown
**Summary**: Write integration and E2E tests for {feature}

**Description**:

## Goal
Ensure {feature} works end-to-end with comprehensive test coverage.

## Scope
Integration tests (API layer) and E2E tests (full user workflows).

## Deliverables
- Integration tests for API endpoints
- E2E tests for user workflows (Cypress/Playwright)
- Test data fixtures
- Test documentation
- CI pipeline integration

## Test Coverage

### Integration Tests (API)
- POST /api/{resource} - success (201)
- POST /api/{resource} - validation error (400)
- GET /api/{resource}/:id - success (200)
- GET /api/{resource}/:id - not found (404)
- PUT /api/{resource}/:id - success (200)
- DELETE /api/{resource}/:id - success (204)

### E2E Tests (User Workflows)
- **Happy path**: User creates {resource} successfully
- **Validation**: User sees error for invalid input
- **Update flow**: User edits existing {resource}
- **Delete flow**: User deletes {resource} with confirmation

## Acceptance Criteria

### AC-1: Integration test coverage
**Given** API implementation complete
**When** integration tests run
**Then** all endpoints covered with happy path and error cases
And test coverage > 80% for business logic

### AC-2: E2E test coverage
**Given** UI and API complete
**When** E2E tests run
**Then** all user workflows covered
And tests run successfully in CI pipeline

### AC-3: Test data management
**Given** tests need data
**When** tests run
**Then** test fixtures provide consistent data
And tests clean up after themselves (no side effects)

## Dependencies
- Blocked by: {TASK-ID} (UI components must exist)
- Blocks: {TASK-ID} (deployment needs passing tests)

## Technical Notes
- Use {testing framework} (Jest, Pytest, etc.)
- Run in isolated test environment (test DB)
- Mock external services (payment gateway, email)
- Parallel test execution for speed
- Generate coverage reports

## Resources
- [Testing guidelines]({Confluence URL})
- [Test utilities]({GitHub URL})
- [CI/CD pipeline]({CI URL})

**Estimated Effort**: Small (2-3 days)
```

---

### Subtask 5: Documentation & Deployment

```markdown
**Summary**: Document and deploy {feature} to staging

**Description**:

## Goal
Finalize documentation and deploy {feature} to staging environment for validation.

## Scope
Documentation, deployment, and monitoring setup.

## Deliverables
- User documentation (help center)
- Developer documentation (README updates)
- Deployment runbook
- Monitoring and alerting setup
- Feature flag configuration

## Documentation

### User Documentation
- How to use {feature}
- Step-by-step guide with screenshots
- FAQ section
- Known limitations

### Developer Documentation
- API documentation (from OpenAPI spec)
- Database schema documentation
- Architecture diagram
- Configuration guide

## Deployment Checklist
- [ ] Feature flag created: `{feature_name}_enabled`
- [ ] Environment variables set
- [ ] Database migrations run in staging
- [ ] Smoke tests pass
- [ ] Monitoring dashboard created
- [ ] Alerts configured (error rate, latency)

## Acceptance Criteria

### AC-1: Feature flag configured
**Given** {feature} deployed
**When** feature flag toggled
**Then** feature enables/disables correctly
And no errors in logs

### AC-2: Monitoring active
**Given** {feature} running in staging
**When** monitoring dashboard accessed
**Then** key metrics visible: request count, error rate, latency
And alerts fire when thresholds exceeded

### AC-3: Documentation published
**Given** {feature} ready for users
**When** user accesses help documentation
**Then** guide is clear, accurate, and has screenshots
And developer docs match actual implementation

## Dependencies
- Blocked by: {TASK-ID} (testing must pass)
- Blocks: None (final subtask)

## Technical Notes
- Deploy to staging first (not production)
- Use gradual rollout (1% → 10% → 50% → 100%)
- Set up log aggregation (CloudWatch, Datadog)
- Create alerts: error rate > 1%, P95 latency > 500ms
- Document rollback procedure

## Resources
- [Deployment guide]({Confluence URL})
- [Monitoring setup]({Grafana URL})
- [Documentation template]({Confluence URL})

**Estimated Effort**: Small (1-2 days)
```

---

## API-Only Breakdown Template

For API-focused tasks:

```markdown
Subtask 1: API design and OpenAPI specification
Subtask 2: Data models and repository/DAO layer
Subtask 3: Business logic and service layer
Subtask 4: API controllers and routing
Subtask 5: API tests and documentation
```

---

## Infrastructure/DevOps Breakdown Template

For infrastructure tasks:

```markdown
Subtask 1: Infrastructure as Code (Terraform/CloudFormation)
Subtask 2: CI/CD pipeline configuration
Subtask 3: Monitoring and alerting setup
Subtask 4: Security and access control
Subtask 5: Documentation and runbooks
```

---

## When to Create Subtasks

### Create subtasks when:
- ✅ Parent task description > 200 words after enrichment
- ✅ Multiple technical domains (frontend + backend + DB)
- ✅ Distinct implementation phases
- ✅ Different skill sets required
- ✅ Natural handoff points exist
- ✅ Parent task estimate > 5 days

### Don't create subtasks when:
- ❌ Simple task (< 2 days total)
- ❌ Tightly coupled work (can't parallelize)
- ❌ Already well-defined with clear ACs
- ❌ Subtasks would be trivial (< 1 day each)

---

## Subtask Naming Conventions

**Good subtask names** (action verb + specific component):
- ✅ "Create database schema for user authentication"
- ✅ "Implement JWT token generation API"
- ✅ "Build login form UI component"
- ✅ "Write E2E tests for login flow"

**Bad subtask names**:
- ❌ "Database work" (too vague)
- ❌ "Backend" (not specific)
- ❌ "Complete authentication" (too large, should be parent)

---

## Subtask Sizing Guidelines

| Size | Effort | When to Use |
|------|--------|-------------|
| Small | 1-2 days | Setup, configuration, simple components |
| Medium | 3-5 days | API implementation, UI components, testing |
| Large | 5-8 days | Complex integrations, major refactoring |

**If a subtask is > 8 days**: It's too large, break it down further.

---

## Extracting Subtask Patterns

When enriching tasks, look for subtask patterns in:

1. **Similar completed tasks**: Check if they had subtasks, copy the structure
2. **Epic context**: See how other tasks in the epic were broken down
3. **Project patterns**: Some projects consistently use same breakdown (setup → implement → test → docs)

---

**Last updated:** 2026-02-06
