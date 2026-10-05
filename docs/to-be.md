SmartEdu

Digital Course and Certification Management Platform

TO-BE DOCUMENT

| Document title | SmartEdu — Digital Course and Certification Management Platform (TO-BE Document) |
| --- | --- |
| Business requirement | End-to-end digitization of enrollment, attendance, assessment, and certification through one centralized SmartEdu platform. |
| User story | Defined for Student, Teacher, and Administrator roles. |
| AC (Acceptance Criteria) | Given/When/Then acceptance criteria have been prepared for the user stories and linked to BR/TC references. |
| IT BA name | Akif Süleymanlı / Documentation contributors: Cəlal Əliyev, Aysel Abbasaliyeva |
| Document status | Draft — v1.0 |

Version history

| Version | Date | Change requester (if any) | Short summary |
| --- | --- | --- | --- |
| v1.0 | 01.08.2026 | Kamil Əliyev (Product Owner) | Initial TO-BE document preparation |

## 1. Introduction and Purpose

This document describes SmartEdu's transition from manual, Excel-based enrollment, attendance, assessment, and certification to the target digital workflow. It brings together scope, TO-BE processes, functional requirements, acceptance criteria, business rules, stakeholders, and elicitation sources.

Students currently apply through WhatsApp/email; administrators manually check availability and information in separate Excel files; teachers maintain separate attendance sheets and email assessment results. SmartEdu aims to replace this fragmented process with one traceable system.

## 2. Business Context and Scope

Included:

1. Centrally manage course, session, teacher, classroom, and student information.

2. Allow students to view available OPEN courses with vacant seats.

3. Enable online student enrollment.

4. Automatically check capacity during enrollment.

5. Automatically check student eligibility when a prerequisite is required.

6. Prevent duplicate active enrollment in the same course instance.

7. Enter attendance for courses/sessions assigned by the course manager.

8. Teachers enter assessment results.

9. Automatically calculate certificate eligibility from attendance percentage and passing score.

10. Automatically generate e-certificates with unique verification codes when eligibility conditions are met.

11. Students track enrollment, attendance, results, and certificate status in one profile.

12. Generate administrator reports.

13. Enforce authorization restrictions.

14. Enforce the enrollment cancellation rule.

15. Enforce BR-01–BR-10 business rules.

16. Model AS-IS and TO-BE processes in BPMN 2.0.

17. Prepare ERD, API, OpenAPI, security, and testing artifacts.

Excluded:

1. Payments, invoices, and financial operations.

2. Native iOS/Android mobile application.

3. Actual LMS/video-conferencing integrations.

4. HR, payroll, and recruitment.

5. Marketing and lead generation.

6. Physical infrastructure management.

7. Multilingual interface.

8. AI recommendation systems.

9. Social media integration.

10. Physical certificate production.

11. Full legacy data migration.

## 3. Target Process Description / TO-BE Workflow

The TO-BE process is represented by a BPMN 2.0 model. Main workflow:

1. The Administrator creates the course, sets parameters, and publishes it.

2. The Student reviews open courses and enrolls.

3. The system automatically checks prerequisites, capacity, and enrollment rules.

4. Enrollment is confirmed when eligible.

5. The course manager enters attendance and assessment information received from the teacher.

6. The system calculates certificate eligibility and generates the e-certificate.

7. The Student tracks results; the Administrator views reports and audits.

## 4. Functional Requirements

| FR ID | Requirement name / User Story | Role | Functional description | Prioritet |
| --- | --- | --- | --- | --- |
| FR-TCH-1 | Statistics | The system must show teachers session attendance percentages and overall course statistics. | FR-TCH-15 | Must |
| FR-TCH-2 | Risk indicator | Students with attendance below the course threshold must be visually flagged for teachers (informational, not decision-making). | FR-TCH-16 | Must |
| FR-TCH-3 | Pending records | Missing attendance and result rows must show PENDING status. | FR-TCH-17 | Must |
| FR-TCH-4 | Certificate restriction | Teachers cannot generate/revoke certificates or change eligibility; calculation is automatic. | FR-TCH-18 | Must |
| FR-TCH-5 | Enrollment restriction | Teachers cannot create/cancel student enrollments or change course capacity. | FR-TCH-19 | Must |
| FR-TCH-6 | Data restriction | Teachers cannot access unassigned students' profiles or other course results. | FR-TCH-20 | Must |
| FR-TCH-7 | Statistics | The system must show teachers session attendance percentages and overall course statistics. | FR-TCH-15 | Must |
| FR-TCH-8 | Risk indicator | Students with attendance below the course threshold must be visually flagged for teachers (informational, not decision-making). | FR-TCH-16 | Must |
| FR-TCH-9 | Pending records | Missing attendance and result rows must show PENDING status. | FR-TCH-17 | Must |
| FRTCH-10 | Certificate restriction | Teachers cannot generate/revoke certificates or change eligibility; calculation is automatic. | FR-TCH-18 | Should |
| FR-TCH-11 | Enrollment restriction | Teachers cannot create/cancel student enrollments or change course capacity. | FR-TCH-19 | Must |
| FR-TCH-12 | Data restriction | Teachers cannot access unassigned students' profiles or other course results. | FR-TCH-20 | Should |

## 5. Acceptance Criteria

| AC ID | US ID | Given | When | Then |
| --- | --- | --- | --- | --- |
| AC-AUTH-001-01 | US-AUTH-001 | An ACTIVE Student provides correct credentials | logs in | The system creates a session and opens the Student dashboard. |
| AC-AUTH-001-02 | US-AUTH-001 | Credentials are incorrect | A login is attempted | A generic error appears and no session is created. |
| AC-AUTH-001-03 | US-AUTH-001 | A Student accesses another Student's URL/API resource | The request is sent | 403 is returned and no data is disclosed. |
| AC-AUTH-002-01 | US-AUTH-002 | A Teacher is assigned to a course instance | They open My Courses | Only assigned instances are shown. |
| AC-AUTH-002-02 | US-AUTH-002 | A Teacher sends a write request for another instance | The request is processed | 403 is returned and nothing is changed. |
| AC-AUTH-002-03 | US-AUTH-002 | The assignment has been revoked | The Teacher refreshes the page | The instance is removed from the write-access list. |
| AC-ADM-001-01 | US-ADM-001 | The new email is unique | The Administrator creates a Student or Teacher | The account is created under the ACTIVE/PENDING rule and an audit entry is recorded. |
| AC-ADM-001-02 | US-ADM-001 | The email already exists | Account creation is attempted | 409 is returned and no duplicate account is created. |
| AC-ADM-001-03 | US-ADM-001 | account becomes INACTIVE | The user logs in | Login is rejected; historical data is retained. |
| AC-CAT-001-01 | US-CAT-001 | An OPEN instance exists | The Student opens the catalog | The instance appears with its schedule and availability. |
| AC-CAT-001-02 | US-CAT-001 | instance is DRAFT/CANCELLED | The Student opens the catalog | The instance is not shown. |
| AC-CAT-001-03 | US-CAT-001 | A keyword/date/Teacher filter is selected | A search is performed | Only matching results are returned. |

## 6. Exception Handling (Business Rules)

| ID | Rule name | Business rule |
| --- | --- | --- |
| BR-001 | Open enrollment only | Enrollment may be created only when Course Instance status=OPEN and the registration window is active. |
| BR-002 | Capacity limit | CONFIRMED enrollments cannot exceed the defined course capacity. |
| BR-003 | No duplicate active enrollment | A Student cannot have more than one PENDING/WAITLISTED/CONFIRMED enrollment for the same Course Instance. |
| BR-004 | Prerequisites met | The Student must have PASSED all mandatory prerequisite courses. |
| BR-005 | Minimum attendance requirement | Certificate eligibility must not be granted below the minimum attendance threshold. |
| BR-006 | Passing score requirement | No certificate may be generated if the final result is below the pass score. |
| BR-007 | Assigned teacher only | Teachers may enter attendance/results only for assigned Course Instances/Sessions. |
| BR-008 | Student access to own data only | Students may view only their own enrollment, attendance, result, and certificate data. |
| BR-009 | Unique verification code | A unique verification code must be generated for each certificate. |
| BR-010 | Enrollment cancellation deadline | Students may cancel enrollment only before the defined cutoff. |

## 7. Stakeholder Register

| Stakeholder | Position | Category | Influence | Interest | Communication type | Contact method | Expectation |
| --- | --- | --- | --- | --- | --- | --- | --- |
| Samir Hüseynov | Student | External | Medium | High | Formal business meetings (Weekly) | Microsoft Teams, Email | Convenient course enrollment and secure, timely tracking of attendance, results, and certificate status in one platform. |
| Gülnar Hacıyeva | Administrator | Internal | High | High | Formal business meetings (Weekly) | Microsoft Teams, Email | Efficient management of courses, enrollments, and certificates in one system, with timely report preparation. |
| Məleykə Məmmədova | Teacher | Internal | Medium | High | Formal business meetings (Weekly) | Microsoft Teams, Email | Easy entry of attendance and assessment data and accurate management of results. |
| Alim Hacıyev | Course Manager | Internal | High | High | Formal business meetings (Weekly) | Microsoft Teams, Email | Effective management of course planning, participant limits, and course information. |
| Backend Developer | Backend developer | Internal | High | Medium | Formal business meetings (Daily) | Microsoft Teams, Email | Clearly presented functional requirements and continuous support during technical development. |
| Firuzə Nağıyeva | Frontend developer | Internal | High | Medium | Formal business meetings (Daily) | Microsoft Teams, Email | A stable, easy-to-use interface that meets approved UI requirements. |
| Aysel Əhmədova | QA Engineer | Internal | Medium | High | Formal business meetings (Weekly) | Microsoft Teams, Email | Testable requirements and timely resolution of identified issues. |
| Sevil Hüseynova | Project Manager | Internal | High | High | Formal business meetings (Weekly) | Microsoft Teams, Email | On-time project delivery within the defined scope and objectives. |
| Cəlal Əliyev | IT Business Analyst | Internal | High | High | Formal business meetings (Weekly) | Microsoft Teams, Email | Accurate analysis and documentation of business requirements and clear communication to the technical team. |
| Məhəmməd Qurbanov | Product Owner | Internal | High | High | Formal business meetings (Weekly) | Microsoft Teams, Email | Product development aligned with business goals and user needs. |

Elicitation techniques used:

Business Interviews

Observation

Document Analysis

Process Description and Modeling

Meetings and focus areas:

| Stakeholder | Texnika | Date | Type | Sources | Fokus |
| --- | --- | --- | --- | --- | --- |
| Administrator | Business Interview | 05.08.2025, 10:00 | Offline | AS-IS Scenario; Registration Excel file; Course Capacity file | Course enrollment, capacity management, certificate eligibility, reports, and pain points. |
| Teacher | Business Interview | 05.08.2025, 11:00 | Offline | AS-IS Scenario; Attendance Sheet; Assessment Records | Attendance recording, assessment results, and information exchange with the administrator. |
| Student | Business Interview | 05.08.2025, 12:00 | Online | AS-IS Scenario; Registration Communication (WhatsApp / Email) | Course enrollment, status tracking, and access to results and certificate status. |

12. Performance/load testing and production deployment.
