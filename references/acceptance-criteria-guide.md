# Acceptance Criteria Writing Guide

This guide helps you write effective, testable acceptance criteria that lead to successful implementations. Based on product management best practices and adapted from the `/product-manager` skill patterns.

## Table of Contents
1. [What are Acceptance Criteria?](#what-are-acceptance-criteria)
2. [Why They Matter](#why-they-matter)
3. [The Given-When-Then Format](#the-given-when-then-format)
4. [INVEST Principles](#invest-principles)
5. [Common Patterns](#common-patterns)
6. [Anti-Patterns to Avoid](#anti-patterns-to-avoid)
7. [Examples](#examples)

---

## What are Acceptance Criteria?

**Acceptance Criteria (ACs)** are the conditions that a software product must satisfy to be accepted by a user, customer, or other stakeholder.

**Purpose:**
- Define "done" for a task
- Provide testable specifications
- Align team on requirements
- Enable estimation and planning

**Key characteristics:**
- Written BEFORE implementation
- Testable (pass/fail, no ambiguity)
- Independent from implementation details
- Focused on behavior, not solution

---

## Why They Matter

### For Developers
- ✅ Clear requirements (what to build)
- ✅ Prevents scope creep (what NOT to build)
- ✅ Guides test writing (what to verify)
- ✅ Enables accurate estimation

### For QA/Testing
- ✅ Test case foundation
- ✅ Coverage guidance
- ✅ Pass/fail criteria

### For Product/Stakeholders
- ✅ Validates understanding
- ✅ Enables acceptance testing
- ✅ Aligns expectations

### For Future Maintenance
- ✅ Documents intent
- ✅ Helps understand "why"
- ✅ Guides refactoring safely

---

## The Given-When-Then Format

**Structure:**
```
Given [preconditions/context]
When [action/event]
Then [expected outcome]
```

**Why this format?**
- Forces specific thinking
- Natural language (readable by all)
- Maps directly to test cases
- Originally from BDD (Behavior-Driven Development)

### Components

#### Given (Preconditions)
Sets up the initial state or context.

**Good examples:**
- Given user is logged in
- Given shopping cart contains 3 items
- Given database has 10,000 products

**Bad examples:**
- Given the system (too vague)
- Given everything is ready (not specific)

#### When (Action/Event)
Describes the action taken or event that occurs.

**Good examples:**
- When user clicks "Submit" button
- When API receives POST request with valid payload
- When scheduled job runs at midnight

**Bad examples:**
- When the feature is used (too vague)
- When something happens (not specific)

#### Then (Expected Outcome)
States the expected result. Must be specific and verifiable.

**Good examples:**
- Then order confirmation email is sent within 30 seconds
- Then API returns 201 Created with order ID
- Then database contains new order record

**Bad examples:**
- Then it works (not specific)
- Then user is happy (not measurable)

---

## INVEST Principles

Good acceptance criteria follow INVEST:

### I - Independent
Each AC should be testable on its own.

**Good:**
```
AC-1: User can log in with email
AC-2: User can log in with phone number
```
(Can test separately)

**Bad:**
```
AC-1: User can log in
AC-2: Login works with email or phone
```
(AC-2 depends on AC-1 definition)

### N - Negotiable
ACs define "what," not "how." Implementation is flexible.

**Good:**
```
AC-1: Password reset email delivered within 5 minutes
```
(Doesn't specify email service or implementation)

**Bad:**
```
AC-1: Password reset email sent via SendGrid using template ID 12345
```
(Over-specifies implementation)

### V - Valuable
Each AC should provide clear value to users/business.

**Good:**
```
AC-1: User receives confirmation email immediately after order
```
(Clear user value: peace of mind)

**Bad:**
```
AC-1: System writes log entry when order is created
```
(Technical, not directly valuable to user)

### E - Estimable
Team can estimate effort from the AC.

**Good:**
```
AC-1: Search returns results within 500ms for queries < 100 characters
```
(Clear scope, can estimate)

**Bad:**
```
AC-1: Search is fast
```
(Can't estimate "fast")

### S - Small
Each AC should be focused and concise.

**Good:**
```
AC-1: User can filter products by price range
AC-2: User can filter products by category
AC-3: User can combine multiple filters
```
(Separated concerns)

**Bad:**
```
AC-1: User can filter products by price, category, brand, rating, and combine multiple filters with sorting options
```
(Too large, should be multiple ACs)

### T - Testable
Must be possible to verify pass/fail.

**Good:**
```
AC-1: Login form validates email format (RFC 5322 compliant)
```
(Testable with test cases)

**Bad:**
```
AC-1: Login form has good UX
```
(Subjective, not testable)

---

## Common Patterns

### Pattern 1: User Input Validation

```markdown
### AC-{N}: {Field name} validation

**Given** user is on {form name}
**When** user enters {invalid input} in {field}
**Then** {field} displays error: "{error message}"
And form cannot be submitted
```

**Example:**
```markdown
### AC-1: Email address validation

**Given** user is on registration form
**When** user enters "invalid-email" in email field
**Then** email field displays error: "Please enter a valid email address"
And "Register" button is disabled
```

---

### Pattern 2: API Response

```markdown
### AC-{N}: {Endpoint} {scenario}

**Given** {API preconditions}
**When** {HTTP method} request sent to {endpoint} with {payload}
**Then** response is {status code} with {response body}
```

**Example:**
```markdown
### AC-2: Create user endpoint success

**Given** API is authenticated with valid token
**When** POST request sent to /api/users with valid user data
```json
{
  "email": "user@example.com",
  "password": "SecurePass123!"
}
```
**Then** response is 201 Created with user object
```json
{
  "id": 12345,
  "email": "user@example.com",
  "created_at": "2026-02-06T10:00:00Z"
}
```
And confirmation email is sent to user@example.com
```

---

### Pattern 3: State Transitions

```markdown
### AC-{N}: {Entity} transitions from {state A} to {state B}

**Given** {entity} is in {state A}
**When** {action performed}
**Then** {entity} transitions to {state B}
And {side effects}
```

**Example:**
```markdown
### AC-3: Order transitions from pending to confirmed

**Given** order #12345 is in "pending" status
**When** payment is successfully processed
**Then** order transitions to "confirmed" status
And confirmation email is sent to customer
And inventory is decremented for all order items
```

---

### Pattern 4: Performance Requirements

```markdown
### AC-{N}: {Operation} performance

**Given** {load conditions}
**When** {operation performed}
**Then** {operation completes within {time}}
And {resource usage within limits}
```

**Example:**
```markdown
### AC-4: Search response time

**Given** product database with 100,000 items
**When** user submits search query
**Then** results display within 500ms (P95 latency)
And CPU usage remains below 70%
```

---

### Pattern 5: Security Requirements

```markdown
### AC-{N}: Protection against {threat}

**Given** {attack scenario}
**When** {malicious action attempted}
**Then** {system defends}
And {security event logged}
```

**Example:**
```markdown
### AC-5: Protection against SQL injection

**Given** malicious user attempts SQL injection in search field
**When** user enters `'; DROP TABLE users; --` in search
**Then** query is safely parameterized (no SQL execution)
And security event logged with attacker IP
And search returns no results (safe handling)
```

---

### Pattern 6: Error Handling

```markdown
### AC-{N}: {Error scenario}

**Given** {error condition}
**When** {action attempted}
**Then** {graceful error handling}
And {user-friendly error message}
And {error logged}
```

**Example:**
```markdown
### AC-6: Database connection failure handling

**Given** database is unreachable
**When** user attempts to load dashboard
**Then** user sees error page: "Service temporarily unavailable. Please try again."
And error logged with timestamp and stack trace
And retry mechanism attempts reconnection every 5 seconds
```

---

### Pattern 7: Data Integrity

```markdown
### AC-{N}: {Data integrity constraint}

**Given** {initial data state}
**When** {operation performed}
**Then** {data constraint maintained}
And {validation enforced}
```

**Example:**
```markdown
### AC-7: Email uniqueness constraint

**Given** user with email "user@example.com" already exists
**When** new registration attempted with same email
**Then** registration fails with 409 Conflict
And error message: "Email already registered"
And existing user data remains unchanged
```

---

## Anti-Patterns to Avoid

### ❌ Anti-Pattern 1: Too Vague

**Bad:**
```
AC-1: The feature should work well
```

**Why it's bad:** Not testable, not specific

**Good:**
```
AC-1: User can complete checkout within 3 clicks
```

---

### ❌ Anti-Pattern 2: Implementation Details

**Bad:**
```
AC-1: Use bcrypt with cost factor 12 to hash passwords
```

**Why it's bad:** Specifies "how," not "what"

**Good:**
```
AC-1: Passwords are securely hashed and cannot be retrieved in plain text
```

---

### ❌ Anti-Pattern 3: Not Testable

**Bad:**
```
AC-1: UI should look beautiful
```

**Why it's bad:** Subjective, can't verify

**Good:**
```
AC-1: UI passes WCAG 2.1 AA accessibility compliance
AC-2: Design matches approved Figma mockup (design review)
```

---

### ❌ Anti-Pattern 4: Too Many in One

**Bad:**
```
AC-1: User can register, login, reset password, update profile, and logout
```

**Why it's bad:** Too large, can't estimate or test atomically

**Good:**
```
AC-1: User can register with email and password
AC-2: User can login with registered credentials
AC-3: User can reset forgotten password via email
AC-4: User can update profile information
AC-5: User can logout and session is terminated
```

---

### ❌ Anti-Pattern 5: No Clear Pass/Fail

**Bad:**
```
AC-1: System is performant
```

**Why it's bad:** No clear threshold

**Good:**
```
AC-1: API response time P95 < 200ms under 100 req/sec load
```

---

### ❌ Anti-Pattern 6: Assumptions Not Stated

**Bad:**
```
When user clicks login
Then user is logged in
```

**Why it's bad:** Missing "Given" (what credentials? valid/invalid?)

**Good:**
```
Given user has valid credentials (email: test@example.com, password: Pass123!)
When user submits login form
Then user is authenticated and redirected to dashboard
```

---

## Examples by Domain

### E-Commerce

```markdown
### AC-1: Add product to cart
**Given** user is viewing product details for "Blue T-Shirt"
**When** user clicks "Add to Cart" button
**Then** cart icon displays badge with item count incremented by 1
And success message appears: "Blue T-Shirt added to cart"
And product remains in cart when user navigates away

### AC-2: Apply discount code
**Given** cart total is $100.00
**When** user enters valid discount code "SAVE20" (20% off)
**Then** cart total updates to $80.00
And discount line item shows: "-$20.00 (SAVE20)"
And discount persists through checkout

### AC-3: Out of stock handling
**Given** product inventory is 0
**When** user views product page
**Then** "Add to Cart" button is disabled
And message displays: "Out of Stock - Notify me when available"
And user can optionally enter email for restock notification
```

---

### Authentication

```markdown
### AC-1: Successful login
**Given** user account exists with email "user@example.com"
**When** user submits login form with correct password
**Then** user is authenticated and redirected to dashboard within 2 seconds
And JWT token with 24-hour expiry is issued
And last login timestamp is updated in database

### AC-2: Failed login - incorrect password
**Given** user account exists with email "user@example.com"
**When** user submits login form with incorrect password
**Then** error message displays: "Invalid email or password"
And user remains on login page
And failed attempt count increments (rate limiting)
And no information revealed about whether email exists

### AC-3: Account lockout after failed attempts
**Given** user has failed login 4 times in past 15 minutes
**When** user attempts 5th login with incorrect password
**Then** account is locked for 15 minutes
And error message: "Too many failed attempts. Try again in 15 minutes."
And account lockout event logged to security audit
```

---

### API Development

```markdown
### AC-1: GET endpoint success
**Given** database contains user with ID 12345
**When** GET /api/users/12345 with valid auth token
**Then** response is 200 OK with user JSON
And response time < 100ms
And response includes: id, email, created_at (no password)

### AC-2: GET endpoint - not found
**Given** database does not contain user with ID 99999
**When** GET /api/users/99999 with valid auth token
**Then** response is 404 Not Found
And error body: {"error": "User not found", "id": 99999}

### AC-3: POST endpoint validation
**Given** API expects user creation payload
**When** POST /api/users with invalid email format
```json
{
  "email": "invalid-email",
  "password": "Pass123!"
}
```
**Then** response is 400 Bad Request
And error body: {"error": "Invalid email format", "field": "email"}
And no database record created
```

---

## How Many ACs Per Task?

**General guidelines:**
- **Minimum**: 3 ACs (happy path, error case, edge case)
- **Typical**: 4-6 ACs for standard features
- **Maximum**: 7-8 ACs (beyond this, consider breaking into subtasks)

**If you need more than 8 ACs:**
- Task is too large → Break into subtasks
- Mixing concerns → Separate into multiple tasks
- Over-specifying → Focus on key acceptance points

---

## Coverage Checklist

Good acceptance criteria should cover:

- ✅ **Happy path** (primary use case works)
- ✅ **Error handling** (gracefully handles failures)
- ✅ **Edge cases** (boundary conditions)
- ✅ **Non-functional** (performance, security, accessibility)
- ✅ **Data validation** (input validation)
- ✅ **State management** (correct state transitions)

**Example task with full coverage:**

```markdown
Task: User registration

AC-1: Happy path ✓
**Given** user provides valid email and password
**When** registration form submitted
**Then** account created and confirmation email sent

AC-2: Error handling ✓
**Given** email service is down
**When** registration form submitted
**Then** account still created but user warned: "Confirmation email delayed"

AC-3: Edge case ✓
**Given** user attempts to register with 254-character email (RFC max)
**When** registration form submitted
**Then** email accepted and account created

AC-4: Non-functional (security) ✓
**Given** user provides password
**When** stored in database
**Then** password is hashed with bcrypt (not plain text)

AC-5: Data validation ✓
**Given** user provides weak password ("password123")
**When** registration form submitted
**Then** error: "Password must be at least 12 characters with mixed case and numbers"

AC-6: State management ✓
**Given** user successfully registers
**When** user logs in before confirming email
**Then** user can login but sees banner: "Please confirm your email"
```

---

## Tips for Writing Great ACs

### DO:
- ✅ Use consistent format (Given-When-Then)
- ✅ Be specific with numbers (not "fast," but "< 500ms")
- ✅ Include actual error messages (helps UX consistency)
- ✅ Write from user perspective (not system internals)
- ✅ Make them testable (QA should be able to verify)
- ✅ Extract patterns from similar completed tasks

### DON'T:
- ❌ Specify implementation ("use Redis cache")
- ❌ Use subjective terms ("good UX," "beautiful")
- ❌ Combine multiple concerns in one AC
- ❌ Assume knowledge ("as expected," "obviously")
- ❌ Write ACs after implementation (defeats the purpose)

---

## From ACs to Tests

Good ACs map directly to test cases:

**AC:**
```markdown
### AC-1: Email validation

**Given** user is on registration form
**When** user enters "invalid-email" in email field
**Then** error displays: "Please enter a valid email"
And form cannot be submitted
```

**Maps to test case:**
```javascript
test('email validation shows error for invalid format', () => {
  // Given: user on registration form
  render(<RegistrationForm />)

  // When: user enters invalid email
  const emailInput = screen.getByLabelText('Email')
  fireEvent.change(emailInput, { target: { value: 'invalid-email' } })
  fireEvent.blur(emailInput)

  // Then: error displays
  expect(screen.getByText('Please enter a valid email')).toBeInTheDocument()

  // And: form cannot be submitted
  const submitButton = screen.getByRole('button', { name: 'Register' })
  expect(submitButton).toBeDisabled()
})
```

**Notice:** The test follows the exact same structure as the AC!

---

## Extracting ACs from Context

When enriching tasks, extract AC patterns from:

### 1. Similar Completed Tasks
- Look for tasks with well-written ACs
- Adapt format and coverage
- Maintain consistency with project patterns

### 2. Documentation
- Technical specs → non-functional requirements
- API docs → endpoint behavior
- Security docs → security requirements

### 3. Epic Context
- Epic goals → business value ACs
- Related tasks → maintain consistency
- Epic acceptance criteria → derive task-level ACs

### 4. Past Bugs/Issues
- Bug reports → error handling ACs
- Reopened tasks → missed edge cases
- Failed tests → add coverage ACs

---

## Quality Checklist

Before finalizing ACs, verify:

- [ ] Each AC follows Given-When-Then format
- [ ] Each AC is testable (clear pass/fail)
- [ ] Each AC is independent (can test separately)
- [ ] Covers happy path, errors, edge cases
- [ ] Includes specific values (not vague terms)
- [ ] No implementation details ("what," not "how")
- [ ] Consistent with similar tasks in project
- [ ] Between 3-7 ACs total (not too few, not too many)
- [ ] Each AC provides clear value
- [ ] Team can estimate from ACs

---

**Last updated:** 2026-02-06
