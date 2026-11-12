# BASMAH — Normalization Analysis

**Project:** Evidence & Source Tracking System  
**Owner:** Diyam — Database Design  
**Status:** Proposed normalization analysis based on the current data dictionary; final schema and functional dependencies require team approval.

## Scope and assumptions

The proposed relational schema contains eight tables: Users, Projects, Sources, Claims, Evidence, Reviews, Versions, and ActivityLogs. Each table has a single-column surrogate primary key (e.g., UserID, ClaimID, EvidenceID). The analysis assumes each primary key uniquely determines all non-key attributes of its row, and that no additional business-rule functional dependencies exist among the non-key columns. This assumption must be checked before claiming formal 3NF compliance.

## First Normal Form (1NF)

**Requirement:** Every table has a primary key; each column contains a single value from its domain; no repeating groups or arrays in a row.

- Users stores one Email and one Role per user record.
- Projects stores one OwnerID per project record.
- Claims stores one ProjectID and one CreatedBy per claim record.
- Evidence stores one ClaimID, one SourceID, and one evidence description per row. Multiple sources or evidence items for a claim become multiple Evidence rows, not a comma-separated list.
- Reviews, Versions, and ActivityLogs represent individual events in separate rows.

**Assessment:** The proposed column structure is consistent with 1NF if values are stored atomically and multi-valued content is not packed into columns. Free-text fields such as ClaimText and EvidenceText are acceptable as single textual values.

## Second Normal Form (2NF)

**Requirement:** The table is in 1NF, and every non-prime attribute is fully functionally dependent on every candidate key; there are no partial dependencies on a proper subset of a composite candidate key.

All eight proposed tables use a **single-column primary key**. Therefore partial dependencies on a subset of that primary key cannot occur. This supports 2NF **under the assumption** that no unmodeled composite candidate keys introduce partial dependencies.

For example:
- EvidenceID → ClaimID, SourceID, EvidenceText, EvidenceType.
- ReviewID → ClaimID, ReviewerID, Decision, ReviewDate.
- VersionID → ClaimID, ChangedBy, PreviousText, ChangedAt.

## Third Normal Form (3NF)

**Requirement:** The table is in 2NF and, for each nontrivial functional dependency X → A, X is a superkey or A is a prime attribute. In practical terms, non-key facts should not be stored redundantly as facts about other non-key columns.

Examples in BASMAH:
- Projects stores OwnerID rather than duplicating the owner's FullName and Email. Those user details belong to Users.
- Claims stores ProjectID and CreatedBy rather than duplicating ProjectName or the creator's Email.
- Evidence stores SourceID rather than copying Sources.Title or URL into each evidence row.
- Reviews stores ReviewerID rather than repeating reviewer information.
- Versions stores ChangedBy rather than copying the editor's name.

**Assessment:** The proposed schema is *designed toward 3NF*, but formal confirmation requires checking all business-rule functional dependencies and candidate keys. For example, if the project later requires a fixed mapping from Role to permissions or from SourceType to metadata, those facts should be modeled separately rather than redundantly copied.

## Summary

| Normal form | Design assessment | Verification needed |
|---|---|---|
| 1NF | Proposed structure supports atomic attributes and separate rows for repeated facts | Confirm actual stored values |
| 2NF | Single-column PKs avoid partial dependencies on those PKs | Confirm any other candidate keys |
| 3NF | User, project, source, and review details are separated to avoid obvious transitive dependencies | Validate all functional dependencies and business rules |

## Design caveats

1. `Claims` and `Sources` form a conceptual M:N relationship represented by `Evidence`, which has its own descriptive attributes and primary key.
2. Surrogate primary keys alone do **not** prove 3NF. The team must verify domain-specific dependencies and uniqueness rules.
3. `Versions.PreviousText` is intentional historical snapshot data, not necessarily a normalization defect; audit/history tables may store prior values for traceability.
4. Do not assume 3NF has been tested in MySQL: normalization is a logical design analysis, and no executable schema has yet been validated.
5. Align this document with the final ER Diagram, Data Dictionary, and MySQL CREATE TABLE scripts before submission.
