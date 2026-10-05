SmartEdu

Digital Course & Certification Management Platform

DECISION LOG

Architecture & Design Decision Records (ADR) — decision rationale

| Project | SmartEdu — Digital Course & Certification Management Platform |
| --- | --- |
| Document type | Decision Log / Architecture Decision Records |
| Version | 1.0 |
| Date | August 2026 |
| Related documents | SmartEdu — BRD v1.1<br>SmartEdu — Technical Document Package |
| Decision count | 8 decisions (DR-01 … DR-08) |

| Document purpose: Record SmartEdu's main technical/design decisions, alternatives, and trade-offs. Preserve decision context for future team members and prevent repeated discussions. All decisions reference requirements in the BRD and Technical Document Package. |
| --- |

TABLE OF CONTENTS

How to read this document	3

DR-01 — Soft delete — resurslar fiziki silinmir	4

DR-01.1 Kontekst	4

DR-01.2 Alternatives considered	4

DR-01.3 Decision	4

DR-01.4 Consequences and trade-offs	4

DR-02 — Database authorization — Row Level Security	6

DR-02.1 Kontekst	6

DR-02.2 Alternatives considered	6

DR-02.3 Decision	6

DR-02.4 Consequences and trade-offs	6

DR-03 — Transaction-level capacity checks	8

DR-03.1 Kontekst	8

DR-03.2 Alternatives considered	8

DR-03.3 Decision	8

DR-03.4 Consequences and trade-offs	8

DR-04 — PATCH for status transitions instead of PUT	10

DR-04.1 Kontekst	10

DR-04.2 Alternatives considered	10

DR-04.3 Decision	10

DR-04.4 Consequences and trade-offs	10

DR-05 — ENUM status fields instead of boolean flags	12

DR-05.1 Kontekst	12

DR-05.2 Alternatives considered	12

DR-05.3 Decision	12

DR-05.4 Consequences and trade-offs	12

DR-06 — UUID primary keys instead of sequential integers	14

DR-06.1 Kontekst	14

DR-06.2 Alternatives considered	14

DR-06.3 Decision	14

DR-06.4 Consequences and trade-offs	14

DR-07 — Self-referencing prerequisite FK instead of a separate table	15

DR-07.1 Kontekst	15

DR-07.2 Alternatives considered	15

DR-07.3 Decision	15

DR-07.4 Consequences and trade-offs	15

DR-08 — Public certificate verification without authentication	17

DR-08.1 Kontekst	17

DR-08.2 Alternatives considered	17

DR-08.3 Decision	17

DR-08.4 Consequences and trade-offs	17

Decision summary	19

## How to read this document

Each decision uses the following structure:

| Section | What it shows |
| --- | --- |
| Kontekst | The problem requiring a decision and the originating requirement/business rule |
| Alternatives considered | Alternatives and their strengths/weaknesses |
| Decision | Selected option and main rationale |
| Consequences and trade-offs | Benefits and accepted compromises |
| Traceability | Related BR / FR / EC / TC identifiers |

Status values:

Accepted — implemented and reflected in the documents.

## DR-01 — Soft delete — resurslar fiziki silinmir

| Status | Field | Related requirements |
| --- | --- | --- |
| Accepted | Data model / lifecycle | BR-005, BR-029, AC-03, FR-ADM-05 |

### DR-01.1 Kontekst

Administrators must remove unused Courses/deactivated users from service. Related instances, enrollments, attendance, assessments, and Certificates must remain for subsequent authenticity verification.

Physical deletion would make verification reference a nonexistent Course, preventing recovery of the certified Course.

### DR-01.2 Alternatives considered

| Variant | Strength | Weakness |
| --- | --- | --- |
| Physical deletion (DELETE) | Simple implementation; smaller database. | Historical data loss; FK violations/cascade deletion risk; broken verification chain. |
| Cascade delete | Referential integrity is automatically preserved. | Deleting a Course removes enrollments/certificates, contradicting audit requirements. |
| Soft delete — status = ARCHIVED | Preserves history, verification, and complete audit trails. | All queries need status filters; database size grows. |

### DR-01.3 Decision

| Choice: No physical deletion of Courses/users. Set Course status ARCHIVED and USER.status DISABLED (BR-005, BR-029). |
| --- |

Main rationale: After certificate issuance, Course/Instance records must remain available indefinitely for meaningful BR-023 public verification.

Audit is another reason: append-only AUDIT_LOG entries lose context if the resource is deleted.

### DR-01.4 Consequences and trade-offs

Benefits

The Certificate verification chain remains intact in all cases.

Accepted compromises

All reads require status filters; forgetting a filter may expose archived records.

| Note: Works with BR-004, which permits OPEN only after required parameters exist. Status handles both lifecycle and deletion. |
| --- |

## DR-02 — Database authorization — Row Level Security

| Status | Field | Related requirements |
| --- | --- | --- |
| Accepted | Security / access control | BR-026, BR-027, FR-STU-02, FR-TCH-02, EC-06, R-04 |

### DR-02.1 Kontekst

Three roles have different boundaries: Students see own enrollment/attendance/results/certificates; Teachers see assigned sessions only (BR-026, BR-027).

R-04 identifies unauthorized access from misconfigured access controls, one of the highest-impact risks.

### DR-02.2 Alternatives considered

| Variant | Strength | Weakness |
| --- | --- | --- |
| Application-level authorization only | Simple implementation; centralized business logic. | Checks must be repeated per endpoint; omissions leak data; direct DB access is unprotected. |
| RLS only | Central, non-bypassable control. | Complex conditions are difficult in policies; errors are less informative to users. |
| Two layers — middleware + RLS | The application provides clear errors; RLS is the final defense. | Rules exist in two places; changes must be synchronized. |

### DR-02.3 Decision

| Choice: Middleware compares token role/resource owner; database RLS checks row ownership using auth.uid(). |
| --- |

Application-only checks would introduce new leakage risks per endpoint. RLS structurally removes this risk through policies applying to every query.

RLS alone would reduce usability: 42501 is not meaningful to users. Middleware supplies understandable errors.

### DR-02.4 Consequences and trade-offs

Benefits

EC-06 is structurally addressed: Teachers cannot enter attendance for others' sessions; 403 Forbidden is returned.

Accepted compromises

Rules must be updated simultaneously in both layers to prevent inconsistent behavior.

## DR-03 — Transaction-level capacity checks

| Status | Field | Related requirements |
| --- | --- | --- |
| Accepted | Concurrency / data integrity | BR-007, BR-008, EC-01, EC-02, TC-13, R-02 |

### DR-03.1 Kontekst

BR-007 prohibits ACTIVE enrollments exceeding capacity. EC-01 describes two simultaneous requests for the final seat.

R-02 identifies parallel requests exceeding capacity; impact is high and likelihood medium.

### DR-03.2 Alternatives considered

| Variant | Strength | Weakness |
| --- | --- | --- |
| Application-level check (read, then write) | Simple, readable code. | Classic race condition: both requests see availability and exceed capacity. |
| Optimistic locking (version field) | Works without locking. | Conflicts require retries and additional user waiting. |
| Atomik tranzaksiya + database constraint | Race conditions are structurally prevented; enforcement is independent of application logic. | High load may cause lock waits; errors arrive as DB codes. |

### DR-03.3 Decision

| Choice: Check capacity and create enrollment in one atomic transaction; uq_active_enrollment blocks duplicates at database level. |
| --- |

Integrity rules belong closest to the data. Application checks protect only their code path; database constraints protect all entry paths.

Actual testing confirmed a second ACTIVE enrollment for the same Student/Instance returns PostgreSQL 23505 for uq_active_enrollment and API 409 Conflict.

### DR-03.4 Consequences and trade-offs

Benefits

EC-01 and EC-02 are addressed without code changes.

Accepted compromises

The API must translate database error 23505 into an understandable user message.

## DR-04 — PATCH for status transitions instead of PUT

| Status | Field | Related requirements |
| --- | --- | --- |
| Accepted | API design | BR-005, BR-010, BR-024, FR-ADM-06 |

### DR-04.1 Kontekst

Enrollment cancellation (BR-010), Course archiving (BR-005), and Certificate revocation (BR-024) change only status; other fields remain untouched.

HTTP method choice requires API documentation rationale and directly affects resource integrity.

### DR-04.2 Alternatives considered

| Variant | Strength | Weakness |
| --- | --- | --- |
| PUT — complete resource replacement | Simple semantics; idempotent. | Clients send all fields; omitted fields may be lost; concurrent updates overwrite data. |
| POST — separate action endpoint | Explicit action name (e.g., /enrollments/{id}/cancel). | Departs from REST resource modeling; each transition needs an endpoint. |
| PATCH — partial update | Sends only changed fields, preserves others, and retains endpoint structure. | Separate validation must restrict editable fields. |

### DR-04.3 Decision

| Choice: Use PATCH /enrollments?id=eq.{enrollmentId} and PATCH /courses?id=eq.{courseId} for status transitions. |
| --- |

PUT would require every field on each transition, risking accidental overwrites of enrollment_date/cancelled_at.

PATCH also reduces concurrent-update risk: operations changing different fields do not erase each other's results.

### DR-04.4 Consequences and trade-offs

Benefits

Untouched resource fields are preserved.

Accepted compromises

Validate allowed transitions separately; CANCELLED/COMPLETED should not return directly to ACTIVE.

| Note: BRD Section 8.2 documents lifecycles. Re-enrollment creates a new Enrollment rather than restoring the old one. |
| --- |

## DR-05 — ENUM status fields instead of boolean flags

| Status | Field | Related requirements |
| --- | --- | --- |
| Accepted | Data model | BR-004, BR-005, BR-010, BR-011, BR-024, BR-025 |

### DR-05.1 Kontekst

Course: DRAFT → OPEN → CLOSED → COMPLETED. Enrollment: ACTIVE, CANCELLED, COMPLETED. Certificate: ELIGIBLE → ISSUED → REVOKED.

Alternatively, separate booleans such as is_active, is_cancelled, is_completed could represent each state.

### DR-05.2 Alternatives considered

| Variant | Strength | Weakness |
| --- | --- | --- |
| Boolean flags | Simple queries and indexing. | Contradictory states are possible (is_active and is_cancelled both true); new states require schema changes. |
| Separate status lookup table | Statuses managed as data; JOIN supports extra metadata. | Extra JOINs create unnecessary complexity for a simple lifecycle. |
| ENUM field | One status at a time; database rejects invalid values; lifecycle directly matches documentation. | New statuses require schema migration. |

### DR-05.3 Decision

| Choice: ENUM for COURSE.status, COURSE_INSTANCE.status, ENROLLMENT.status, ASSESSMENT_RESULT.result_status, CERTIFICATE.status, ATTENDANCE.status, USER.status, and USER.role. |
| --- |

Boolean contradictions could mark Enrollment active and cancelled simultaneously, breaking eligibility calculation.

ENUM directly represents BRD Section 8.2 lifecycles in the data model, avoiding translation between documentation and schema.

### DR-05.4 Consequences and trade-offs

Benefits

Contradictory statuses are structurally impossible.

Accepted compromises

Plan schema migrations for new statuses in future phases.

## DR-06 — UUID primary keys instead of sequential integers

| Status | Field | Related requirements |
| --- | --- | --- |
| Accepted | Data model / security | BR-023, BR-027, FR-STU-02 |

### DR-06.1 Kontekst

Primary keys were needed for every entity. The system has public verification and Students access own resources by ID.

### DR-06.2 Alternatives considered

| Variant | Strength | Weakness |
| --- | --- | --- |
| Sequential integer (SERIAL / IDENTITY) | Compact, readable, good index performance. | Predictable IDs risk enumeration and expose record counts. |
| Composite key | Carries business meaning. | Complicates FKs; changing business fields changes keys. |
| UUID | Unpredictable; enumeration is impractical; unique in distributed systems. | Larger, slightly slower indexes than sequential keys, unreadable. |

### DR-06.3 Decision

| Choice: UUID primary keys for all entities; a separate unique Certificate verification_code (BR-023). |
| --- |

Sequential enrollment IDs would let Students try neighboring records. RLS blocks this, but avoiding reliance on one defense is preferable.

Sequential IDs also indirectly disclose approximate record counts as business information.

A separate verification code is deliberate: public sharing exposes the dedicated code rather than the Certificate primary key.

### DR-06.4 Consequences and trade-offs

Benefits

Enumeration is practically impossible.

Accepted compromises

Unreadable UUIDs complicate debugging and support.

## DR-07 — Self-referencing prerequisite FK instead of a separate table

| Status | Field | Related requirements |
| --- | --- | --- |
| Accepted | Data model | BR-009, EC-04, AC-06, US-05 |

### DR-07.1 Kontekst

A Course may require a PASSED result in a previous Course before enrollment (BR-009).

Model this with a self-referencing COURSE FK or separate course_prerequisites table.

### DR-07.2 Alternatives considered

| Variant | Strength | Weakness |
| --- | --- | --- |
| Separate course_prerequisites table | Supports multiple prerequisites and AND/OR logic. | More complex queries; current requirements specify one prerequisite. |
| Prerequisite as text | Simplest implementation. | Cannot check automatically or enforce BR-009. |
| Self-referencing FK — prerequisite_course_id | Simple query, referential integrity, full current-requirement fit. | Only one prerequisite; multiple prerequisites need schema changes. |

### DR-07.3 Decision

| Choice: NULLABLE COURSE.prerequisite_course_id references COURSE.id through a self-referencing FK. Cardinality: 1 : 0..N. |
| --- |

Current requirements specify one prerequisite. A separate table would add complexity for an unused capability.

NULLABLE suits most Courses having no prerequisite and is simpler than a special “no prerequisite” value.

### DR-07.4 Consequences and trade-offs

Benefits

Prerequisites require one JOIN; SQL is documented in Technical Document Section 2.2.

Accepted compromises

Multiple prerequisites per Course will require schema changes.

| Note: Behavior for ARCHIVED prerequisite Courses remains open; see Gap Register. |
| --- |

## DR-08 — Public certificate verification without authentication

| Status | Field | Related requirements |
| --- | --- | --- |
| Conditional | Security / business | BR-023, Q4, C-05 |

### DR-08.1 Kontekst

Third parties such as employers must verify authenticity without having system accounts.

BRD Section 2.5 requires public verification to return only authorized, minimal data.

### DR-08.2 Alternatives considered

| Variant | Strength | Weakness |
| --- | --- | --- |
| Require authentication | Full control; every request linked to a user. | Third parties must create accounts, reducing practical value. |
| Add a signature/QR to the PDF | No online request required. | Cannot detect REVOKED certificates or show current status. |
| Public endpoint + anon role RLS policy | No account required; RLS returns ISSUED only; no personal data disclosed. | The public endpoint needs protection against code guessing. |

### DR-08.3 Decision

| Choice: Public GET /certificates?verification_code=eq.{code}, without JWT. Supabase anon RLS SELECT permits ISSUED certificates only. |
| --- |

Authentication-free verification creates the business value; otherwise employers cannot verify and the function loses meaning.

Security relies on limited output: RLS hides non-ISSUED certificates and responses exclude personal Student information.

### DR-08.4 Consequences and trade-offs

Benefits

Third parties verify without accounts.

Accepted compromises

Public access permits brute-force code attempts; rate limiting and sufficient code entropy are required.

| Note: Conditional because Q4 in BRD Section 2.6 (public fields) awaits Business Owner approval. Rate limiting is also undocumented; see Gap Register. |
| --- |

## Decision summary

This table consolidates decisions, coverage, and related requirements.

| ID | Decision | Field | Status | Traceability |
| --- | --- | --- | --- | --- |
| DR-01 | Soft delete — ARCHIVED / DISABLED | Data model | Accepted | BR-005, BR-029, AC-03 |
| DR-02 | Two-layer authorization — middleware + RLS | Security | Accepted | BR-026, BR-027, EC-06 |
| DR-03 | Atomic transaction capacity checks | Concurrency | Accepted | BR-007, BR-008, EC-01 |
| DR-04 | PATCH status transitions | API design | Accepted | BR-005, BR-010, BR-024 |
| DR-05 | ENUM status fields | Data model | Accepted | BR-004, BR-010, BR-024 |
| DR-06 | UUID primary key | Data model | Accepted | BR-023, BR-027 |
| DR-07 | Prerequisite self-referencing FK | Data model | Accepted | BR-009, EC-04, AC-06 |
| DR-08 | Certificate verification public | Security | Conditional | BR-023, Q4, C-05 |

| Next step: Mark DR-08 Accepted after the Business Owner answers Q4. Add new decisions from DR-09. When replacing a decision, retain the old record as Superseded with a reference to the new decision. |
| --- |

## Source headers and footers

SmartEdu — Decision Log (ADR) · Version 1.0 · August 2026
 / 
