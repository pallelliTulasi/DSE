## CONSTRAINTS
* Oracle server uses constraints to prevent invalid data entry into tables.
* Types: `NOT NULL`, `UNIQUE`, `PRIMARY KEY`, `FOREIGN KEY`, `CHECK`.
* Create a constraint at either same time as creation of table or after the creation of table.
* Define constraint at col or table level.
* View a constraint in data dictionary.
* If you do not name your constraint, the oracle server generates a name with the format `SYS_Cn` (n->integer).

### Constraint Levels
* **Column-level constraints:**
    * define along with the column definition.
    * applied only to that specified column.
* **Table-level constraints:**
    * define after all column definitions.
    * can be applied to one or more columns.

**Note:** `NOT NULL` constraint must be defined at col level.

### Types of Constraints

**NOT NULL constraint:**
* must always be defined at column level.
* Ensure a column can't contain NULL values.

**CHECK constraint:**
* a condition that each row must satisfy.
* The condition must evaluate to TRUE or UNKNOWN for row to be accepted.
* if condition evaluates to FALSE, the row is rejected.

**UNIQUE constraint:**
* It requires every value in column to be unique.
* No two rows can have duplicate values in specified col.

**PRIMARY KEY constraint:**
* uniquely identifies each row in table.
* A table can have only one pk.
* can be defined at column or table level.

**FOREIGN KEY constraint:**
* A Fk in one table refers to Pk of another table.
* maintains referential integrity.
* prevents entering values that do not exists in parent table.
* Fk can contain NULL values unless specifies as NOT NULL.
* can be determines as column or table level.

---

## Alter/Enable/Disable/Drop Constraints

### Alter table:
* Add columns, change the data type of column.
* add constraints, remove/rename a column (or) table.
* you want to put table into read-only mode.
* Syntax: `ALTER TABLE table_name alter_table_clause`

### DROP table:
* drop will delete an object.
* Dropping an object, rather than 'deleting'.
* delete relates to data.
* Syntax: `DROP TABLE table_name;`

---

## READ ONLY MODE
* specify `READ ONLY MODE` to place a table in read-only mode.
* When table is in read only mode, you **can't issue** any DML statements (affect the table).
* You **can issue** DDL statements (don't modify any data in table).
* Operations on indexes associated with the table are allowed.
* `READ/WRITE` to return a read only table to read/write mode.

### FLASHBACK TABLE
* `FLASHBACK TABLE` statement to restore a dropped table from the recycle bin.
