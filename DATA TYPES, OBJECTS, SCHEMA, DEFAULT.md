# Database Concepts & SQL Notes

## Datatypes
* Can't store everything in a text value - inefficient and hard to manipulate.

**NUMBER:**
* Numeric data type, stores whole numbers and decimals.
* `NUMBER(precision, scale)`
    * **precision:** total no. of digits
    * **scale:** no. of decimals

**Other Datatypes:**
* **DATE:** stores date & time.
* **VARCHAR2(size):** min size 1 to max size 4000. (variable len)
* **CHAR(size):** 1 to 2000 (fixed length char data).
* **LONG:** variable length char data (up to 2gb)
* **CLOB:** character data (up to 4GB)
* **RAW & LONG RAW:** Raw binary data.
* **BLOB:** binary data (up to 4gb)
* **BFILE:** binary data stored in external file (4gb)
* **ROWID:** a base-64 - represent unique address
* **TIMESTAMP:** date with fractional seconds.
* **INTERVAL YEAR TO MONTH:** stored as an interval of years and months.
* **INTERVAL DAY TO SECOND:** stored as an interval of days.

---

## Database Objects
* Each structure should be outlined in database design so that it can be created during the build stage of database development.

| Object | Description |
| :--- | :--- |
| **Table** | stores data. |
| **View** | subset of data from one or more tables. |
| **Schema** | same as owner's name |
| **Sequence** | generates numeric values |
| **Synonym** | gives alternative name to an object. |

### Rule for Table and Column Names:
* Begin with a letter.
* Contains only `A-Z`, `a-z`, `0-9`, `_`, `$`, `#`.
* No duplicate the name of another object by same user.
* Not be an oracle server reserved word.
* Be 1-30 characters long.
* Names are not case sensitive.

### Schema:
* Tables belonging to other users are not in user's schema.
* You should use the owner's name as prefix to those tables.
* A schema is a collection of logical structures of data or schema object.
* A schema is owned by a database user and has the same name as that user. Each user owns a single schema.
* Schema objects can be created and manipulated with SQL.
* Includes tables, views, synonyms, sequences, stored procedures, indexes, clusters and database links.
* If the table doesn't belong to the user, the owner name must be prefixed to the table.

---

## DEFAULT Keyword
* When you specify/define a table, you can specify that the column should be given a default value by using `DEFAULT`.
* This option prevents null values from entering the columns when a row is inserted without a value for the column.
* The default value is inserted only when **no value** is provided for that column during `INSERT`.
* A default value can be a literal value, expression, sql function (Eg: sysdate, user).
* A default value can't be another col_name, a pseudocolumn (eg: NEXTVAL, CURRVAL).
* Default val must match the data type of column.

**Example:**
`Insert into hire_dates values (45, NULL);`

### Additional Constraint Note:
* A row can't be deleted if its PK (Primary Key) is being used as a foreign key in another table.
