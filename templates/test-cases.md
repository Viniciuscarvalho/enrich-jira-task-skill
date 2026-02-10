# Test Case Templates

Use these templates to generate comprehensive test cases from acceptance criteria.

---

## Standard Test Case Template

```markdown
### TC-{N}: {Test scenario name}

**Objective**: {What aspect is being tested}

**Preconditions**:
- {Setup requirement 1}
- {Setup requirement 2}
- {Test data needed}

**Test Steps**:
1. {Action step 1}
2. {Action step 2}
3. {Verification step}

**Expected Result**:
{Specific expected outcome with concrete values}

**Test Data**:
- Input: {Sample input}
- Expected output: {Sample output}

**Priority**: {High/Medium/Low}
**Type**: {Manual/Automated}
**Category**: {Functional/Integration/E2E/Performance/Security}
```

---

## Test Case by Category

### 1. Happy Path Test

**Purpose**: Verify primary functionality works under normal conditions

```markdown
### TC-{N}: {Feature} - happy path

**Objective**: Verify {main functionality} works correctly with valid inputs

**Preconditions**:
- {Valid test environment}
- {Valid test data available}
- {System in expected state}

**Test Steps**:
1. Navigate to {page/endpoint}
2. Provide valid input: {specific values}
3. Submit/execute action
4. Verify response/result

**Expected Result**:
- Action completes successfully
- {Specific outcome 1}
- {Specific outcome 2}
- No errors in logs

**Test Data**:
- {Field 1}: {Valid value}
- {Field 2}: {Valid value}

**Priority**: High (core functionality)
**Type**: Automated (E2E test)
**Category**: Functional
```

**Example:**
```markdown
### TC-1: User login - happy path

**Objective**: Verify user can log in with valid credentials

**Preconditions**:
- Test user account exists: testuser@example.com / SecurePass123!
- User account is active (not locked or suspended)
- Login service is available

**Test Steps**:
1. Navigate to /login page
2. Enter email: "testuser@example.com"
3. Enter password: "SecurePass123!"
4. Click "Login" button
5. Verify redirect to dashboard

**Expected Result**:
- Login completes within 2 seconds
- User redirected to /dashboard
- Welcome message displays: "Welcome back, Test User"
- JWT token stored in secure HTTP-only cookie
- Last login timestamp updated in database

**Test Data**:
- Email: testuser@example.com
- Password: SecurePass123!
- Expected user ID: 12345

**Priority**: High
**Type**: Automated (E2E with Cypress)
**Category**: Functional
```

---

### 2. Validation / Edge Case Test

**Purpose**: Verify system handles boundary conditions correctly

```markdown
### TC-{N}: {Feature} - {edge case scenario}

**Objective**: Verify system handles {boundary condition} correctly

**Preconditions**:
- {Setup for edge condition}

**Test Steps**:
1. {Create edge condition}
2. {Perform action}
3. {Verify handling}

**Expected Result**:
- {Specific handling of edge case}
- {System remains stable}
- {Appropriate user feedback}

**Test Data**:
- {Boundary value input}

**Priority**: Medium
**Type**: Automated (Unit/Integration test)
**Category**: Functional
```

**Example:**
```markdown
### TC-2: Email validation - maximum length

**Objective**: Verify system accepts maximum valid email length (254 chars per RFC 5322)

**Preconditions**:
- Registration form accessible
- Email validation implemented

**Test Steps**:
1. Navigate to /register
2. Enter 254-character email: "very.long.email.address...@example.com"
3. Enter valid password
4. Submit form
5. Verify acceptance

**Expected Result**:
- Email field accepts all 254 characters (no truncation)
- Form validates successfully
- Account created with full email address
- Confirmation email sent to full address

**Test Data**:
- Email: {254-char valid email}
- Password: SecurePass123!

**Priority**: Medium
**Type**: Automated (Integration test)
**Category**: Functional
```

---

### 3. Error Handling Test

**Purpose**: Verify graceful error handling and user feedback

```markdown
### TC-{N}: {Feature} - {error scenario}

**Objective**: Verify system gracefully handles {error condition}

**Preconditions**:
- {Setup for error condition}

**Test Steps**:
1. {Create error condition}
2. {Perform action that triggers error}
3. {Verify error handling}

**Expected Result**:
- {Specific error response}
- {User-friendly error message}
- {System state remains stable}
- {Error logged appropriately}

**Test Data**:
- {Invalid input that triggers error}

**Priority**: High (if common error) / Medium (if rare)
**Type**: Automated (Integration test)
**Category**: Functional (Error handling)
```

**Example:**
```markdown
### TC-3: Login - incorrect password

**Objective**: Verify system handles authentication failure securely

**Preconditions**:
- Test user account exists: testuser@example.com
- Account not locked (< 5 failed attempts)

**Test Steps**:
1. Navigate to /login
2. Enter valid email: "testuser@example.com"
3. Enter incorrect password: "WrongPassword123"
4. Submit login form
5. Verify error handling

**Expected Result**:
- HTTP 401 Unauthorized response
- Error message: "Invalid email or password. Please try again."
  (Does NOT reveal which field is incorrect - security)
- Failed attempt counter incremented in database
- Error logged: "Failed login attempt for email hash: abc123..."
  (Email hashed in logs, not plain text)
- User remains on login page (no redirect)
- No timing attack vulnerability (constant response time)

**Test Data**:
- Email: testuser@example.com
- Password: WrongPassword123 (incorrect)

**Priority**: High
**Type**: Automated (Integration test)
**Category**: Functional + Security
```

---

### 4. Performance Test

**Purpose**: Verify system meets performance requirements under load

```markdown
### TC-{N}: {Feature} - performance under {load condition}

**Objective**: Verify {functionality} meets {metric} target under {load}

**Preconditions**:
- {Load environment setup}
- {Performance baseline established}

**Test Configuration**:
- **Load**: {Concurrent users / requests per second}
- **Duration**: {Test duration}
- **Tool**: {Load testing tool}
- **Environment**: {Staging/Performance test environment}

**Test Steps**:
1. Configure load test with {tool}
2. Execute load scenario for {duration}
3. Collect metrics: {metrics}
4. Verify against targets

**Expected Result**:
- **{Metric 1}**: {Target value}
- **{Metric 2}**: {Target value}
- **Error rate**: < {threshold}%
- **Resource usage**: {CPU/Memory targets}

**Acceptance Threshold**:
{Pass/fail criteria}

**Test Data**:
- {Load scenario description}

**Priority**: High (for critical path) / Medium (otherwise)
**Type**: Automated (Performance test suite)
**Category**: Performance
```

**Example:**
```markdown
### TC-4: Login endpoint - performance under normal load

**Objective**: Verify authentication endpoint meets P95 latency target

**Preconditions**:
- Staging environment with production-like config
- Database with 10,000 test user accounts
- Load balancer and caching configured

**Test Configuration**:
- **Load**: 100 requests/second
- **Duration**: 5 minutes (300 seconds)
- **Tool**: artillery.io
- **Environment**: Staging (staging.example.com)
- **Scenario**: 50% valid logins, 50% invalid (realistic mix)

**Test Steps**:
1. Configure artillery with 100 virtual users
2. Each user performs login attempts (valid and invalid)
3. Run for 5 minutes
4. Collect metrics: P50, P95, P99 latency, error rate, throughput

**Expected Result**:
- **P50 latency**: ≤ 100ms
- **P95 latency**: ≤ 200ms (requirement)
- **P99 latency**: ≤ 500ms
- **Error rate**: < 0.1% (excluding intentional auth failures)
- **Throughput**: Sustains 100 req/sec
- **CPU usage**: < 70% average
- **Memory usage**: Stable (no leaks)

**Acceptance Threshold**:
P95 latency < 200ms AND error rate < 0.1% = PASS

**Test Data**:
- Valid credentials: 50% of requests
- Invalid credentials: 50% of requests
- Randomized user IDs from test dataset

**Priority**: High
**Type**: Automated (CI pipeline performance test)
**Category**: Performance
```

---

### 5. Security Test

**Purpose**: Verify protection against security threats

```markdown
### TC-{N}: Protection against {security threat}

**Objective**: Verify system defends against {attack type}

**Threat Model**: {OWASP category or CVE reference}

**Preconditions**:
- {Security test environment}
- {Attack simulation tools}

**Attack Simulation**:
1. {Setup attack scenario}
2. {Execute attack}
3. {Observe defense mechanism}

**Expected Result**:
- {Attack blocked/mitigated}
- {Security event logged}
- {System remains stable}
- {No data leaked}

**Security Requirements Verified**:
- {Requirement 1}
- {Requirement 2}

**Priority**: High
**Type**: Automated (Security test suite) / Manual (Penetration test)
**Category**: Security
```

**Example:**
```markdown
### TC-5: Protection against brute force login attacks

**Objective**: Verify rate limiting prevents credential stuffing

**Threat Model**: OWASP A07:2021 – Identification and Authentication Failures

**Preconditions**:
- Test environment with rate limiting enabled
- Test user account: testuser@example.com
- Rate limit configuration: 5 attempts per 15 minutes per IP

**Attack Simulation**:
1. Attempt login with incorrect password
2. Repeat 5 times in quick succession (< 1 minute)
3. Attempt 6th login (should be rate limited)
4. Wait 15 minutes
5. Attempt login again (should be allowed)

**Expected Result**:
- **After 5 failed attempts**: HTTP 429 Too Many Requests
- **Error message**: "Too many login attempts. Try again in 15 minutes."
- **Account NOT locked**: Rate limit is per IP, not per account (prevents account enumeration)
- **Security event logged**: All failed attempts logged with IP address
- **After 15 minutes**: Rate limit resets, login allowed
- **Legitimate users unaffected**: Different IP can still login

**Security Requirements Verified**:
- Rate limiting prevents brute force
- No account enumeration (same response for valid/invalid emails)
- Security events logged for monitoring
- Distributed attack detection (multiple IPs monitored separately)

**Test Data**:
- Target email: testuser@example.com
- Invalid password: WrongPass1, WrongPass2, ... (different each attempt)
- Source IP: 203.0.113.100 (test IP)

**Priority**: High
**Type**: Automated (Security test suite)
**Category**: Security
```

---

### 6. Integration Test

**Purpose**: Verify multiple components work together correctly

```markdown
### TC-{N}: {Component A} and {Component B} integration

**Objective**: Verify {Component A} correctly integrates with {Component B}

**Preconditions**:
- {Component A} deployed and accessible
- {Component B} deployed and accessible
- {Integration configured}

**Test Steps**:
1. {Action in Component A}
2. {Verify communication to Component B}
3. {Verify expected behavior in Component B}
4. {Verify response back to Component A}

**Expected Result**:
- {Data flows correctly between components}
- {State synchronized}
- {No integration errors}

**Test Data**:
- {Data used for integration test}

**Priority**: High
**Type**: Automated (Integration test)
**Category**: Integration
```

**Example:**
```markdown
### TC-6: Login service and session management integration

**Objective**: Verify login service correctly creates sessions in session store

**Preconditions**:
- Authentication API running
- Redis session store running and accessible
- Session configuration: 24-hour expiry

**Test Steps**:
1. POST /api/auth/login with valid credentials
2. Verify JWT token returned in response
3. Query Redis for session data using user ID
4. Verify session contains expected data
5. Decode JWT token and verify payload

**Expected Result**:
- **Login API**: Returns 200 OK with JWT token
- **Redis**: Contains session entry with key "session:{user_id}"
- **Session data**: Contains user_id, email, created_at, expires_at
- **Session expiry**: Set to 24 hours from creation
- **JWT token**: Contains user_id, exp (24h from now), iat (issued at)
- **JWT signature**: Valid (verifies with secret key)

**Test Data**:
- Email: testuser@example.com
- Password: SecurePass123!
- Expected user ID: 12345
- Redis key: session:12345

**Priority**: High
**Type**: Automated (Integration test)
**Category**: Integration
```

---

### 7. Accessibility Test

**Purpose**: Verify WCAG compliance and assistive technology support

```markdown
### TC-{N}: {Feature} - accessibility compliance

**Objective**: Verify {feature} meets WCAG {level} standards

**Preconditions**:
- {Feature} deployed and accessible
- {Accessibility testing tools} available

**Test Steps**:
1. {Navigate to feature}
2. {Run automated accessibility scanner}
3. {Perform manual keyboard navigation test}
4. {Test with screen reader}
5. {Verify ARIA labels}

**Expected Result**:
- **Automated scan**: 0 critical issues
- **Keyboard navigation**: All interactive elements accessible via Tab/Enter/Esc
- **Screen reader**: All content readable and properly announced
- **ARIA labels**: All form fields, buttons, links have descriptive labels
- **Color contrast**: Meets WCAG AA (4.5:1 for text)

**WCAG Criteria Verified**:
- {Criterion 1}: {e.g., 1.3.1 Info and Relationships}
- {Criterion 2}: {e.g., 2.1.1 Keyboard}
- {Criterion 3}: {e.g., 4.1.2 Name, Role, Value}

**Priority**: High
**Type**: Manual (with automated assist)
**Category**: Accessibility
```

---

## Deriving Test Cases from Acceptance Criteria

**Rule**: Each acceptance criterion should map to at least one test case.

### Example Mapping:

**AC-1: User registration**
```markdown
Given user provides valid email and password
When registration form is submitted
Then account is created and confirmation email sent
```

**Maps to test cases:**
- TC-1: Successful registration (happy path)
- TC-2: Registration with existing email (error handling)
- TC-3: Registration with weak password (validation)
- TC-4: Email delivery failure handling (error handling)

---

## Test Case Prioritization

### Priority Levels:

**High Priority**:
- Happy path (core functionality)
- Security tests (authentication, authorization, input validation)
- Data integrity tests (prevent data corruption)
- Critical error handling (payment failures, etc.)

**Medium Priority**:
- Edge cases (boundary conditions)
- Performance tests (non-critical paths)
- Integration tests (non-core integrations)
- Accessibility tests

**Low Priority**:
- Rare edge cases
- Nice-to-have features
- Cosmetic issues

---

## Test Data Management

### Test Data Best Practices:

1. **Use realistic data**: Real-world examples, not "test123"
2. **Include edge cases**: Empty strings, null, very long strings, special characters
3. **Separate test environments**: Never test against production data
4. **Clean up after tests**: Delete test data to avoid pollution
5. **Use fixtures**: Predefined test data sets for consistency

### Example Test Data Sets:

**Valid user data:**
```json
{
  "email": "alice.smith@example.com",
  "password": "SecurePass123!",
  "name": "Alice Smith"
}
```

**Edge case - maximum length email:**
```json
{
  "email": "very.long.email.address.that.is.exactly.254.characters.long@subdomain.example-domain.com...",
  "password": "SecurePass123!"
}
```

**Invalid data - SQL injection attempt:**
```json
{
  "email": "test@example.com'; DROP TABLE users; --",
  "password": "SecurePass123!"
}
```

---

## Test Automation Guidelines

### When to Automate:

- ✅ Repetitive tests (regression testing)
- ✅ Happy path tests (run frequently)
- ✅ API tests (easy to automate)
- ✅ Performance tests (requires automation)

### When to Keep Manual:

- ❌ Exploratory testing (finding unknown issues)
- ❌ UX/visual testing (subjective evaluation)
- ❌ One-time tests (not worth automation effort)
- ❌ Complex setup (automation too brittle)

---

## Test Case Organization

### By Test Level:

**Unit Tests**: Test individual functions/methods
- Fast, isolated, many of them
- Example: Password validation function

**Integration Tests**: Test component interactions
- Moderate speed, test APIs, database interactions
- Example: Login API endpoint test

**E2E Tests**: Test complete user workflows
- Slow, test through UI, entire stack
- Example: Full registration and login flow

### Test Pyramid:

```
       /\
      /  \  E2E (few)
     /----\
    /      \  Integration (moderate)
   /--------\
  /          \  Unit (many)
 /------------\
```

Aim for: Many unit tests, moderate integration tests, few E2E tests.

---

## Coverage Goals

### Target Coverage:

- **Happy path**: 100% (all core functionality)
- **Error handling**: 90% (common errors)
- **Edge cases**: 70% (important boundaries)
- **Security**: 100% (all threat vectors)
- **Performance**: Key endpoints only

### Don't Aim for 100% Total Coverage:

- Diminishing returns after ~80-85%
- Some code not testable (UI animations, etc.)
- Focus on high-risk, high-value areas

---

**Last updated:** 2026-02-06
