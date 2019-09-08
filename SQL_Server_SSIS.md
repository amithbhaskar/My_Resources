# SQL Server / SSIS
Last Updated on : 12 May 2019

Microsoft SQL Server is a relational database management system developed by Microsoft. As a database server, it is a software product with the primary function of storing and retrieving data as requested by other software applications—which may run either on the same computer or on another computer across a network (including the Internet).

## Contents
[SQL Server Features](#sql-server-features)
- [Limitations](#limitations)
- [Architecture](#architecture)
- [Datatypes](#datatypes)

Basics SQL
- [Types of Creating a Table](#types-&-variants-of-creating-a-table)
- [DDLs - Data Definition Language](#ddls---data-definition-language)
- [DMLs - Data Manipulation Language](#dmls---data-manipulation-language)
- [DCL - Data Control Language](#dcl---data-control-language)
- [TCL - Transaction Control Language)](#tcl---transaction-control-language)
- [Constraints](#constraints)
- [Foreign Key Constraints](#foreign-key-constraints)
- [Data Dictionary](#data-dictionary)
- [Joins in SQL](#joins-in-sql)
- [Merge Join](#merge-join)
- [Subquery](#subquery)
- [Union vs Union All](#union-vs-union-all)
- [LOOPs](#loops)
- [IF ELSE Condition](#if-else-condition)
- [WHILE Loop](#while-loop)
- [CASE ](#case)
- [Between Clause](#between-clause)
- [Sequences](#sequences)
- [Stored Procedures](#stored-procedures)
- [Clustered and Non-Clustered Index](#clustered-and-non-clustered-index)
- [Window functions ](#window-functions)
- [Exception Handler](#exception-handler)

Intermediate SQL : Conversions & Data Mockups
- [Schema](#schema)
- [Convert Queries](#convert-queries)
- [Dateadd queries](#dateadd-queries)
- [Financial Year](#financial-year)
- [Generate Random String](#generate-random-string)
- [Update by Row Number](#update-by-row-number)
- [Update Random date](#update-random-date)
- [Random number with normalized distribution:](#random-number-with-normalized-distribution)
- [Split a string to a table using T-SQL ](#split-a-string-to-a-table-using-t-sql)
- [STRING_SPLIT ( string , separator)](#string_split-(-string-,-separator))
- [dbo.Split()](#dbo.split())
- [Deduping Data](#deduping-data)
- [Triggers](#triggers)
- [Dynamic SQL](#dynamic-sql)
- [Pagination](#pagination)
- [Bulk Insert](#bulk-insert)
- [SSMS Tricks](#ssms-tricks)
- [Data Dictionary](#data-dictionary)
- [OPEN RowSET](#open-rowset)
- [JSON data in SQL Server](#json-data-in-sql-server)

Interview Questions
- [Highest Salary Queries](#highest-salary-queries)
- [Difference between TRUNCATE and DELETE in SQL](difference-between-truncate-and-delete-in-sql)
- [Where condition faster than Join ?](#where-condition-faster-than-join-?)
- [Interview Prep](#interview-prep)

Azure Stuff
- [Blob Storage](#blob-storage)
- [Blob Lingo](#blob-lingo)
- [Container Locations](#container-locations)
- [Format Files for BLOB Storage](#format-files-for-blob-storage)

Under Construction / Topics to Research
- [Search Topics](#search-topics)
- [Query Execution Plans](#query-execution-plans)

## SQL Server Features
In SQL Server 2016, PolyBase support was introduced. With PolyBase, you can query NoSQL data like csv files stored in Azure Blob Storage or in HDInsight.
Another interesting feature is JSON support which includes new features to handle JSON data. 
- SQLCMD : SQLCMD is a command line application that comes with Microsoft SQL Server, and exposes the management features of SQL Server. It allows SQL queries to be written and executed from the command prompt. It can also act as a scripting language to create and run a set of SQL statements as a script. Such scripts are stored as a .sql file, and are used either for management of databases or to create the database schema during the deployment of a database.
- SSMS : SQL Server Management Studio is a GUI tool included with SQL Server 2005 and later for configuring, managing, and administering all components within Microsoft SQL Server. The tool includes both script editors and graphical tools that work with objects and features of the server. A central feature of SQL Server Management Studio is the Object Explorer, which allows the user to browse, select, and act upon any of the objects within the server. It can be used to visually observe and analyze query plans and optimize the database performance, among others. SQL Server Management Studio can also be used to create a new database, alter any existing database schema by adding or modifying tables and indexes, or analyze performance. It includes the query windows which provide a GUI based interface to write and execute queries.
- SQL Server also includes an assortment of add-on services. While these are not essential for the operation of the database system, they provide value added services on top of the core database management system. These services either run as a part of some SQL Server component or out-of-process as Windows Service and presents their own API to control and interact with them.
- SSTD / BIDS : Business Intelligence Development Studio (BIDS) is the IDE from Microsoft used for developing data analysis and Business Intelligence solutions utilizing the Microsoft SQL Server Analysis Services, Reporting Services and Integration Services. It is based on the Microsoft Visual Studio development environment but is customized with the SQL Server services-specific extensions and project types, including tools, controls and projects for reports (using Reporting Services), Cubes and data mining structures (using Analysis Services). For SQL Server 2012 and later, this IDE has been renamed SQL Server Data Tools (SSDT).

## Limitations

## Architecture
Data storage is a database, which is a collection of tables with typed columns. SQL Server supports different data types, including primitive types such as Integer, Float, Decimal, Char (including character strings), Varchar (variable length character strings), binary (for unstructured blobs of data), Text (for textual data) among others. 
Microsoft SQL Server also allows user-defined composite types (UDTs) to be defined and used. It also makes server statistics available as virtual tables and views (called Dynamic Management Views or DMVs). In addition to tables, a database can also contain other objects including views, stored procedures, indexes and constraints, along with a transaction log. 
The data in the database are stored in primary data files with an extension .mdf. Secondary data files, identified with a .ndf extension, are used to allow the data of a single database to be spread across more than one file, and optionally across more than one file system. Log files are identified with the .ldf extension.
Storage space allocated to a database is divided into sequentially numbered pages, each 8 KB in size. A page is the basic unit of I/O for SQL Server operations. A page is marked with a 96-byte header which stores metadata about the page including the page number, page type, free space on the page and the ID of the object that owns it. Page type defines the data contained in the page: data stored in the database, index, allocation map which holds information about how pages are allocated to tables and indexes, change map which holds information about the changes made to other pages since last backup or logging, or contain large data types such as image or text. While page is the basic unit of an I/O operation, space is actually managed in terms of an extent which consists of 8 pages. A database object can either span all 8 pages in an extent ("uniform extent") or share an extent with up to 7 more objects ("mixed extent").
A row in a database table cannot span more than one page, so is limited to 8 KB in size. However, if the data exceeds 8 KB and the row contains varchar or varbinary data, the data in those columns are moved to a new page (or possibly a sequence of pages, called an allocation unit) and replaced with a pointer to the data.
For physical storage of a table, its rows are divided into a series of partitions (numbered 1 to n). The partition size is user defined; by default all rows are in a single partition. A table is split into multiple partitions in order to spread a database over a computer cluster. Rows in each partition are stored in either B-tree or heap structure. If the table has an associated, clustered index to allow fast retrieval of rows, the rows are stored in-order according to their index values, with a B-tree providing the index. The data is in the leaf node of the leaves, and other nodes storing the index values for the leaf data reachable from the respective nodes. If the index is non-clustered, the rows are not sorted according to the index keys. An indexed view has the same storage structure as an indexed table. A table without a clustered index is stored in an unordered heap structure. However, the table may have non-clustered indices to allow fast retrieval of rows. In some situations the heap structure has performance advantages over the clustered structure. Both heaps and B-trees can span multiple allocation units.
Buffer management : SQL Server buffers pages in RAM to minimize disk I/O. Any 8 KB page can be buffered in-memory, and the set of all pages currently buffered is called the buffer cache. The amount of memory available to SQL Server decides how many pages will be cached in memory. The buffer cache is managed by the Buffer Manager. Either reading from or writing to any page copies it to the buffer cache. Subsequent reads or writes are redirected to the in-memory copy, rather than the on-disc version. The page is updated on the disc by the Buffer Manager only if the in-memory cache has not been referenced for some time. While writing pages back to disc, asynchronous I/O is used whereby the I/O operation is done in a background thread so that other operations do not have to wait for the I/O operation to complete. Each page is written along with its checksum when it is written. When reading the page back, its checksum is computed again and matched with the stored version to ensure the page has not been damaged or tampered with in the meantime.
Concurrency and locking: SQL Server allows multiple clients to use the same database concurrently. As such, it needs to control concurrent access to shared data, to ensure data integrity—when multiple clients update the same data, or clients attempt to read data that is in the process of being changed by another client. SQL Server provides two modes of concurrency control: pessimistic concurrency and optimistic concurrency. When pessimistic concurrency control is being used, SQL Server controls concurrent access by using locks. Locks can be either shared or exclusive. Exclusive lock grants the user exclusive access to the data—no other user can access the data as long as the lock is held. Shared locks are used when some data is being read—multiple users can read from data locked with a shared lock, but not acquire an exclusive lock. The latter would have to wait for all shared locks to be released.

The main mode of retrieving data from a SQL Server database is querying for it. The query is expressed using a variant of SQL called T-SQL, a dialect Microsoft SQL Server 
The query declaratively specifies what is to be retrieved. It is processed by the query processor, which figures out the sequence of steps that will be necessary to retrieve the requested data. The sequence of actions necessary to execute a query is called a query plan. There might be multiple ways to process the same query. For example, for a query that contains a join statement and a select statement, executing join on both the tables and then executing select on the results would give the same result as selecting from each table and then executing the join, but result in different execution plans. In such case, SQL Server chooses the plan that is expected to yield the results in the shortest possible time. This is called query optimization and is performed by the query processor itself.
SQL Server includes a cost-based query optimizer which tries to optimize on the cost, in terms of the resources it will take to execute the query. Given a query, then the query optimizer looks at the database schema, the database statistics and the system load at that time. It then decides which sequence to access the tables referred in the query, which sequence to execute the operations and what access method to be used to access the tables. For example, if the table has an associated index, whether the index should be used or not: if the index is on a column which is not unique for most of the columns (low "selectivity"), it might not be worthwhile to use the index to access the data. Finally, it decides whether to execute the query concurrently or not.
Once a query plan is generated for a query, it is temporarily cached. For further invocations of the same query, the cached plan is used. Unused plans are discarded after some time.
SQL Server also allows stored procedures to be defined. Stored procedures are parameterized T-SQL queries, that are stored in the server itself (and not issued by the client application as is the case with general queries). Stored procedures can accept values sent by the client as input parameters, and send back results as output parameters. They can call defined functions, and other stored procedures, including the same stored procedure (up to a set number of times). They can be selectively provided access to. Unlike other queries, stored procedures have an associated name, which is used at runtime to resolve into the actual queries. Also because the code need not be sent from the client every time (as it can be accessed by name), it reduces network traffic and somewhat improves performance. Execution plans for stored procedures are also cached as necessary.

## Datatypes
- CHAR is fixed length. CHAR takes up 1 byte per character. So, a CHAR(100) field (or variable) takes up 100 bytes on disk, regardless of the string it holds. 
- VARCHAR is a variable length string data type, so it holds only the characters you assign to it. VARCHAR takes up 1 byte per character, + 2 bytes to hold length information.  For example, if you set a VARCHAR(100) data type = ‘Jen’, then it would take up 3 bytes (for J, E, and N) plus 2 bytes, or 5 bytes in all.
Use varchar(max) when the sizes of the column data entries vary considerably, and the string length might exceed 8,000 bytes.
The "N" in NVARCHAR means uNicode.  If you have requirements to store UNICODE or multilingual data, nvarchar is the choice. Varchar stores ASCII data and should be your data type of choice for normal use.
The biggest concern is that nvarchar uses 2 bytes per character, whereas varchar uses 1. Thus, nvarchar(4000) uses the same amount of storage space as varchar(8000)

## Types of Creating a Table
- User Tables (Regular Tables)
- Local Temporary Tables (#TableName)
  Stored in a tempdb under system databases.
  Local temporary tables are visible only to their creators during the same connection to an instance of SQL Server as when the tables were first created or referenced.
  If a local temporary table created in a stored procedure, it is dropped automatically when the stored procedure is finished.
  You can create a Local Temporary Table with the same name but in a different connection, and it is stored with the same name along with various random values.
- Global Temporary Tables (##TableName) 
  Global temporary tables are visible to any user and any connection after they are created.
  Global temporary table is automatically dropped when the session that created the table ends and the last active Transact-SQL statement (not session) referencing this table in other sessions ends.
Limitations: You cannot access local and global temporary tables in functions (UDFs).
- Tempdb permanent tables 
   (USE tempdb CREATE TABLE t) are visible to everyone, and are deleted when the server is restarted.
- Table Variable (@TableName)
	- Create newTable as SELECT colnames FROM tableName;
	- Select colnames INTO newTable from tablename;

## DDLs - Data Definition Language
CREATE – is used to create the database or its objects (like table, index, function, views, store procedure and triggers).
```
Create TABLE [dbo].[Employee]  (  
	[EmpID] 		[int] IDENTITY(1,1) NOT NULL,  
	[EmpName] 		[varchar](30) NULL,  
	[CreateName] 	[char] (10) NOT NULL,
	[CreateDTM] 	[DATETIME] NOT NULL,
	[UpdateDTM] 	[DATETIME] DEFAULT NULL,
	[UpdateName] 	[char](10) DEFAULT NULL,
	[EndDTM] 		[DATETIME] DEFAULT NULL			)
```
DROP – is used to delete objects from the database.
` DROP TABLE tableName;
ALTER-is used to alter the structure of the database.
```
 ALTER TABLE employees ADD last_name VARCHAR(50);
 ALTER TABLE tableName MODIFY colName CHARACTER VARYING(20);
 ALTER TABLE tableName RENAME TO newTableName;
 ALTER TABLE tableName RENAME COLUMN colA to colB;
 ALTER TABLE tableName DROP COLUMN col2;
```
TRUNCATE : is used to remove all records from a table, including all spaces allocated for the records are removed.
` TRUNCATE tableName;

COMMENT : is used to add comments to the data dictionary.
RENAME : is used to rename an object existing in the database.
` EXEC sp_RENAME 'TableName.OldColumnName' , 'NewColumnName', 'COLUMN'

## DMLs - Data Manipulation Language
SELECT : is used to retrieve data from the a database.
INSERT : is used to insert data into a table.
UPDATE : is used to update existing data within a table.
DELETE : is used to delete records from a database table.

## DCL - Data Control Language
GRANT-gives user’s access privileges to database.
REVOKE-withdraw user’s access privileges given by using the GRANT command.

## TCL - Transaction Control Language
COMMIT– commits a Transaction.
ROLLBACK– rollbacks a transaction in case of any error occurs.
SAVEPOINT–sets a savepoint within a transaction.
SET TRANSACTION–specify characteristics for the transaction.

## Constraints 
Constraints are used to limit the type of data that can go into a table. This ensures the accuracy and reliability of the data in the table.
- NOT NULL : Ensures that a column cannot have a NULL value
- UNIQUE : Ensures that all values in a column are different
- PRIMARY KEY : A combination of a NOT NULL and UNIQUE. Uniquely identifies each row in a table
- FOREIGN KEY : Uniquely identifies a row/record in another table
- CHECK : Ensures that all values in a column satisfies a specific condition
- DEFAULT : Sets a default value for a column when no value is specified
- INDEX : A database index is a data structure that provides quick lookup of data in a column or columns of a table. It enhances the speed of operations accessing data from a database table at the cost of additional writes and memory to maintain the index data structure.
  - Unique : ensures that no two rows of data in a table have identical key values. Once a unique index has been defined for a table, uniqueness is enforced whenever keys are added or changed within the index
  - Non-Unique Index : are used solely to improve query performance by maintaining a sorted order of data values that are used frequently. 
 
Scenario:
```
ALTER TABLE [Production].[ProductCostHistory] WITH CHECK ADD 
CONSTRAINT [FK_ProductCostHistory_Product_ProductID] FOREIGN KEY([ProductID])
REFERENCES [Production].[Product] ([ProductID])
GO
ALTER TABLE [Production].[ProductCostHistory] CHECK CONSTRAINT     
[FK_ProductCostHistory_Product_ProductID]
GO
```
Checks for key existing in parent table.
[Refer](https://stackoverflow.com/questions/529941/with-check-add-constraint-followed-by-check-constraint-vs-add-constraint)

## Foreign Key Constraints
Add constraints first and then check 
```
ALTER TABLE [anx2].[tblGetIsdc]  WITH CHECK ADD  CONSTRAINT [tblGetIsdc_tblANX2RegisterInvoice_FK] FOREIGN KEY([A2RegisterInvoiceID])
REFERENCES [anx2].[tblA2RegisterInvoice] ([ID])
GO
ALTER TABLE [anx2].[tblGetIsdc] CHECK CONSTRAINT [tblGetIsdc_tblANX2RegisterInvoice_FK]
GO
```

## Joins in SQL
- INNER JOIN : returns only those rows where there is a match in both tables.
- LEFT JOIN  : returns all rows from the left table, even if there are no matches in the right table.
- RIGHT JOIN : returns all rows from the right table, even if there are no matches in the left table.
- FULL JOIN  : returns rows when there is a match in one of the tables.
- SELF JOIN  : is used to join a table to itself as if the table were two tables, temporarily renaming at least one table in the SQL statement. The self join can be viewed as a join of two copies of the same table. The table is not actually copied, but SQL performs the command as though it were.
  Ex: If we want a list of employees and the names of their supervisors, we’ll have to JOIN the EMPLOYEE table to itself to get this list.
	```SELECT a.emp_id AS "Emp_ID",a.emp_name AS "Employee Name",
		b.emp_id AS "Supervisor ID",b.emp_name AS "Supervisor Name"
		FROM employee a, employee b
		WHERE a.emp_supv = b.emp_id;```
- CARTESIAN JOIN/CROSS JOIN : there is a join for each row of one table to every row of another table.
		```SELECT Student.NAME, Student.AGE, StudentCourse.COURSE_ID
		FROM Student
		CROSS JOIN StudentCourse;```

### Join duplications
SQL joins will give you as many rows as specified in the join key condition.
```
TableA	TableB
ColA	ColB
1		1
1		1
NULL	1
(2 rows)(3 rows)
SELECT * FROM TableA A
INNER JOIN TableB B on A.ColA=B.ColB 
ColA	ColB
1		1
1		1
1		1
1		1
1		1
1		1
1		1
(6 rows)
SELECT * FROM TableA A
LEFT JOIN TableB B on A.ColA=B.ColB 
ColA	ColB
1		1
1		1
1		1
1		1
1		1
1		1
1		1
NULL	NULL
(7 rows)
SELECT * FROM TableA A
RIGHT JOIN TableB B on A.ColA=B.ColB 
ColA	ColB
1		1
1		1
1		1
1		1
1		1
1		1
1		1
(6 rows)
SELECT * FROM TableA A
FULL JOIN TableB B on A.ColA=B.ColB 
ColA	ColB
1		1
1		1
1		1
1		1
1		1
1		1
1		1
NULL	NULL
(7 rows)
SELECT * FROM TableA CROSS JOIN TableB;
colA	colB
1		1
1		1
NULL	1
1		1
1		1
NULL	1
1		1
1		1
NULL	1
(15 rows)
```

## Merge Join
The Merge Join operator is one of four operators that join data from two input streams into a single combined output stream. As such, it has two inputs, called the left and right input. Merge Join is the most effective of all join operators. However, it requires all input data to be sorted by the join columns. Often this means that a Merge Join can’t be used without adding extra Sort operators. These extra sorts increase the total plan cost. In such cases, the optimizer tends to choose other join operators instead.
```
MERGE <target_table> [AS TARGET]
USING <table_source> [AS SOURCE]
ON <search_condition>
[WHEN MATCHED 
   THEN <merge_matched> ]
[WHEN NOT MATCHED [BY TARGET]
   THEN <merge_not_matched> ]
[WHEN NOT MATCHED BY SOURCE
   THEN <merge_matched> ];-------------------------------OR--
MERGE INTO my_target_table tableE
      USING (SELECT * FROM ) tableS
  ON   tableE.UID   =  tableS.UID
  WHEN MATCHED  AND tableE.DATAFIELD <> tableS.DATAFIELD THEN
         UPDATE SET tableE.DATAFIELD = tableS.DATAFIELD ,tableE.UPDATED = CURRENT_TIMESTAMP
  WHEN NOT MATCHED  THEN
         INSERT VALUES (tableS.UID,tableS.DATAFIELD,CURRENT_TIMESTAMP,CURRENT_TIMESTAMP);
```
[Read in Detail](https://www.mssqltips.com/sqlservertip/1704/using-merge-in-sql-server-to-insert-update-and-delete-at-the-same-time/)
[Explanations](https://sqlserverfast.com/epr/merge-join/)

## Subquery
A subquery is a query within another query.
There are two types of subquery - Correlated and Non-Correlated.
 - A correlated subquery cannot be considered as an independent query, but it can refer the column in a table listed in the FROM of the main query.
 - A non-correlated subquery can be considered as an independent query and the output of subquery is substituted in the main query.

## Union vs Union All
Union all includes duplicates where as only Union doesnt.

## LOOPs
In SQL Server, there is no FOR LOOP. 

### IF ELSE Condition
Execute code when a condition is TRUE, or execute different code when false.
```
IF @site_value < 25
   PRINT 'TechOnTheNet.com';
ELSE
BEGIN
   IF @site_value < 50
      PRINT 'CheckYourMath.com';
   ELSE
      PRINT 'BigActivities.com';
END;
```

### WHILE Loop
If not sure how many times we want to execute the code:
```
WHILE @cnt < cnt_total
BEGIN
   {...statements...}
   SET @cnt = @cnt + 1;
END;
```

### CASE 
Case is not a statement it is an expression, the CASE statement must be part of the expression, not the expression itself.
```
case when first_condition
      then first_condition_result_true
    else
      case when second_condition 
        then second_condition_result_true
      else
          second_condition_result_false                              
      end
    end
  end as qty

DECLARE @FirstName VARCHAR(100)
SET @FirstName = ''
 
DECLARE @LastName VARCHAR(100)
SET @LastName = 'Dave' 
 
SELECT FirstName, LastName
FROM Contacts
WHERE  
    FirstName = CASE
					WHEN LEN(@FirstName) > 0 THEN  @FirstName 
					ELSE FirstName 
				END
AND
    LastName = CASE
					WHEN LEN(@LastName) > 0 THEN  @LastName 
					ELSE LastName 
			   END
GO
```

## Between Clause
Between Clause is inclusive of starting and ending limits. 
Using Greater & lesser than will be exclusive limits.

## Sequences
A sequence is a user-defined schema-bound object that generates a sequence of numeric values according to the specification with which the sequence was created. The sequence of numeric values is generated in an ascending or descending order at a defined interval and may cycle (repeat) as requested. Sequences, unlike identity columns, are not associated with tables. An application refers to a sequence object to receive its next value. The relationship between sequences and tables is controlled by the application. User applications can reference a sequence object and coordinate the values keys across multiple rows and tables.
Unlike identity column values, which are generated when rows are inserted, an application can obtain the next sequence number before inserting the row by calling the NEXT VALUE FOR function. The sequence number is allocated when NEXT VALUE FOR is called even if the number is never inserted into a table. The NEXT VALUE FOR function can be used as the default value for a column in a table definition. Use sp_sequence_get_range to get a range of multiple sequence numbers at once.
A sequence can be defined as any integer data type. If the data type is not specified, a sequence defaults to bigint.
Unlike identity, the next number for the column value will be retrieved from memory rather than from the disk – this makes Sequence significantly faster than Identity. Using the identity attribute for a column, you can easily generate auto-incrementing numbers (which as often used as a primary key).
```
CREATE SEQUENCE contacts_seq
  AS BIGINT
  START WITH 1
  INCREMENT BY 1
  MINVALUE 1
  MAXVALUE 99999
  NO CYCLE
  CACHE 10;
INSERT INTO contacts (contact_id, last_name)
VALUES (NEXT VALUE FOR contacts_seq, 'Smith');
```

## Stored Procedures
SP with IF ELSE blocks
	```
	CREATE PROCEDURE CheckPassword
	-- Add the parameters for the stored procedure here
		@username VARCHAR(20),
		@password varchar(20)
	AS
	BEGIN
	SET NOCOUNT ON  -- SET NOCOUNT ON added to prevent extra result sets from interfering with SELECT statements.
	IF EXISTS(SELECT * FROM usertable WHERE username = @username AND password = @password)
		SELECT 'true' AS UserExists
	ELSE
		SELECT 'false' AS UserExists
	END
	GO
	// OUTPUT: EXEC CheckPassword '123456'; 
	```

## Clustered and Non-Clustered Index

Clustered indexes are indexes whose order of the rows in the database correspond to the order of the rows in the index. This is why only one clustered index can exist in a given table, whereas, multiple non-clustered indexes can exist in the table.
The only difference between clustered and non-clustered indexes is that the database manager attempts to keep the data in the database in the same order as the corresponding keys appear in the clustered index.
Clustering index can improve the performance of most query operations because they provide a linear-access path to data stored in the database.
The differences can be broken down into three small factors -
 - Clustered index modifies the way records are stored in a database based on the indexed column. Non-clustered index creates a separate entity within the table which references the original table.
 - Clustered index is used for easy and speedy retrieval of data from the database, whereas, fetching records from the non-clustered index is relatively slower.
 - In SQL, a table can have a single clustered index whereas it can have multiple non-clustered indexes.

## Window functions 
Window functions operate on a set of rows and return a single value for each row from the underlying query. You can also include the optional PARTITION BY and ORDER BY clauses in a query. The PARTITION BY clause subdivides the window into partitions. The ORDER BY clause defines the logical order of the rows within each partition of the result set. Window functions are applied to the rows within each partition and sorted according to the order specification.
A window function does not cause rows to become grouped into a single output row wheras The aggregate functions perform calculations across a set of rows and return a single output row.
[Read in Detail](http://www.sqltutorial.org/sql-window-functions/)

Types :
- Value window functions
  - FIRST_VALUE() : The FIRST_VALUE window function returns the value of the first row in the window frame.
  - LAG()		  : The LAG() window function returns the value for the row before the current row in a partition. If no row exists, null is returned.
  - LAST_VALUE()  : The LAST_VALUE window function returns the value of the last row in the window frame.
  - LEAD()		  : The LEAD() window function returns the value for the row after the current row in a partition. If no row exists, null is returned.
- Ranking window functions
  - NTILE()		  : divides the rows for each window partition, into a specified number of ranked groups based on the ORDER BY clause.
  - RANK()		  : Rows with equal values recieve repeated rankings, next value will skip the next rank based on count and might not be consecutive numbers. Ex:1,2,2,4,5,6
  - DENSE_RANK()  : Rows with equal values receive the same rank. There are no gaps in the sequence of ranked values if two or more rows have the same rank. Ex:1,2,2,3,4,5,6
  - PERCENT_RANK(): The PERCENT_RANK () window function calculates the percent rank of the current row using the following formula: (x - 1) / (number of rows in window partition - 1) where x is the rank of the current row.
  - ROW_NUMBER()  : The ordinal number of the current row within its partition determined by the ORDER BY clause. Rows with equal values receive different row numbers nondeterministically.
  - CUME_DIST()   : The CUME_DIST() window function calculates the relative rank of the current row within a window partition: (number of rows preceding or peer with current row) / (total rows in the window partition)
- Aggregate window functions
  - AVG()		  : SELECT dealer_id, sales, AVG(sales) OVER (PARTITION BY dealer_id) AS avgsales FROM q1_sales;
  - COUNT()		  : SELECT dealer_id, sales, COUNT(sales) OVER(PARTITION BY dealer_id) AS `count` FROM q1_sales;
  - SUM()		  : SELECT dealer_id, emp_name, sales, SUM(sales) OVER(PARTITION BY dealer_id) AS `sum` FROM q1_sales;
  - MAX()		  : SELECT emp_name, dealer_id, sales, MAX(sales) OVER(PARTITION BY dealer_id) AS `max` FROM q1_sales;
  - MIN()		  : SELECT emp_name, dealer_id, sales, MIN(sales) OVER(PARTITION BY dealer_id) AS `min` FROM q1_sales;

SYNTAX:
> window_function_name ( expression ) OVER (				#target expression/column on which the window function operates (mandatory clause)
>     					     partition_by_clause	#if not specified, then the whole result set is treated as a single partition
>     					     order_by_clause [ASC | DESC]  [NULL {FIRST| LAST}]
>     					     frame_clause 	)

A frame is the subset of the current partition. The frame_clause supports the following frames:
> RANGE UNBOUNDED PRECEDING
> RANGE BETWEEN CURRENT ROW AND CURRENT ROW
> [RANGE | ROWS] BETWEEN UNBOUNDED PRECEDING AND CURRENT ROW
> [RANGE | ROWS] BETWEEN UNBOUNDED PRECEDING AND UNBOUNDED FOLLOWING
To define the frame, you use one of the following syntaxes for window frame_start and frame_end :
where frame_start is one of the following options:
> N PRECEDING
> UNBOUNDED PRECEDING : The window frame starts with the first row of the partition.
> CURRENT ROW : CURRENT ROW means that the frame starts with the current row.
and frame_end is one of the following options:
> CURRENT ROW : CURRENT ROW means that the frame ends with the current row.
> UNBOUNDED FOLLOWING : The window frame ends with the last row of the partition, for both ROW and RANGE modes.
> N FOLLOWING					

## Exception Handler
SQL Server provides TRY, CATCH blocks for exception handling. We can put all T-SQL statements into a TRY BLOCK and the code for exception handling can be put into a CATCH block. We can also generate user-defined errors using a THROW block.
SQL Server contains the following two types of exceptions:
- System Defined: exceptions (errors) are generated by the system
	```
	Declare @val1 int;  
	Declare @val2 int;  
	BEGIN TRY  
		Set @val1=8;  
		Set @val2=@val1/0; /* Error Occurs Here */  
	END TRY  
	BEGIN CATCH  
		Print 'Error Occur that is:'  
		Print Error_Message()  
	END CATCH  
	```
	The following keywords can be used within a catch block:
	- @@ERROR
	- ERROR_NUMBER()
	- ERROR_STATE()
	- ERROR_LINE()
	- ERROR_MESSAGE()
	- ERROR_PROCEDURE()
	- ERROR_SEVERITY()
	- RAISERROR()
	- GOTO()
- User Defined: exception is user generated, not system generated.
	```
	Declare @val1 int;  
	Declare @val2 int;  
	BEGIN TRY  
		Set @val1=8;  
		Set @val2=@val1%2;   
		if @val1=1  
			Print 'No Error here'  
		else  
			Begin  
				Print 'Error Occurs';  
				Throw 60000,'Number Is Even',5  
			End  
	END TRY  
	BEGIN CATCH  
		Print 'Error Occur that is:'  
		Print Error_Message()  
	END CATCH  
	```
SP with try catch
```
	CREATE PROCEDURE [dbo].[PL_GEN_PROVN_NO1]        
		@GAD_COMP_CODE  VARCHAR(2) =NULL, 
		@@voucher_no numeric =null output 
		AS         
	BEGIN  
	
		BEGIN TRY 
			-- your proc code
		END TRY
	
		BEGIN CATCH
			-- what you want to do in catch
		END CATCH    
	END -- proc end
```
[Refer](https://stackoverflow.com/questions/1480881/how-to-add-a-try-catch-to-sql-stored-procedure)

## Schema
```
IF NOT EXISTS (SELECT schema_name 
    FROM information_schema.schemata 
    WHERE schema_name = 'newSchemaName')
BEGIN
    EXEC sp_executesql N'CREATE SCHEMA NewSchemaName;';
END
```
or
```
CREATE SCHEMA Schema_Name;
GO
```
Moving dbo from other schemas to required schema
```
ALTER SCHEMA stg
TRANSFER [anx1].[stg_tblGetEcom]
GO
```

## Convert Queries
- [Date Conversions](https://www.mssqltips.com/sqlservertip/1145/date-and-time-conversions-using-sql-server/)
```
SELECT CONVERT(varchar, getdate(), 107) 		-- From text [Dec 30, 2006] to DB default [2006-12-30]
SELECT CONVERT(varchar, getdate(), 105) 		-- From text [30-12-2006] to DB default [2006-12-30]
SELECT CONVERT(varchar, getdate(), 101) 		-- From text [12/30/2006] to DB default [2006-12-30]
SELECT CONVERT(varchar, getdate(), 110) 		-- From text [12-30-2006] to DB default [2006-12-30]
```

## Dateadd queries
To add time to datetime
```
SELECT DATEADD(DAY,   10, GETDATE()), '10 Days Later'
SELECT DATEADD(DAY,  –10, GETDATE()), '10 Days Earlier'
SELECT DATEADD(MONTH,  1, GETDATE()), 'Next Month'
SELECT DATEADD(MONTH, –1, GETDATE()), 'Previous Month'
SELECT DATEADD(YEAR,   1, GETDATE()), 'Next Year'
SELECT DATEADD(YEAR,  –1, GETDATE()), 'Previous Year'
```

## Financial Year
Lesser or Equal to April month falls to previous FinYear, else next FinYear
```
DECLARE @Year VARCHAR(20)  
	SELECT @Year =(CASE WHEN (MONTH(GETDATE()))<= 3 THEN convert(varchar(4),YEAR(GETDATE())-1)+'-'+convert(varchar(4),YEAR(GETDATE())%100)  
													ELSE convert(varchar(4),YEAR(GETDATE()))  +'-'+convert(varchar(4),(YEAR(GETDATE())%100)+1)
				   END)  
SELECT @Year ASCurrentFinancialYear
```

## Generate Random String
`SELECT SUBSTRING(CONVERT(varchar(40), NEWID()),0,9);
[Large Amount of Random Data](https://www.mssqltips.com/sqlservertip/5148/populate-large-tables-with-random-data-for-sql-server-performance-testing/)
```
Declare @count int,
		@CGSTIN VARCHAR(15),
		@LowerLimitForNum int,
		@UpperLimitForNum int
Set @count = 1
Set	@LowerLimitForNum = 1
Set	@UpperLimitForNum = 100

While @count <= 50000
Begin 

   Select @CGSTIN = CONCAT(SUBSTRING(CONVERT(varchar(40), NEWID()),0,9),SUBSTRING(CONVERT(varchar(40), NEWID()),0,6));

   --SELECT '23GSPMP0501G0ZA','102019',@CGSTIN,SUBSTRING(@CGSTIN,3,10),'B2B','INV',ROUND(((@UpperLimitForNum-@LowerLimitForNum)*RAND())+@LowerLimitForNum,0)
   Insert Into [anx1].[tblgstnresponsesummary] ([sgstin],[taxperiod],[cgstin],[cpan],[gstnsavesection],[DocumentType],[invoicecount]) 
   values ('23GSPMP0501G0ZA','102019',@CGSTIN,SUBSTRING(@CGSTIN,3,10),'B2B','INV',ROUND(((@UpperLimitForNum-@LowerLimitForNum)*RAND())+@LowerLimitForNum,0))
   Print @count
   Set @count = @count + 1
End
```

## Update by Row Number
```
SELECT ROW_NUMBER() OVER (ORDER BY [GSTfilingDate])
	  ,[GSTINid]
      ,[StateName]
      ,[GSTfilingDate]
      ,[RetFiledDate]
  FROM [dbo].[GST_Return_Filing_MonthWise]
GO

UPDATE UpdateTarget 
SET GSTfilingDate='01-01-2017'
FROM (
SELECT ROW_NUMBER() OVER (ORDER BY [GSTfilingDate]) AS rOWnUM
	  ,[GSTINid]
      ,[StateName]
      ,[GSTfilingDate]
      ,[RetFiledDate]
  FROM [dbo].[GST_Return_Filing_MonthWise]
) AS UpdateTarget
WHERE UpdateTarget.rOWnUM=1
```

## Update Random date
```
SELECT DATEADD(DAY, ABS(CHECKSUM(NEWID()) % 50), (right(taxperiod,4)+'-'+left(taxperiod,2)+'-01') ) as randDT
FROM master.tblGstinReturnFilingStatus
where len(taxperiod) =6
order by randDT;
UPDATE master.tblGstinReturnFilingStatus
SET SubmitDate = DATEADD(DAY, ABS(CHECKSUM(NEWID()) % 50), (right(taxperiod,4)+'-'+left(taxperiod,2)+'-01') ) 
where len(taxperiod) =6;
```
## Random number with a normalized distribution
Random number between 0 and 13 inclusive with a normalized distribution
```
ABS(CHECKSUM(NewId())) % 14
```
[Random Number in normalized distribution](https://stackoverflow.com/questions/1045138/how-do-i-generate-random-number-for-each-row-in-a-tsql-select)

## Split a string to a table using T-SQL 
Splitting a string based on delimiter.

### STRING_SPLIT ( string , separator)
A table-valued function for Microsoft SQL Server 2016 and above, that splits a string into rows of substrings, based on a specified separator character.
Following query transforms each item in the list of tags and joins them with the original row:
```
SELECT ProductId, Name, value  
FROM Product  
    CROSS APPLY STRING_SPLIT(Tags, ',');
```
[Refer](https://docs.microsoft.com/en-us/sql/t-sql/functions/string-split-transact-sql?view=sql-server-2017)

### dbo.Split()
There is no built-in function to split a delimited string in till Microsoft SQL Server 2016, but it is very easy to create your own as shown below.
The Table-Valued Function (TVF) will split a string with a custom delimiter, and return the results as an ordered table with each value. The output table has the column “Id” containing the original index of the value in the string. The column “Data” contains each string value. 
```
CREATE FUNCTION [dbo].[Split]
(
    @String NVARCHAR(4000),
    @Delimiter NCHAR(1)
)
RETURNS TABLE
AS
RETURN
(
    WITH Split(stpos,endpos)
    AS(
        SELECT 0 AS stpos, CHARINDEX(@Delimiter,@String) AS endpos
        UNION ALL
        SELECT endpos+1, CHARINDEX(@Delimiter,@String,endpos+1)
            FROM Split
            WHERE endpos > 0
  )
    SELECT 'Id' = ROW_NUMBER() OVER (ORDER BY (SELECT 1)),
         'Data' = SUBSTRING(@String,stpos,COALESCE(NULLIF(endpos,0),LEN(@String)+1)-stpos)
    FROM Split
)
GO
DECLARE @DelimitedString NVARCHAR(128)
SET @DelimitedString = 'Duckman,Cornfed,Ajax,Charles,Mambo'
SELECT ID,Data FROM dbo.Split(@DelimitedString, ',')
```

## Deduping Data
Microsoft SQL Server tables should never contain duplicate rows, nor non-unique primary keys. Duplicate PKs are a violation of entity integrity, and should be disallowed in a relational system. SQL Server has various mechanisms for enforcing entity integrity, including indexes, UNIQUE constraints, PRIMARY KEY constraints, and triggers.
For deduplicate / dedupe / remove duplication / remove repeated rows / To find duplicate records among large amounts of data, such as those found in databases managed with SQL Server, we use
- distinct : The downside of this is that you need enough free data space in your database (or in tempdb) to store the entire table again, plus plenty of log space when you are de-duplicating large tables.
- group by : 
- bounce out into a temp table delete from original and re-insert without the dupes seems to be the simplest : CTE for de-duplication. I am using the ROW_NUMBER function to return the sequential number of each row within a partition of a result set which is a grouping based.  and then I am deleting all records except where the sequential number is 1. This means keeping one record from the group and deleting all other similar/duplicate records. This is one of the efficient methods to delete records
- derived tables using subquery (The inner query selects all the names in the table that are duplicated, and the minimum value of the Ident column for each name. These results are compared against the main table in order to select the records for deletion) : With this technique you may be able to get away with having much less free room than the earlier technique, but this depends on the ratio of duplicated to unique records--the more records are duplicated, the more space SQL Server needs to temporarily store, and work with, the results that makeup the derived table. The derived table technique is the one I usually try first.
- Correlated sub-queries can be pretty slow and inefficient to run against large tables.
[Refer](https://support.microsoft.com/en-us/help/139444/how-to-remove-duplicate-rows-from-a-table-in-sql-server)
- rowid. This is a physical locator. That is, it states where on disk Oracle stores the row. This unique to each row. So you can use this value to identify and remove copies. To do this, replace min() with min(rowid) in the uncorrelated delete
[Examples](https://www.mssqltips.com/sqlservertip/1918/different-strategies-for-removing-duplicate-records-in-sql-server/)
- Using MERGE Statement: to perform INSERT/UPDATE/DELETE operations in a single statement. This new command is similar to the UPSERT (fusion of the words UPDATE and INSERT.) command of Oracle.  It inserts rows that don't exist and updates the rows that do exist. With the introduction of the MERGE SQL command, developers can more effectively handle common data warehousing scenarios, like checking whether a row exists and then executing an insert or update or delete.
The MERGE statement basically merges data from a source result set to a target table based on a condition that you specify and if the data from the source already exists in the target or not. The new SQL command combines the sequence of conditional INSERT, UPDATE and DELETE commands in a single atomic statement, depending on the existence of a record. With this you can make sure no duplicate records are being inserted into the target table, but rather updated if there is any change and only new records are inserted which do not already exist in the target
```
SELECT A,B,C,D,E, COUNT(*)
  FROM tableName
  GROUP BY 1,2,3,4,5
  HAVING COUNT(*)>1;
`

` SELECT AllCols
              FROM (  SELECT *,
                      ROW_NUMBER() OVER( PARTITION BY AllCols ORDER BY AllCols ) AS rowNumb
                      FROM TableName) AS aliasName
              WHERE rowNumb=1;
```

# Triggers
Creates a DDL(CREATE/ALTER/DROP), DML(INSERT/UPDATE/DELETE), or logon trigger, which is a special type of stored procedure that automatically runs when an event occurs in the database server.
DML triggers are frequently used for enforcing business rules and data integrity. CREATE TRIGGER must be the first statement in the batch and can apply to only one table.
- DML Trigger: The following statement loads a table named production.copyof_target_table to record information when an INSERT or DELETE event occurs against the production.target_table.
```
CREATE TRIGGER production.copyof_target_table
ON production.target_table
AFTER INSERT, DELETE
AS
BEGIN
	<sql statements>
END
```
- INSTEAD OF trigger: is a trigger that allows you to skip an INSERT, DELETE, or UPDATE statement to a table or a view and execute other statements defined in the trigger instead. The actual insert, delete, or update operation does not occur at all.
Ex. Suppose, an application needs to insert new brands into the production.brands table. However, the new brands should be stored in another table called production.brand_approvals for approval before inserting into the production.brands table.
```
CREATE TRIGGER production.trg_vw_brands 
ON production.vw_brands
INSTEAD OF INSERT
AS
BEGIN
BEGIN
	<sql statements>
END
```
- DDL Trigger: respond to server or database events rather than to table data modifications. These events created by the Transact-SQL statement that normally starts with one of the following keywords CREATE, ALTER, DROP, GRANT, DENY, REVOKE, or UPDATE STATISTICS. They are used to :
Record changes in the database schema.
Prevent some specific changes to the database schema.
Respond to a change in the database schema.
- List All Triggers
```
SELECT  name, is_instead_of_trigger
FROM sys.triggers  
WHERE type = 'TR';
```

## Dynamic SQL
Drawback: One issue is the potential for SQL Injection where malicious code is inserted into the command that is being built. Also possible performance issues
```
DECLARE @city varchar(75)
SET @city = 'London'
SELECT * FROM Person.Address WHERE City = @city
```
Dynamic SQL commands using EXEC
```
DECLARE @sqlCommand varchar(1000)
DECLARE @columnList varchar(75)
DECLARE @city varchar(75)

SET @columnList = 'AddressID, AddressLine1, City'
SET @city = '''London'''
SET @sqlCommand = 'SELECT ' + @columnList + ' FROM Person.Address WHERE City = ' + @city

EXEC (@sqlCommand)
```
To send NULLs as values, we can use SET @Id = ''

## Pagination
1. OFFSET-FETCH Clause
OFFSET(to identify the starting point, excluding the first set of records) and FETCH(The FETCH argument is used to return a set of number of rows) Clause are used in conjunction with SELECT and ORDER BY clause to provide a means to retrieve a range of records.
TOP cannot be combined with OFFSET and FETCH. OFFSET value must be greater than or equal to zero. It cannot be negative, else return error.
```
SELECT column_name(s)
FROM table_name
ORDER BY column_name
OFFSET rows_to_skip
FETCH NEXT number_of_rows ROWS ONLY;
```
2. The SET ROWCOUNT limits the result set to the number of rows specified. The SQL Server stops the query processing once the specified number of rows are reached. The ROWCOUNT effects DML statements. However it is recommended to use TOP with Insert, Update and Delete statements as ROWCOUNT won’t affect these statements in future releases.
```
DECLARE @ID BIGINT,
        @OFFSET BIGINT,
        @LIMITINT  INT
 SET @LIMITINT = CAST(@LIMIT AS INT)
 SET @OFFSET = (@PAGENO-1)*(@LIMITINT)

		IF @OFFSET > 0
		BEGIN
			SET ROWCOUNT @OFFSET;
			
			SELECT @ID = A.ID  
			FROM TableA A
			INNER JOIN TableB B
		END

 SET @ID = ISNULL(@ID,0);
 SET ROWCOUNT @LIMITINT;
 --Returns rows from @ID less than Rowcount Limit 
 SELECT A.ID, A.Col1
 FROM  TableA A
 INNER JOIN TableB B ON A.ID > @ID 
```

## Bulk Insert
It is very helpful to quickly transfer a large amount of data from Text File or CSV file to SQL Server Table or Views.
```
BULK INSERT [SQL Server Tutorials].[dbo].[DimGeography] 
      FROM 'F:\MS BI\FILE EXAMPLES\Geography.txt' 
  WITH  
    ( 
       DATAFILETYPE    = 'char', 
       FIELDTERMINATOR = ',', 
       ROWTERMINATOR   = '\n' 
    );
```
TIP: If you want to send the data in multiple batches then use ROWS_PER_BATCH

## SSMS Tricks
To Find tables like a particular name
`EXEC sp_tables @table_name = '%TableName%'
ctrl+D : displays query result set as data table.
Ctrl+T : displays query result set as text.

### Data Dictionary
To Get all Columns of a table (Alt+F1)
```
SELECT COLUMN_NAME
FROM INFORMATION_SCHEMA.COLUMNS
WHERE TABLE_NAME = 'Your Table Name'
ORDER BY ORDINAL_POSITION

select T.name as TableName,
	   S.Name as SchemaName,
	   C.name as ColumnName,
	 --  Z.columnName as ColumnFullName,
	   D.Name as DataType,
	   H.CHARACTER_MAXIMUM_LENGTH as length,
	 --  D.Max_Length,
	   D.Precision,
	   D.Scale,
	   C.Is_identity,
	   C.Is_Nullable,
	   CAST(UPPER(REPLACE(REPLACE(object_definition(C.default_object_id),'(',''),')','')) AS NVARCHAR(255)) as DefaultValue,
	   A.CONSTRAINT_NAME,
	   A.CONSTRAINT_Type

	   FROM sys.tables T
			INNER JOIN sys.Schemas S
			ON T.schema_id=S.schema_id 
			INNER JOIN sys.Columns C
			ON T.object_id=C.object_id
			INNER JOIN information_schema.columns H
			on 
			   H.TABLE_SCHEMA=S.name
			and H.TABLE_NAME=T.name
			and H.column_name=C.name

			INNER JOIN sys.types D
			ON C.user_type_id=D.user_type_id
			LEFT JOIN (SELECT 
							  A.TABLE_SCHEMA,A.TABLE_NAME,B.COLUMN_NAME,A.CONSTRAINT_TYPE, A.CONSTRAINT_NAME
					   FROM INFORMATION_SCHEMA.TABLE_CONSTRAINTS A
					   INNER JOIN INFORMATION_SCHEMA.CONSTRAINT_COLUMN_USAGE B
					   ON A.TABLE_CATALOG = B.TABLE_CATALOG
					   AND A.TABLE_SCHEMA=B.TABLE_SCHEMA
					   AND A.TABLE_NAME=B.TABLE_NAME
					   AND A.CONSTRAINT_NAME=B.CONSTRAINT_NAME
					   WHERE A.TABLE_NAME NOT LIKE 'sys%')  A
			ON A.TABLE_SCHEMA=S.NAME
			AND A.TABLE_NAME=T.NAME
			AND A.COLUMN_NAME=C.NAME
			--LEFT JOIN master.tblpurchasemetadata Z on Z.columnCode=C.Name
where S.name in ('anx1','anx2','anx','stg')
and T.name like '%tblGet%'

--select * from information_schema.columns
--select * from sys.tables
```

## OPEN RowSET
In SQL Server, OPENROWSET can read from a data file without loading the data into a target table. 
OPENROWSET supports bulk operations through a built-in BULK provider that enables data from a file to be read and returned as a rowset. 

## JSON data in SQL Server
JSON is a popular textual data format that's used for exchanging data in modern web and mobile applications. JSON is also used for storing unstructured data in log files or NoSQL databases such as Microsoft Azure Cosmos DB.
JSON is also the main format for exchanging data between webpages and web servers by using AJAX calls.
Convert JSON data to rows and columns by calling the OPENJSON rowset function, which transforms the array of objects that is stored in the @json variable to a rowset that can be queried with a standard SQL SELECT statement.
```
  declare @JSON varchar(MAX) = '[
  {
    "id": "2",
    "requestID": "1",
    "matchID": "101",
    "userAction": "Accept"
  },
  {
    "id": "3",
    "requestID": "1",
    "matchID": "102",
    "userAction": "Accept"
  }
]' 
 
    DROP TABLE IF EXISTS #JSON    
    
 CREATE TABLE #JSON     
 (    
  ID bigint,
  RequestID varchar(10),
  MatchID varchar(10),
  UserAction varchar(10),
  Recordstatus int
 )    
    
 INSERT INTO #JSON (ID,RequestID,MatchID,UserAction,Recordstatus)    
 SELECT ID AS ID,
		REQUESTID AS REQUESTID,
		MATCHID AS MATCHID,
		USERACTION AS USERACTION,
		0 AS Recordstatus
 from openjson(@JSON)
 with (
		id bigint,
		requestID varchar(10),
		matchID varchar(10),
		userAction varchar(10),
		Recordstatus int
	)

SELECT * FROM #JSON;
```
ISJSON (Transact-SQL) tests whether a string contains valid JSON.
JSON_VALUE (Transact-SQL) extracts a scalar value from a JSON string.
JSON_QUERY (Transact-SQL) extracts an object or an array from a JSON string.
JSON_MODIFY (Transact-SQL) changes a value in a JSON string.

### Highest Salary Queries
- Second Highest Salary :
  - Sub query along with Max() function as shown below:
	```
	Select Max(Salary) from Employees
	where Salary < ( Select Max(Salary) from Employees );
	```
- Nth highest salary using Sub-Query:
	```
	SELECT TOP 1 SALARY
	FROM (
		SELECT DISTINCT TOP N SALARY
		FROM EMPLOYEES
		ORDER BY SALARY DESC
		) RESULT
	ORDER BY SALARY ASC;
	```
## Difference between TRUNCATE and DELETE in SQL
```
+----------------------------------------								+----------------------------------------------+
|                Truncate                								|                    Delete                    |
+----------------------------------------								+----------------------------------------------+
| We cannnot Rollback after performing   								| We can Rollback after delete.                |
| Truncate.                              								|                                              |
| Faster than Delete                     								| Slower than Truncate							|
  TRUNCATE simply adjusts a pointer in the DB for the table 			  DELETE must read the records, check constraints, update the block, 
  & the data is gone,  it doesn't keep any logs.						  update indexes, and generate redo/undo. All of that takes time.
  
| Example:                               								| Example:                                     |
| BEGIN TRAN                             								| BEGIN TRAN                                   |
| TRUNCATE TABLE tranTest                								| DELETE FROM tranTest                         |
| SELECT * FROM tranTest                 								| SELECT * FROM tranTest                       |
| ROLLBACK                               								| ROLLBACK                                     |
| SELECT * FROM tranTest                 								| SELECT * FROM tranTest                       |
+----------------------------------------								+----------------------------------------------+
| Truncate reset identity of table.      								| Delete does not reset identity of table.     |
+----------------------------------------								+----------------------------------------------+
| It locks the entire table.             								| It locks the table row.                      |
+----------------------------------------								+----------------------------------------------+
| Its DDL(Data Definition Language)      								| Its DML(Data Manipulation Language)          |
| command.                               								| command.                                     |
+----------------------------------------								+----------------------------------------------+
| We can't use WHERE clause with it.     								| We can use WHERE to filter data to delete.   |
+----------------------------------------								+----------------------------------------------+
| Trigger is not fired while truncate.   								| Trigger is fired.                            |
+----------------------------------------								+----------------------------------------------+
| Syntax :                               								| Syntax :                                     |
| 1) TRUNCATE TABLE table_name           								| 1) DELETE FROM table_name                    |
|                                        								| 2) DELETE FROM table_name WHERE              |
|                                        								|    example_column_id IN (1,2,3)              |
+----------------------------------------								+----------------------------------------------+
```

## Where condition faster than Join ?
Theoretically, no, it shouldn't be any faster. The query optimizer should be able to generate an identical execution plan. However, some DB engines can produce better execution plans for one of them (for complex enough ones). You should test both and see (on your DB engine). 
The JOIN-before-WHERE is logical processing not actual processing and the modern optimisers are clever enough to realise this. The problem most likely to be resolved by indexing the table.
On certain situations where NULLs are involved - having a filtering condition at the ON or at the WHERE can make a big difference in such cases. This query execution order is how Microsoft SQL server will execute queries. Alternatively Sqlite first translates the join-syntax into the where-syntax before executing the query.
To summarize, Where is faster compared to join but it depends on the DBs and its Query excecution order.

## Interview Prep
[All Topics](https://www.interviewbit.com/sql-interview-questions/)

## Blob Storage
Blob is most like the data found in a traditional file-system but with a few key differences.  
First, blob does not support nested directories.  Instead. data is partitioned in folders that go only one level deep. In blob parlance, these are called containers and the data objects within them are called blobs.  
Second, blob data contains only limited automatic metadata.  
	For example, there is only one timestamp on any give blob and it represents the time the blob was stored to the service.  Contrast that to a Windows NTFS file where you get 3 timesstamps – creation time, last access time and last write time.  Any additional metadata on a blob needs to be deliberately put there by the user. 
Finally, a blob can be written in one of two formats:  
- Block Blob : Block blobs are optimized for streaming and are relatively small in size. 
- Page Blob : Page blobs are optimized for random access and can be very large, with a single blob consuming  up to 1TB.

### Blob Lingo
Containers: contain Blobs (folder-like objects) that allow us to partition blob data. They can only go one level deep.
Blob: These are data objects (i.e. files) themselves. They are organized in containers.
Storage Account: Every storage service (i.e. Blob, Table, Queue or File) is provisioned as part of a Storage Account.  Every account has a unique identity called a Name, and each service within it has a specific FQDN.  Although, since we are using PowerShell, we don’t care about the FQDN, only the name.
Storage Context:  Another new concept not mentioned above.  This is an object that allows us to authenticate to a given storage account.  By default, public access to your blob data is not available.  To access it, you first need to authenticate to the service.  The Storage Context is the means with which to do this.
Storage Keys: A new concept, not mentioned above.  This is akin to a password and is one of two pieces of data that are needed to create a Storage Context.  The other piece is the account Name.

### Container Locations
To get container locations of blob storage.
```
SELECT * FROM sys.external_data_sources;
```

## Format Files for BLOB Storage
To create format files to be placed in blob storage
```
select ROW_NUMBER() over (order by Ordinal_position) as SlNo
	, 'SQLCHAR' as SQLCHAR
	, '0' as Zeros
	, case DATA_TYPE
		   when 'date'    then 11
		   when 'decimal' then 41
		   ELSE CHARACTER_MAXIMUM_LENGTH
	  END
	, '","' as FieldRowDelimiter
	, ROW_NUMBER() over (order by Ordinal_position) as Ordinal_position
	, COLUMN_NAME
	,ISNULL(COLLATION_NAME,'""') as COLLATION_NAME
	, DATA_TYPE --SELECT *
from information_schema.columns
WHERE TABLE_SCHEMA= 'ANX1'
AND TABLE_NAME='tblGetDE'
AND COLUMN_NAME NOT IN ('ID','gstin','taxperiod','createdtm','createname','updatedtm','updatename','enddtm','deletedtm');
```

## Query Execution Plans
Query Cost: https://stackoverflow.com/questions/35627923/union-versus-select-distinct-and-union-all-performance

## Search Topics
deep indexes questions
User-defined function & Table Valued Functions
transaction log for each deleted row
begin end transaction 

SSIS questions:
checkpoints vs breakpoints
Execute sql task
Parameters : Connection String Dynamic
Temp table in SSIS
Event handler in ControlFlow and DataFlow duplication of warnings
Containers: Diff bw for & For each loop
delay validation true property
Concurrency management and control 
Implement staging using Basic Features of SSIS
Able to troubleshoot performance issues using Basic Features of SSIS
Control Flow - Analysis Services Processing Task, Rebuild Index Task, Reorganize Index Task
Data Flow Tasks - pivot, unpivot, fuzzylookup, fuzzygrouping, SCD
Incremental loading,Scheduling a job and Secure Packages
On Error Event in Event Handler
log audit - error description in event handlers, deprecated in sql 2014, is sorted,  pass table name between packages, flat file connections, xml file connection,https://www.slideshare.net/sandip1004/ms-business-intelligence-with-sql-server-2005
Able to identify staging needs and implement staging using Basic Features of SSIS
Able to troubleshoot performance issues using Basic Features of SSIS
Control Flow - Analysis Services Processing Task, Rebuild Index Task, Reorganize Index Task
Data Flow Tasks - pivot, unpivot, fuzzylookup, fuzzygrouping, SCD
Incremental loading,Scheduling a job and Secure Packages
On Error Event in Event Handler
