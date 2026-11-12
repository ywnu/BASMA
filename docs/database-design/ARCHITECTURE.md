# BASMAH — Proposed Database Architecture

**Project:** BASMAH – Evidence & Source Tracking System  
**Owner:** Diyam — Database Design  
**Status:** Proposed logical architecture for documentation; not a deployed or verified implementation.

## 1. Architectural overview

BASMAH is modeled as a **three-tier architecture** for clarity and separation of responsibilities:

1. **Presentation layer (User Interface):** Users create projects, enter claims, link supporting evidence, and review records through a future interface.
2. **Application layer (Business Logic / API):** A future application service authenticates users, validates requests, applies role-based rules, manages claims and reviews, and communicates with the database. The specific language, framework, endpoints, and deployment model have not yet been selected.
3. **Data layer (MySQL):** Stores relational records and enforces primary keys, foreign keys, uniqueness, and other agreed database constraints.

### Logical flow

```text
[User / Reviewer]
        |
        v
[Presentation Layer / UI]
        |
        v
[Application Layer / Business Logic / API]
        |
        v
[MySQL Relational Database]
   | Users        | Projects
   | Sources      | Claims
   | Evidence     | Reviews
   | Versions     | ActivityLogs
```

This is a **proposed architecture diagram**, not evidence that a frontend or backend has already been built.

## 2. Database organization

| Functional area | Tables | Purpose |
|---|---|---|
| Identity and ownership | Users, Projects | Users and the projects they own |
| Claims and supporting sources | Claims, Sources, Evidence | Track statements and their linked evidence; Evidence resolves the conceptual M:N association between Claims and Sources |
| Review workflow | Reviews | Record reviewer decisions for claims |
| Change history and auditing | Versions, ActivityLogs | Preserve claim edits and record user actions |

## 3. Data integrity

- Every table has a proposed single-column primary key.
- Foreign keys connect projects to owners, claims to projects and authors, evidence to claims and sources, reviews to claims and reviewers, versions to claims and editors, and logs to users.
- `Users.Email` is proposed to be unique.
- Foreign-key deletion rules must be agreed with the schema/integrity teammates. Avoid accidental deletion of evidence, reviews, and history.
- Application-layer validation and database constraints serve different purposes; database constraints remain necessary even if a UI validates input.

## 4. Example logical workflow (proposed)

1. A user creates a project; the application stores a row in `Projects` linked by `OwnerID`.
2. The user creates a claim; the application stores it in `Claims` with `ProjectID` and `CreatedBy`.
3. The user associates a source with supporting evidence; the application inserts a row in `Evidence` referencing `ClaimID` and `SourceID`.
4. A reviewer records a decision in `Reviews` with `ReviewerID`.
5. Later edits can be recorded in `Versions`; user activity can be recorded in `ActivityLogs`, subject to final workflow decisions.

## 5. Scope and pending decisions

- **Confirmed design scope:** Eight proposed relational tables and their documented keys/relationships.
- **Proposed, not implemented:** UI, application/API layer, authentication, permissions, and end-to-end workflow.
- **Pending:** Exact implementation stack, hosting, SQL DDL, transaction boundaries, access control rules, retention policy, and verification against the final ERD.
- The architecture must be updated if the team decides to deliver a database-only SQL project without a working UI/API; in that case the UI and API remain conceptual context only.

**Related design documents:** `DATA_DICTIONARY.md`, `RELATIONSHIPS.md`, `NORMALIZATION.md`.
