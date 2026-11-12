# BASMAH — Database Architecture and Network Type

**Project:** BASMAH – Evidence & Source Tracking System  
**Owner:** Diyam — Database Design  
**Status:** Database design documentation. Actual runtime environment must be confirmed with the MySQL implementation team.

## 1. Database architecture

BASMAH is designed around a **centralized relational database** managed by **MySQL**. The proposed schema stores users, projects, claims, sources, evidence, reviews, version history, and activity logs in one database.

The current course deliverable focuses on the database schema and required SQL features: cascade behavior, triggers, transactions, indexing, and views. **A user interface or a database-to-interface connection is not required** under the instructor's updated instructions.

### Database-only logical structure

```text
[MySQL Client / SQL Editor]
            |
            v
[MySQL Database Server]
            |
            v
[BASMAH Centralized Database]
 Users | Projects | Sources | Claims
 Evidence | Reviews | Versions | ActivityLogs
```

The diagram illustrates a database administration/testing workflow. It does not imply that a custom web application, API, or distributed system has been implemented.

## 2. Network type and access model

- **Database organization:** Centralized, not distributed.
- **DBMS:** MySQL.
- **Access model:** MySQL follows a client–server model, even when the client and server run on the same computer.
- **Local development scenario:** A MySQL client or SQL editor can connect to a MySQL server on the same computer using `localhost`. This does **not** require a LAN or WAN.
- **LAN/WAN:** No LAN or WAN deployment is required for this project. If the database is later hosted on another machine, a LAN or other network connection could be configured, but that is outside the current project scope.

**Implementation verification:** The team should confirm whether the final SQL scripts were actually executed on a local MySQL server before describing `localhost` as the tested setup.

## 3. Database organization

| Functional area | Tables | Purpose |
|---|---|---|
| Identity and ownership | Users, Projects | Store users and projects they own |
| Claims and sources | Claims, Sources, Evidence | Connect claims with supporting evidence and sources |
| Review workflow | Reviews | Store review decisions |
| History and auditing | Versions, ActivityLogs | Record claim changes and user actions |

The Evidence table resolves the conceptual many-to-many association between Claims and Sources.

## 4. Integrity and SQL responsibilities

- Primary keys uniquely identify records.
- Foreign keys link related tables.
- The proposed `Users.Email` field is unique.
- Cascade and restriction policies must be reviewed to avoid unintended loss of evidence or history.
- Transactions, triggers, views, and indexes will be implemented and tested in MySQL by the assigned teammates.
- Final column names and foreign-key directions must match the approved EER diagram and the actual MySQL schema.

## 5. Scope and validation

**Designed/documented:** Centralized MySQL database structure, eight proposed tables, relationships, and an access/network explanation.

**Not required:** Custom frontend, API, or interface-to-database integration.

**Pending confirmation:** Final EER-to-relational mapping, executed MySQL scripts, actual test environment, and integration results.

**Related documents:** `DATA_DICTIONARY.md`, `RELATIONSHIPS.md`, `NORMALIZATION.md`.
