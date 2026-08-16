# Oracle Database Administration – Managing the Database Instance

## Overview

This repository contains notes and practical examples from the **Oracle Database 11g Release 2 – Database Administration** lesson **“Managing the Database Instance.”**

The material covers Oracle Enterprise Manager Database Control, SQL*Plus, initialization parameters, database startup and shutdown, the alert log, trace files, Dynamic Performance Views, and Data Dictionary Views.

## Learning Objectives

After completing this lesson, you should be able to:

- Start and stop the Oracle database and components
- Use Oracle Enterprise Manager
- Access a database with SQL*Plus
- Modify database initialization parameters
- Describe the stages of database startup
- Describe database shutdown options
- View the alert log
- Access Dynamic Performance Views

---

## Table of Contents

- [Management Framework](#management-framework)
- [Oracle Enterprise Manager Database Control](#oracle-enterprise-manager-database-control)
- [Other Oracle Tools](#other-oracle-tools)
- [SQLPlus](#sqlplus)
- [Calling SQLPlus from a Shell Script](#calling-sqlplus-from-a-shell-script)
- [Calling a SQL Script from SQLPlus](#calling-a-sql-script-from-sqlplus)
- [Initialization Parameter Files](#initialization-parameter-files)
- [Initialization Parameters](#initialization-parameters)
- [Viewing Initialization Parameters](#viewing-initialization-parameters)
- [Changing Initialization Parameters](#changing-initialization-parameters)
- [Database Startup](#database-startup)
- [Database Shutdown](#database-shutdown)
- [Shutdown Modes](#shutdown-modes)
- [Alert Log](#alert-log)
- [Trace Files and ADR](#trace-files-and-adr)
- [Dynamic Performance Views](#dynamic-performance-views)
- [Data Dictionary Views](#data-dictionary-views)
- [Practice Overview](#practice-overview)

---

## Management Framework

The Oracle Database 11g Release 2 management framework consists of:

- **Database instance**
- **Listener**
- **Management interface**
  - Database Control
  - Management Agent when using Grid Control

---

## Oracle Enterprise Manager Database Control

Oracle Enterprise Manager Database Control provides a graphical interface for managing and monitoring an Oracle database instance.

The Database Home page shown in the material provides information such as:

- General database status
- Host CPU
- Active sessions
- SQL response time
- Diagnostic summary
- Space summary
- High availability information
- Database instance properties

### Start Database Control

```bash
emctl start dbconsole
```

The material shows the command being executed after setting the Oracle environment, for example:

```bash
. oraenv
emctl start dbconsole
```

### Stop Database Control

```bash
emctl stop dbconsole
```

The Enterprise Manager Database Control interface can be accessed through the displayed Enterprise Manager URL and requires appropriate database credentials. The material shows connecting as `SYSDBA`.

---

## Other Oracle Tools

### SQL*Plus

SQL*Plus provides an additional interface to the database and can be used to:

- Perform database management operations
- Execute SQL commands to query data
- Insert data
- Update data
- Delete data

### SQL Developer

SQL Developer is described in the material as:

- A graphical user interface for accessing an Oracle Database instance
- Supporting development in both SQL and PL/SQL
- Available in the default installation of Oracle Database shown in the lesson

---

## SQL*Plus

SQL*Plus is:

- A command-line tool
- Used interactively or in batch mode

### Connect to Oracle using SQL*Plus

```bash
sqlplus hr
```

After executing the command, SQL*Plus asks for the password and connects to the Oracle database.

### Example SQL command

```sql
SELECT last_name
FROM employees;
```

---

## Calling SQL*Plus from a Shell Script

SQL*Plus can be called from a shell script to execute database operations in batch mode.

Example based on the material:

```bash
#!/bin/sh

sqlplus hr/hr <<EOF

SELECT COUNT(*) FROM employees;

UPDATE employees
SET salary = salary * 1.10;

COMMIT;

QUIT;
END

The example:

1. Connects to the database as `hr/hr`.
2. Counts employees.
3. Increases employee salary by 10%.
4. Commits the changes.
5. Exits SQL*Plus.

---

## Calling a SQL Script from SQL*Plus

A SQL script can be stored in a `.sql` file and executed from SQL*Plus.

Example `script.sql`:

```sql
SELECT *
FROM departments
WHERE location_id = 1400;

QUIT;
```

Execute it with:

```bash
sqlplus hr/hr @script.sql
```

---

# Initialization Parameter Files

The lesson shows Oracle initialization parameter files such as:

```text
spfileorcl.ora
```

or

```text
initorcl.ora
```

These files contain initialization parameters used when starting the Oracle instance.

---

# Initialization Parameters

The material presents basic and advanced initialization parameters.

## Basic Parameters

Examples include:

- `CONTROL_FILES`
- `DB_BLOCK_SIZE`
- `PROCESSES`
- `UNDO_TABLESPACE`

## Advanced Parameters

Examples include:

- `DB_CACHE_SIZE`
- `DB_FILE_MULTIBLOCK_READ_COUNT`
- `SHARED_POOL_SIZE`

### Parameter Examples

| Parameter | Description from the material |
|---|---|
| `CONTROL_FILES` | One or more control file names |
| `DB_FILES` | Maximum number of database files |
| `PROCESSES` | Maximum number of OS user processes that can simultaneously connect |
| `DB_BLOCK_SIZE` | Standard database block size used by all tablespaces |
| `DB_CACHE_SIZE` | Size of the standard block buffer cache |
| `PGA_AGGREGATE_TARGET` | Amount of PGA memory allocated to all server processes |
| `SHARED_POOL_SIZE` | Size of the shared pool, in bytes |
| `UNDO_MANAGEMENT` | Undo space management mode to be used |

### Memory-related Parameters

The lesson also illustrates:

- System Global Area (SGA)
- Program Global Area (PGA)
- `SGA_TARGET` – total size of SGA components
- `MEMORY_TARGET` – total size of system-wide usable memory
- Shared pool
- Database buffer cache
- Redo log buffer
- Large pool
- Java pool
- Streams pool
- Keep buffer pool
- Recycle buffer pool
- `nK` buffer cache

---

## Viewing Initialization Parameters

SQL*Plus can be used to display initialization parameters with `SHOW PARAMETER`.

```sql
SHOW PARAMETER SHARED_POOL_SIZE;
```

General parameter search:

```sql
SHOW PARAMETER <parameter_name>;
```

---

# Changing Initialization Parameters

Initialization parameters are divided into **static** and **dynamic** parameters.

## Static Parameters

According to the material, static parameters:

- Can be changed only in the parameter file
- Require restarting the instance before the change takes effect
- Account for about 110 parameters in the lesson

## Dynamic Parameters

Dynamic parameters:

- Can be changed while the database is online
- Can be altered at session level
- Can be altered at system level
- Are valid for the duration of the session or according to the `SCOPE` setting
- Are changed using `ALTER SESSION` and `ALTER SYSTEM`
- Account for about 234 parameters in the lesson

### ALTER SESSION Example

```sql
ALTER SESSION
SET NLS_DATE_FORMAT = 'mon dd yyyy';
```

```sql
SELECT SYSDATE FROM dual;
```

### ALTER SYSTEM Example

```sql
ALTER SYSTEM SET
SEC_MAX_FAILED_LOGIN_ATTEMPTS = 2
COMMENT = 'Reduce from 10 for tighter security.'
SCOPE = SPFILE;
```

---

# Database Startup

The lesson presents database startup as:

```text
STARTUP
   |
   v
NOMOUNT
   |
   v
MOUNT
   |
   v
OPEN
```

## NOMOUNT

- The instance is started.
- The database is not mounted.
- The control file has not yet been opened.

## MOUNT

- The instance is started.
- The control file is opened for the instance.
- The database is in the mounted state.

## OPEN

- The instance is started.
- The control file is opened.
- The database is open.
- All files are opened as described by the control file for the instance.

---

## Startup Options

Enterprise Manager provides startup options including:

- Start the database along with dependent resources
- Start the database only
- Mount the database
- Open the database
- Restrict access to the database
- Force the instance to start
- Perform recovery

The material also notes Oracle Restart and the `srvctl` utility for starting a database resource and dependent resources such as the listener or ASM instance.

---

# Database Startup and Shutdown Credentials

To change database status through Enterprise Manager, credentials may be required for:

### Host Credentials

- Operating-system username
- Operating-system password

### Database Credentials

- Database username
- Database password
- Connect as `SYSDBA` or `SYSOPER`

The material notes that the database status must be changed by logging in as `SYSDBA` or `SYSOPER`.

---

# Database Shutdown

Oracle Database provides:

- `NORMAL`
- `TRANSACTIONAL`
- `IMMEDIATE`
- `ABORT`

```sql
SHUTDOWN NORMAL;
SHUTDOWN TRANSACTIONAL;
SHUTDOWN IMMEDIATE;
SHUTDOWN ABORT;
```

## Shutdown Mode Comparison

| Mode | Behavior |
|---|---|
| `NORMAL` | Waits for currently connected users to disconnect |
| `TRANSACTIONAL` | Waits until current transactions end and then disconnects users |
| `IMMEDIATE` | Rolls back active transactions and disconnects connected users |
| `ABORT` | Instantaneous shutdown by aborting the database instance |

### Shutdown Characteristics

| Behavior | NORMAL | TRANSACTIONAL | IMMEDIATE | ABORT |
|---|---:|---:|---:|---:|
| Allows new connections | No | No | No | No |
| Waits for current sessions to end | Yes | No | No | No |
| Waits for current transactions to end | Yes | Yes | No | No |
| Forces a checkpoint and closes files | Yes | Yes | Yes | No |

### Shutdown and Recovery

For a clean shutdown, the material illustrates that:

- Uncommitted changes are rolled back where applicable.
- Database buffer cache contents are written to data files.
- Resources are released.
- No instance recovery is required after a clean shutdown.

For an abort/instance failure scenario:

- Modified buffers may not have been written to data files.
- Uncommitted changes may not have been rolled back.
- Online redo log files are used to reapply changes.
- Undo information is used to roll back uncommitted changes.
- Instance recovery is required.
- Resources are released.

---

# Oracle Restart / SRVCTL

The lesson shows use of the `srvctl` utility with Oracle Restart.

Example shown:

```bash
srvctl stop database -d orcl -o abort
```

---

# Alert Log

The Alert Log can be viewed from Oracle Enterprise Manager through the **Alert Log Content** area.

The material shows entries containing:

- Date and time
- Notification messages
- Background process information
- Database events
- Automatic SQL Tuning Advisor messages

The Alert Log is useful for reviewing important database events and notifications.

---

# Trace Files and ADR

## Trace Files

Each server and background process can write to an associated trace file.

Error information is written to the corresponding trace file.

## Automatic Diagnostic Repository (ADR)

The **Automatic Diagnostic Repository (ADR)** is described as a systemwide central tracing and logging repository.

It stores database diagnostic data such as:

- Traces
- Alert log
- Health Monitor reports

---

# Dynamic Performance Views

Dynamic Performance Views provide access to information about changing states of the Oracle instance.

The material lists:

- Instance memory structures
- Wait events
- Memory allocations
- Running SQL
- Undo usage
- Open cursors
- Redo log usage
- Other dynamic instance information

These views are commonly referred to as **V$ views** or **V-dollar views**.

### Example Queries

```sql
SELECT sid, logon_time
FROM v$session
WHERE machine = 'EDRSR25P1'
  AND logon_time > SYSDATE - 1;
```

```sql
SELECT sid, ctime
FROM v$lock
WHERE block > 0;
```

## Dynamic Performance View Considerations

According to the material:

- These views are owned by the `SYS` user.
- Different views are available at different times: instance started, database mounted, or database open.
- `V$FIXED_TABLE` can be queried to see the views.
- They are often called V-dollar views.
- Read consistency is not guaranteed because the data is dynamic.

```sql
SELECT * FROM v$fixed_table;
```

---

# Data Dictionary

The Oracle Data Dictionary contains information about database objects and related metadata.

The lesson illustrates:

- Schema
- Constraints
- Indexes
- Views
- Sequences
- Temporary tables
- Procedures
- Other database objects

```sql
SELECT * FROM dictionary;
```

---

# Data Dictionary Views

The lesson groups Data Dictionary Views into:

- `DBA_` views
- `ALL_` views
- `USER_` views

| View | Contents |
|---|---|
| `DBA_` | Everything in the database, with additional columns intended for DBA use where applicable |
| `ALL_` | Everything the user has privileges to see, including own objects and objects granted to the user |
| `USER_` | Everything the user owns |

---

# Data Dictionary Examples

### View Sequences

```sql
SELECT sequence_name,
       min_value,
       max_value,
       increment_by
FROM all_sequences
WHERE sequence_owner IN ('MDSYS', 'XDB');
```

### View Open User Accounts

```sql
SELECT username,
       account_status
FROM dba_users
WHERE account_status = 'OPEN';
```

The material also references:

```text
dba_indexes
```

and the SQL*Plus command:

```sql
DESCRIBE <object_name>;
```

---

# Quick Revision

### Management Framework

```text
Oracle Database 11g Release 2
│
├── Database Instance
├── Listener
└── Management Interface
    ├── Database Control
    └── Management Agent
```

### Initialization Parameters

```text
Initialization Parameters
├── Static
│   └── Restart required
└── Dynamic
    ├── Session level
    └── System level
```

### Startup

```text
STARTUP → NOMOUNT → MOUNT → OPEN
```

### Shutdown

```text
NORMAL
TRANSACTIONAL
IMMEDIATE
ABORT
```

### Diagnostics

```text
Diagnostics
├── Alert Log
├── Trace Files
└── ADR
```

### Views

```text
Oracle Views
├── V$ Dynamic Performance Views
└── Data Dictionary Views
    ├── DBA_
    ├── ALL_
    └── USER_
```

---

# Important Commands

```bash
# Enterprise Manager Database Control
emctl start dbconsole
emctl stop dbconsole

# SQL*Plus
sqlplus hr
sqlplus hr/hr @script.sql
```

```sql
-- Initialization parameters
SHOW PARAMETER SHARED_POOL_SIZE;
SHOW PARAMETER <parameter_name>;

-- Session parameter
ALTER SESSION SET NLS_DATE_FORMAT = 'mon dd yyyy';

-- System parameter
ALTER SYSTEM SET
SEC_MAX_FAILED_LOGIN_ATTEMPTS = 2
COMMENT = 'Reduce from 10 for tighter security.'
SCOPE = SPFILE;

-- Startup
STARTUP;

-- Shutdown
SHUTDOWN NORMAL;
SHUTDOWN TRANSACTIONAL;
SHUTDOWN IMMEDIATE;
SHUTDOWN ABORT;

-- Dynamic Performance Views
SELECT * FROM v$instance;
SELECT * FROM v$database;
SELECT * FROM v$fixed_table;

-- Data Dictionary
SELECT * FROM dictionary;
```

---

# Practice Overview

The final practice, **Practice 4: Managing the Oracle Instance**, covers:

1. Navigating in Enterprise Manager
2. Viewing and modifying initialization parameters
3. Stopping and starting the database instance
4. Viewing the alert log
5. Connecting to the database using SQL*Plus

---

## Source

Prepared directly from the uploaded **TM8 → Database Administration** material, lesson **Managing the Database Instance**. The source is an image/scanned PDF, so the README preserves the topics, terminology, organization, and examples presented in the slides rather than adding unrelated material.
