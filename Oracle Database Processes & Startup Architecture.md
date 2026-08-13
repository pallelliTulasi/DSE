# Oracle Database Processes & Startup Architecture

## 1. Overview

An Oracle database uses a set of **processes** to manage database operations, execute SQL statements, maintain memory, perform recovery, and communicate with client applications.

Oracle processes can be broadly classified into:

1. **User Processes**
2. **Oracle Database Processes**
3. **Daemon / Application Processes**

---

## 2. User Processes

A **user process** is a process created by a user application or tool to interact with the Oracle database.

### Main Functions

* Connects an application or tool to the Oracle server.
* Sends SQL statements and requests to the database.
* Receives the results returned by the database server.

**Example:** SQL Developer, an application, or another client tool can create a user process.

---

## 3. Oracle Database Processes

Oracle database processes are responsible for executing SQL, managing database resources, writing data, recovery, and other database operations.

They are mainly divided into:

### A. Server Processes

A **server process** responds to requests from a user process.

### Functions

* Receives SQL statements from the user process.
* Executes SQL statements.
* Performs required database operations.
* Returns the results to the user process.

### B. Background Processes

Background processes start when the Oracle instance starts and perform important maintenance and database management tasks automatically.

---

# 4. Important Oracle Background Processes

## 4.1 DBWn - Database Writer

**DBWn (Database Writer)** writes modified or dirty data blocks from the database buffer cache to the data files on disk.

### Key Point

**DBWn -> Writes data blocks to data files**

---

## 4.2 LGWR - Log Writer

**LGWR (Log Writer)** writes redo information from the redo log buffer to the online redo log files.

### Key Point

**LGWR -> Writes redo information to online redo log files**

LGWR is especially important during transaction commits because Oracle must ensure the required redo information is safely written before confirming the commit.

---

## 4.3 CKPT - Checkpoint Process

**CKPT (Checkpoint Process)** coordinates checkpoint activity.

### Functions

* Records checkpoint information.
* Signals DBWn to write required dirty buffers.
* Updates checkpoint information in control files and data file headers.

### Key Point

**CKPT -> Coordinates checkpoints**

---

## 4.4 SMON - System Monitor

**SMON (System Monitor)** performs instance recovery and other system-level cleanup tasks.

### Functions

* Performs instance recovery when required.
* Performs cleanup after instance startup.
* Recovers transactions that need recovery.
* Cleans up temporary segments when necessary.

### Key Point

**SMON -> Instance recovery and system cleanup**

---

## 4.5 PMON - Process Monitor

**PMON (Process Monitor)** monitors server processes and performs cleanup when a process fails.

### Functions

* Cleans up resources held by failed user/server processes.
* Releases database resources.
* Performs cleanup of failed processes.
* Helps with service registration in Oracle environments.

### Key Point

**PMON -> Cleans up after failed processes**

---

## 4.6 RECO - Recoverer Process

**RECO (Recoverer)** is used in distributed transaction recovery.

### Function

* Resolves **in-doubt distributed transactions** after failures.

### Key Point

**RECO -> Resolves in-doubt distributed transactions**

---

## 4.7 ARCn - Archiver Process

**ARCn (Archiver)** copies filled online redo log files to archive destinations when the database is running in **ARCHIVELOG mode**.

### Functions

* Archives redo log files after a log switch.
* Copies redo data to configured archive destinations.
* Supports media recovery and standby database operations.

### Key Point

**ARCn -> Online redo logs -> Archived redo logs**

---

# 5. Daemon / Application Processes

Oracle environments can also use daemon or application processes that provide supporting services.

Examples include:

* **Oracle Net Listener** - Accepts incoming client connection requests.
* **Oracle Grid Infrastructure daemons** - Provide cluster and high-availability services.
* **Cluster-related processes** - Support synchronization, monitoring, and cluster management.

---

# 6. Oracle Grid Infrastructure Processes

In an Oracle Grid Infrastructure environment, several processes and daemons are started to provide cluster and high-availability services.

Important examples include:

### 1. `ohasd.bin`

Oracle High Availability Services daemon.

### 2. `oraagent`

Agent process used to manage Oracle resources.

### 3. `orarootagent`

Root agent process used to manage resources requiring root privileges.

### 4. `diskmon`

Disk monitoring process.

### 5. `evmd` / Related Event Processes

Processes associated with cluster event management.

### 6. CSS-Related Processes

Cluster Synchronization Services processes help maintain cluster membership and synchronization.

> The exact set of Grid Infrastructure processes can vary depending on the Oracle version and configuration.

---

# 7. Oracle Instance, Listener, and Database

Three important components to understand are:

### ASM Instance

**ASM (Automatic Storage Management)** manages database storage using ASM disk groups.

### Oracle Listener

The **Oracle Net Listener** accepts incoming client connection requests and helps establish connections to the database service.

### Database Instance

The **Oracle instance** consists of the memory structures and background processes required to access and manage the database.

---

# 8. Oracle Process Startup Sequence

A simplified Oracle/Grid Infrastructure startup sequence is:

## Step 1: Operating System Initialization

* When the operating system boots, its initialization system starts.
* The initialization system triggers the startup of required Oracle/Grid Infrastructure services.

## Step 2: Grid Infrastructure Startup

* Grid Infrastructure startup scripts/services are invoked.
* Required Grid Infrastructure daemons are started.
* High-availability and cluster services are initialized.

## Step 3: Grid Infrastructure Daemons

Important services/processes are started to support:

* High availability
* Resource management
* Cluster synchronization
* Disk monitoring
* Cluster event management

## Step 4: ASM Instance, Listener, and Database Instance

After the required infrastructure is available:

1. **ASM instance** starts when ASM is configured.
2. **Oracle Net Listener** starts and accepts client connection requests.
3. **Oracle database instance** starts.
4. The database is opened according to its configured startup mode.

## Step 5: User-Defined Applications

Finally, the database environment becomes ready for user-defined applications and client connections.

---

# 9. Simple Startup Flow

```text
Operating System Boot
        |
        v
OS Initialization
        |
        v
Grid Infrastructure Startup
        |
        v
Grid Infrastructure Daemons
        |
        v
ASM Instance
        |
        v
Oracle Listener
        |
        v
Oracle Database Instance
        |
        v
Database Ready
        |
        v
User Applications / Client Connections
```

---

# 10. Quick Revision Table

| Process        | Full Form                    | Main Function                               |
| -------------- | ---------------------------- | ------------------------------------------- |
| User Process   | -                            | Sends requests to Oracle                    |
| Server Process | -                            | Executes SQL and serves user requests       |
| DBWn           | Database Writer              | Writes dirty buffers to data files          |
| LGWR           | Log Writer                   | Writes redo information to online redo logs |
| CKPT           | Checkpoint                   | Coordinates checkpoint activity             |
| SMON           | System Monitor               | Instance recovery and cleanup               |
| PMON           | Process Monitor              | Cleans up failed processes                  |
| RECO           | Recoverer                    | Resolves in-doubt distributed transactions  |
| ARCn           | Archiver                     | Archives filled redo logs                   |
| Listener       | Oracle Net Listener          | Accepts client connection requests          |
| ASM            | Automatic Storage Management | Manages ASM storage                         |

---

# 11. Exam Important Points

* **DBWn -> Data files**
* **LGWR -> Online redo log files**
* **CKPT -> Checkpoint coordination**
* **SMON -> Instance recovery**
* **PMON -> Failed process cleanup**
* **RECO -> Distributed transaction recovery**
* **ARCn -> Archive redo logs**
* **Listener -> Client connection requests**
* **ASM -> Storage management**
* **Server process -> Executes SQL for user requests**

## One-Line Memory Trick

**DBWn = Data, LGWR = Logs, CKPT = Checkpoint, SMON = System Recovery, PMON = Process Cleanup, RECO = Recovery of Distributed Transactions, ARCn = Archive.**
