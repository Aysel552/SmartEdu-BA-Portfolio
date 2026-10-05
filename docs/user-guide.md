SmartEdu

Digital Course & Certification Management Platform

USER GUIDE

(User Guide / User Manual)

| Parameter | Information |
| --- | --- |
| Document title | SmartEdu User Guide |
| Version | 1.0 |
| Date | 12 August 2026 |
| Status | Final |
| Scope | Student, Teacher, and Administrator roles |
| Related documents | SmartEdu Business Requirements Document (BRD) v1.0; SmartEdu Data Model & API Design / technical documents |
| Confidentiality | Internal use only |

This guide explains step by step how to use SmartEdu as a Student, Teacher, or Administrator.

## Contents

## 1. Introduction

SmartEdu is a digital course/certificate management platform for training centers. It centralizes course creation, student enrollment, attendance, assessment results, and electronic certification.

This guide explains daily use step by step for Students, Teachers, and Administrators. It also covers public certificate verification and common errors.

### 1.1 Document purpose

Provide clear, simple, step-by-step instructions for:

Logging in;

Using functions for your role;

Enrolling in or cancelling courses;

Entering attendance/assessment data (Teachers);

Obtaining and verifying certificates;

Understanding common errors and solutions.

### 1.2 Main system roles

| Role | Description |
| --- | --- |
| STUDENT | Enrolls in courses and tracks own attendance, results, and certificate status. |
| TEACHER | Enters attendance/assessment only for assigned courses/sessions. |
| ADMIN (Administrator) | Centrally manages courses, users, enrollments, and certificates; views reports. |

### 1.3 Example users and courses

These example names clarify instructions. In actual use, each user logs in with their own account.

| Role | Example user |
| --- | --- |
| Teacher | Demo Teacher 1 |
| Teacher | Demo Teacher 2 |
| Student | Demo Student 1 |
| Student | Demo Student 2 |

| Course name | Note |
| --- | --- |
| Basic course | Introductory course — no prerequisite. |
| Course with a prerequisite | Advanced course — requires PASSED completion of “Basic course”. |

Note: The BRD specifies no demo names. These examples explain this guide only and are neither requirements nor production data.

## 2. Login

Protected resources require authentication. Successful authentication returns access/refresh tokens; role and ownership rules restrict data and operations.

### 2.1 Login steps

Open SmartEdu's login page in your browser.

Enter your registered email in “Email”.

Enter your password in “Password”.

Click “Login”.

After login, use protected resources permitted by your role and ownership.

#### Example

Demo Teacher 1 logs in with email/password and is directed to the Teacher Panel, showing only assigned sessions.

Important: Invalid credentials or missing JWT returns 401 Unauthorized. DISABLED accounts have restricted protected operations; contact the Administrator.

### 2.2 Password and account security

Never share your password.

Log out after using the system.

Passwords must not be stored in plain text; use secure hashing. RBAC/RLS protects access.

Invalid/expired JWT rejects protected requests with 401 Unauthorized and requires reauthentication.

Exception: Public Certificate Verification requires no JWT for ISSUED Certificates.

## 3. Student Guide

This section explains use for Students such as Demo Student 1/2.

### 3.1 View the course catalog

After login, “Courses” shows available OPEN courses with vacant seats. Each course must show:

Name and short description;

Start date;

Available seats;

Assigned teacher;

Prerequisite, if any.

Search/filter by name and date.

#### Example

Demo Student 2 sees “Basic course” as OPEN and may enroll directly because no prerequisite exists.

“Course with a prerequisite” appears in the catalog, but enrollment is enabled only after PASSED completion of “Basic course”. Otherwise the button is disabled with an explanation.

BRD alignment note: Available seats are required; BRD UAT-02 records available_seats as GAP-10, requiring a separate SQL/view for the current API.

### 3.2 Enroll in a course

Choose a suitable course in “Courses” (e.g., Basic course).

Check start date, teacher, and available seats in course details.

Click “Enroll”.

The system checks registration window, OPEN Course status, instance capacity, prerequisites, and absence of a second ACTIVE enrollment in the same instance.

If all conditions are met, ACTIVE enrollment is created and a system notification should show the outcome; failed attempts should show rejection reasons.

Note: Students may enroll in several different courses simultaneously. Multiple ACTIVE enrollments in one instance are prohibited; CANCELLED/COMPLETED are excluded from this duplicate check.

#### Reasons enrollment may fail

| Cause | System response |
| --- | --- |
| No vacant seats (capacity full) | Enrollment rejected; 409 Conflict appears. |
| Prerequisite unmet | Enrollment button disabled/request rejected; no enrollment created. |
| An active enrollment already exists for the course | 409 Conflict; no second ACTIVE enrollment. |
| Course is not OPEN (DRAFT / CLOSED / CANCELLED / COMPLETED / ARCHIVED) | Enrollment rejected; no new ACTIVE enrollment. |
| Outside the registration window | No enrollment; registration-window control applies. |

### 3.3 Cancel enrollment

Students may self-cancel ACTIVE enrollment sufficiently before instance start, within the defined cutoff.

Open “My Enrollments”.

Select the course to cancel.

Click “Cancel”.

The system checks the cutoff.

If within the permitted period, status becomes CANCELLED, cancelled_at/audit is saved, and the freed seat becomes available as ACTIVE count decreases.

Important: After the cutoff, students cannot self-cancel; contact the Administrator.

### 3.4 Track attendance and results

The personal Dashboard provides real-time tracking of:

Profile: name, contact information, completed courses;

Course enrollment status (ACTIVE / CANCELLED / COMPLETED);

Session attendance (PRESENT / ABSENT / EXCUSED) and overall percentage;

Teacher-confirmed SUBMITTED results (score, PASSED/FAILED);

Eligibility (NOT_ELIGIBLE / ELIGIBLE) and its reason.

Note: The BRD requires students to see SUBMITTED results only, with DRAFT hidden. UAT-07 records this workflow as current-API GAP-02.

Data boundary: Students access own enrollment, attendance, assessment, and certificate information only; RLS/authorization rejects others' resources.

#### Example

Demo Student 1 views Basic course attendance and the score entered by Demo Teacher 2 (e.g., 87.50 — PASSED).

### 3.5 Obtain and download certificates

The system automatically evaluates eligibility when required data exists and:

Enrollment is COMPLETED;

Required attendance and Assessment Result exist;

Attendance is at least min_attendance_percent;

Assessment is PASSED.

When these conditions are met:

Certificate becomes ELIGIBLE.

Administrators may mark ELIGIBLE Certificates ISSUED; the system generates unique verification_code values and blocks duplicates.

Students view certificates in “My Certificates” and download PDFs when available.

A system notification must announce readiness. Whether email is used and for which processes remains open under BRD Q5.

#### Example

Demo Student 2 completes Basic course with sufficient attendance and 87.50 (PASSED because pass_score=70). Eligibility becomes ELIGIBLE, then ISSUED after administrator confirmation, with a unique code (e.g., SE-2026-BC67F118).

Note: Changed attendance/assessment requires reassessment; unmet conditions may cause REVOKED. PDF storage remains open under BRD Q2.

## 4. Teacher Guide

This section explains use for Demo Teacher 1/2. Teachers operate only on assigned Course Instances.

### 4.1 View assigned courses

“My Courses” shows assigned current/past sessions: name, date, status, and enrolled count.

#### Example

If Demo Teacher 1 is assigned Basic course, only that course/sessions appear. Demo Teacher 2 assigned Course with a prerequisite sees only that course.

Important: Unassigned course/session operations are prohibited and return 403 Forbidden.

### 4.2 Enter attendance

Select a session in “My Courses”.

Select session_date.

Only ACTIVE enrolled students appear.

Select PRESENT, ABSENT, or EXCUSED for each student.

Click “Save” to confirm.

Note: PRESENT and EXCUSED count as attendance; ABSENT does not.

BRD alignment note: FR-TCH-05/Glossary also mention LATE, but VAL-032, TC-15, UAT-06 use PRESENT / ABSENT / EXCUSED; LATE is tested as invalid. These steps follow current validation.

#### Example

Demo Teacher 2 records Demo Student 1 as PRESENT in Course with a prerequisite. The system saves recorded_by (Demo Teacher 2) and recorded_at (record date).

Important: No future-date attendance or second record for the same student/session date.

Teachers may correct attendance within the edit period; store updated_by, updated_at, and previous value in audit data.

Undelivered sessions must be CANCELLED and excluded from attendance percentage.

### 4.3 Enter assessment results

Select a student in the relevant session.

Enter assessment score within the course's permitted range.

Out-of-range scores must return 400 Bad Request and not be saved.

Click “Save”.

The system compares score with pass_score and automatically assigns PASSED/FAILED.

Note: At/above pass_score means PASSED; below means FAILED. Teachers cannot change status manually.

The BRD requires DRAFT/SUBMITTED workflows with SUBMITTED only visible to students. UAT-07 records GAP-02 for this workflow.

Missing attendance/assessment rows must show PENDING in the UI.

#### Example

Demo Teacher 1 enters 87.50 for Demo Student 2 in Basic course. pass_score is 70, so PASSED is automatic.

### 4.4 View students and results

Teachers may view/operate on attendance/assessments only for assigned instances, not unassigned students' profiles or other course results.

### 4.5 Teacher restrictions

Teachers cannot create/cancel student enrollments.

Teachers cannot change capacity.

Teachers cannot change Course status; unauthorized attempts return 403 Forbidden.

Teachers cannot generate/revoke certificates or change eligibility; calculation is automatic.

Changing SUBMITTED results requires administrator approval and audit logging of the previous value.

## 5. Administrator Guide

Administrators centrally manage courses, users, enrollments, certificates, and reports.

### 5.1 Create and manage courses

Choose “New Course” in “Courses”, or open an existing Course to edit.

Enter name, description, assessment range, pass_score, and min_attendance_percent.

Define prerequisites if needed (e.g., Basic course for Course with a prerequisite).

OPEN requires pass_score, min_attendance_percent, and prerequisite configuration. BRD lifecycle: DRAFT → OPEN → CLOSED → COMPLETED; CANCELLED where applicable, ARCHIVED for soft deletion.

BRD alignment note: Lifecycle/FR-ADM-06 include COMPLETED/CANCELLED, but VAL-006 lists DRAFT / OPEN / CLOSED / ARCHIVED. This internal discrepancy needs harmonization; the lifecycle above represents target functional requirements.

#### Example

The Administrator selects Basic course's identifier as prerequisite_course_id for Course with a prerequisite. Only students who PASSED Basic course (e.g., Demo Student 2) can enroll.

#### Create a Course Instance

Select a course and add “New Course Instance”.

Enter mandatory start_date/end_date; end_date cannot precede start_date.

Set capacity. Reducing it below ACTIVE enrollment count must return 409 Conflict.

Assign an ACTIVE TEACHER user (e.g., Demo Teacher 1/2).

Administrators must create/edit sessions with date, time, and teacher. Lifecycle: SCHEDULED → ONGOING → COMPLETED; CANCELLED is possible at any stage.

BRD alignment note: UAT-01 identifies current-API GAP-09 for Course creation/session endpoints. This section describes target behavior.

### 5.2 Deactivate courses (soft delete)

Courses are not physically deleted. Administrators may mark them ARCHIVED. New enrollment is prohibited while historical data remains.

### 5.3 User management

Create/edit STUDENT, TEACHER, ADMIN accounts;

Assign/change roles (audit every change);

Manage ACTIVE/DISABLED status;

Assign/revoke teachers for courses/sessions.

No new enrollment for DISABLED Students or assignments for DISABLED Teachers; retain history.

Important: Administrators cannot deactivate their own account or change their own role (BR-028).

### 5.4 Enrollment management

Administrators can search/filter all enrollments by course, student, and status. They may also:

Create manual enrollment for students, still enforcing capacity, prerequisites, and general enrollment rules;

Cancel administratively after cutoff, with mandatory reason;

Store acting Administrator, date, and required reason in audit fields for manual enrollment/administrative cancellation;

View affected active enrollments when courses/sessions are cancelled.

#### Example

The Administrator may cancel Demo Student 1's Basic course enrollment after cutoff with a reason (e.g., “student's personal request”).

### 5.5 Certificate management

Review ELIGIBLE students in the Eligibility list.

Mark certificates ISSUED individually or in bulk.

The system automatically generates unique verification_code values.

Only Administrators may REVOKE ISSUED Certificates with reasons; audit both reason and operation.

Note: Eligibility is automatic and must recalculate after confirmed attendance/assessment changes. Administrators cannot bypass rules for individuals, only configure course thresholds. Unmet conditions may REVOKE ISSUED Certificates.

BRD alignment note: UAT-15 retains current-API endpoint/trigger gaps GAP-03/GAP-04 for REVOKED and reassessment.

### 5.6 Reports

Administrators may access these real-time reports:

Course: enrollment count, occupancy, average attendance, pass rate;

Student: active enrollments, completed courses, certificates;

PENDING: missing teacher attendance/results;

Administrator Course/result/Certificate report view;

Audit searches by user, object, and date.

Filter reports by date range/course/status and export CSV/Excel.

### 5.7 Administrator restrictions

No attendance/assessment entry on teachers' behalf; correction approval only;

No individual eligibility bypass; course threshold configuration only;

No audit deletion/editing.

## 6. Public Certificate Verification

Every e-certificate has a unique verification code. Third parties such as employers may verify authenticity without login/JWT.

Note the certificate's code.

Open “Certificate Verification”.

Enter the code and click “Verify”.

Only ISSUED information appears; otherwise nothing is disclosed.

#### Example

An unauthenticated third-party request for an ISSUED code returns only fields allowed by public policy. Nonexistent codes return empty/no data.

Note: REVOKED and ELIGIBLE certificates are hidden publicly; only ISSUED appears.

## 7. Common Errors

The table lists common errors, causes, and recommended solutions.

| Error / Notification | Cause | What to do |
| --- | --- | --- |
| 401 Unauthorized | Invalid/missing credentials/JWT or expired token. | Check email/password or log in again. |
| 403 Forbidden | The data/operation is outside your role/ownership. | Use only your resources; contact the Administrator if needed. |
| 409 Conflict — capacity | No vacant seats in the selected instance. | Choose another available instance or wait for a seat. |
| 409 Conflict — duplicate ACTIVE enrollment | You already have an active enrollment in this session. | Check “My Enrollments”. |
| Enrollment button disabled | No PASSED prerequisite result. | Complete the prerequisite successfully first. |
| Attendance/result cannot be entered | Unassigned instance, future date, invalid status, or duplicate attendance. | Use assigned instances, current/past dates, and accepted attendance enum values. |
| Certificate not visible | Enrollment not COMPLETED, incomplete attendance/assessment, or unmet eligibility. | Check Dashboard eligibility status/reason. |
| 400 Bad Request — assessment score | Score outside the course range. | Enter a score within the defined range. |
| Enrollment outside registration window | Enrollment is not within the permitted window. | Retry while registration is open. |
| DISABLED account | New DISABLED Student enrollments and DISABLED Teacher assignments are restricted. | Contact the Administrator. |
| Public verification returns no result | Code nonexistent or Certificate not ISSUED. | Check the code; only ISSUED Certificates are public. |

## 8. Frequently Asked Questions (FAQ)

#### Q: Can I enroll in several courses simultaneously?

A: Yes, several different courses, but no multiple active enrollments in the same session.

#### Q: Can I retake a course after FAILED?

A: BRD Q3 remains an open business decision. No final re-enrollment behavior is established until Product Owner approval; contact Administrator/Product Owner.

#### Q: How can I download my certificate?

A: After ISSUED, download the PDF from “My Certificates” when available.

#### Q: Will notifications always be emailed?

A: The BRD requires system notifications; email coverage is a Product Owner decision under Q5.

#### Q: Can a Teacher view unassigned courses?

A: No. Teachers access student, attendance, and assessment information only for assigned sessions.

## 9. Support and Contact

Contact your organization's SmartEdu Administrator for technical/administrative questions. Product Owner (and Admin for Q4) must finalize open cutoff, FAILED retake, public Certificate field, and email notification decisions.

— End of document —

## Source headers and footers

SmartEdu — User Guide
Page / 
