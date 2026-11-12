# BASMAH — Data Dictionary

**Project:** Evidence & Source Tracking System  
**Owner:** Diyam — Database Design  
**Status:** Proposed design, pending team approval and ERD alignment. Not implemented or tested in MySQL.

This dictionary is the working specification for Jumanah's MySQL `CREATE TABLE` scripts. It documents 8 tables, 36 columns, 8 primary keys, and 10 foreign-key columns.

## Users

System users.

| Column | MySQL data type | Proposed constraints | Meaning |
|---|---|---|---|
| UserID | INT | PK, AUTO_INCREMENT | Unique user identifier |
| FullName | VARCHAR(100) | NOT NULL | User full name |
| Email | VARCHAR(150) | NOT NULL, UNIQUE | User email |
| Role | VARCHAR(30) | NOT NULL | User role |

## Projects

Projects owned by users.

| Column | MySQL data type | Proposed constraints | Meaning |
|---|---|---|---|
| ProjectID | INT | PK, AUTO_INCREMENT | Unique project identifier |
| ProjectName | VARCHAR(150) | NOT NULL | Project title |
| Description | TEXT | NULL | Optional project description |
| OwnerID | INT | FK → Users.UserID, NOT NULL | Project owner |

## Sources

Reference sources.

| Column | MySQL data type | Proposed constraints | Meaning |
|---|---|---|---|
| SourceID | INT | PK, AUTO_INCREMENT | Unique source identifier |
| Title | VARCHAR(200) | NOT NULL | Source title |
| URL | VARCHAR(500) | NULL | Optional source URL |
| SourceType | VARCHAR(50) | NOT NULL | Source category |

## Claims

Statements tracked within projects.

| Column | MySQL data type | Proposed constraints | Meaning |
|---|---|---|---|
| ClaimID | INT | PK, AUTO_INCREMENT | Unique claim identifier |
| ProjectID | INT | FK → Projects.ProjectID, NOT NULL | Parent project |
| CreatedBy | INT | FK → Users.UserID, NOT NULL | Claim author |
| ClaimText | TEXT | NOT NULL | Claim statement |
| Status | VARCHAR(30) | NOT NULL | Claim workflow status |

## Evidence

Evidence linked to claims and sources.

| Column | MySQL data type | Proposed constraints | Meaning |
|---|---|---|---|
| EvidenceID | INT | PK, AUTO_INCREMENT | Unique evidence identifier |
| ClaimID | INT | FK → Claims.ClaimID, NOT NULL | Supported claim |
| SourceID | INT | FK → Sources.SourceID, NOT NULL | Evidence source |
| EvidenceText | TEXT | NOT NULL | Supporting excerpt or description |
| EvidenceType | VARCHAR(30) | NOT NULL | Evidence category |

## Reviews

Claim review decisions.

| Column | MySQL data type | Proposed constraints | Meaning |
|---|---|---|---|
| ReviewID | INT | PK, AUTO_INCREMENT | Unique review identifier |
| ClaimID | INT | FK → Claims.ClaimID, NOT NULL | Reviewed claim |
| ReviewerID | INT | FK → Users.UserID, NOT NULL | Reviewer |
| Decision | VARCHAR(30) | NOT NULL | Review decision |
| ReviewDate | DATETIME | NOT NULL | Review timestamp |

## Versions

Claim edit history.

| Column | MySQL data type | Proposed constraints | Meaning |
|---|---|---|---|
| VersionID | INT | PK, AUTO_INCREMENT | Unique version identifier |
| ClaimID | INT | FK → Claims.ClaimID, NOT NULL | Edited claim |
| ChangedBy | INT | FK → Users.UserID, NOT NULL | User making change |
| PreviousText | TEXT | NOT NULL | Prior claim text |
| ChangedAt | DATETIME | NOT NULL | Change timestamp |

## ActivityLogs

User action audit trail.

| Column | MySQL data type | Proposed constraints | Meaning |
|---|---|---|---|
| LogID | INT | PK, AUTO_INCREMENT | Unique log identifier |
| UserID | INT | FK → Users.UserID, NOT NULL | Actor |
| Action | VARCHAR(100) | NOT NULL | Action description |
| ActionDate | DATETIME | NOT NULL | Action timestamp |

## Design decisions to confirm

- Confirm all table/column names and nullability with Jumanah before implementation.
- Define allowed values and defaults for `Claims.Status`, `Reviews.Decision`, `Evidence.EvidenceType`, `Sources.SourceType`, and `Users.Role`.
- Confirm delete/update rules with Hanan. Do **not** cascade-delete evidence, reviews, or versions by default; protect audit history.
- Review whether `ActivityLogs.UserID` should be nullable for system-generated actions and how user deletion affects audit logs.
- Confirm that a `Source` can support many `Claims` via `Evidence`, and that duplicate evidence links are handled appropriately.
- Agree on timestamp defaults, character set/collation, and optional URL validation.
- Align final ERD cardinalities and participation with the approved schema before calling this final.

**Scope:** Documentation only. No SQL execution or database testing has been performed.
