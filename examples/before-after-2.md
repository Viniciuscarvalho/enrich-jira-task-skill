# Example 2: Complex Epic-Level Task → Subtask Breakdown

This example demonstrates breaking down a complex, epic-level task into well-defined subtasks with clear dependencies and acceptance criteria.

---

## BEFORE Enrichment

### Task: NOTIF-200

**Summary**: Implement notification system

**Type**: Epic

**Description**:
```
Users need notifications when important events happen.
Should support email and in-app notifications.
```

**Acceptance Criteria**:
```
- Users receive notifications
- Notifications work
```

**Subtasks**:
```
(none)
```

**Identified Gaps**:
- ❌ Vague requirements ("important events" - which ones?)
- ❌ Non-specific ACs ("notifications work" - how to verify?)
- ❌ Large scope (multiple delivery channels, no breakdown)
- ❌ No technical approach defined
- ❌ No consideration of architecture, scalability

---

## AFTER Enrichment

### Task: NOTIF-200

**Summary**: Implement notification system

**Type**: Epic

**Description**:

```markdown
## Overview
Implement a comprehensive notification system to keep users informed of important events through multiple channels (email and in-app notifications). The system should be scalable, reliable, and provide a consistent user experience across delivery methods.

## Context
Based on notification patterns documented in "Notification Architecture RFC" and similar implementations in SHOP-150 (Order Notifications) and ALERT-75 (System Alerts).

This epic is part of the Q1 2026 "User Engagement" initiative and supports the product goal of improving user retention through timely, relevant notifications.

## Detailed Requirements

### Core Functionality

#### Notification Types to Support
1. **Transactional Notifications** (high priority):
   - Order confirmations
   - Payment receipts
   - Account security alerts (password changes, new logins)
   - Password reset confirmations

2. **Engagement Notifications** (medium priority):
   - New messages/comments
   - Mentions (@username)
   - Activity on followed items

3. **Marketing Notifications** (low priority, future phase):
   - Product updates
   - Feature announcements
   - Promotional campaigns

#### Delivery Channels
1. **Email**: Via SendGrid integration
   - HTML templates with brand styling
   - Plain text fallback
   - Unsubscribe functionality

2. **In-App**: Via WebSocket real-time delivery
   - Toast notifications
   - Notification bell icon with badge count
   - Notification center (list of all notifications)

### Non-Functional Requirements

#### Performance
- Real-time delivery: In-app notifications appear within 2 seconds of event
- Email delivery: Queued and sent within 5 minutes
- Notification API: P95 latency < 100ms for fetch/mark-read operations

#### Scalability
- Handle 100,000 notifications per hour
- Support 10,000 concurrent WebSocket connections
- Horizontal scaling for notification service

#### Reliability
- 99.9% delivery success rate
- Retry failed deliveries (exponential backoff)
- Dead letter queue for failed notifications after 3 retries

#### User Experience
- User notification preferences (opt-in/opt-out per notification type)
- Batch digests (option to receive daily/weekly summaries instead of real-time)
- Mark as read/unread
- Delete notifications

## Use Cases

### UC-1: Send Transactional Notification (Order Confirmation)
**Actor**: System (triggered by order creation)
**Goal**: Notify user of successful order

**Flow**:
1. Order service creates new order (ORDER-12345)
2. Order service publishes "order.created" event to message queue
3. Notification service consumes event
4. Notification service creates notification record in database
5. Notification service sends email via SendGrid
6. If user online: Send in-app notification via WebSocket
7. Notification marked as "sent"

**Example**:
- Event: `{"type": "order.created", "order_id": 12345, "user_id": 789, "total": 99.99}`
- Email sent to: user@example.com
- In-app notification: "Your order #12345 for $99.99 has been confirmed!"

### UC-2: User Views In-App Notifications
**Actor**: User
**Goal**: Check recent notifications

**Flow**:
1. User clicks notification bell icon
2. Frontend fetches unread notifications: GET /api/notifications?unread=true
3. Notification center displays list (max 50 recent)
4. User clicks notification to view details
5. Notification marked as read: PATCH /api/notifications/:id (read: true)
6. Badge count decrements

### UC-3: User Configures Notification Preferences
**Actor**: User
**Goal**: Reduce notification noise by opting out of certain types

**Flow**:
1. User navigates to Settings → Notifications
2. User sees list of notification types with toggle switches
3. User disables "New comment notifications" (email and in-app)
4. Preference saved: PATCH /api/users/:id/preferences
5. Future comment notifications not sent to this user

## Edge Cases to Consider

### Edge Case 1: User Offline When Notification Sent
- **Handling**: Store notification in database, deliver via email, show in notification center when user next logs in

### Edge Case 2: Email Delivery Fails (Invalid Email)
- **Handling**: Log failure, retry twice, then move to dead letter queue, mark notification as "failed"

### Edge Case 3: High Notification Volume (1000 notifications in 1 minute)
- **Handling**: Queue notifications, process in batches, consider batch digest option

### Edge Case 4: WebSocket Connection Drops Mid-Notification
- **Handling**: Notification already persisted in DB, user will see in notification center on reconnect

### Edge Case 5: User Deletes All Notifications
- **Handling**: Soft delete (keep in DB for 30 days), hard delete after retention period

## Out of Scope (Current Epic)
- Push notifications (mobile apps) - Track in NOTIF-300
- SMS notifications - Future consideration
- Slack integration - Track in NOTIF-350
- Admin dashboard for notification analytics - Track in NOTIF-400
- A/B testing notification content - Future enhancement

## Architecture

### High-Level Design
```
┌─────────────┐
│   Services  │ (Order, Payment, etc.)
└──────┬──────┘
       │ Publish events
       ▼
┌─────────────────┐
│  Message Queue  │ (RabbitMQ)
└────────┬────────┘
         │ Consume events
         ▼
┌─────────────────────┐
│ Notification Service│
│  - Event consumers  │
│  - Email sender     │
│  - WebSocket server │
└─────────┬───────────┘
          │
    ┌─────┴─────┬─────────┐
    ▼           ▼         ▼
┌────────┐ ┌─────────┐ ┌──────────┐
│Database│ │SendGrid │ │WebSocket │
│(notifs)│ │ (Email) │ │ Clients  │
└────────┘ └─────────┘ └──────────┘
```

### Technology Stack
- **Message Queue**: RabbitMQ (existing infrastructure)
- **Email Service**: SendGrid (existing integration)
- **WebSocket**: Socket.io
- **Database**: PostgreSQL (notifications table)
- **Cache**: Redis (notification counts, online users)

## References
- [Notification Architecture RFC](https://confluence.example.com/notif-architecture): System design and patterns
- [SHOP-150: Order Notifications](https://jira.example.com/SHOP-150): Similar email notification implementation
- [ALERT-75: System Alerts](https://jira.example.com/ALERT-75): Real-time alert delivery patterns
- [SendGrid API Docs](https://sendgrid.com/docs/api-reference/): Email API reference
```

**Acceptance Criteria**:

```markdown
## Acceptance Criteria

### AC-1: Event-Driven Notification Creation
**Given** Order service publishes "order.created" event
**When** Notification service consumes event
**Then** notification record created in database with type "order_confirmation", user_id, and event data

**Validation**: Create test order, verify notification record in notifications table

---

### AC-2: Email Delivery
**Given** notification created for user with email preferences enabled
**When** notification is processed
**Then** email sent via SendGrid within 5 minutes
And email contains branded template with notification content
And unsubscribe link included

**Validation**: Trigger notification, check SendGrid dashboard and user inbox

---

### AC-3: In-App Real-Time Delivery
**Given** user is online (active WebSocket connection)
**When** notification is created for that user
**Then** notification pushed to user's WebSocket client within 2 seconds
And notification bell badge count incremented
And toast message displayed (dismissable)

**Validation**: Trigger notification with user logged in, verify real-time delivery

---

### AC-4: Notification Persistence and Retrieval
**Given** user has 10 unread notifications
**When** user calls GET /api/notifications?unread=true
**Then** API returns 10 unread notifications ordered by created_at DESC
And response time < 100ms (P95)

**Validation**: Load test with 50 unread notifications per user

---

### AC-5: Mark as Read
**Given** user has unread notification with ID 12345
**When** user calls PATCH /api/notifications/12345 with {read: true}
**Then** notification marked as read in database
And badge count decremented
And WebSocket event sent to update UI

**Validation**: Mark notification as read via API, verify DB update and UI update

---

### AC-6: User Preferences (Opt-Out)
**Given** user disabled "comment_notification" in preferences
**When** comment event triggers notification
**Then** notification NOT created for this user
And other users with preferences enabled still receive notification

**Validation**: Set preference to disabled, trigger comment event, verify no notification

---

### AC-7: Failed Email Retry Logic
**Given** email delivery fails (SendGrid returns 500 error)
**When** notification service processes failure
**Then** notification re-queued for retry with exponential backoff
And retry attempted up to 3 times
And after 3 failures, notification moved to dead letter queue

**Validation**: Mock SendGrid failure, verify retry attempts in logs
```

**Subtasks Created**:

```markdown
## Subtasks

### NOTIF-201: Design notification data model and create database schema
**Type**: Sub-task
**Estimated Effort**: Small (1-2 days)

**Description**:
Design database schema for notifications, user preferences, and event types.

**Deliverables**:
- Migration scripts for notifications, notification_preferences, notification_types tables
- Indexes for user_id, created_at, read_status queries
- ER diagram

**Acceptance Criteria**:
- AC-1: Notifications table with columns: id, user_id, type, title, body, read, created_at
- AC-2: notification_preferences table for user opt-in/opt-out settings
- AC-3: Indexes created for fast queries: (user_id, read, created_at)

**Dependencies**: None (first subtask)

---

### NOTIF-202: Build notification API endpoints
**Type**: Sub-task
**Estimated Effort**: Medium (3-4 days)

**Description**:
Implement REST API for fetching, marking as read, and deleting notifications.

**Deliverables**:
- GET /api/notifications (list with pagination, filters)
- GET /api/notifications/:id (single notification)
- PATCH /api/notifications/:id (mark as read/unread)
- DELETE /api/notifications/:id (soft delete)
- GET /api/notifications/unread-count
- PATCH /api/users/:id/preferences (update notification preferences)

**Acceptance Criteria**:
- AC-1: All endpoints return correct status codes (200, 404, etc.)
- AC-2: Pagination implemented (limit/offset)
- AC-3: P95 latency < 100ms for GET endpoints

**Dependencies**:
- Blocked by: NOTIF-201 (database schema)

---

### NOTIF-203: Implement WebSocket server for real-time delivery
**Type**: Sub-task
**Estimated Effort**: Medium (3-4 days)

**Description**:
Build WebSocket server using Socket.io for real-time notification push to connected clients.

**Deliverables**:
- WebSocket server with Socket.io
- Connection management (authenticate, track online users)
- Emit notification events to specific users
- Handle disconnections and reconnections

**Acceptance Criteria**:
- AC-1: Users connect via WebSocket with JWT authentication
- AC-2: Notifications pushed to correct user within 2 seconds
- AC-3: Supports 10,000 concurrent connections

**Dependencies**:
- Blocked by: NOTIF-202 (needs notification data structure)

---

### NOTIF-204: Integrate email delivery via SendGrid
**Type**: Sub-task
**Estimated Effort**: Medium (3 days)

**Description**:
Implement email notification delivery using SendGrid API with HTML templates.

**Deliverables**:
- SendGrid integration module
- Email templates for each notification type (order confirmation, password reset, etc.)
- Unsubscribe link handling
- Retry logic for failed deliveries

**Acceptance Criteria**:
- AC-1: Emails sent within 5 minutes of notification creation
- AC-2: HTML templates render correctly in Gmail, Outlook, Apple Mail
- AC-3: Unsubscribe functionality works (updates user preferences)
- AC-4: Failed deliveries retried up to 3 times

**Dependencies**:
- Blocked by: NOTIF-201 (database schema)

---

### NOTIF-205: Build event consumer for notification triggers
**Type**: Sub-task
**Estimated Effort**: Medium (3-4 days)

**Description**:
Implement message queue consumers to listen for events from other services and create notifications.

**Deliverables**:
- RabbitMQ consumers for supported event types
- Event-to-notification mapping logic
- Notification creation service
- Check user preferences before creating notification

**Acceptance Criteria**:
- AC-1: Consumer processes "order.created", "payment.received", "comment.created" events
- AC-2: Notification created in database within 1 second of event
- AC-3: User preferences respected (no notification if opt-out)

**Dependencies**:
- Blocked by: NOTIF-201 (database schema)
- Blocked by: NOTIF-204 (email service)

---

### NOTIF-206: Implement notification preferences UI
**Type**: Sub-task
**Estimated Effort**: Small (2 days)

**Description**:
Build user settings page for managing notification preferences (opt-in/opt-out per type and channel).

**Deliverables**:
- Settings page: /settings/notifications
- Toggle switches for each notification type (email and in-app)
- Save preferences (calls PATCH /api/users/:id/preferences)
- Real-time preview of changes

**Acceptance Criteria**:
- AC-1: User can toggle email/in-app preferences per notification type
- AC-2: Preferences saved successfully
- AC-3: Changes take effect immediately (no cache delay)

**Dependencies**:
- Blocked by: NOTIF-202 (preferences API)

---

### NOTIF-207: Build in-app notification center UI
**Type**: Sub-task
**Estimated Effort**: Medium (3 days)

**Description**:
Implement notification bell icon, badge count, and notification center dropdown in main app UI.

**Deliverables**:
- Notification bell icon with unread badge count
- Dropdown notification center (list recent notifications)
- Mark as read on click
- "View all" link to full notification history page
- Real-time updates via WebSocket

**Acceptance Criteria**:
- AC-1: Bell icon displays unread count badge
- AC-2: Clicking bell opens dropdown with max 10 recent notifications
- AC-3: Real-time updates when new notification arrives
- AC-4: Responsive design (mobile and desktop)

**Dependencies**:
- Blocked by: NOTIF-202 (API endpoints)
- Blocked by: NOTIF-203 (WebSocket server)

---

### NOTIF-208: Write comprehensive tests for notification system
**Type**: Sub-task
**Estimated Effort**: Medium (3 days)

**Description**:
Test coverage for all notification flows: API, WebSocket, email delivery, event processing.

**Deliverables**:
- Unit tests for notification service logic
- Integration tests for API endpoints
- E2E tests for notification creation and delivery
- Load tests for WebSocket scalability

**Acceptance Criteria**:
- AC-1: Test coverage > 85% for notification service
- AC-2: All notification types covered in E2E tests
- AC-3: Load test validates 100,000 notifications/hour throughput

**Dependencies**:
- Blocked by: NOTIF-207 (all features complete)

---

### NOTIF-209: Setup monitoring and alerting for notification system
**Type**: Sub-task
**Estimated Effort**: Small (2 days)

**Description**:
Configure monitoring dashboards and alerts for notification delivery metrics.

**Deliverables**:
- Grafana dashboard for notification metrics
- Alerts for: email delivery failures > 1%, WebSocket disconnections spike
- Logging for all notification events (structured logs)
- Dead letter queue monitoring

**Acceptance Criteria**:
- AC-1: Dashboard displays: notifications sent/hour, delivery success rate, P95 latency
- AC-2: Alerts fire when email failure rate > 1%
- AC-3: Dead letter queue monitored (alert if > 100 messages)

**Dependencies**:
- Blocked by: NOTIF-208 (testing complete)

---

### NOTIF-210: Documentation and deployment
**Type**: Sub-task
**Estimated Effort**: Small (1-2 days)

**Description**:
Write user and developer documentation, deploy to staging.

**Deliverables**:
- User guide: How to manage notification preferences
- Developer docs: How to trigger notifications from services
- API documentation (OpenAPI spec)
- Deployment runbook

**Acceptance Criteria**:
- AC-1: User guide published to help center
- AC-2: Developer docs include code examples for each event type
- AC-3: Deployed to staging with feature flag

**Dependencies**:
- Blocked by: NOTIF-209 (monitoring setup)
```

**Technical Context Comment Added**:

```markdown
## 🔔 Technical Context

### Dependencies

**Blocks** (cannot start until this epic completes):
- **USER-400**: User Engagement Dashboard
  - Reason: Dashboard displays notification history
- **MOBILE-100**: Mobile App Notifications
  - Reason: Mobile app will use same notification API

**Related**:
- **SHOP-150**: Order Notifications (reference implementation for email templates)
- **ALERT-75**: System Alerts (reference implementation for real-time delivery)
- **EMAIL-30**: Email Service Integration (shared SendGrid account)

---

### Risks & Considerations

#### Risk-1: High Notification Volume Overwhelming Email Service
**Likelihood**: Medium (during promotional campaigns)
**Impact**: High (emails delayed or not delivered)
**Description**: SendGrid has rate limits (10,000 emails/hour on current plan)

**Mitigation**:
- Implement notification queue with rate limiting
- Batch digest option for non-critical notifications
- Upgrade SendGrid plan if needed (monitor usage)
- Add circuit breaker to prevent cascading failures

**Source**: SHOP-150 experienced email delays during Black Friday sale

**Contingency**:
Priority levels: Critical notifications (password reset) sent immediately, engagement notifications can be batched

---

#### Risk-2: WebSocket Connections Don't Scale Horizontally
**Likelihood**: Medium (when > 10,000 concurrent users)
**Impact**: High (can't scale notification service)
**Description**: Socket.io requires sticky sessions, complicates horizontal scaling

**Mitigation**:
- Use Socket.io Redis adapter for multi-instance support
- Configure load balancer for sticky sessions (session affinity)
- Monitor concurrent connection count
- Plan for vertical scaling initially (larger instance)

**Source**: ALERT-75 research on real-time scaling

**Contingency**:
Fall back to polling (GET /api/notifications every 30 seconds) if WebSocket connections exceed capacity

---

#### Risk-3: Message Queue Lag During High Event Volume
**Likelihood**: Low (RabbitMQ is robust)
**Impact**: Medium (notifications delayed)
**Description**: If event volume spikes (e.g., bulk order import), queue might lag

**Mitigation**:
- Monitor queue depth and consumer lag
- Auto-scale notification service consumers
- Set max queue size with overflow to disk
- Add backpressure mechanism (slow down event publishers if needed)

**Contingency**:
Email notifications can be delayed up to 30 minutes without critical impact (in-app is priority)

---

### Resources

#### Documentation
- [Notification Architecture RFC](https://confluence.example.com/notif-architecture): Detailed system design
- [Event Schema Registry](https://confluence.example.com/event-schemas): All event types and payloads
- [Email Template Guidelines](https://confluence.example.com/email-templates): Branding and design standards

#### Similar Implementations
- [SHOP-150: Order Notifications](https://jira.example.com/SHOP-150): Email template patterns
- [ALERT-75: System Alerts](https://jira.example.com/ALERT-75): WebSocket real-time delivery
- [USER-200: User Preferences](https://jira.example.com/USER-200): Preference management UI

#### External References
- [SendGrid API](https://sendgrid.com/docs/api-reference/): Email API documentation
- [Socket.io Docs](https://socket.io/docs/v4/): WebSocket library
- [RabbitMQ Best Practices](https://www.rabbitmq.com/tutorials/tutorial-one-javascript.html): Message queue patterns

#### Code References
- `services/order-service/events.ts`: Example event publishing
- `shared/email-templates/`: Existing email templates to extend
- `backend/middleware/websocket-auth.ts`: WebSocket authentication middleware

---

### Technical Notes

#### Architecture Decisions
- **Event-driven architecture**: Services publish events, notification service consumes (loose coupling)
- **Push + Pull hybrid**: WebSocket for real-time push, API for pull (notification history)
- **Soft delete notifications**: Keep for 30 days (compliance, user can recover)

#### Performance Targets
- **Notification creation**: < 1 second from event to database record
- **Email delivery**: < 5 minutes (queued)
- **In-app delivery**: < 2 seconds (real-time)
- **API latency**: P95 < 100ms for GET operations
- **Throughput**: 100,000 notifications/hour

#### Scalability Plan
- **Phase 1** (MVP): Single notification service instance, 10K concurrent WebSocket connections
- **Phase 2** (3 months): Horizontal scaling with Redis adapter, 50K concurrent connections
- **Phase 3** (6 months): Multi-region deployment, 100K+ concurrent connections

#### Configuration
**Environment Variables**:
- `SENDGRID_API_KEY`: SendGrid API key
- `RABBITMQ_URL`: Message queue connection string
- `REDIS_URL`: Redis for Socket.io adapter and caching
- `NOTIFICATION_RETENTION_DAYS`: Soft delete retention (default: 30)

**Feature Flags**:
- `notifications_enabled`: Master toggle
- `email_notifications_enabled`: Email delivery toggle
- `websocket_notifications_enabled`: Real-time delivery toggle
- `notification_types_{type}_enabled`: Per-type toggles (e.g., `notification_types_comment_enabled`)

---

🤖 *Generated by /enrich-jira-task skill*
*Context sources: 2 similar tasks (SHOP-150, ALERT-75), 3 Confluence docs (Architecture RFC, Event Schemas, Email Guidelines)*
```

---

## Summary of Enrichment

### What Was Added:

1. **Enriched Description** (+1,500 words):
   - Clear overview and context
   - Detailed requirements (notification types, delivery channels)
   - Non-functional requirements (performance, scalability, reliability)
   - 3 detailed use cases
   - 5 edge cases
   - Architecture diagram
   - Technology stack
   - Out of scope items

2. **Acceptance Criteria** (7 ACs):
   - Event-driven creation
   - Email delivery
   - Real-time in-app delivery
   - API retrieval
   - Mark as read
   - User preferences
   - Retry logic

3. **Subtasks** (10 subtasks):
   - NOTIF-201: Database schema (1-2 days)
   - NOTIF-202: API endpoints (3-4 days)
   - NOTIF-203: WebSocket server (3-4 days)
   - NOTIF-204: Email integration (3 days)
   - NOTIF-205: Event consumer (3-4 days)
   - NOTIF-206: Preferences UI (2 days)
   - NOTIF-207: Notification center UI (3 days)
   - NOTIF-208: Testing (3 days)
   - NOTIF-209: Monitoring (2 days)
   - NOTIF-210: Documentation (1-2 days)

   **Total**: 24-29 days of work, broken into manageable pieces

4. **Technical Context**:
   - Dependencies and blocked tasks
   - 3 major risks with detailed mitigations
   - Extensive resources and references
   - Architecture decisions and scaling plan
   - Configuration requirements

### Subtask Dependencies Visualized:

```
NOTIF-201 (Database)
    │
    ├──→ NOTIF-202 (API)
    │       │
    │       ├──→ NOTIF-206 (Preferences UI)
    │       └──→ NOTIF-207 (Notification Center UI)
    │               │
    │               └──→ NOTIF-208 (Testing)
    │                       │
    │                       └──→ NOTIF-209 (Monitoring)
    │                               │
    │                               └──→ NOTIF-210 (Documentation)
    │
    ├──→ NOTIF-203 (WebSocket)
    │       │
    │       └──→ NOTIF-207 (Notification Center UI)
    │
    ├──→ NOTIF-204 (Email)
    │       │
    │       └──→ NOTIF-205 (Event Consumer)
    │
    └──→ NOTIF-205 (Event Consumer)
```

### Context Sources Used:

- **Similar tasks**: SHOP-150 (email templates), ALERT-75 (real-time delivery)
- **Documentation**: Notification Architecture RFC, Event Schema Registry, Email Template Guidelines
- **Existing code**: Order service events, email templates, WebSocket auth middleware

### Value Delivered:

- ✅ **Clear scope**: Exactly what's in/out of scope
- ✅ **Work breakdown**: 10 subtasks with dependencies and estimates (24-29 days total)
- ✅ **Risk awareness**: 3 major risks identified with mitigations
- ✅ **Architecture clarity**: Technology choices justified
- ✅ **Team alignment**: Everyone knows what to build and in what order
- ✅ **Ready to start**: Teams can pick up subtasks and begin work immediately

---

**Time to enrich**: ~15 minutes (larger epic, more context to gather)

**Time saved**: ~4-6 hours of planning meetings, architecture discussions, and documentation

**Quality improvement**: Comprehensive breakdown prevents scope creep, parallel work enabled, risks identified upfront

---

## Key Differences from Example 1

**Example 1** (Simple Feature):
- Single task enriched with description and ACs
- 5 subtasks (full-stack breakdown)
- Focus: Implementation details

**Example 2** (Epic):
- Complex epic broken down into 10 subtasks
- Focus: Architecture, scalability, dependencies
- More emphasis on risks and technical decisions

Both demonstrate the skill's ability to adapt enrichment depth to task complexity.
