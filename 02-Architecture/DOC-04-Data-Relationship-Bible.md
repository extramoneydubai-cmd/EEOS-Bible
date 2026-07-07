# DOC-04 — EEOS Data Relationship Bible

Version: 1.0

Status: Architecture

Owner

Techno Pratham

Steve

---

# Purpose

This document defines every core entity inside EEOS
and how they relate.

No module may create duplicate relationships.

Everything must connect through this document.

---

# Core Philosophy

EEOS is NOT module-based.

EEOS is ENTITY based.

Modules only provide UI.

Entities store business truth.

---

# Core Entities

Organization

Company

Branch

Department

Team

Designation

Employee

Lead

Family

Student

Faculty

Academic Session

Board

Vertical

Sub Vertical

Program

Subject

Batch Type

Batch

Fee Plan

Invoice

Payment

Task

Approval

Notification

Document

Activity Timeline

Communication

Automation

---

# Organization Hierarchy

Organization

↓

Companies

↓

Branches

↓

Departments

↓

Teams

↓

Users

---

# CRM Hierarchy

Lead

↓

Family

↓

Counselling

↓

Admission

↓

Student

↓

Alumni

---

# Academic Hierarchy

Academic Session

↓

Board

↓

Vertical

↓

Sub Vertical

↓

Program

↓

Subject

↓

Batch Type

↓

Batch

↓

Student

---

# Finance Hierarchy

Fee Structure

↓

Invoice

↓

Collection

↓

Receipt

↓

Ledger

↓

Reports

---

# Communication Hierarchy

Notification

↓

Email

↓

SMS

↓

WhatsApp

↓

Push

↓

Activity Timeline

---

# Workflow Hierarchy

Workflow

↓

Task

↓

Approval

↓

Automation

↓

Notification

↓

Completion

---

# Every Entity Owns Its Data

Lead

owns

Lead Information

Family

owns

Parent Information

Student

owns

Academic Information

Batch

owns

Learning Schedule

Invoice

owns

Financial Records

Task

owns

Execution

---

# Never Duplicate

Wrong

Student Name inside Payments

Correct

Payment

↓

StudentID

↓

Student Table

Wrong

Parent Mobile inside Student

Correct

Student

↓

FamilyID

↓

Family Table

Wrong

Program Name inside Batch

Correct

Batch

↓

ProgramID

↓

Program Table

---

# Relationship Types

One to One

Student

↓

Current Batch

One to Many

Program

↓

Batches

Many to Many

Programs

↓

Subjects

Faculty

↓

Subjects

Students

↓

Subjects

Faculty

↓

Batches

---

# Shared Services

Activity Timeline

used by

CRM

Students

Finance

HR

Collections

Tasks

Communication

Documents

Automation

Notifications

Approval

Every module shares them.

Never create module-specific versions.

---

# Entity Ownership

CRM

Lead

Family

Counselling

Academic

Program

Batch

Subject

Finance

Invoice

Payment

Ledger

HR

Employee

Leave

Payroll

Operations

Tasks

Workflow

Approvals

Communication

Notifications

Templates

Automation

Rules

Triggers

Actions

---

# Global Timeline

Every entity produces events.

Lead Created

Call

WhatsApp

Task

Admission

Invoice

Payment

Attendance

Exam

Certificate

Complaint

Everything becomes one timeline.

---

# Global Search

Search once.

Return

Lead

Student

Parent

Employee

Invoice

Payment

Task

Document

Batch

Program

Everything searchable.

---

# AI Context

AI never queries modules.

AI queries entities.

Example

"Show unpaid JEE students"

AI

↓

Students

↓

Programs

↓

Invoices

↓

Payments

Result

---

# Golden Rules

Every entity has one owner.

Every relationship has one source.

Every module consumes entities.

No duplicate tables.

No duplicate fields.

No duplicate business logic.

Entity first.

Module second.

Always.
