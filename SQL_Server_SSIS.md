# SQL Server / SSIS
Last Updated on : 12 May 2019

## Different types of joins in SQL
- INNER JOIN : returns only those rows where there is a match in both tables.
- LEFT JOIN  : returns all rows from the left table, even if there are no matches in the right table.
- RIGHT JOIN : returns all rows from the right table, even if there are no matches in the left table.
- FULL JOIN  : returns rows when there is a match in one of the tables.
- SELF JOIN  : is used to join a table to itself as if the table were two tables, temporarily renaming at least one table in the SQL statement. The self join can be viewed as a join of two copies of the same table. The table is not actually copied, but SQL performs the command as though it were.
  Ex: If we want a list of employees and the names of their supervisors, we’ll have to JOIN the EMPLOYEE table to itself to get this list.
		```SELECT a.emp_id AS "Emp_ID",a.emp_name AS "Employee Name",
		b.emp_id AS "Supervisor ID",b.emp_name AS "Supervisor Name"
		FROM employee a, employee b
		WHERE a.emp_supv = b.emp_id;```
- CARTESIAN JOIN/CROSS JOIN : there is a join for each row of one table to every row of another table.
		```SELECT Student.NAME, Student.AGE, StudentCourse.COURSE_ID
		FROM Student
		CROSS JOIN StudentCourse;```

### Most asked Queries :
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

## Union vs Union All
Union all includes duplicates where as only Union doesnt.

## Between Clause
Between Clause is inclusive of starting and ending limits. 
Using Greater & lesser than will be exclusive limits.

## Stored Procedures
	```
	CREATE PROCEDURE CheckPassword
	-- Add the parameters for the stored procedure here
		@username VARCHAR(20),
		@password varchar(20)
	AS
	BEGIN
	SET NOCOUNT ON  -- SET NOCOUNT ON added to prevent extra result sets from interfering with SELECT statements.
	IF EXISTS(SELECT * FROM usertable WHERE username = @username AND password = @password)
		SELECT 'true' AS UserExists
	ELSE
		SELECT 'false' AS UserExists
	END
	GO
	// OUTPUT: EXEC CheckPassword '123456'; 
	```

## Where condition faster than Join ?
Theoretically, no, it shouldn't be any faster. The query optimizer should be able to generate an identical execution plan. However, some DB engines can produce better execution plans for one of them (for complex enough ones). You should test both and see (on your DB engine). 
The JOIN-before-WHERE is logical processing not actual processing and the modern optimisers are clever enough to realise this. The problem most likely to be resolved by indexing the table.
On certain situations where NULLs are involved - having a filtering condition at the ON or at the WHERE can make a big difference in such cases. This query execution order is how Microsoft SQL server will execute queries. Alternatively Sqlite first translates the join-syntax into the where-syntax before executing the query.
To summarize, Where is faster compared to join but it depends on the DBs and its Query excecution order.

## Merge Join
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
  ON   tableE.UID   =  tableS.UID
  WHEN MATCHED  AND tableE.DATAFIELD <> tableS.DATAFIELD THEN
         UPDATE SET tableE.DATAFIELD = tableS.DATAFIELD ,tableE.UPDATED = CURRENT_TIMESTAMP
  WHEN NOT MATCHED  THEN
         INSERT VALUES (tableS.UID,tableS.DATAFIELD,CURRENT_TIMESTAMP,CURRENT_TIMESTAMP);
```
[Read in Detail](https://www.mssqltips.com/sqlservertip/1704/using-merge-in-sql-server-to-insert-update-and-delete-at-the-same-time/)

## Window functions 
Window functions operate on a set of rows and return a single value for each row from the underlying query. You can also include the optional PARTITION BY and ORDER BY clauses in a query. The PARTITION BY clause subdivides the window into partitions. The ORDER BY clause defines the logical order of the rows within each partition of the result set. Window functions are applied to the rows within each partition and sorted according to the order specification.
A window function does not cause rows to become grouped into a single output row wheras The aggregate functions perform calculations across a set of rows and return a single output row.
[Read in Detail](http://www.sqltutorial.org/sql-window-functions/)
Types :
- Value window functions
  - FIRST_VALUE() : The FIRST_VALUE window function returns the value of the first row in the window frame.
  - LAG()		  : The LAG() window function returns the value for the row before the current row in a partition. If no row exists, null is returned.
  - LAST_VALUE()  : The LAST_VALUE window function returns the value of the last row in the window frame.
  - LEAD()		  : The LEAD() window function returns the value for the row after the current row in a partition. If no row exists, null is returned.
- Ranking window functions
  - NTILE()		  : divides the rows for each window partition, into a specified number of ranked groups based on the ORDER BY clause.
  - RANK()		  : Rows with equal values recieve repeated rankings, next value will skip the next rank based on count and might not be consecutive numbers.
  - DENSE_RANK()  : Rows with equal values receive the same rank. There are no gaps in the sequence of ranked values if two or more rows have the same rank.
  - PERCENT_RANK(): The PERCENT_RANK () window function calculates the percent rank of the current row using the following formula: (x - 1) / (number of rows in window partition - 1) where x is the rank of the current row.
  - ROW_NUMBER()  : The ordinal number of the current row within its partition determined by the ORDER BY clause. Rows with equal values receive different row numbers nondeterministically.
  - CUME_DIST()   : The CUME_DIST() window function calculates the relative rank of the current row within a window partition: (number of rows preceding or peer with current row) / (total rows in the window partition)
- Aggregate window functions
  - AVG()		  : SELECT dealer_id, sales, AVG(sales) OVER (PARTITION BY dealer_id) AS avgsales FROM q1_sales;
  - COUNT()		  : SELECT dealer_id, sales, COUNT(sales) OVER(PARTITION BY dealer_id) AS `count` FROM q1_sales;
  - SUM()		  : SELECT dealer_id, emp_name, sales, SUM(sales) OVER(PARTITION BY dealer_id) AS `sum` FROM q1_sales;
  - MAX()		  : SELECT emp_name, dealer_id, sales, MAX(sales) OVER(PARTITION BY dealer_id) AS `max` FROM q1_sales;
  - MIN()		  : SELECT emp_name, dealer_id, sales, MIN(sales) OVER(PARTITION BY dealer_id) AS `min` FROM q1_sales;

SYNTAX:
> window_function_name ( expression ) OVER (				#target expression/column on which the window function operates (mandatory clause)
>     					     partition_by_clause	#if not specified, then the whole result set is treated as a single partition
>     					     order_by_clause [ASC | DESC]  [NULL {FIRST| LAST}]
>     					     frame_clause 	)

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

## Constraints : 
Constraints are used to limit the type of data that can go into a table. This ensures the accuracy and reliability of the data in the table.
- NOT NULL - Ensures that a column cannot have a NULL value
- UNIQUE - Ensures that all values in a column are different
- PRIMARY KEY - A combination of a NOT NULL and UNIQUE. Uniquely identifies each row in a table
- FOREIGN KEY - Uniquely identifies a row/record in another table
- CHECK - Ensures that all values in a column satisfies a specific condition
- DEFAULT - Sets a default value for a column when no value is specified
- INDEX - Used to create and retrieve data from the database very quickly

## Search Topics
triggers, sequence, Sub query writing, MDX queries, Error Handling techniques
temp & global tables 
difference bw Delete & Truncate?
aggregate functions, triggers 
deep indexes questions , Concurrency management and control 
Execute sql task, joins , top clause , aggregate functions, triggers
deep indexes questions , Concurrency management and control
Able to identify staging needs and implement staging using Basic Features of SSIS
Able to troubleshoot performance issues using Basic Features of SSIS
Control Flow - Analysis Services Processing Task, Rebuild Index Task, Reorganize Index Task
Data Flow Tasks - pivot, unpivot, fuzzylookup, fuzzygrouping, SCD
Incremental loading,Scheduling a job and Secure Packages
On Error Event in Event Handler
log audit - error description in event handlers, deprecated in sql 2014, is sorted,  pass table name between packages, flat file connections, xml file connection,https://www.slideshare.net/sandip1004/ms-business-intelligence-with-sql-server-2005
Able to identify staging needs and implement staging using Basic Features of SSIS
Able to troubleshoot performance issues using Basic Features of SSIS
Control Flow - Analysis Services Processing Task, Rebuild Index Task, Reorganize Index Task
Data Flow Tasks - pivot, unpivot, fuzzylookup, fuzzygrouping, SCD
Incremental loading,Scheduling a job and Secure Packages
On Error Event in Event Handler
constraints, triggers, sequence, Sub query writing, MDX queries, Error Handling techniques
difference bw Delete & Truncate?

### SSIS Assessments topics
Merge join, execute sql task, log audit - error description in event handlers, deprecated in sql 2014, is sorted,  pass table name between packages, flat file connections, xml file connection,

## Links:
https://www.slideshare.net/sandip1004/ms-business-intelligence-with-sql-server-2005
SSIS
https://www.databasejournal.com/features/mssql/article.php/3886556/Characteristics-and-Common-Usage-Scenarios-of-SSIS-Variables.htm
https://docs.microsoft.com/en-us/sql/integration-services/integration-services-ssis-packages
SSDT
https://channel9.msdn.com/posts/SQL11UPD00-REC-02
http://blog.nwcadence.com/sql-server-data-tools-clearing-up-the-confusion/
https://blogs.msdn.microsoft.com/ssdt/
http://whitepages.unlimitedviz.com/2015/04/sql-server-data-tools-bids-visual-studio/
https://www.youtube.com/watch?v=hdlwDx5o-cc
https://technet.microsoft.com/en-us/library/0d2eb34d-78c8-41ff-b92d-49b62c16b2ac(v=sql.110)
Adventure Works : http://msftdbprodsamples.codeplex.com/
https://moidulhassan.wordpress.com/tag/adventureworks/
Schema : https://moidulhassan.files.wordpress.com/2014/07/adventureworksdw2008.png
Github: https://github.com/infinuendo/AdventureWorks
QnA Exercises : http://sqlzoo.net/wiki/AdventureWorks_easy_questions
MSBI (SSIS, SSRS, SSAS)
https://www.quora.com/What-are-some-of-the-best-resources-to-learn-MSBI-SSIS-SSRS-SSAS
SSIS
http://www.jasonstrate.com/2011/01/31-days-of-ssis-the-introduction/
http://www.codeproject.com/Articles/155829/SQL-Server-Integration-Services-SSIS-Part-Basics#_articleTop
http://www.tutorialgateway.org/ssis/
SCD1 - http://www.techbrothersit.com/2013/10/ssis-load-slowly-changing-dimension-scd.html
Databases:-------------------------------------------------------------------------------------
http://www.vertabelo.com/blog/notes-from-the-lab/18-best-online-resources-for-learning-sql-and-database
https://mitseu.files.wordpress.com/2014/08/microsoft-sql-server-2012-step-by-step-pre-press.pdf
SQL
http://www.cs.rtu.lv/PharePub/Teach%20Yourself%20Sql%20In%2021%20Days%202nd%20Edition/Default.htm
http://www.cheat-sheets.org/sites/sql.su/
SQL Server - SSMS
https://technet.microsoft.com/en-us/library/ms144275(v=sql.110).aspx
http://www.quackit.com/sql_server/sql_server_2014/tutorial/
https://learningsqlserver.wordpress.com/2011/02/04/sql-server-management-studio-ssms-basics/
http://www.codeproject.com/Articles/751447/Learn-Microsoft-Business-intelligence-step-by-step
http://www.codeproject.com/Articles/652108/Create-First-Data-WareHouse
Stored proc SSMS
https://www.youtube.com/playlist?list=PLNIs-AWhQzcleQWADpUgriRxebMkMmi4H
https://www.youtube.com/watch?v=QQdSL41PCck
https://www.youtube.com/watch?v=ckFnl6UduDs
Visual Studio
https://msdn.microsoft.com/en-us/library/mt204009.aspx
Netezza
http://dwgeek.com/commonly-used-netezza-utilities.html/
https://nrkblog.com/2015/12/26/ibm-netezza-interview-questions-23/
https://nrkblog.com/2015/12/26/ibm-netezza-interview-questions/
https://nrkblog.com/2015/12/26/ibm-netezza-interview-questions33/
Databases:-------------------------------------------------------------------------------------
http://www.vertabelo.com/blog/notes-from-the-lab/18-best-online-resources-for-learning-sql-and-database
https://mitseu.files.wordpress.com/2014/08/microsoft-sql-server-2012-step-by-step-pre-press.pdf
SQL 
http://www.cs.rtu.lv/PharePub/Teach%20Yourself%20Sql%20In%2021%20Days%202nd%20Edition/Default.htm
http://www.cheat-sheets.org/sites/sql.su/
SQL Server - SSMS
https://technet.microsoft.com/en-us/library/ms144275(v=sql.110).aspx
http://www.quackit.com/sql_server/sql_server_2014/tutorial/
https://learningsqlserver.wordpress.com/2011/02/04/sql-server-management-studio-ssms-basics/
http://www.codeproject.com/Articles/751447/Learn-Microsoft-Business-intelligence-step-by-step
http://www.codeproject.com/Articles/652108/Create-First-Data-WareHouse
Stored proc SSMS
https://www.youtube.com/playlist?list=PLNIs-AWhQzcleQWADpUgriRxebMkMmi4H
https://www.youtube.com/watch?v=QQdSL41PCck
https://www.youtube.com/watch?v=ckFnl6UduDs
Visual Studio
https://msdn.microsoft.com/en-us/library/mt204009.aspx
Netezza
http://dwgeek.com/commonly-used-netezza-utilities.html/ 
https://nrkblog.com/2015/12/26/ibm-netezza-interview-questions-23/ 
https://nrkblog.com/2015/12/26/ibm-netezza-interview-questions/ 
https://nrkblog.com/2015/12/26/ibm-netezza-interview-questions33/
SSIS
https://www.databasejournal.com/features/mssql/article.php/3886556/Characteristics-and-Common-Usage-Scenarios-of-SSIS-Variables.htm
https://docs.microsoft.com/en-us/sql/integration-services/integration-services-ssis-packages
SSDT
https://channel9.msdn.com/posts/SQL11UPD00-REC-02
http://blog.nwcadence.com/sql-server-data-tools-clearing-up-the-confusion/
https://blogs.msdn.microsoft.com/ssdt/
http://whitepages.unlimitedviz.com/2015/04/sql-server-data-tools-bids-visual-studio/
https://www.youtube.com/watch?v=hdlwDx5o-cc
https://technet.microsoft.com/en-us/library/0d2eb34d-78c8-41ff-b92d-49b62c16b2ac(v=sql.110)
Adventure Works : http://msftdbprodsamples.codeplex.com/
https://moidulhassan.wordpress.com/tag/adventureworks/
Schema : https://moidulhassan.files.wordpress.com/2014/07/adventureworksdw2008.png
Github: https://github.com/infinuendo/AdventureWorks
QnA Exercises : http://sqlzoo.net/wiki/AdventureWorks_easy_questions
MSBI (SSIS, SSRS, SSAS)
https://www.quora.com/What-are-some-of-the-best-resources-to-learn-MSBI-SSIS-SSRS-SSAS
SSIS
http://www.jasonstrate.com/2011/01/31-days-of-ssis-the-introduction/
http://www.codeproject.com/Articles/155829/SQL-Server-Integration-Services-SSIS-Part-Basics#_articleTop
http://www.tutorialgateway.org/ssis/
SCD1 - http://www.techbrothersit.com/2013/10/ssis-load-slowly-changing-dimension-scd.html