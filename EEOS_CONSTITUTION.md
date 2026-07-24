# **EEOS\_CONSTITUTION.md**

Version: 1.0

Status: NON-NEGOTIABLE

Owner: CTO

---

# **Article 1 — Platform First**

EEOS is an Enterprise Operating System.

It is NOT

* School ERP  
* CRM  
* LMS  
* HRMS

Those are business domains built on the platform.

Platform services always take precedence over business modules.

---

# **Article 2 — Business Engines**

No business logic inside

Pages

Components

API Routes

Controllers

Everything belongs inside Business Engines.

Architecture

UI

↓

Business Engine

↓

Platform Services

↓

Database

---

# **Article 3 — Single Source of Truth**

Every domain owns only its own data.

Examples

Identity

↓

People Registry

Academic

↓

Academic Engine

Finance

↓

Finance Engine

Attendance

↓

Attendance Engine

No duplicated data.

---

# **Article 4 — Identity**

Every human is represented by one Person.

One person may have

Employee

Student

Faculty

Parent

Vendor

Lead

Visitor

Guardian

Applicant

Profiles.

Never duplicate identities.

---

# **Article 5 — Organization**

Organization hierarchy is independent from Academic hierarchy.

Never mix them.

Governance

↓

Company

↓

Branch

↓

Department

↓

Team

Academic

↓

Program

↓

Course

↓

Batch

↓

Subject

---

# **Article 6 — Configuration**

Never hardcode

Statuses

Roles

Departments

Stages

Permissions

Hierarchy

Dashboards

Forms

Menus

Approval Chains

Everything configurable.

---

# **Article 7 — Event First**

Every important action creates an event.

Example

Student Enrolled

Employee Joined

Lead Assigned

Fee Paid

Attendance Marked

Workflow Approved

Events power

Timeline

Notifications

Automation

Analytics

AI

Audit

---

# **Article 8 — Timeline**

Every entity owns an immutable timeline.

Timeline is never deleted.

---

# **Article 9 — Workflows**

No module directly changes critical state.

Everything goes through Workflow Engine when approval is required.

---

# **Article 10 — Visibility**

Every query must pass through

Visibility Engine

Never expose records directly.

Security Layers

Record

↓

Section

↓

Field

↓

Action

↓

Audit

---

# **Article 11 — Communication**

No module sends

Email

WhatsApp

SMS

Push

directly.

Everything uses Communication Hub.

---

# **Article 12 — Documents**

Documents never belong only to modules.

Documents are platform resources.

Can link to

Person

Student

Employee

Lead

Invoice

Task

Meeting

Project

Workflow

---

# **Article 13 — Search**

Every business object is searchable.

Every search respects permissions.

---

# **Article 14 — Metadata**

Every entity contains

ID

Created By

Updated By

Timeline

Tags

Status

Visibility

Organization Context

Owner

Version

AI Metadata

---

# **Article 15 — AI Readiness**

Every engine exposes capabilities.

Example

Attendance

markAttendance()

Finance

collectPayment()

CRM

assignLead()

AI consumes capabilities.

Never UI.

Never database.

---

# **Article 16 — Extensibility**

Customers must extend the platform

without modifying core code.

Support

Custom Modules

Custom Fields

Custom Workflows

Custom Dashboards

Custom Reports

Custom AI Agents

---

# **Article 17 — APIs**

Every engine exposes

Internal API

Public API

Webhook Events

Capability Registry

---

# **Article 18 — Analytics**

Analytics never calculates directly from UI.

Analytics consumes platform events.

---

# **Article 19 — Performance**

Support

Lazy Loading

Pagination

Caching

Background Jobs

Streaming

Event Queue

Horizontal Scaling

---

# **Article 20 — Enterprise Scale**

Every feature must support

Multiple Companies

Multiple Branches

Multiple Departments

Multiple Academic Verticals

Multiple Countries

Multiple Time Zones

Multiple Languages

without redesign.

---

# **Article 21 — AI Constitution**

Future AI Agents must

Understand

Reason

Recommend

Automate

Execute

only through

Permission Engine

Business Engines

Workflow Engine

Communication Hub

Never bypass business rules.

---

# **Article 22 — CTO Review Checklist**

Before approving any feature ask

Does it duplicate existing capability?

Can another module reuse it?

Should this become a Platform Service?

Can AI consume it?

Can it scale to thousands of branches?

Does it violate Single Source of Truth?

Is it configurable?

Is it event-driven?

Does it produce timeline events?

Does it respect visibility rules?

If any answer is NO

Reject the design and redesign before implementation.

---

# **Final Principle**

EEOS is not software.

EEOS is a Digital Enterprise Operating System.

Every feature must strengthen the Platform.

Never build shortcuts that make one module easier but weaken the architecture.

Protect the Platform above all else.

