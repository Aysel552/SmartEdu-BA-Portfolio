### Functional Requirements

#### Administrator

| ID | Requirement name | Functional description | Priority |
| --- | --- | --- | --- |
| FR-ADM-01 | Autentifikasiya | The system must authenticate administrators with the ADMIN role. | High |
| FR-ADM-02 | User management | The system must allow administrators to create, edit, and deactivate STUDENT, TEACHER, and ADMIN accounts. | High |
| FR-ADM-03 | Role assignment | Administrators must be able to assign and change user roles. | High |
| FR-ADM-04 | Teacher assignment | Administrators must be able to create and revoke teacher assignments to specific course instances/sessions. | High |
| FR-ADM-05 | Course management | Administrators must be able to create, edit, delete, or CANCEL courses. | High |
| FR-ADM-06 | Course status | Administrators must be able to manage course instance status. | High |
| FR-ADM-07 | Capacity control | Administrators must not reduce course instance capacity below the current active enrollment count. | Medium |
| FR-ADM-08 | Session management | Administrators must be able to create/edit course instance sessions (date, time, teacher). | High |
| FR-ADM-09 | Prerequisite configuration | Administrators must be able to define course prerequisites. | Medium |
| FR-ADM-10 | Cancellation impact | When a course instance/session is cancelled, the system must identify affected active enrollments and show the administrator an impact list. | Medium |
| FR-ADM-11 | Enrollment search | Administrators must be able to search/filter all enrollments by course, student, and status. | High |
| FR-ADM-12 | Manual enrollment | Administrators must be able to create manual enrollments on students' behalf. | Medium |
| FR-ADM-13 | Administrative cancellation | Administrators must be able to cancel enrollments after the deadline. | Medium |
| FR-ADM-14 | Operation audit | Manual enrollment/cancellation must store the operation date and reason in audit fields. | High |
| FR-ADM-15 | Result correction approval | Administrators must be able to approve corrections to teacher-confirmed assessment results. | Medium |
| FR-ADM-16 | Eligibility list | The system must list certificate eligibility for all students to administrators. | High |
| FR-ADM-17 | Avtomatik eligibility | Eligibility must be calculated automatically from attendance percentage and pass score. | High |
| FR-ADM-18 | Bulk certification | Administrators must be able to bulk-generate e-certificates for ELIGIBLE students. | Medium |
| FR-ADM-19 | Verification code | A unique verification code must be generated for each certificate. | High |
| FR-ADM-20 | Certificate revocation | Administrators must be able to REVOKE ISSUED certificates with a reason. | Medium |
| FR-ADM-21 | Course report | Course instance reports must show enrollment count, occupancy percentage, average attendance, and pass rate. | Medium |
| FR-ADM-22 | Student report | Student reports must show active enrollments, completed courses, and received certificates. | Medium |
| FR-ADM-23 | Pending records | Missing teacher attendance/results (PENDING) must appear on the administrator dashboard. | Medium |
| FR-ADM-24 | Report export | Reports must support date range, course, and status filters and CSV/Excel export. | Low |
| FR-ADM-25 | Audit log search | The system must log all critical operations and allow administrator searches by user, object, and date. | High |
| FR-ADM-26 | Eligibility restriction | Administrators must not manually bypass eligibility rules for individual students. | High |
| FR-ADM-27 | Result restriction | Administrators must not enter attendance/assessment on teachers' behalf; they may approve corrections only. | High |
| FR-ADM-28 | Audit integrity | Administrators must not enter attendance/assessment on teachers' behalf. | High |

#### Teacher

| ID | Requirement name | Functional description | Priority |
| --- | --- | --- | --- |
| FR-TCH-01 | Autentifikasiya | The system must authenticate TEACHER users and return 401 Unauthorized on failure. | High |
| FR-TCH-02 | Access rights | Teachers may access only assigned course instances/sessions; otherwise return 403 Forbidden. | High |
| FR-TCH-03 | Course list | Teachers must see assigned current/past instances/sessions (name, date, status, enrolled student count). | High |
| FR-TCH-04 | Student list | Teachers must see only actively enrolled students for the selected session. | High |
| FR-TCH-05 | Attendance recording | Teachers must be able to enter PRESENT / ABSENT / LATE attendance for each student. | High |
| FR-TCH-06 | Correction and audit | Teachers may edit attendance within the defined period; changes must store updated_by, updated_at, and the previous value in audit fields. | Medium |
| FR-TCH-07 | Cancelled session | Teachers must be able to mark undelivered sessions CANCELLED; such sessions must be excluded from attendance percentage calculation. | Medium |
| FR-TCH-08 | Result entry | Teachers must be able to enter each student's assessment score for assigned course instances. | High |
| FR-TCH-09 | Score validation | Scores must be checked against the course's defined range; out-of-range values return 400 Bad Request. | High |
| FR-TCH-10 | Automatic result | The system must automatically assign PASSED/FAILED by comparing score with pass score and prohibit manual status changes. | High |
| FR-TCH-11 | Assessment workflow statusu | Assessment workflow must distinguish DRAFT and SUBMITTED; students see only SUBMITTED results. | Medium |
| FR-TCH-12 | Result correction | Teachers must not change confirmed results without administrator approval. | Medium |
| FR-TCH-13 | Pending records | Missing attendance/result rows must display PENDING in the UI. | Medium |
| FR-TCH-14 | Certificate restriction | Teachers must not generate/revoke certificates or change eligibility. | High |
| FR-TCH-15 | Enrollment restriction | Teachers must not create/cancel student enrollments or change course capacity. | High |
| FR-TCH-16 | Data restriction | Teachers must not access unassigned students' profiles or other course results. | High |

#### Student

| ID | Requirement name | Functional description | Priority |
| --- | --- | --- | --- |
| FR-STU-01 | Autentifikasiya | The system must authenticate STUDENT users and return 401 Unauthorized on failure. | High |
| FR-STU-02 | Data isolation | Students see only their own enrollment, attendance, result, and certificate data; access to another student's resource returns 403 Forbidden. | High |
| FR-STU-03 | Personal profile | Students must see their personal profile (name, contact information). | Medium |
| FR-STU-04 | Course catalog | Students must see only OPEN course instances in the catalog. | High |
| FR-STU-05 | Course details | Students must see remaining seats, start date, teacher, and prerequisites. | Medium |
| FR-STU-06 | Search and filters | Students must be able to search/filter visible instances by name and date. | Low |
| FR-STU-07 | Prerequisite display | Students who do not meet prerequisites may see the instance, but enrollment must be disabled with an explanation. | Medium |
| FR-STU-08 | Self-service enrollment | Students must be able to self-enroll in a selected course instance. | High |
| FR-STU-09 | Capacity check | Capacity must be checked automatically before enrollment; no seats returns 409 Conflict. | High |
| FR-STU-10 | Prerequisite check | The prerequisite course's PASSED result must be checked automatically; unmet conditions reject enrollment. | High |
| FR-STU-11 | Duplicate enrollment control | A second active enrollment for the same student/instance must be prohibited with 409 Conflict. | High |
| FR-STU-12 | Course status control | Enrollment must be rejected unless the instance is OPEN (DRAFT, CLOSED, CANCELLED, COMPLETED are rejected). | High |
| FR-STU-13 | Enrollment status | Successful enrollment status must immediately appear in the student's profile. | High |
| FR-STU-14 | Enrollment notification | Students must receive enrollment outcome notifications (success or rejection reason). | Medium |
| FR-STU-15 | Self-service cancellation | Students may cancel when the time remaining before instance start exceeds the defined period. | Medium |
| FR-STU-16 | Cancellation deadline control | After the deadline, self-service cancellation must be blocked and the student directed to the administrator. | Medium |
| FR-STU-17 | Seat restoration | Cancellation must automatically update available seats and store CANCELLED status with audit fields. | High |
| FR-STU-18 | Attendance tracking | Students must see attendance records (session, date, status) and overall percentage for each instance. | High |
| FR-STU-19 | Result tracking | Students see only teacher-confirmed SUBMITTED results; DRAFT results remain hidden. | High |
| FR-STU-20 | Vahid dashboard | Enrollment, attendance, result, eligibility, and certificate status must appear in one dashboard. | High |
| FR-STU-21 | Eligibility statusu | Students must see why they are ineligible (attendance or pass score). | High |
| FR-STU-22 | Certificate download | When attendance and pass score requirements are met, the system must generate a downloadable e-certificate. | High |
| FR-STU-23 | Verification code | Each certificate must have a unique verification code displayed on it. | High |
| FR-STU-24 | Certificate notification | Students must be notified when the certificate is ready. | Low |

## User Stories and Acceptance Criteria

## Epic Structure

### User Stories

#### EP-01 — Course and Course Instance management

#### EP-02 — Course catalog and enrollment

#### EP-03 — Enrollment management

#### EP-04 — Attendance and assessment

#### EP-05 — Student tracking experience

#### EP-06 — Certificate

#### EP-07 — Users, access, and reporting

### Acceptance Criteria

#### Course and Course Instance Management

Table 29. Course and Course Instance Management

#### Enrollment and Cancellation

#### Attendance and Assessment

#### Student Tracking and Certificate

#### Authorization and Reporting

#### Profile, Audit, and Certificate Operations

## Business Rules / Validations / Edge Cases

### Business Rules Catalog

#### Course and Course Instance configuration

#### Enrollment

#### Enrollment cancellation

#### Attendance and Assesment

#### Certification

#### Access Rights and Data Boundaries

### Validation Catalog

#### Course Validations

#### Session Validations

#### User Validations

#### Enrollment Validations

#### Attendance Validations

#### Assessment Validations

#### Certificate Validations

### Edge Case Catalog
