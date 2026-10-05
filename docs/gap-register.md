SmartEdu

Digital Course & Certification Management Platform

GAP REGISTER

Gaps between artifacts, open questions, and closure plan

| Project | SmartEdu — Digital Course & Certification Management Platform |
| --- | --- |
| Document type | Gap Register / Open Items Log |
| Version | 1.0 |
| Date | August 2026 |
| Number of gaps | 14 records (GAP-01 … GAP-14) |
| Source | BRD v1.1 RTM · Section 2.6 · Risk Register · Technical Document Package |

| Document purpose: Record known gaps, undocumented areas, and questions awaiting business decisions across SmartEdu artifacts. Most were already identified in the RTM and BRD Section 2.6; this register consolidates them and defines each owner, impact, and closure plan. |
| --- |

## 1. Methodology and classification

Gaps fall into four categories. Each category identifies who can close the gap, rather than its nature, and determines the closure-plan owner.

| Category | Meaning | Owner |
| --- | --- | --- |
| Documentation | Functionality exists in the data model/business rules, but the API endpoint is undocumented. | IT Business Analyst |
| Business decision | The requirement is incomplete; the Business Owner must decide. | Business Owner |
| Technical decision | The technical team must select a solution approach. | Technical Team |
| Artifact completeness | A missing or damaged element in an existing document. | IT Business Analyst |

### 1.1 Priority criteria

| Prioritet | Criterion |
| --- | --- |
| High | Development cannot start before closure, or the gap directly affects security/data integrity. |
| Medium | Development can start, but the gap prevents full implementation of this functionality. |
| Low | Affects document quality, not functional implementation. |

| Important: These gaps do not mean the current project phase failed. Most are decisions deliberately deferred to the next phase. The register's value is making gaps visible and assigning an owner to each. |
| --- |

## 2. Gap Register

| ID | Category | Gap description | Discovery source | Traceability | Prioritet | Impact | Closure plan | Owner | Status |
| --- | --- | --- | --- | --- | --- | --- | --- | --- | --- |
| GAP-01 | Documentation | PATCH /course_instances is undocumented | BR-001 allows capacity changes, but the corresponding API endpoint is absent. | BR-001, BR-002<br>FR-ADM-07, FR-ADM-08<br>TC-13 | High | Instance capacity/dates cannot be updated; DR-04 cannot be applied to this endpoint. | Add PATCH /course_instances to the API document and a 409 Conflict example for capacity reduction. | IT BA | Open |
| GAP-02 | Documentation | Teacher assignment endpoint is undocumented | BR-003 requires Teacher assignment to instances; no assignment operation appears in the API document. | BR-003<br>FR-ADM-04<br>TC-13 | High | BR-026 cannot be applied because assignment creation is undefined. | Document teacher_id assignment through PATCH /course_instances; add ACTIVE TEACHER validation to the catalog. | IT BA | Open |
| GAP-03 | Documentation | User management endpoints are absent | BR-028/BR-029 require account creation, role assignment, and deactivation, absent from the API document. | BR-028, BR-029<br>FR-ADM-02, FR-ADM-03<br>UAT-14, UAT-15 | High | Core administrator management is undefined at API level; UAT-14/UAT-15 cannot be fully executed. | Document POST/PATCH /users, role-change audit requirements, and the DISABLED scenario. | IT BA | Open |
| GAP-04 | Documentation | PATCH /certificates (REVOKED) is undocumented | BR-024/BR-025 require ISSUED/REVOKED transitions; the endpoint is absent. | BR-024, BR-025<br>UAT-15 | Medium | Erroneous certificate revocation is technically undefined; DR-08's REVOKED filter cannot be tested practically. | Document PATCH /certificates, permitted transitions, and audit logging of revocation reasons. | IT BA | Open |
| GAP-05 | Documentation | No Course OPEN validation example | BR-004 requires parameters before OPEN. PATCH /courses exists but lacks a failed-validation example. | BR-004<br>FR-ADM-05, FR-ADM-06<br>TC-36 | Medium | Behavior when opening an incompletely configured Course is unclear. | Add a 400 Bad Request example for a failed OPEN attempt. | IT BA | Open |
| GAP-06 | Business decision | Enrollment cancellation deadline is undefined | BRD Section 2.6, Q1: How should the Student cancellation deadline be defined? | BR-010, BR-011<br>Q1 | High | BR-010 describes cancellation, but without a time limit students could cancel on the final course day, disrupting capacity planning. | Business Owner approves a cutoff (e.g., before start_date); add a new BR and validation. | Business Owner | Open |
| GAP-07 | Business decision | No re-enrollment rule after FAILED | BRD Section 2.6, Q3: May a Student retake the same Course after FAILED? | BR-008, BR-016, BR-017<br>Q3 | High | BR-008 prohibits a second ACTIVE enrollment in the same instance. If retakes are allowed, clarify whether the rule applies at instance or course level. | Business Owner decides; review BR-008 scope and unique-constraint composition accordingly. | Business Owner | Open |
| GAP-08 | Business decision | Public verification fields are unapproved | BRD Section 2.6, Q4: Which certificate information may public users see? | BR-023<br>C-05, DR-08 | High | DR-08 is Conditional. The public endpoint specification cannot close until response fields are approved. | Business Owner approves minimal fields; align the API response example. | Business Owner | Open |
| GAP-09 | Business decision | Email notification scope is undefined | BRD Section 2.6, Q5: Which processes require notifications? | Q5<br>D-06 (dependency) | Low | Notifications are unapproved in current scope; if approved, email-service selection is a dependency. | Business Owner approves notification list; then create a separate functional requirement and technical dependency. | Business Owner | Open |
| GAP-10 | Technical decision | E-certificate PDF storage is unselected | BRD Section 2.6, Q2: Store PDFs within the system or in separate storage? | Q2<br>CERTIFICATE.pdf_url | Medium | CERTIFICATE.pdf_url exists, but storage location and access control are undefined. | Technical Team selects storage; add DR-09 to the Decision Log. | Technical Team | Open |
| GAP-11 | Technical decision | Public endpoint rate limiting is undocumented | DR-08 notes that public verification permits code guessing. No document defines rate limiting or code entropy. | BR-023<br>DR-08, R-04 | High | No protection is defined against brute-force verification-code discovery. | Add a rate-limiting NFR; clarify verification_code format and entropy. | Technical Team | Open |
| GAP-12 | Technical decision | ARCHIVED prerequisite Course scenario is uncovered | DR-01 soft delete intersects DR-07 prerequisites: dependent Course behavior is undefined when a prerequisite Course is ARCHIVED. | BR-005, BR-009<br>DR-01, DR-07<br>EC-04 | Medium | The edge-case catalog omits this scenario; prerequisite checks for archived prior Courses are uncertain. | Add an EC and clarify whether a prior PASSED archived Course counts as a prerequisite. | IT BA | Open |
| GAP-13 | Artifact completeness | Technical Document 5.4 — 5 screenshots missing | Links to screenshots for 401 (×2), 400, 403, and 404 are broken in Real Error examples. | Technical Document Section 5.4 | Low | Errors have text descriptions but no visual evidence of actual validation. | Recapture Postman screenshots and insert them. | IT BA | Open |
| GAP-14 | Artifact completeness | Success criteria are not measurable | BRD Section 1.6 uses qualitative wording such as “significantly reduced,” with no baseline or target. | BRD Section 1.6<br>C-04 | Medium | Success cannot be measured objectively. Approval of numeric NFRs during Technical Design is a documented constraint. | Define baseline method, metric, and target with the Business Owner; measure before TO-BE implementation. | IT BA / Business Owner | Open |

## 3. Summary and next steps

### 3.1 Breakdown by category

| Category | Count | High | Medium | Low | Owner |
| --- | --- | --- | --- | --- | --- |
| Documentation | 5 | 3 | 2 | 0 | IT Business Analyst |
| Business decision | 4 | 3 | 0 | 1 | Business Owner |
| Technical decision | 3 | 1 | 2 | 0 | Technical Team |
| Artifact completeness | 2 | 0 | 1 | 1 | IT BA / Business Owner |
| Total | 14 | 7 | 5 | 2 |  |

### 3.2 Closure sequence

Gaps are interdependent; this sequence prevents rework.

| Step | Action | Reason for sequence |
| --- | --- | --- |
| 1 | Business Owner answers Q1, Q3, Q4 (GAP-06, GAP-07, GAP-08). | These decisions change business-rule scope; earlier API documentation would need rework. |
| 2 | Review BR-008 and the unique constraint based on GAP-07. | If scope changes, update DR-03 and TC-13 too. |
| 3 | Complete API documentation: GAP-01 … GAP-05. | Finalize endpoints after business decisions. |
| 4 | Add rate limiting as an NFR under GAP-11. | Required to close DR-08; perform alongside GAP-08. |
| 5 | Add the GAP-12 edge case and decide GAP-10 storage. | Needed before development, independent of preceding steps. |
| 6 | Close GAP-13/GAP-14 document quality gaps. | No functional barrier; may run in parallel. |

### 3.3 Register management

Add new gaps starting at GAP-15; do not delete existing records.

| Note: Eleven gaps were identified by project artifacts: seven from the RTM technical-impact column, four from BRD Section 2.6 open questions. This demonstrates the RTM's use as an operational control tool, beyond formal documentation. |
| --- |

## Source headers and footers

SmartEdu — Gap Register · Version 1.0 · August 2026
 / 
