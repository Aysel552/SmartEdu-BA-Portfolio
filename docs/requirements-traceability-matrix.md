# Requirements Traceability Matrix

## FR Traceability Matrix

| Business need ID | Business rule ID | FR ID | Functional Requirement Description | Prioritet | Dizayn Elementi | Test Case ID | Cari Status |
| --- | --- | --- | --- | --- | --- | --- | --- |
| BN-06 | — | FR-ADM-01 | The system must authenticate administrators with the ADMIN role. | High | Login Screen / Authentication Service | TC-33 | Covered |
| BN-06 | BR-028, BR-029 | FR-ADM-02 | The system must allow administrators to create, edit, and deactivate STUDENT, TEACHER, and ADMIN accounts. | High | User Management Page | TC-40, TC-41 | Covered |
| BN-06 | BR-028 | FR-ADM-03 | Administrators must be able to assign and change user roles. | High | Role Assignment Control | TC-40 | Covered |
| BN-01 | BR-003, BR-029 | FR-ADM-04 | Administrators must be able to create and revoke teacher assignments to specific course instances/sessions. | High | Teacher Assignment Form | TC-13, TC-41 | Partially covered |
| BN-01 | BR-005 | FR-ADM-05 | Administrators must be able to create, edit, delete, or CANCEL courses. | High | Course Management Page | TC-12 | Partially covered |
| BN-01, BN-02 | BR-004, BR-005 | FR-ADM-06 | Administrators must be able to manage course instance status. | High | Course Status Control | TC-12, TC-36 | Covered |
| BN-02 | BR-001 | FR-ADM-07 | Administrators must not reduce course instance capacity below the current active enrollment count. | Medium | Capacity Input Validation | TC-13 | Covered |
| BN-03 | BR-002, BR-003 | FR-ADM-08 | Administrators must be able to create/edit course instance sessions (date, time, teacher). | High | Session Management Form | TC-13 | Partially covered |
| BN-02 | BR-004, BR-009 | FR-ADM-09 | Administrators must be able to define course prerequisites. | Medium | Prerequisite Configuration Form | — | Planned: TC to be prepared |
| BN-01, BN-02 | — | FR-ADM-10 | When a course instance/session is cancelled, the system must identify affected active enrollments and show the administrator an impact list. | Medium | Cancellation Impact Modal | — | Planned: TC to be prepared |
| BN-01 | — | FR-ADM-11 | Administrators must be able to search/filter all enrollments by course, student, and status. | High | Enrollment Management Page / Filter Panel | — | Planned: TC to be prepared |
| BN-01, BN-02 | BR-006, BR-007, BR-008, BR-009, BR-029 | FR-ADM-12 | Administrators must be able to create manual enrollments on students' behalf. | Medium | Manual Enrollment Form | — | Planned: TC to be prepared |
| BN-01 | BR-010 | FR-ADM-13 | Administrators must be able to cancel enrollments after the deadline. | Medium | Enrollment Cancellation Dialog | TC-37 | Covered |
| BN-06 | — | FR-ADM-14 | Manual enrollment/cancellation must store the operation date and reason in audit fields. | High | Audit Metadata / Audit Trail | TC-37 | Partially covered |
| BN-03, BN-06 | BR-016, BR-017, BR-025 | FR-ADM-15 | Administrators must be able to approve corrections to teacher-confirmed assessment results. | Medium | Result Correction Approval Dialog | — | Planned: TC to be prepared |
| BN-04, BN-07 | BR-018, BR-019, BR-020, BR-021, BR-022 | FR-ADM-16 | The system must list certificate eligibility for all students to administrators. | High | Eligibility Management Table | — | Planned: TC to be prepared |
| BN-04 | BR-018, BR-019, BR-020, BR-021, BR-022, BR-025 | FR-ADM-17 | Eligibility must be calculated automatically from attendance percentage and pass score. | High | Eligibility Rules Engine | TC-25, TC-28, TC-39 | Covered |
| BN-04 | BR-022, BR-023 | FR-ADM-18 | Administrators must be able to bulk-generate e-certificates for ELIGIBLE students. | Medium | Bulk Certificate Generation Panel | — | Planned: TC to be prepared |
| BN-04 | BR-023 | FR-ADM-19 | A unique verification code must be generated for each certificate. | High | Certificate Code Generator | TC-30 | Partially covered |
| BN-04 | BR-024 | FR-ADM-20 | Administrators must be able to REVOKE ISSUED certificates with a reason. | Medium | Certificate Revocation Dialog | TC-32, TC-38 | Covered |
| BN-07 | — | FR-ADM-21 | Course instance reports must show enrollment count, occupancy percentage, average attendance, and pass rate. | Medium | Course Report View | TC-35 | Covered |
| BN-07 | — | FR-ADM-22 | Student reports must show active enrollments, completed courses, and received certificates. | Medium | Student Report View | TC-35 | Covered |
| BN-03, BN-07 | BR-019 | FR-ADM-23 | Missing teacher attendance/results (PENDING) must appear on the administrator dashboard. | Medium | Admin Dashboard / Pending Status Indicator | — | Planned: TC to be prepared |
| BN-07 | — | FR-ADM-24 | Reports must support date range, course, and status filters and CSV/Excel export. | Low | Report Filter & Export Panel | — | Planned: TC to be prepared |
| BN-06 | — | FR-ADM-25 | The system must log all critical operations and allow administrator searches by user, object, and date. | High | Audit Log Viewer / Filter Panel | — | Planned: TC to be prepared |
| BN-04, BN-06 | BR-018, BR-019, BR-020, BR-021, BR-022 | FR-ADM-26 | Administrators must not manually bypass eligibility rules for individual students. | High | Eligibility Rules Engine / Permission Control | TC-39 | Partially covered |
| BN-03, BN-06 | BR-026 | FR-ADM-27 | Administrators must not enter attendance/assessment on teachers' behalf; they may approve corrections only. | High | Authorization Control / Result Approval Workflow | — | Planned: TC to be prepared |
| BN-06 | BR-026 | FR-ADM-28 | Administrators must not enter attendance/assessment on teachers' behalf. | High | Authorization Control / Audit Trail | — | Planned: TC to be prepared |
| BN-06 | — | FR-TCH-01 | The system must authenticate TEACHER users and return 401 Unauthorized on failure. | High | Login Screen / Authentication Service | TC-33 | Covered |
| BN-06 | BR-026 | FR-TCH-02 | Teachers may access only assigned course instances/sessions; otherwise return 403 Forbidden. | High | Role-Based Access Control (RBAC/RLS) | TC-18, TC-23 | Covered |
| BN-03 | BR-026 | FR-TCH-03 | Teachers must see assigned current/past instances/sessions (name, date, status, enrolled student count). | High | Teacher Course Dashboard | — | Planned: TC to be prepared |
| BN-03 | BR-012, BR-026 | FR-TCH-04 | Teachers must see only actively enrolled students for the selected session. | High | Student Roster Table | — | Planned: TC to be prepared |
| BN-03 | BR-012, BR-013, BR-014, BR-015 | FR-TCH-05 | Teachers must be able to enter PRESENT / ABSENT / LATE attendance for each student. | High | Attendance Entry Grid | TC-14, TC-15, TC-16, TC-17 | Partially covered |
| BN-03, BN-06 | — | FR-TCH-06 | Teachers may edit attendance within the defined period; changes must store updated_by, updated_at, and the previous value in audit fields. | Medium | Attendance Edit Form / Audit Trail | — | Planned: TC to be prepared |
| BN-03 | BR-015 | FR-TCH-07 | Teachers must be able to mark undelivered sessions CANCELLED; such sessions must be excluded from attendance percentage calculation. | Medium | Session Status Control | — | Planned: TC to be prepared |
| BN-03 | BR-012, BR-016, BR-017, BR-026 | FR-TCH-08 | Teachers must be able to enter each student's assessment score for assigned course instances. | High | Assessment Entry Form | TC-19, TC-22 | Covered |
| BN-03 | — | FR-TCH-09 | Scores must be checked against the course's defined range; out-of-range values return 400 Bad Request. | High | Score Input Validation | TC-21 | Partially covered |
| BN-03 | BR-016, BR-017 | FR-TCH-10 | The system must automatically assign PASSED/FAILED by comparing score with pass score and prohibit manual status changes. | High | Result Status Indicator / Calculation Rule | TC-19, TC-20 | Covered |
| BN-03 | — | FR-TCH-11 | Assessment workflow must distinguish DRAFT and SUBMITTED; students see only SUBMITTED results. | Medium | Assessment Workflow Status Control | — | Planned: TC to be prepared |
| BN-03, BN-06 | BR-016, BR-017, BR-025 | FR-TCH-12 | Teachers must not change confirmed results without administrator approval. | Medium | Result Correction Workflow | — | Planned: TC to be prepared |
| BN-03 | BR-019 | FR-TCH-13 | Missing attendance/result rows must display PENDING in the UI. | Medium | Pending Status Indicator | — | Planned: TC to be prepared |
| BN-04, BN-06 | BR-024 | FR-TCH-14 | Teachers must not generate/revoke certificates or change eligibility. | High | Certificate Permission Control | TC-38 | Partially covered |
| BN-01, BN-06 | — | FR-TCH-15 | Teachers must not create/cancel student enrollments or change course capacity. | High | Enrollment Permission Control | — | Planned: TC to be prepared |
| BN-06 | BR-026 | FR-TCH-16 | Teachers must not access unassigned students' profiles or other course results. | High | Role-Based Access Control (RBAC/RLS) | TC-18, TC-23 | Covered |
| BN-06 | — | FR-STU-01 | The system must authenticate STUDENT users and return 401 Unauthorized on failure. | High | Login Screen / Authentication Service | TC-33 | Covered |
| BN-06 | BR-027 | FR-STU-02 | Students see only their own enrollment, attendance, result, and certificate data; access to another student's resource returns 403 Forbidden. | High | Ownership-Based Access Control (RLS) | TC-08, TC-34 | Covered |
| BN-05 | — | FR-STU-03 | Students must see their personal profile (name, contact information). | Medium | User Profile Page | — | Planned: TC to be prepared |
| BN-01 | BR-006 | FR-STU-04 | Students must see only OPEN course instances in the catalog. | High | Course Catalog | TC-01 | Covered |
| BN-01, BN-02 | BR-004, BR-009 | FR-STU-05 | Students must see remaining seats, start date, teacher, and prerequisites. | Medium | Course Detail View | TC-02 | Covered |
| BN-01 | — | FR-STU-06 | Students must be able to search/filter visible instances by name and date. | Low | Search Bar / Filter Panel | — | Planned: TC to be prepared |
| BN-02 | BR-009 | FR-STU-07 | Students who do not meet prerequisites may see the instance, but enrollment must be disabled with an explanation. | Medium | Prerequisite Status Indicator / Enrollment Button | TC-02, TC-06 | Covered |
| BN-01 | BR-006, BR-007, BR-008, BR-009 | FR-STU-08 | Students must be able to self-enroll in a selected course instance. | High | Enrollment Action Button / Confirmation Dialog | TC-03 | Covered |
| BN-02 | BR-007 | FR-STU-09 | Capacity must be checked automatically before enrollment; no seats returns 409 Conflict. | High | Capacity Validation Service / Notification Alert | TC-05 | Covered |
| BN-02 | BR-009 | FR-STU-10 | The prerequisite course's PASSED result must be checked automatically; unmet conditions reject enrollment. | High | Prerequisite Validation Service / Notification Alert | TC-06 | Covered |
| BN-02 | BR-008 | FR-STU-11 | A second active enrollment for the same student/instance must be prohibited with 409 Conflict. | High | Duplicate Enrollment Validation | TC-04 | Covered |
| BN-02 | BR-005, BR-006 | FR-STU-12 | Enrollment must be rejected unless the instance is OPEN (DRAFT, CLOSED, CANCELLED, COMPLETED are rejected). | High | Course Status Validation | TC-07 | Covered |
| BN-05 | — | FR-STU-13 | Successful enrollment status must immediately appear in the student's profile. | High | Enrollment Status Card | TC-03 | Covered |
| BN-01, BN-05 | — | FR-STU-14 | Students must receive enrollment outcome notifications (success or rejection reason). | Medium | Notification / Alert | — | Planned: TC to be prepared |
| BN-01 | BR-010, BR-011 | FR-STU-15 | Students may cancel when the time remaining before instance start exceeds the defined period. | Medium | Enrollment Cancellation Dialog | TC-09 | Covered |
| BN-01, BN-02 | BR-010 | FR-STU-16 | After the deadline, self-service cancellation must be blocked and the student directed to the administrator. | Medium | Cancellation Validation / Notification Alert | TC-10 | Covered |
| BN-02 | BR-011 | FR-STU-17 | Cancellation must automatically update available seats and store CANCELLED status with audit fields. | High | Capacity Update Service | TC-09, TC-11 | Covered |
| BN-05 | BR-015, BR-027 | FR-STU-18 | Students must see attendance records (session, date, status) and overall percentage for each instance. | High | Attendance History View | TC-24 | Covered |
| BN-05 | BR-016, BR-017, BR-027 | FR-STU-19 | Students see only teacher-confirmed SUBMITTED results; DRAFT results remain hidden. | High | Assessment Results View | — | Planned: TC to be prepared |
| BN-05 | BR-027 | FR-STU-20 | Enrollment, attendance, result, eligibility, and certificate status must appear in one dashboard. | High | Student Dashboard | TC-08 | Partially covered |
| BN-04, BN-05 | BR-018, BR-019, BR-020, BR-021, BR-022, BR-027 | FR-STU-21 | Students must see why they are ineligible (attendance or pass score). | High | Eligibility Status Card | TC-25, TC-26, TC-27, TC-28 | Covered |
| BN-04, BN-05 | BR-018, BR-019, BR-020, BR-021, BR-022, BR-023, BR-027 | FR-STU-22 | When attendance and pass score requirements are met, the system must generate a downloadable e-certificate. | High | Certificate Download Component | TC-25, TC-29 | Partially covered |
| BN-04 | BR-023 | FR-STU-23 | Each certificate must have a unique verification code displayed on it. | High | Certificate Verification Code / Verification View | TC-30, TC-31, TC-32 | Partially covered |
| BN-04, BN-05 | — | FR-STU-24 | Students must be notified when the certificate is ready. | Low | Notification / Alert | — | Planned: TC to be prepared |

## Coverage Check

| Coverage Check |  |
| --- | --- |
|  |  |
| Indicator | Value |
| Total functional requirements | 68 |
| Full TC coverage | 31 / 68 — 45.6% |
| Partial TC coverage | 12 / 68 — 17.6% |
| TC preparation planned | 25 / 68 — 36.8% |
| Document status | in progress |
| Date | 2026-08-13 00:00:00 |
