# DOC-22 — EEOS Core Platform Engines & Shared Services Bible

> **Version**: 1.0  
> **Status**: Architecture Blueprint  
> **Owner**: EEOS Architecture Team  
> **Last Updated**: 2026-07-14  
> **Suggested Path**: `04-Platform/DOC-22 — EEOS Core Platform Engines & Shared Services Bible.md`

---

## Table of Contents

1. [Purpose](#1-purpose)
2. [Platform Architecture Overview](#2-platform-architecture-overview)
3. [Engine Inventory](#3-engine-inventory)
4. [Engine Dependency Matrix](#4-engine-dependency-matrix)
5. [Module Dependency Matrix](#5-module-dependency-matrix)
6. [Database Blueprint](#6-database-blueprint)
7. [Event Architecture](#7-event-architecture)
8. [API Architecture](#8-api-architecture)
9. [Security](#9-security)
10. [Performance](#10-performance)
11. [AI Readiness](#11-ai-readiness)
12. [Development Roadmap](#12-development-roadmap)
13. [Golden Rules](#13-golden-rules)

---

## 1. Purpose

### Why Engine-Driven?

EEOS is **engine-driven, not module-driven**. This is the single most important architectural decision in the platform.

A module-driven architecture creates silos. CRM has its own notification system. Finance has its own approval system. HR has its own document storage. The result is:

- Duplicate code
- Inconsistent user experience
- Fragmented data
- Expensive maintenance
- Impossible cross-module reporting

An **engine-driven architecture** solves all of this by extracting shared capabilities into reusable platform engines. Every module consumes the same engines. Every feature benefits from improvements to any engine.

### Benefits

**Reusability**: Build once, use everywhere. The Notification Engine powers CRM follow-ups, Finance reminders, HR alerts, and Academic announcements — from a single codebase.

**Scalability**: Engines can be optimized independently. Cache the Sequence Engine. Queue the Notification Engine. Index the Search Engine independently of any module.

**Maintainability**: Fix a bug in the Activity Engine once. Every module that uses activities gets the fix automatically. No hunting through 12 modules for the same pattern.

**Low Code Future**: Configuration-driven engines mean business users can define workflows, approvals, notifications, and reports without developer intervention. Every engine exposes a configuration surface.

**AI Readiness**: Every engine emits structured events and exposes clean APIs. The future AI Engine consumes these events to train models, predict outcomes, recommend actions, and automate decisions. Without engines, AI has no clean data to consume.

**Audit & Compliance**: A single Audit Engine tracks every change across every module. No module can bypass auditing. Compliance reporting is a simple query against one table.

### Design Philosophy

> Every shared capability is an engine. Every engine has one responsibility. Every module uses engines. No engine knows about modules.

---

## 2. Platform Architecture Overview
┌─────────────────────────────────────────────────────────────┐

│ UI LAYER │

│ React 19 · Tailwind · shadcn/ui · Framer Motion │

│ DashboardLayout · StudioLayout · CommandPalette │

└──────────────────────────┬──────────────────────────────────┘

│

┌──────────────────────────▼──────────────────────────────────┐

│ STUDIOS │

│ Organization │ Master Data │ Access Control │ Workflow │

│ Task Mgmt │ Settings │ Analytics │

└──────────────────────────┬──────────────────────────────────┘

│

┌──────────────────────────▼──────────────────────────────────┐

│ BUSINESS MODULES │

│ CRM │ Sales │ Admissions │ Student │ Academic │ Finance │

│ HR │ Marketing │ Admin │ Technology │ Communication │

└──────────────────────────┬──────────────────────────────────┘

│

┌──────────────────────────▼──────────────────────────────────┐

│ SHARED ENGINES │

│ │

│ ┌──────────┐ ┌──────────┐ ┌──────────┐ ┌──────────┐ │

│ │ Sequence │ │ Activity │ │Notificatn│ │Attachmnt │ │

│ │ Engine │ │ Engine │ │ Engine │ │ Engine │ │

│ └──────────┘ └──────────┘ └──────────┘ └──────────┘ │

│ │

│ ┌──────────┐ ┌──────────┐ ┌──────────┐ ┌──────────┐ │

│ │ Comment │ │ Timeline │ │ Audit │ │ Approval │ │

│ │ Engine │ │ Engine │ │ Engine │ │ Engine │ │

│ └──────────┘ └──────────┘ └──────────┘ └──────────┘ │

│ │

│ ┌──────────┐ ┌──────────┐ ┌──────────┐ ┌──────────┐ │

│ │ Workflow │ │ Search │ │ Tag │ │ Label │ │

│ │ Engine │ │ Engine │ │ Engine │ │ Engine │ │

│ └──────────┘ └──────────┘ └──────────┘ └──────────┘ │

│ │

│ ┌──────────┐ ┌──────────┐ ┌──────────┐ ┌──────────┐ │

│ │ Template │ │ Communic │ │ Calendar │ │ Reminder │ │

│ │ Engine │ │ Engine │ │ Engine │ │ Engine │ │

│ └──────────┘ └──────────┘ └──────────┘ └──────────┘ │

│ │

│ ┌──────────┐ ┌──────────┐ ┌──────────┐ ┌──────────┐ │

│ │Reporting │ │Dashboard │ │Import/Exp│ │ File │ │

│ │ Engine │ │ Engine │ │ Engine │ │ Storage │ │

│ └──────────┘ └──────────┘ └──────────┘ └──────────┘ │

│ │

│ ┌──────────┐ ┌──────────┐ │

│ │Integratn │ │ AI │ │

│ │ Engine │ │ Engine │ (Future) │

│ └──────────┘ └──────────┘ │

└──────────────────────────┬──────────────────────────────────┘

│

┌──────────────────────────▼──────────────────────────────────┐

│ INFRASTRUCTURE │

│ Convex (DB/Backend) · Auth · File Storage · Email · SMS │

│ Webhooks · Queues · Caching · Search Index │

└─────────────────────────────────────────────────────────────┘


### Dependency Flow
UI Layer → Studios → Business Modules → Shared Engines → Infrastructure


---

## 3. Engine Inventory

### 3.1 Sequence Engine

**Priority**: P0 — Foundation

**Purpose**: Automatic, configurable numbering for all business entities.

**Responsibilities**:
- Generate sequential numbers with configurable prefixes
- Support per-entity, per-branch, and per-year sequences
- Reset sequences annually, monthly, or never
- Format: `{PREFIX}-{YYYY}-{NNNNNN}` or custom
- Prevent gaps, validate uniqueness within scope

**Consumers**: CRM, Admissions, Student, Finance, HR, Tasks, Academic, all modules.

**Database Entities**:
sequences

├── id, entityType, prefix, branchId, academicSessionId

├── year, counter, format, resetFrequency, isActive

sequence_history

├── id, sequenceId, generatedNumber, entityId, generatedAt, generatedBy


**APIs**: `generateNumber()`, `previewNextNumber()`, `getLastNumber()`

**Events**: `sequence.number_generated`

**Dependencies**: None

---

### 3.2 Activity Engine

**Priority**: P0 — Foundation

**Purpose**: Unified activity timeline for every entity.

**Database Entities**:
activities

├── id, entityType, entityId, activityType, title, description

├── userId, metadata (JSON), isSystem, parentId

├── createdAt, updatedAt


**APIs**: `logActivity()`, `getActivities()`, `getRecentActivities()`, `getActivityFeed()`

**Events**: `activity.created`

**Dependencies**: None

---

### 3.3 Notification Engine

**Priority**: P0 — Foundation

**Purpose**: Universal notification delivery across all channels (In-App, Email, WhatsApp, SMS, Push).

**Database Entities**: `notification_templates`, `notifications`, `notification_preferences`

**APIs**: `sendNotification()`, `sendBroadcast()`, `sendScheduledNotification()`, `getNotifications()`, `markAsRead()`, `getUnreadCount()`

**Events**: `notification.sent`, `notification.delivered`, `notification.read`, `notification.failed`

**Dependencies**: Template Engine, Activity Engine

---

### 3.4 Attachment Engine

**Priority**: P0 — Foundation

**Purpose**: Reusable document management with versioning, categorization, storage abstraction.

**Database Entities**: `attachments`

**APIs**: `uploadAttachment()`, `getAttachments()`, `deleteAttachment()`, `replaceAttachment()`, `generateUploadUrl()`, `generateDownloadUrl()`

**Dependencies**: Activity Engine, File Storage Engine

---

### 3.5 Comment Engine

**Priority**: P1 — Core

**Purpose**: Universal discussion system with @mentions, attachments, reactions, threading.

**Database Entities**: `comments`, `comment_reactions`

**APIs**: `addComment()`, `getComments()`, `addReaction()`, `pinComment()`, `getMentionSuggestions()`

**Dependencies**: Notification Engine, Attachment Engine, Activity Engine

---

### 3.6 Timeline Engine

**Priority**: P1 — Core

**Purpose**: Unified chronological history of state changes with before/after values and time-travel.

**Database Entities**: `timeline_events`

**APIs**: `recordEvent()`, `getTimeline()`, `getEntityStateAt()`

**Dependencies**: None

---

### 3.7 Audit Engine

**Priority**: P1 — Core

**Purpose**: Immutable record of every data change. Compliance-ready (DPDP, FERPA, SOC2, GDPR).

**Database Entities**: `audit_logs` (with checksums)

**APIs**: `recordAudit()`, `getAuditLog()`, `verifyIntegrity()`, `exportAuditLog()`

**Events**: `audit.recorded`

**Dependencies**: None (lowest-level engine)

---

### 3.8 Approval Engine

**Priority**: P1 — Core

**Purpose**: Universal multi-level approval with parallel/sequential/conditional chains, delegation, SLA.

**Database Entities**: `approval_chains`, `approval_chain_steps`, `approvals`, `approval_decisions`

**APIs**: `submitForApproval()`, `approve()`, `reject()`, `delegate()`, `escalate()`

**Events**: `approval.submitted`, `approval.approved`, `approval.rejected`, `approval.completed`, `approval.escalated`

**Dependencies**: Notification Engine, Activity Engine, Audit Engine

---

### 3.9 Workflow Engine

**Priority**: P1 — Core

**Purpose**: Business process automation. Trigger → Condition → Action chains.

**Database Entities**: `workflows`, `workflow_triggers`, `workflow_conditions`, `workflow_actions`, `workflow_execution_logs`

**APIs**: `createWorkflow()`, `executeWorkflow()`, `testWorkflow()`, `getExecutionHistory()`

**Events**: `workflow.executed`, `workflow.failed`

**Dependencies**: Notification Engine, Task Engine, Activity Engine, Approval Engine

---

### 3.10 Search Engine

**Priority**: P1 — Core

**Purpose**: Global enterprise search. Permission-aware, cross-entity, real-time.

**Database Entities**: `search_index`, `search_suggestions`

**APIs**: `search()`, `getSuggestions()`, `indexEntity()`, `rebuildIndex()`

**Events**: `search.indexed`, `search.reindexed`

**Dependencies**: All modules (for indexing)

---

### 3.11 Tag Engine

**Priority**: P2 — Enhancement

**Purpose**: Reusable tagging system with groups, colors, cross-entity assignment.

**Database Entities**: `tags`, `entity_tags`

**APIs**: `createTag()`, `assignTag()`, `removeTag()`, `mergeTags()`

**Events**: `tag.assigned`, `tag.removed`

**Dependencies**: None

---

### 3.12 Label Engine

**Priority**: P2 — Enhancement

**Purpose**: Reusable status, priority, and category labels. Master Data driven.

**Database Entities**: `label_types`, `label_values`

**APIs**: Standard CRUD via Master Data Studio

**Dependencies**: None

---

### 3.13 Template Engine

**Priority**: P2 — Enhancement

**Purpose**: Reusable templates for email, SMS, WhatsApp, certificates, receipts. `{{variable}}` syntax.

**Database Entities**: `templates`

**APIs**: `renderTemplate()`, `previewTemplate()`

**Dependencies**: None

---

### 3.14 Communication Engine

**Priority**: P2 — Enhancement

**Purpose**: Omnichannel conversation management. Internal inbox, broadcasts, announcements.

**Database Entities**: `conversations`, `messages`

**APIs**: `createConversation()`, `sendMessage()`, `broadcastMessage()`

**Events**: `message.sent`, `conversation.created`

**Dependencies**: Attachment Engine, Notification Engine, Template Engine

---

### 3.15 Calendar Engine

**Priority**: P2 — Enhancement

**Purpose**: Unified calendar for meetings, leaves, exams, holidays, birthdays. Recurring events.

**Database Entities**: `calendar_events`

**APIs**: `createEvent()`, `getEvents()`, `checkConflicts()`

**Events**: `event.created`, `event.updated`, `event.deleted`

**Dependencies**: Notification Engine

---

### 3.16 Reminder Engine

**Priority**: P2 — Enhancement

**Purpose**: Configurable reminders with snooze, escalation, recurrence, multi-channel delivery.

**Database Entities**: `reminders`

**APIs**: `createReminder()`, `snoozeReminder()`, `getOverdueReminders()`

**Events**: `reminder.sent`, `reminder.completed`

**Dependencies**: Notification Engine

---

### 3.17 Reporting Engine

**Priority**: P2 — Enhancement

**Purpose**: Configurable reports with charts, exports (PDF/Excel/CSV/JSON), scheduling.

**Database Entities**: `report_templates`, `saved_reports`

**APIs**: `generateReport()`, `exportReport()`, `scheduleReport()`

**Dependencies**: All engines (as data sources)

---

### 3.18 Dashboard Engine

**Priority**: P2 — Enhancement

**Purpose**: Configurable, role-based dashboards with widget library. CEO/COO/CFO views.

**Database Entities**: `dashboards`, `dashboard_widgets`

**APIs**: `getDashboard()`, `addWidget()`, `getWidgetData()`

**Dependencies**: Reporting Engine

---

### 3.19 Import/Export Engine

**Priority**: P3 — Advanced

**Purpose**: Bulk data operations for all entities. CSV, Excel, PDF, JSON.

**Database Entities**: `import_jobs`, `export_jobs`

**APIs**: `importFile()`, `getImportPreview()`, `exportEntity()`

**Dependencies**: File Storage Engine

---

### 3.20 File Storage Engine

**Priority**: P3 — Advanced

**Purpose**: Storage abstraction (local, S3, GCS, Azure). Signed URLs, chunked upload.

**APIs**: `upload()`, `generateUploadUrl()`, `generateDownloadUrl()`

**Dependencies**: None

---

### 3.21 Integration Engine

**Priority**: P3 — Advanced

**Purpose**: External system integration. REST APIs, webhooks, third-party connectors.

**Database Entities**: `integrations`, `webhook_endpoints`, `integration_logs`

**APIs**: `registerWebhook()`, `triggerWebhook()`, `testConnection()`

**Events**: `integration.webhook_received`, `integration.webhook_sent`, `integration.failed`

**Dependencies**: Audit Engine

---

### 3.22 AI Engine

**Priority**: P4 — Future

**Purpose**: Enterprise AI. Assistant, search, predictions, recommendations, automation, analytics.

**Dependencies**: All engines (as data sources)

---

## 4. Engine Dependency Matrix
Engine Depends On Depended By

────────────────────── ────────────────────────────── ─────────────────────────────

Sequence Engine — All modules

Activity Engine — All modules, Notification Engine

Notification Engine Template Engine, Activity Engine Approval Engine, Workflow Engine, Reminder Engine, Communication Engine

Attachment Engine Activity Engine, File Storage Comment Engine, Communication Engine

Comment Engine Notification, Attachment, Activity CRM, Sales, Tasks, Student, HR

Timeline Engine — All modules (state tracking)

Audit Engine — All modules (mandatory)

Approval Engine Notification, Activity, Audit CRM, Finance, HR, Admissions, Tasks

Workflow Engine Notification, Activity, Approval All modules (automation)

Search Engine All modules Global Search UI

Tag Engine — CRM, Tasks, Marketing, Student

Label Engine — All modules (status/priority)

Template Engine — Notification, Communication, Reporting

Communication Engine Attachment, Notification, Template CRM, Sales, Support

Calendar Engine Notification Dashboard, HR, Academic

Reminder Engine Notification CRM, Finance, HR, Tasks

Reporting Engine All engines Dashboard Engine, Analytics

Dashboard Engine Reporting Engine CEO/COO/CFO Dashboards

Import/Export Engine File Storage Engine All modules

File Storage Engine — Attachment, Import/Export

Integration Engine Audit Engine All modules (external APIs)

AI Engine All engines AI Studio (Future)


---

## 5. Module Dependency Matrix

| Module | Engines Consumed |
|--------|-----------------|
| CRM | Sequence, Activity, Notification, Attachment, Comment, Timeline, Audit, Approval, Workflow, Search, Tag, Label, Template, Communication, Calendar, Reminder, Import/Export |
| Sales | Sequence, Activity, Notification, Attachment, Comment, Timeline, Audit, Approval, Workflow, Search, Tag, Label, Template, Communication, Calendar, Reminder, Import/Export |
| Admissions | Sequence, Activity, Notification, Attachment, Comment, Timeline, Audit, Approval, Workflow, Search, Tag, Label, Template, Communication, Calendar, Reminder, Import/Export |
| Student | Sequence, Activity, Notification, Attachment, Comment, Timeline, Audit, Workflow, Search, Tag, Label, Template, Communication, Calendar, Import/Export |
| Academic | Sequence, Activity, Notification, Attachment, Timeline, Audit, Search, Label, Template, Calendar, Reminder, Reporting, Import/Export |
| Finance | Sequence, Activity, Notification, Attachment, Comment, Timeline, Audit, Approval, Workflow, Search, Label, Template, Calendar, Reminder, Reporting, Import/Export |
| HR | Sequence, Activity, Notification, Attachment, Comment, Timeline, Audit, Approval, Workflow, Search, Tag, Label, Template, Calendar, Reminder, Reporting, Import/Export |
| Marketing | Sequence, Activity, Notification, Attachment, Timeline, Audit, Workflow, Search, Tag, Label, Template, Communication, Calendar, Reporting, Import/Export |
| Administration | Sequence, Activity, Notification, Attachment, Timeline, Audit, Workflow, Search, Label, Template, Calendar, Reminder, Import/Export |
| Technology | Sequence, Activity, Notification, Attachment, Timeline, Audit, Search, Label, Template, Calendar, Reminder, Import/Export |
| Communication | Activity, Notification, Attachment, Template, Search, Audit |
| Analytics | Activity, Timeline, Audit, Reporting, Dashboard, Search, Calendar (Read-only) |
| Tasks | Sequence, Activity, Notification, Attachment, Comment, Timeline, Audit, Approval, Workflow, Search, Tag, Label, Calendar, Reminder, Import/Export |

---

## 6. Database Blueprint

| Engine | Tables |
|--------|--------|
| Sequence Engine | `sequences`, `sequence_history` |
| Activity Engine | `activities` |
| Notification Engine | `notification_templates`, `notifications`, `notification_preferences` |
| Attachment Engine | `attachments` |
| Comment Engine | `comments`, `comment_reactions` |
| Timeline Engine | `timeline_events` |
| Audit Engine | `audit_logs` |
| Approval Engine | `approval_chains`, `approval_chain_steps`, `approvals`, `approval_decisions` |
| Workflow Engine | `workflows`, `workflow_triggers`, `workflow_conditions`, `workflow_actions`, `workflow_execution_logs` |
| Search Engine | `search_index`, `search_suggestions` |
| Tag Engine | `tags`, `entity_tags` |
| Label Engine | `label_types`, `label_values` |
| Template Engine | `templates` |
| Communication Engine | `conversations`, `messages` |
| Calendar Engine | `calendar_events` |
| Reminder Engine | `reminders` |
| Reporting Engine | `report_templates`, `saved_reports` |
| Dashboard Engine | `dashboards`, `dashboard_widgets` |
| Import/Export Engine | `import_jobs`, `export_jobs` |
| Integration Engine | `integrations`, `webhook_endpoints`, `integration_logs` |

**Total estimated tables**: 45+

---

## 7. Event Architecture

### Event Bus Model

EEOS uses publish-subscribe. Engines and modules publish events. Other engines and modules subscribe.

### Event Payload Standard
{

"event": "entity.action",

"timestamp": "2026-07-14T12:00:00Z",

"source": "module_name",

"entityType": "lead",

"entityId": "jd8sj3kd8",

"actor": { "userId": "user_123", "role": "admin" },

"payload": {},

"metadata": { "requestId": "req_abc", "sessionId": "sess_xyz" }

}


### Key Events Catalogue

| Event | Publisher | Subscribers |
|-------|-----------|-------------|
| `entity.created` | All modules | Search, Timeline, Audit, Workflow |
| `entity.updated` | All modules | Search, Timeline, Audit, Workflow |
| `entity.deleted` | All modules | Search, Timeline, Audit, Workflow |
| `entity.status_changed` | All modules | Activity, Notification, Workflow |
| `sequence.number_generated` | Sequence Engine | Activity |
| `activity.created` | Activity Engine | Search, Notification |
| `notification.sent/delivered/read/failed` | Notification Engine | Activity |
| `attachment.uploaded/deleted` | Attachment Engine | Activity, Search |
| `comment.created/mentioned` | Comment Engine | Activity, Notification |
| `approval.submitted/approved/rejected/completed/escalated` | Approval Engine | Notification, Activity, Workflow |
| `workflow.executed/failed` | Workflow Engine | Activity, Notification |
| `reminder.sent/completed` | Reminder Engine | Activity |
| `message.sent` | Communication Engine | Activity, Notification |
| `event.created/updated/deleted` | Calendar Engine | Notification |
| `integration.webhook_received/failed` | Integration Engine | Workflow, Activity |

---

## 8. API Architecture

**Naming Convention**: `{engine}.{action}` — e.g. `sequences.generateNumber`, `activities.logActivity`, `notifications.send`

**Response Format**:
{ "success": true, "data": {}, "pagination": { "cursor": "...", "hasMore": true }, "error": null }


**Error Format**:
{ "success": false, "data": null, "error": { "code": "NOT_FOUND", "message": "Entity not found" } }


---

## 9. Security

- **Authentication**: Convex Auth (handled by platform)
- **Authorization**: Permission check on every API call
- **Scope**: Organization/branch/department/role based
- **Audit**: Every access logged by Audit Engine
- **Multi-tenancy**: Data scoped by `organizationId`
- **Encryption**: At rest (Convex), in transit (TLS), secrets encrypted in Integration Engine
- **Compliance**: DPDP, FERPA, GDPR ready

---

## 10. Performance

**Caching**: Sequence counters (in-memory), Activity feeds (30s), Templates (forever), Search (dedicated index)

**Queue Architecture**: Email/SMS/WhatsApp (async, retry), Search indexing (async), Activity logging (fire-and-forget), Webhooks (async, retry), Import/Export (background job)

**Index Strategy**: Compound indexes on `(entityType, entityId, createdAt)` and `(userId, createdAt)`. Full-text for Search. Partial indexes for filtered queries.

---

## 11. AI Readiness

Every engine designed for AI consumption:

1. **Structured Events** → AI training data
2. **Clean APIs** → AI tool calling (LangChain, Vercel AI SDK)
3. **Activity Engine** → Recommendation model training data
4. **Timeline Engine** → Time-series predictions
5. **Audit Engine** → Ground truth for evaluation
6. **Search Engine** → RAG retrieval layer
7. **Workflow Engine** → AI automation targets
8. **Notifications** → AI-generated personalized content

---

## 12. Development Roadmap
Phase 1 — Foundation (Current Sprint)

├── 1.1 Sequence Engine (P0 — No dependencies)

├── 1.2 Activity Engine (P0 — No dependencies)

└── 1.3 Notification Engine (P0 — Depends on Template Engine)

Phase 2 — Core (Next Sprint)

├── 2.1 Attachment Engine (P0)

├── 2.2 Comment Engine (P1)

├── 2.3 Timeline Engine (P1)

└── 2.4 Audit Engine (P1)

Phase 3 — Governance

├── 3.1 Approval Engine (P1)

├── 3.2 Workflow Engine (P1)

└── 3.3 Search Engine (P1)

Phase 4 — Enhancement

├── 4.1 Tag Engine (P2)

├── 4.2 Label Engine (P2)

├── 4.3 Template Engine (P2)

├── 4.4 Communication Engine (P2)

├── 4.5 Calendar Engine (P2)

└── 4.6 Reminder Engine (P2)

Phase 5 — Advanced

├── 5.1 Reporting Engine (P2)

├── 5.2 Dashboard Engine (P2)

├── 5.3 Import/Export Engine (P3)

└── 5.4 File Storage Engine (P3)

Phase 6 — Enterprise

├── 6.1 Integration Engine (P3)

└── 6.2 AI Engine (P4 — Future)


---

## 13. Golden Rules

**Rule 1** — No Duplicate Engines. Every shared capability has one engine.

**Rule 2** — No Duplicate Notification Systems. The Notification Engine is the only way.

**Rule 3** — No Duplicate Comments. The Comment Engine is the only discussion system.

**Rule 4** — No Duplicate Activity Logs. The Activity Engine is the only timeline.

**Rule 5** — No Duplicate Approvals. The Approval Engine is the only approval system.

**Rule 6** — No Module-Specific Search. The Search Engine is the only search.

**Rule 7** — Everything Permission-Aware. Every API checks permissions.

**Rule 8** — Everything Audit-Ready. Every data change passes through Audit Engine.

**Rule 9** — Everything API-First. No module accesses another module's database directly.

**Rule 10** — Everything Multi-Tenant Ready. Every engine scopes data by organization.

**Rule 11** — Engines Own Their Data. Each engine is the single source of truth.

**Rule 12** — Engines Don't Know About Modules. An engine never imports a module.

**Rule 13** — Events Before Actions. Engines emit events. Modules subscribe.

**Rule 14** — Configuration Before Customization. Every behavior configurable through Master Data.

**Rule 15** — Backend Enforces Everything. Frontend is never trusted for security.

---

## Architecture Notes

### Key Design Decisions

1. **Convex as Runtime**: All engines run on Convex as mutations, queries, and actions
2. **Polymorphic Entity References**: `(entityType, entityId)` pairs for single-engine multi-module support
3. **Eventual Consistency**: Non-critical paths use async patterns. Critical paths (approvals, sequences) are synchronous
4. **No Engine-to-Engine Imports**: Engines communicate through public APIs and events

### Recommendation

Implement **Sequence Engine** and **Activity Engine** first — zero external dependencies, consumed by every module.

---

## Future Improvements

1. Engine Health Dashboard
2. Visual Workflow Builder
3. AI-Assisted Engine Configuration
4. Engine Versioning
5. Engine Marketplace
6. Event Sourcing for critical engines
7. Webhook Event Delivery
8. Rate Limiting per Engine
9. Engine Documentation Generator
10. Load Testing Framework
