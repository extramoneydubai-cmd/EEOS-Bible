DOC-99 — EEOS Core Business Object Model (CBOM)

Version: 1.0

Priority: ⭐⭐⭐⭐⭐ (Foundation)

Status: Mandatory Architecture

Purpose

The Core Business Object Model (CBOM) defines the universal entities of EEOS.

Every engine, studio, module, workflow, API, automation, report, notification, dashboard, approval, AI feature, and integration MUST use these objects.

No engine should create duplicate concepts.

Example:

❌ Student Module creates "Student"

❌ CRM creates "Lead"

❌ HR creates "Employee"

Instead

✅ Person Object

with different lifecycle states.

EEOS Philosophy

EEOS is NOT a collection of independent modules.

It is ONE operating system where every module talks to every other module using the same objects.

CRM
 │
Sales
 │
Admissions
 │
Academics
 │
Finance
 │
HR
 │
Production
 │
Inventory
 │
Administration
 │
Analytics

↓

Same Business Objects
Universal Business Objects

EEOS contains only 10 primary business objects.

Everything else extends these.

1. PERSON

The most important object.

Never create separate models for:

Lead
Student
Parent
Employee
Teacher
Vendor Contact
Guest
Alumni

Everything is a Person.

Person

↓

Type

Lead
Student
Parent
Employee
Faculty
Vendor Contact
Visitor
Alumni

Common fields

Name
Mobile
Email
Gender
DOB
Address
Documents
Tags
Notes
Timeline

Each engine extends Person.

CRM

adds

Lead Source
Campaign
Stage

Admissions

adds

Enrollment

HR

adds

Salary
Designation

Faculty

adds

Subjects
Timetable
Person Lifecycle
Visitor

↓

Lead

↓

Prospect

↓

Demo Student

↓

Student

↓

Alumni

or

Candidate

↓

Employee

↓

Manager

↓

Department Head

↓

Exited
2. ORGANIZATION

Defines company hierarchy.

Organization

↓

Company

↓

Branch

↓

Department

↓

Team

↓

Designation

↓

User

Never hardcode.

Everything dynamic.

3. RESOURCE

Anything that can be reserved.

Examples

Physical

Classroom
Lab
Meeting Room
Studio
Library

Virtual

Zoom
Google Meet
Teams

Equipment

Camera
Laptop
Projector
Printer

Vehicle

Furniture

Any engine needing booking uses Resource.

4. EVENT

Every scheduled activity.

Types

Lecture
Meeting
Demo
Video Shoot
Parent Meeting
Interview
Webinar
Workshop
Holiday
Exam
Internal Discussion

Common fields

Start

End

Venue

Organizer

Participants

Resources

Status

Event Example
Demo Lecture

↓

Person

Lead

↓

Faculty

↓

Classroom

↓

Attendance

↓

Feedback

↓

Conversion
5. TASK

Universal task object.

Used by

CRM

Sales

HR

Admin

Production

Procurement

Finance

Task contains

Owner

Assignee

Priority

Status

Due Date

Comments

Attachments

Checklist

Approvals

Dependencies

6. DOCUMENT

Every file.

Examples

Invoice

Receipt

Agreement

Certificate

Salary Slip

ID Card

PDC Scan

GST Bill

Video Script

Question Paper

Images

Everything uses same Document Engine.

7. TRANSACTION

Universal financial object.

Includes

Fee

Payment

Refund

Expense

Salary

Vendor Payment

Purchase

Collection

GST

PDC

Ledger Entry

Cash Entry

Bank Entry

No module creates separate payment systems.

Transaction Lifecycle
Invoice

↓

Payment

↓

Receipt

↓

Ledger

↓

GST

↓

Reports
8. INVENTORY ITEM

Everything stored.

Consumable

Books

Stationery

Uniform

Assets

Laptop

Camera

Vehicle

Furniture

Electronics

Linked with

Procurement

Assets

Inventory

Production

9. COMMUNICATION

Universal communication object.

Email

SMS

WhatsApp

Push

Notification

Call

Meeting Invite

Every message stored.

Timeline uses same object.

10. WORKFLOW OBJECT

Anything requiring approval.

Admission

Expense

Purchase

Leave

Quotation

Salary

Production

Content

Vendor

Student Request

Everything follows

Draft

↓

Submitted

↓

Approved

↓

Rejected

↓

Completed
Relationships
Person

↓

Organization

↓

Task

↓

Event

↓

Resource

↓

Document

↓

Transaction

↓

Workflow

↓

Communication

Everything connects.

Engine Dependency Map

CRM

Uses

Person

Task

Communication

Event

Document

Sales

Uses

Person

Transaction

Workflow

Task

Document

Admissions

Uses

Person

Event

Transaction

Workflow

Document

Academics

Uses

Person

Event

Resource

Task

HR

Uses

Person

Organization

Task

Event

Transaction

Production

Uses

Task

Resource

Event

Document

Inventory

Uses

Inventory Item

Transaction

Workflow

Administration

Uses

Resource

Event

Task

Finance

Uses

Transaction

Workflow

Document

Communication

Uses

Person

Communication

Event

Golden Rules
Rule 1

Never duplicate objects.

Rule 2

Every screen edits a business object.

Never random tables.

Rule 3

Everything has Timeline.

Rule 4

Everything has Attachments.

Rule 5

Everything has Comments.

Rule 6

Everything has Audit Log.

Rule 7

Everything has Notifications.

Rule 8

Everything follows Organization Scope.

Company

↓

Branch

↓

Department

↓

Team

↓

User

Rule 9

Everything is API First.

Every business object exposes

Create
Read
Update
Delete
Timeline
Audit
Comments
Attachments
Notifications
Search
Rule 10

Every future engine MUST extend these objects.

No new engine may invent its own versions of:

User
Student
Employee
Vendor
Payment
Document
Task
Event
Resource
Communication

It must extend the Core Business Object Model.

Final Principle

EEOS is not a CRM, ERP, LMS, HRMS, or Finance system. EEOS is an Enterprise Operating System where every business function is built on a single shared business object model. Modules are merely different views and workflows over the same core entities.

