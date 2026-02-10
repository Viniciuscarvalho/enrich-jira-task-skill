# Example 1: Minimal Task → Fully Enriched

This example demonstrates enriching a task with minimal description into a comprehensive, actionable work item.

---

## BEFORE Enrichment

### Task: AUTH-123

**Summary**: Add user authentication

**Type**: Story

**Description**:
```
(empty)
```

**Acceptance Criteria**:
```
(none)
```

**Subtasks**:
```
(none)
```

**Comments**:
```
(none)
```

**Identified Gaps**:
- ❌ No description at all
- ❌ No acceptance criteria
- ❌ No technical context
- ❌ Vague title (what kind of authentication?)

---

## AFTER Enrichment

### Task: AUTH-123

**Summary**: Add user authentication

**Type**: Story

**Description**:

```markdown
## Overview
Implement user authentication system to secure application access and enable personalized user experiences.

## Context
Based on authentication patterns used in AUTH-100 (OAuth integration) and security architecture documented in "Security Architecture v2.0" Confluence page.

This feature is part of the "User Management" epic (AUTH-50) and builds upon existing database schema from AUTH-110 (User Profile Management).

## Detailed Requirements

### Core Functionality
- Support email/password authentication
- Implement JWT token-based session management
- Add password reset functionality via email
- Support OAuth 2.0 providers (Google, GitHub)
- Implement secure password hashing (bcrypt)

### Non-Functional Requirements
- **Performance**: Login endpoint P95 latency < 200ms under 100 req/sec
- **Security**:
  - Passwords hashed with bcrypt cost factor 12
  - Rate limiting: 5 failed login attempts per 15 minutes per IP
  - HTTPS required (no HTTP fallback)
  - Protection against timing attacks
- **Scalability**: Support 10,000 concurrent authenticated sessions
- **Accessibility**: Login form WCAG 2.1 AA compliant

## Use Cases

### UC-1: Email/Password Registration
**Actor**: New User
**Goal**: Create account to access application
**Flow**:
1. User navigates to /register
2. User provides email and password (meeting strength requirements)
3. User submits registration form
4. System creates account and sends confirmation email
5. User clicks confirmation link
6. Account is activated

**Example**:
- Input: email "alice@example.com", password "SecurePass123!"
- Output: Account created, confirmation email sent to alice@example.com

### UC-2: Login with Email/Password
**Actor**: Registered User
**Goal**: Authenticate to access protected resources
**Flow**:
1. User navigates to /login
2. User enters email and password
3. User submits login form
4. System validates credentials
5. System issues JWT token (24-hour expiry)
6. User redirected to dashboard

**Example**:
- Input: email "alice@example.com", password "SecurePass123!"
- Output: JWT token, redirect to /dashboard

### UC-3: Password Reset
**Actor**: Registered User (forgot password)
**Goal**: Regain access to account
**Flow**:
1. User clicks "Forgot Password" on login page
2. User enters email address
3. System sends password reset email (if account exists)
4. User clicks reset link (valid for 1 hour)
5. User enters new password
6. Password is updated and user can login

**Example**:
- Input: email "alice@example.com"
- Output: Password reset email sent with time-limited link

### UC-4: OAuth Login (Google)
**Actor**: User (prefers OAuth)
**Goal**: Authenticate using existing Google account
**Flow**:
1. User clicks "Login with Google" button
2. User redirected to Google OAuth consent screen
3. User approves access
4. System receives OAuth token from Google
5. System creates/updates user account with Google ID
6. JWT token issued and user redirected to dashboard

## Edge Cases to Consider

### Edge Case 1: Email Already Registered
- **Scenario**: User attempts to register with email already in system
- **Handling**: Display error "Email already registered" without revealing account existence to unauthenticated users (security)

### Edge Case 2: Password Reset for Non-Existent Email
- **Scenario**: User requests password reset for email not in system
- **Handling**: Display same success message as valid email (prevent email enumeration), but don't send email

### Edge Case 3: Expired Password Reset Link
- **Scenario**: User clicks reset link after 1-hour expiry
- **Handling**: Display "Link expired. Please request a new password reset."

### Edge Case 4: Session Expiry While Active
- **Scenario**: User's JWT token expires while using the app
- **Handling**: Detect expired token on next API call, redirect to login with message "Session expired. Please log in again."

### Edge Case 5: Maximum Email Length (254 chars)
- **Scenario**: User registers with RFC 5322 maximum email length
- **Handling**: Accept full 254 characters, store without truncation

## Examples

### Example 1: Successful Registration
**Input**:
```json
POST /api/auth/register
{
  "email": "alice@example.com",
  "password": "SecurePass123!",
  "name": "Alice Smith"
}
```

**Expected Response** (201 Created):
```json
{
  "id": 12345,
  "email": "alice@example.com",
  "name": "Alice Smith",
  "created_at": "2026-02-06T10:00:00Z",
  "email_confirmed": false
}
```

### Example 2: Login Success
**Input**:
```json
POST /api/auth/login
{
  "email": "alice@example.com",
  "password": "SecurePass123!"
}
```

**Expected Response** (200 OK):
```json
{
  "token": "eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9...",
  "expires_at": "2026-02-07T10:00:00Z",
  "user": {
    "id": 12345,
    "email": "alice@example.com",
    "name": "Alice Smith"
  }
}
```

### Example 3: Login Failure (Invalid Password)
**Input**:
```json
POST /api/auth/login
{
  "email": "alice@example.com",
  "password": "WrongPassword"
}
```

**Expected Response** (401 Unauthorized):
```json
{
  "error": "Invalid email or password. Please try again."
}
```

## Out of Scope
To prevent scope creep, the following are explicitly NOT included in this task:
- Two-factor authentication (2FA) - Track separately in AUTH-150
- Social login providers beyond Google/GitHub (Facebook, Twitter) - Future enhancement
- Remember me / persistent sessions - Track in AUTH-151
- Account deletion / deactivation - Part of User Management epic
- Email change workflow - Future enhancement
- Password strength meter UI - Nice to have, not required

## References
- [Security Architecture v2.0](https://confluence.example.com/auth-v2): System security standards
- [AUTH-100: OAuth Integration](https://jira.example.com/AUTH-100): Reference OAuth implementation
- [AUTH-110: User Profile Management](https://jira.example.com/AUTH-110): Database schema for users table
- [OWASP Authentication Cheat Sheet](https://cheatsheetseries.owasp.org/cheatsheets/Authentication_Cheat_Sheet.html): Security best practices
```

**Acceptance Criteria**:

```markdown
## Acceptance Criteria

### AC-1: User Registration with Email/Password
**Given** user provides valid email "alice@example.com" and password "SecurePass123!"
**When** registration form is submitted
**Then** account is created in database
And confirmation email is sent to alice@example.com within 30 seconds
And password is hashed with bcrypt (cost factor 12)

**Validation**: Create test account and verify in database (password_hash field contains bcrypt hash)

---

### AC-2: Secure Password Storage
**Given** user password is provided during registration
**When** password is stored in database
**Then** password is hashed with bcrypt (cost factor 12)
And plain text password is never stored
And password hash cannot be reversed to plain text

**Validation**: Database audit - no plain text passwords in users table

---

### AC-3: JWT Token Generation
**Given** user successfully authenticates with valid credentials
**When** login completes
**Then** valid JWT token with 24-hour expiry is returned
And token contains user_id, email, exp (expiration), iat (issued at)
And token is signed with secure secret key

**Validation**: Decode JWT token and verify payload structure

---

### AC-4: Password Strength Validation
**Given** user attempts to register with weak password
**When** password "password123" is submitted
**Then** registration fails with 400 Bad Request
And error message: "Password must be at least 12 characters with uppercase, lowercase, number, and special character"
And no account is created

**Validation**: Test with weak passwords: "password", "12345678", "qwerty"

---

### AC-5: Rate Limiting for Failed Login Attempts
**Given** user has failed login 5 times in past 15 minutes from IP 203.0.113.100
**When** user attempts 6th login
**Then** request is rejected with 429 Too Many Requests
And error message: "Too many login attempts. Try again in 15 minutes."
And failed attempt logged to security audit system

**Validation**: Automated test - attempt 6 logins in quick succession

---

### AC-6: OAuth Login (Google)
**Given** user clicks "Login with Google" button
**When** user completes Google OAuth flow
**Then** user account is created/updated with Google ID
And JWT token is issued
And user is redirected to /dashboard

**Validation**: Manual test with Google OAuth sandbox account

---

### AC-7: Password Reset Email Delivery
**Given** user requests password reset for registered email
**When** reset request is submitted
**Then** password reset email is sent within 30 seconds
And email contains reset link valid for 1 hour
And link format: https://app.example.com/reset-password?token={secure_token}

**Validation**: Trigger password reset and check email inbox
```

**Subtasks Created**:

```markdown
## Subtasks

### AUTH-124: Create database schema for authentication
**Type**: Sub-task
**Description**: Design and implement database tables for user authentication (users, password_resets, oauth_providers)

**Deliverables**:
- Migration scripts for users, password_resets, oauth_providers tables
- Database indexes for email lookups, token validation
- Rollback scripts

**Acceptance Criteria**:
- AC-1: Migration runs successfully on clean database
- AC-2: Rollback restores previous state
- AC-3: Indexes created for users(email), password_resets(token)

**Estimated Effort**: Small (1-2 days)

---

### AUTH-125: Implement JWT token generation/validation
**Type**: Sub-task
**Description**: Build JWT service for token creation, signing, and validation

**Deliverables**:
- JWT service module (generate, verify, refresh)
- Unit tests for token operations
- Configuration for secret key rotation

**Acceptance Criteria**:
- AC-1: Tokens generated with 24-hour expiry
- AC-2: Tokens signed with HS256 algorithm
- AC-3: Expired tokens rejected on validation

**Dependencies**:
- Blocked by: AUTH-124 (needs users table)

**Estimated Effort**: Small (2 days)

---

### AUTH-126: Build password reset flow
**Type**: Sub-task
**Description**: Implement password reset request and password update endpoints

**Deliverables**:
- POST /api/auth/forgot-password endpoint
- POST /api/auth/reset-password endpoint
- Password reset email template
- Token expiry handling (1 hour)

**Acceptance Criteria**:
- AC-1: Reset email sent within 30 seconds
- AC-2: Token expires after 1 hour
- AC-3: Same response for valid/invalid emails (security)

**Dependencies**:
- Blocked by: AUTH-124 (needs password_resets table)

**Estimated Effort**: Medium (3 days)

---

### AUTH-127: Add OAuth provider integration (Google, GitHub)
**Type**: Sub-task
**Description**: Integrate OAuth 2.0 authentication for Google and GitHub providers

**Deliverables**:
- OAuth callback handlers for Google, GitHub
- User account creation/linking with OAuth ID
- OAuth configuration (client ID, secret)

**Acceptance Criteria**:
- AC-1: Google OAuth flow completes successfully
- AC-2: GitHub OAuth flow completes successfully
- AC-3: Existing user accounts linked with OAuth ID

**Dependencies**:
- Blocked by: AUTH-124 (needs oauth_providers table)
- Blocked by: AUTH-125 (needs JWT generation)

**Estimated Effort**: Medium (4 days)

---

### AUTH-128: Write authentication tests
**Type**: Sub-task
**Description**: Comprehensive test coverage for authentication flows

**Deliverables**:
- Unit tests for password hashing, JWT operations
- Integration tests for API endpoints
- E2E tests for registration and login flows
- Security tests (rate limiting, SQL injection)

**Acceptance Criteria**:
- AC-1: Test coverage > 85% for authentication code
- AC-2: All happy path and error scenarios covered
- AC-3: Security tests verify rate limiting, injection protection

**Dependencies**:
- Blocked by: AUTH-126, AUTH-127 (needs all features complete)

**Estimated Effort**: Medium (3 days)
```

**Technical Context Comment Added**:

```markdown
## 🔒 Technical Context

### Dependencies
**Blocked By**:
- **AUTH-110**: User Profile Management
  - Reason: Needs users table schema
  - Status: Done ✓
  - Impact: Database schema is available

**Blocks**:
- **AUTH-150**: Two-Factor Authentication
  - Reason: 2FA requires authentication system

- **DASH-50**: User Dashboard
  - Reason: Dashboard requires authenticated users

**Related**:
- **AUTH-100**: OAuth Integration (reference implementation)
- **EMAIL-30**: Email Service Integration (for confirmation emails)

---

### Risks & Considerations

#### Risk-1: Password Reset Flow Vulnerable to Timing Attacks
**Likelihood**: Medium
**Impact**: High (security vulnerability)
**Description**: Timing differences in email validation could reveal which emails are registered in the system

**Mitigation**:
- Use constant-time string comparison for email checks
- Add artificial delay (200ms) to all password reset responses
- Return same response for valid/invalid emails
- Rate limit password reset requests (3 per hour per IP)

**Source**: Similar issue identified in AUTH-85 (resolved)

**Contingency**:
If exploited, immediately deploy mitigation and rotate affected user credentials

---

#### Risk-2: JWT Secret Key Rotation Not Yet Implemented
**Likelihood**: Low (manual process)
**Impact**: High (if secret compromised, all sessions invalid)
**Description**: No automated secret rotation mechanism exists

**Mitigation**:
- Track as separate task: AUTH-160 (JWT Secret Rotation)
- Document manual rotation procedure in runbook
- Set calendar reminder for quarterly rotation
- Store secret in secure vault (not environment variable)

**Source**: Security review recommendation

**Contingency**:
Manual emergency rotation procedure documented in security runbook

---

#### Risk-3: OAuth Provider Downtime Affecting User Login
**Likelihood**: Low (Google/GitHub SLA 99.9%)
**Impact**: Medium (users can't login via OAuth)
**Description**: If OAuth provider is down, OAuth login fails

**Mitigation**:
- Display clear error message: "Google login temporarily unavailable"
- Allow users to login with email/password as fallback
- Monitor OAuth provider status pages
- Implement retry with exponential backoff

**Contingency**:
Email/password login always available as backup

---

### Resources

#### Documentation
- [Security Architecture v2.0](https://confluence.example.com/security-arch-v2): Company security standards and best practices
- [API Authentication Guide](https://confluence.example.com/api-auth): API authentication patterns
- [Password Policy](https://confluence.example.com/password-policy): Password requirements and hashing standards

#### Similar Implementations
- [AUTH-100: OAuth Integration](https://jira.example.com/AUTH-100): Reference OAuth implementation (use same pattern)
- [AUTH-85: Login Rate Limiting](https://jira.example.com/AUTH-85): Rate limiting implementation (apply similar approach)
- [USER-50: User Registration Flow](https://jira.example.com/USER-50): Similar registration UX

#### External References
- [OWASP Authentication Cheat Sheet](https://cheatsheetseries.owasp.org/cheatsheets/Authentication_Cheat_Sheet.html): Security guidelines
- [JWT Best Practices (RFC 8725)](https://datatracker.ietf.org/doc/html/rfc8725): JWT security recommendations
- [bcrypt Documentation](https://github.com/kelektiv/node.bcrypt.js): Password hashing library

#### Code References
- `src/auth/oauth-provider.ts`: Existing OAuth implementation in AUTH-100 (extend this)
- `backend-common` repository: Shared authentication utilities and middleware
- `src/middleware/rate-limiter.ts`: Existing rate limiting middleware (reuse)

---

### Technical Notes

#### Architecture Constraints
- Must use existing Redis instance for session storage (no new infrastructure)
- Authentication microservice deployed separately from main application
- Follow monorepo structure: packages/auth-service/

#### Performance Considerations
- Password hashing must not block event loop (use worker threads or async bcrypt)
- Login endpoint P95 latency target: < 200ms
- Support 1000 concurrent login requests without degradation

#### Security Requirements
- All passwords hashed with bcrypt cost factor 12 (company standard)
- Failed login attempts logged to security audit system (Splunk)
- HTTPS required for all auth endpoints (no HTTP fallback)
- CORS configuration: whitelist only known frontend origins

#### Configuration
**Environment Variables Needed**:
- `JWT_SECRET`: Secret key for JWT signing (rotate quarterly)
- `JWT_EXPIRY`: Token expiry duration (default: 24h)
- `SESSION_REDIS_URL`: Redis connection string for sessions
- `OAUTH_GOOGLE_CLIENT_ID`: Google OAuth client ID
- `OAUTH_GOOGLE_CLIENT_SECRET`: Google OAuth secret
- `OAUTH_GITHUB_CLIENT_ID`: GitHub OAuth client ID
- `OAUTH_GITHUB_CLIENT_SECRET`: GitHub OAuth secret

**Feature Flags**:
- `auth_v2_enabled`: Gradual rollout (1% → 10% → 50% → 100%)
- `oauth_google_enabled`: Toggle Google OAuth
- `oauth_github_enabled`: Toggle GitHub OAuth

---

🤖 *Generated by /enrich-jira-task skill*
*Context sources: 3 similar tasks (AUTH-100, AUTH-85, USER-50), 2 Confluence docs*
```

---

## Summary of Enrichment

### What Was Added:

1. **Enriched Description** (+1,200 words):
   - Overview and context
   - Detailed requirements (functional and non-functional)
   - 4 use cases with examples
   - 5 edge cases
   - Concrete API examples
   - Out of scope items
   - References to documentation

2. **Acceptance Criteria** (7 ACs):
   - Registration flow
   - Password security
   - JWT token generation
   - Validation rules
   - Rate limiting
   - OAuth flow
   - Password reset

3. **Subtasks** (5 subtasks):
   - Database schema (AUTH-124)
   - JWT service (AUTH-125)
   - Password reset (AUTH-126)
   - OAuth integration (AUTH-127)
   - Testing (AUTH-128)

4. **Technical Context**:
   - Dependencies (blocked by, blocks, related)
   - 3 risks with mitigations
   - Resources and references
   - Technical notes and configuration

### Context Sources Used:

- **Similar tasks analyzed**: AUTH-100, AUTH-85, USER-50
- **Documentation referenced**: Security Architecture v2.0, API Authentication Guide
- **Epic context**: User Management epic (AUTH-50)
- **Best practices**: OWASP guidelines, JWT RFC

### Value Delivered:

- ✅ Clear, specific requirements (no ambiguity)
- ✅ Testable acceptance criteria
- ✅ Work broken down into manageable subtasks
- ✅ Risks identified with mitigations
- ✅ Consistent with project patterns
- ✅ Ready for implementation (no questions needed)

---

**Time to enrich**: ~10 minutes (automated context gathering + AI synthesis)

**Time saved**: ~2-3 hours of manual research, writing, and back-and-forth clarifications

**Quality improvement**: Comprehensive, consistent, follows established patterns
