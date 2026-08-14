# Oracle Database Storage Architecture

## 1. Database Storage Architecture

Database storage architecture in Oracle describes how database files and
storage structures are organized and managed.

### Main Storage Components

1.  **Control Files**
    -   Store important database metadata and the database structure.
    -   Maintain information such as database name, datafile names, redo
        log information, and checkpoints.
2.  **Data Files**
    -   Store the actual user and application data.
    -   Data files contain tables, indexes, and other database objects.
3.  **Online Redo Log Files**
    -   Store information about changes made to the database.
    -   Used for database recovery.
4.  **Parameter File**
    -   Stores initialization parameters required to start an Oracle
        database instance.
    -   Examples include memory and database configuration parameters.
5.  **Password File**
    -   Used for authentication of privileged users, especially for
        remote administrative connections.
6.  **Backup Files**
    -   Used to restore and recover the database when required.
7.  **Archived Redo Log Files**
    -   Archived copies of filled online redo log files.
    -   Used for media recovery and point-in-time recovery.
8.  **Alert Log and Trace Files**
    -   Store information about database errors, events, warnings, and
        diagnostic activities.
    -   Useful for monitoring and troubleshooting.

------------------------------------------------------------------------

## 2. Logical Database Structure

The **logical database structure** describes how data is logically
organized inside an Oracle database.

### Hierarchy

**Tablespace → Segment → Extent → Oracle Data Block**

### 2.1 Tablespace

-   A tablespace is a logical storage container.
-   It groups related logical storage structures and is associated with
    one or more physical data files.

### 2.2 Segment

-   A segment is the storage allocated for a specific database object.
-   Examples include table segments and index segments.

### 2.3 Extent

-   An extent is a set of contiguous data blocks allocated to a segment.
-   A segment can contain multiple extents.

### 2.4 Oracle Data Block

-   The Oracle data block is the smallest logical unit of database
    storage.
-   It is the basic unit in which Oracle reads and writes data.

------------------------------------------------------------------------

## 3. Physical Database Structure

The **physical database structure** represents the actual files and
storage units maintained on disk.

### Main Physical Structures

1.  **Data Files**
    -   Store the actual database data.
    -   Physically represent the storage associated with tablespaces.
2.  **Control Files**
    -   Store database metadata and information about the physical
        database structure.
3.  **Redo Log Files**
    -   Store information about changes made to the database.
    -   Essential for instance and media recovery.
4.  **Disk Blocks**
    -   Represent the physical storage units used by the operating
        system.
    -   Oracle data blocks are stored within physical storage.

------------------------------------------------------------------------

## 4. Logical vs Physical Database Structure

  -----------------------------------------------------------------------
  Logical Structure                   Physical Structure
  ----------------------------------- -----------------------------------
  Describes how data is organized     Describes actual files/storage on
  logically                           disk

  Tablespace                          Data files

  Segment                             Physical storage for database
                                      objects

  Extent                              Allocation of physical storage

  Oracle data block                   Data stored in physical
                                      blocks/files
  -----------------------------------------------------------------------

------------------------------------------------------------------------

## 5. Automatic Storage Management (ASM)

**Automatic Storage Management (ASM)** is an Oracle storage management
feature used to manage database files and storage.

ASM simplifies storage administration by providing automatic file
placement, striping, and mirroring.

### ASM Components

#### 5.1 ASM Disk

-   A physical storage device or partition presented to ASM.
-   ASM uses disks to store database files.

#### 5.2 ASM Allocation Unit (AU)

-   The smallest unit of storage that ASM allocates to a file.
-   ASM manages storage by allocating allocation units from ASM disk
    groups.

#### 5.3 ASM Extent

-   A group of allocation units used to allocate space for an ASM file.
-   ASM can manage file extents automatically.

#### 5.4 ASM Disk Group

-   A collection of one or more ASM disks.
-   Provides a logical storage pool for database files.
-   ASM distributes and manages files across the disks in the disk
    group.

#### 5.5 ASM File

-   A database file stored and managed by ASM.
-   Examples include data files, control files, online redo logs, and
    other Oracle database files.

### ASM Storage Flow

**ASM Disks → ASM Disk Group → Allocation Units / Extents → ASM Files**

------------------------------------------------------------------------

## 6. Quick Revision

-   **Control File** → Database structure and metadata
-   **Data File** → Actual database data
-   **Online Redo Log** → Records database changes
-   **Parameter File** → Instance startup parameters
-   **Password File** → Privileged-user authentication
-   **Backup Files** → Restore and recovery
-   **Archived Redo Logs** → Recovery using archived redo information
-   **Alert/Trace Files** → Errors, events, and diagnostics
-   **Tablespace** → Logical storage container
-   **Segment** → Storage for a database object
-   **Extent** → Set of contiguous data blocks
-   **Oracle Data Block** → Smallest logical storage unit
-   **ASM Disk Group** → Logical storage pool made from ASM disks
-   **ASM File** → Database file managed by ASM
