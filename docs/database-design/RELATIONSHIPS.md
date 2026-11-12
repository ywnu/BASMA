# BASMAH — Relationship Descriptions

**Project:** Evidence & Source Tracking System  
**Owner:** Diyam — Database Design  
**Status:** Proposed; requires ER Diagram and team review before approval.

## Relationship inventory

All ten proposed foreign-key relationships are **one-to-many (1:N)** from the referenced parent entity to the referencing child entity. The child-side FK is currently `NOT NULL`; a parent may have zero or more children. These describe logical cardinalities, not a claim that ERDPlus diagram connections have been verified.

| # | Parent (1) | Child (N) | Suggested ER label | Business meaning | FK implementation |
|---|---|---|---|---|---|
| 1 | Users | Projects | Owns | One user may own many projects; each project has one owner. | Projects.OwnerID → Users.UserID |
| 2 | Users | Claims | Creates | One user may create many claims; each claim has one creator. | Claims.CreatedBy → Users.UserID |
| 3 | Projects | Claims | Contains | One project may contain many claims; each claim belongs to one project. | Claims.ProjectID → Projects.ProjectID |
| 4 | Claims | Evidence | HasEvidence | One claim may have many evidence entries; each evidence entry refers to one claim. | Evidence.ClaimID → Claims.ClaimID |
| 5 | Sources | Evidence | Provides | One source may be referenced by many evidence entries; each evidence entry refers to one source. | Evidence.SourceID → Sources.SourceID |
| 6 | Claims | Reviews | Receives | One claim may receive many reviews; each review is for one claim. | Reviews.ClaimID → Claims.ClaimID |
| 7 | Users | Reviews | Performs | One user may perform many reviews; each review has one reviewer. | Reviews.ReviewerID → Users.UserID |
| 8 | Claims | Versions | HasVersions | One claim may have many version records; each version belongs to one claim. | Versions.ClaimID → Claims.ClaimID |
| 9 | Users | Versions | Changes | One user may make many changes; each version record identifies one user. | Versions.ChangedBy → Users.UserID |
| 10 | Users | ActivityLogs | Generates | One user may have many activity log records; each log currently refers to one user. | ActivityLogs.UserID → Users.UserID |

## Important conceptual relationship

`Claims` and `Sources` have a **many-to-many (M:N)** conceptual association resolved through the associative entity `Evidence`. `Evidence` has its own primary key (`EvidenceID`) and descriptive attributes (`EvidenceText`, `EvidenceType`). Thus the relational schema uses `Claims (1) → Evidence (N)` and `Sources (1) → Evidence (N)` rather than a direct FK between `Claims` and `Sources`.

## Participation and implementation notes

- With the currently proposed `NOT NULL` FKs, each child row must reference exactly one existing parent; a parent can have zero related child rows. This is a proposed constraint, not a verified diagram setting.
- `Users` can play multiple roles (owner, creator, reviewer, editor, activity actor). These are distinct relationships and should be drawn separately.
- Do not confuse `Users → Reviews` (`Performs`) with `Claims → Reviews` (`Receives`): each review needs both a reviewer and a reviewed claim.
- Review `ActivityLogs.UserID` nullability for automated/system actions.
- Decide on `ON DELETE` / `ON UPDATE` behavior with Jumanah and Hanan. Avoid automatic deletion of audit, evidence, reviews, and version history by default.
- The approved ERD, data dictionary, and SQL DDL must use identical FK targets and cardinalities.

**Status:** Documentation drafted; not implemented or tested.
