# BASMAH — Database Design Section (Draft for Final Report)

**Project:** BASMAH – Evidence & Source Tracking System  
**Section owner:** Diyam — Database Design  
**Document status:** Draft based on the current proposed schema. Final approval requires ER/EER diagram review and alignment with the team's MySQL implementation.

## 1. Database Design Overview

BASMAH is a proposed relational database system for organizing projects, claims, supporting sources, evidence, review decisions, and change history. The database design aims to make each claim traceable to its project, author, evidence, source, and reviewer while reducing unnecessary duplication of data. MySQL is the planned database management system.

## 2. Entities and Attributes

The proposed schema consists of eight entities:

| Entity | Primary Key | Main attributes | Purpose |
|---|---|---|---|
| Users | UserID | FullName, Email, Role | Store users and their roles |
| Projects | ProjectID | ProjectName, Description, OwnerID (FK) | Store projects owned by users |
| Sources | SourceID | Title, URL, SourceType | Store reference sources |
| Claims | ClaimID | ProjectID (FK), CreatedBy (FK), ClaimText, Status | Store statements to be supported and reviewed |
| Evidence | EvidenceID | ClaimID (FK), SourceID (FK), EvidenceText, EvidenceType | Link claims to sources with supporting evidence |
| Reviews | ReviewID | ClaimID (FK), ReviewerID (FK), Decision, ReviewDate | Record review decisions |
| Versions | VersionID | ClaimID (FK), ChangedBy (FK), PreviousText, ChangedAt | Record previous claim text and editor |
| ActivityLogs | LogID | UserID (FK), Action, ActionDate | Record user activity |

The full proposed types, nullability, keys, and constraints are documented in `DATA_DICTIONARY.md`.

## 3. Relationships and Cardinalities

The design currently specifies ten one-to-many (1:N) relationships:

1. Users → Projects (owns)
2. Users → Claims (creates)
3. Projects → Claims (contains)
4. Claims → Evidence (has evidence)
5. Sources → Evidence (provides)
6. Claims → Reviews (receives)
7. Users → Reviews (performs)
8. Claims → Versions (has versions)
9. Users → Versions (changes)
10. Users → ActivityLogs (generates)

The conceptual many-to-many (M:N) relationship between Claims and Sources is resolved through the associative entity Evidence. Each Evidence row refers to one Claim and one Source, and may include evidence-specific text and type. These cardinalities are **proposed** and must be checked against the final ER/EER diagram. See `RELATIONSHIPS.md` for the detailed descriptions and foreign-key targets.

## 4. Normalization

The proposed schema was organized to support the first three normal forms:

- **1NF:** Tables use primary keys, atomic column values, and separate rows for repeated evidence, reviews, and version events.
- **2NF:** Each proposed table uses a single-column primary key, so no partial dependency on a subset of that primary key can exist. Other candidate keys must still be considered.
- **3NF:** User, project, and source facts are stored in their respective tables rather than repeatedly copied into claims, evidence, or reviews. Formal 3NF verification requires checking all functional dependencies and business rules.

The current structure is designed toward 3NF; compliance is **not yet formally verified**. Detailed reasoning and caveats are in `NORMALIZATION.md`.

## 5. Database Architecture

For conceptual documentation, BASMAH uses a proposed three-tier model:

```text
User / Reviewer
       |
Presentation Layer (UI)
       |
Application Layer (Business Logic / API)
       |
MySQL Data Layer
  Users | Projects | Sources | Claims
  Evidence | Reviews | Versions | ActivityLogs
```

The UI and application layers are **conceptual**. The project has not yet demonstrated an implemented UI, API, or deployed three-tier application. If the final deliverable is SQL-only, this diagram remains illustrative rather than an implemented architecture. See `ARCHITECTURE.md`.

## 6. Integrity and Design Considerations

- Every proposed table has a primary key.
- Ten foreign-key columns represent the documented relationships.
- Users.Email is proposed as unique.
- Foreign keys are proposed as NOT NULL; exact deletion and update actions need team agreement.
- Deletion policies should protect review, evidence, and change history.
- Allowed status/decision/type values, timestamp defaults, and additional business constraints remain to be finalized.

## 7. ER/EER Diagram — Pending

**Insert the final approved ER/EER diagram here.**

The diagram has **not** been finalized. Before submission, verify entity names, all attributes, relationship participation/cardinality, and agreement with the MySQL schema. Do not mark the diagram as complete until it has been reviewed.

## 8. Conclusion

The current BASMAH design documents a proposed eight-entity relational model with explicit keys and relationships. Its separation of users, projects, claims, evidence, sources, reviews, and history provides a foundation for implementing a traceable evidence-management database. Final validation depends on completing the ER/EER diagram, agreeing on business rules, and verifying the team's MySQL scripts.

## Related design files

- `DATA_DICTIONARY.md` — detailed table/column definitions
- `RELATIONSHIPS.md` — relationship meanings, cardinalities, foreign keys
- `NORMALIZATION.md` — 1NF, 2NF, and 3NF analysis
- `ARCHITECTURE.md` — proposed conceptual architecture
