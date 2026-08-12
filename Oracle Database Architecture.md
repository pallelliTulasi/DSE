# Oracle Database Architecture â€“ 

## 1. Oracle Architecture

**Oracle Architecture** describes how the different components of an Oracle database work together to manage and process data.

### Oracle Database

Oracle is a relational database management system (RDBMS) that provides an open, comprehensive, integrated approach to information management.

---

## 2. Connecting to a Server

When a client connects to an Oracle database, the request can pass through a middle tier/application layer before reaching the database server.

### Basic flow

```text
Client  â†’  Middle Tier  â†’  Server
```

- The **client** sends requests.
- The **middle tier** can handle application processing.
- The **database server** processes the database operations.

---

# 3. Oracle Database Server Architecture

Oracle Database Server Architecture mainly consists of:

1. Database Instance
2. Memory Structure
3. Process Structure
4. Storage Structure

> **Note:** Process Architecture is intentionally excluded from these notes as requested.

### Architecture overview

```text
                  Oracle Database Server
                         |
              +----------+----------+
              |                     |
           Instance              Database
              |                     |
       +------+-------+       Storage Structures
       |              |
   Memory Structure  Process Structure
      (SGA)
```

---

# 4. Oracle Database Instance

An **Oracle database instance** works with the physical database files.

An Oracle instance mainly consists of:

- **Memory structures**
- **Background processes**

The instance provides the memory and processing environment required to access and manage the database.

---

## 5. Types of Configurations

Oracle database environments can be configured as:

### A. Non-Clustered System

- A **single instance** is connected to a **single database**.
- It uses **local storage** for data files.
- It is typical for **standalone servers**.

```text
Single Instance
      |
      v
Single Database
      |
      v
Local Storage
```

### B. Clustered System

- **Multiple instances** are connected to a **single database**.
- It uses **shared storage**.
- This architecture is the basis of **Oracle Real Application Clusters (RAC)**.
- RAC provides **scalability** and **high availability**.

```text
Instance 1 ----\
                \
Instance 2 ------> Single Database
                /
Instance 3 ----/
       |
  Shared Storage
```

---

# 6. Connecting to a Database Instance

When a user wants to interact with Oracle, the connection goes through processes and sessions.

### Connection

A **connection** is the communication link between the user process and the Oracle instance.

### Session

A **session** represents the logical state of a user inside the database.

```text
User
 |
 | Connection
 v
Oracle Instance
 |
 | Session
 v
Logical state of the user inside DB
```

---

# 7. Oracle Database Memory Structure

Oracle database memory is divided into two main areas:

1. **System Global Area (SGA)**
2. **Program Global Area (PGA)**

```text
Oracle Database Memory
        |
   +----+----+
   |         |
  SGA       PGA
Shared     Private
Memory     Memory
```

---

# 8. System Global Area (SGA)

The **System Global Area (SGA)** is a shared memory area associated with an Oracle instance.

It contains several important memory components.

## Components of SGA

### 1. Shared Pool

- Contains the **library cache**.
- Stores **SQL, PL/SQL code** and related information.
- Helps Oracle reuse previously parsed SQL and PL/SQL information.

### 2. Database Buffer Cache

- Stores data blocks read from the **data files**.
- Frequently accessed data can be kept in memory for faster access.

### 3. Redo Log Buffer

- A circular buffer.
- Stores information about **database changes**.
- The information is used for **redo/recovery** purposes.

### 4. Large Pool

Used for:

- Shared server operations
- Backup and restore operations
- Large allocations for parallel queries

### 5. Java Pool

- Stores **Java code and session-related data** used by the JVM.

### 6. Streams Pool

- Provides memory allocation for **Oracle Streams**.

---

# 9. Program Global Area (PGA)

The **Program Global Area (PGA)** is a private memory region associated with a server process.

It can contain information such as:

- Cursor state
- Sort area
- Hash area
- User session-related information
- SQL working areas

SQL working areas are useful for operations such as:

- Joins
- Sorts
- Bitmaps

```text
PGA
 |
 +-- Cursor State
 +-- Sort Area
 +-- Hash Area
 +-- User Session Data
 +-- SQL Working Areas
       |
       +-- Joins
       +-- Sorts
       +-- Bitmaps
```

---

# 10. Quick Revision

## Oracle Architecture

Main components:

- Database Instance
- Memory Structure
- Process Structure
- Storage Structure

## Oracle Instance

Mainly consists of:

- Memory structures
- Background processes

Works with:

- Physical database files

## Configurations

### Non-Clustered

- One instance
- One database
- Local storage
- Standalone server

### Clustered

- Multiple instances
- One database
- Shared storage
- Basis of Oracle RAC
- Scalability and high availability

## Connection and Session

- **Connection:** Communication link between the user process and Oracle instance.
- **Session:** Logical state of a user inside the database.

## Memory Structure

### SGA â€“ Shared

- Shared Pool
- Database Buffer Cache
- Redo Log Buffer
- Large Pool
- Java Pool
- Streams Pool

### PGA â€“ Private

- Cursor State
- Sort Area
- Hash Area
- User Session Data
- SQL Working Areas

---

# 11. One-Line Exam Definitions

**Oracle Architecture:** The structure that describes how Oracle components work together to manage and process database information.

**Database Instance:** A combination of memory structures and background processes that works with a physical Oracle database.

**SGA:** Shared memory area used by an Oracle instance.

**PGA:** Private memory area associated with a server process.

**Connection:** Communication link between a user process and an Oracle instance.

**Session:** Logical state of a user inside the database.

**Non-Clustered System:** A system where a single instance is connected to a single database.

**Clustered System:** A system where multiple instances are connected to a single database using shared storage.

**RAC:** Oracle Real Application Clusters, which provides scalability and high availability by allowing multiple instances to access one database.

**Shared Pool:** SGA component that contains the library cache and stores SQL/PL/SQL-related information.

**Database Buffer Cache:** SGA area that stores data blocks read from data files.

**Redo Log Buffer:** SGA area that stores information about database changes.

**Large Pool:** SGA area used for shared server, backup/restore, and large parallel-query allocations.

**Java Pool:** Memory area used for Java code and session-related data in the JVM.

**Streams Pool:** Memory area used for Oracle Streams.
