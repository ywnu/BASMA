# BASMA — Evidence & Source Tracking System

BASMA is a MySQL database course project for organizing projects, claims, sources, evidence, reviews and version history. The team is building the database deliverable first; AI and a web interface are future possibilities, not part of the current committed scope.

## Team ownership

| Member | Responsibility | Branch |
|---|---|---|
| Deem | Database Design | `feature/deem-database-design` |
| Jumanah | Database Structure & Integration | `feature/jumanah-schema-integration` |
| Aseel | SQL Operations & Sample Data | `feature/aseel-sql-operations` |
| Hanan | Integrity & Transactions | `feature/hanan-integrity-transactions` |
| Bayader | Views, Triggers & Indexing | `feature/bayader-views-triggers-indexes` |

## Work plan (Weeks 12–14)

| Week | Goal | Required checkpoint |
|---|---|---|
| 12 | Agree on EER, entities/columns, schema, and sample-data plan | Approved design and initial CREATE TABLE scripts |
| 13 | Implement SQL operations, integrity/transactions, views/triggers/indexes | Executable scripts and evidence for every lab topic |
| 14 | Integrate, test, document, and prepare the presentation | Reproducible final database, screenshots, report, slides |

## Individual deliverables

### Deem — Database Design
1. Define the BASMA entities and attributes: Users, Projects, Sources, Claims, Evidence, Reviews, Versions; propose ActivityLogs if needed.
2. Draw the EER diagram with primary keys, foreign keys, relationship cardinalities and optional/mandatory participation.
3. Explain the relationships and normalize the design; coordinate final column names and data types with Jumanah.
4. Export the EER diagram and data dictionary for the report.

**Deliverables:** EER diagram, entity/relationship description, data dictionary, and design section for the report.

**Working branch:** `feature/deem-database-design`

### Jumanah — Database Structure & Integration
1. Review Deem's approved EER diagram and map it to a relational schema.
2. Implement MySQL CREATE TABLE scripts with PK, FK, data types, NOT NULL and other agreed constraints.
3. Define a safe creation order and deletion behavior; do not cascade-delete verification history without approval.
4. Integrate other members' scripts, resolve dependencies, and produce a reproducible database setup script.
5. Run end-to-end setup and coordinate final integration and tests.

**Deliverables:** Relational schema, MySQL DDL scripts, integrated setup/run order, integration test evidence.

**Working branch:** `feature/jumanah-schema-integration`

### Aseel — SQL Operations & Sample Data
1. Create representative INSERT data for all approved tables, respecting foreign-key order.
2. Write SELECT queries with filtering, sorting, JOINs, GROUP BY and aggregates as appropriate.
3. Provide tested UPDATE and DELETE examples with safe conditions and document expected results.
4. Capture query outputs/screenshots and describe what each query demonstrates.
5. Coordinate test data IDs and column names with Jumanah.

**Deliverables:** Seed-data SQL, operations/queries SQL, results and screenshots.

**Working branch:** `feature/aseel-sql-operations`

### Hanan — Integrity & Transactions
1. Test primary/foreign-key, uniqueness, NOT NULL and other approved integrity constraints.
2. Demonstrate an agreed ON DELETE CASCADE relationship only where deletion is safe; document restricted alternatives.
3. Prepare MySQL START TRANSACTION, COMMIT and ROLLBACK demonstrations.
4. Demonstrate sequential/concurrent transaction behavior using separate sessions where applicable.
5. Record expected and actual results, including failed constraint tests.

**Deliverables:** Integrity tests, transaction scripts, cascade/restrict examples, test evidence.

**Working branch:** `feature/hanan-integrity-transactions`

### Bayader — Views, Triggers & Indexing
1. Create useful MySQL views based on the approved schema (e.g., claims with sources/review status).
2. Implement an AFTER INSERT audit trigger with a dedicated log table agreed with Jumanah.
3. Create indexes on selected lookup/join columns and explain the rationale.
4. Test views, trigger audit rows and SHOW INDEX output; include evidence.
5. Avoid duplicate indexes already provided by primary/unique keys.

**Deliverables:** Views SQL, trigger/audit SQL, index SQL, screenshots and tests.

**Working branch:** `feature/bayader-views-triggers-indexes`

## Collaboration rules

1. Work only in your assigned feature branch; do not commit directly to `main`.
2. Coordinate table names, columns, data types and keys before writing dependent SQL.
3. Use MySQL syntax throughout; do not mix SQL Server syntax (such as `GO`, `IDENTITY`, or `TOP`).
4. Submit a pull request into `main` when your scripts and documentation are ready. Jumanah coordinates integration; review changes before merging.
5. Include instructions for running SQL scripts, test evidence, and screenshots of required outputs.
6. Preserve important verification/audit history; do not apply cascading deletes to evidence, reviews or versions by default.

## Proposed entities (subject to EER approval)

`Users`, `Projects`, `Sources`, `Claims`, `Evidence`, `Reviews`, `Versions`; `ActivityLogs` may be added for audit triggers. Final columns and relationships must match the approved EER.

## Final run order (to be confirmed after implementation)

1. Create schema/tables and constraints.
2. Insert seed data in FK-safe order.
3. Run queries and operation demonstrations.
4. Create and test views, triggers and indexes.
5. Run integrity, cascade and transaction demonstrations.
6. Capture test results for the report.

**Status:** This README documents the implementation plan. SQL deliverables are not claimed complete until committed and tested.
