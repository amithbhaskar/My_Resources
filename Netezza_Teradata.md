# NETEZZA
Last Updated on : 24 May 2019

Netezza is a powerful data warehouse appliance that can handle gigabyte volume of data and used widely is many medium and large scale organizations.
Netezza’s primary product - TwinFin. In 2012 the products were re-branded as IBM PureData for Analytics. IT has a two tier system. The first tier is a high-performance Linux SMP host that compiles data query tasks received from business intelligence applications, and generates query execution plans. It then divides a query into a sequence of sub-tasks, or snippets that can be executed in parallel, and distributes the snippets to the second tier for execution.
The second tier consists of one to hundreds of snippet processing blades, or S-Blades, where all the primary processing work of the appliance is executed. The S-Blades are intelligent processing nodes that make up the massively parallel processing (MPP) engine of the appliance. Each S-Blade is an independent server that contains multi-core Intel-based CPUs and Netezza’s proprietary multi-engine, high-throughput FPGAs. The S-Blade is composed of a standard blade-server combined with a special Netezza Database Accelerator card that snaps alongside the blade. Each S-Blade is, in turn, connected to multiple disk drives processing multiple data streams in parallel in TwinFin or Skimmer.
SPU - (computer on an integrated circuit card)
Netezza uses asymetric massive parallel processing and have multiple processing units to make query processing faster and run in parallel.
As the data is loaded into the Appliance, it intelligently separates each table across the 108 SPUs. Imagine 108 of these spinning up at once, loading a small piece of the table. After a piece of the table is loaded and stored on each SPU, each column is analyzed to gain descriptive statistics such as minimum and maximum values. Instead of indexes, which take time to create, updated and take up unnecessary space, these values are stored on each of the 108 SPUs.
When it is time to query the data, Only the SPUs that contain appropriate data return information, therefore less movement of information across the network to the Business Intelligence/Analytics Server.
For joining data, it gets even better. The Appliance distributes data in multiple tables across multiple SPUs by a key. Each SPU contains partial data for multiple tables. It joins parts of each table locally on each SPU returning only the local result. All of the ‘local results’ are assembled internally in the cabinet and then returned to the Business Intelligence/Analytics Server as a query result. This methodology also contributes to the speed story.
The key to all of this is ‘less movement of data across the network’. The Appliance only returns data required back to the Business Intelligence/Analytics server across the organization’s 1000/100 MB network.
This is very different from traditional processing where the Business Intelligence/Analytics software typically extracts most of the data from the database to do its processing on its own server. The database does the work to determine the data needed, returning a smaller subset result to the Business Intelligence/Analytics server.
No tedious performance tuning required like in oracle or sql server. i.e. stuff like adding indexes, creating partitioning etc is not required. Only distribution keys have to be set properly on the tables.
Migration of data from one environment to another is very easy using tools such as nz_migrate, nz_sql, nzload.
Netezza utilities: NZSQL, NZLOAD, NZ_MIGRATE, NZUNLOAD
- http://dwgeek.com/commonly-used-netezza-utilities.html/
- http://dwbitechguru.blogspot.com/2014/11/extract-load-migrate-filesdata-from.html
Environment variables: NZ_HOST, NZ_DATABASE, NZ_USER and NZ_PASSWORD

## Netezza Features:
High-Performance: Netezza users are used to DW software and hardware engineered together for optimized performance. Netezza DB is a variant of PostgreSQL stripped down and optimized specifically for DW analytics. It’s architected for x86 Xeon servers. The FPGA accelerates DW SQL queries. Netezza administrators expect exceptional performance without adjusting or tuning.
Relational Databases may store huge data, but NETEZZA can handle multiple concurrent users.

## Netezza Disadvantages:
Twinfin and Striper models are retiring.
Have heard scaling the HW is a difficulty
Netezza IIAS(IBM® Integrated Analytics System) M4001/M4002
The IBM Netezza successor is the IBM IAS (IIAS) M4001/M4002. It’s built on generic general-purpose CPUs, flash storage, and networking.

## Netezza® architecture:
The key hardware components include:
- Snippet blades (S-Blades), also called snippet processing units (SPUs)
  - S-Blade/Snippet Processing Units (SPUs): are synonymous and are often used interchangeably. The S-Blade is a specialized processing board which combines the CPU processing power of a blade server with the query analysis intelligence of the IBM® Netezza® Database Accelerator card. The Netezza Database Accelerator card contains the FPGA query engines, memory, and I/O for processing the data from the disks where user data is stored.
- Hosts
  - Netezza hosts: The Netezza host server (host) is a Linux server that runs the IBM Netezza software and utilities.
- Storage arrays
  - Storage arrays: The storage arrays contain the disks that store the user data and related processing files to support the query activity on the Netezza system.

### View the data in a table
` SELECT * FROM tableName;`

### Case Statement for nulls
` SELECT CASE WHEN DT IS NOT NULL THEN DATE(DT) ELSE NULL END AS DT; `

### List all created tables sorted by table name
```
  SELECT TABLENAME,
       OBJTYPE,
       OWNER,
       CREATEDATE,
       USED_BYTES,
       USED_BYTES/1073741824 as USED_GB,
       RELTUPLES as "ROWS"
  FROM _V_TABLE_ONLY_STORAGE_STAT
  WHERE OBJTYPE='TABLE' AND TABLENAME like '%tableName%'
  ORDER BY TABLENAME;
```

### List table columns and type in Netezza
```
  SELECT RC.NAME,
         RC.ATTNAME,
         RC.FORMAT_TYPE
  FROM _V_TABLE T
  LEFT JOIN _V_RELATION_COLUMN RC ON (T.OBJID = RC.OBJID)
  WHERE T.TABLENAME IN ('tableNameA','tableNameB')
  ORDER BY NAME,ATTNUM;
```

### Create a new or Backup Table from Select Query
```
  CREATE TABLE newTableName
  AS SELECT cols FROM tableName;
```

### Rename a table:
` ALTER TABLE tableName RENAME TO newTableName; `

### Modify length of string field
` ALTER TABLE tableName MODIFY colName CHARACTER VARYING(20); `

### Rename a column
` ALTER TABLE tableName RENAME COLUMN colA to colB;`

### Remove a column from table
` ALTER TABLE tableName DROP COLUMN col2; `

### Truncate a table
` TRUNCATE tableName;`

### Delete the table
` DROP TABLE tableName; `

### Find numerical/alphabet values in a column
` LIKE '%[a-z]%' OR  '%[0-9]%' `

### Find-duplicates-from-netezza-table
```
  SELECT A,B,C,D,E, COUNT(*)
  FROM tableName
  GROUP BY 1,2,3,4,5
  HAVING COUNT(*)>1;
`

` SELECT AllCols
              FROM (  SELECT *,
                      ROW_NUMBER() OVER( PARTITION BY AllCols ORDER BY AllCols ) AS rowNumb
                      FROM TableName) AS aliasName
              WHERE rowNumb=1;
`
```

## Delete Commands 

###  Delete certain rows in a table
```
  DELETE FROM tableName WHERE col = 'Value';
  DELETE FROM tableName
  WHERE col IN (  SELECT A.colA FROM tableA A
                  INNER JOIN tableB B
                  ON A.colA=B.colB );
```

### Delete the duplicate records using rowid column keeping the latest record
```
  DELETE FROM tableName
  WHERE ROWID NOT IN ( SELECT MAX(ROWID) FROM tableName GROUP BY COLS );
```

## Insert/Update Commands 

### Insert data into the table
`  INSERT INTO tableName VALUES  ( 2018,'Val',30 ) `

### Update a value in the table
```
  UPDATE tableName
  SET VAL=(SELECT MAX(col) FROM tableA)
  WHERE colA='Value';
```

## Null Checks / Records Have '' / Blanks 
```
  SELECT * FROM tableName WHERE DTTM IS NULL;
  SELECT CASE WHEN colA = '' THEN NULL ELSE colA END AS colA;
  SELECT CASE WHEN LENGTH(colA)> 0 THEN CAST(colA AS INT) ELSE NULL END AS colA;
```

## Data Type Conversions 

### string to date
` TO_DATE(substring(DTTM from 1 for 10),'YYYY-MM-DD'); `

### Substring in DDMMYYYY to date conversion
` SELECT TO_DATE(SUBSTRING(colA FROM 5 FOR 4)|| SUBSTRING(colA FROM 3 FOR 2)|| SUBSTRING(colA FROM 1 FOR 2),'YYYYMMDD') AS DATED;--03121990 to 19901203 `

### string to timestamp
` TO_TIMESTAMP(colA, 'yyyy-mm-dd HH:MI') `

### timestamp to string
`  TO_CHAR(colA,'YYYY-MM-DD HH24:MI:SS')`

### Find values present in tableA but not in tableB
```
  SELECT DISTINCT colA FROM tableA
  MINUS
  SELECT DISTINCT colA FROM tableB;
```

## SCD1 implementation for DIM tables / MERGE
```
  MERGE INTO my_target_table tableE
      USING (SELECT * FROM ) tableS
  ON   tableE.UID   =  tableS.UID
  WHEN MATCHED  AND tableE.DATAFIELD <> tableS.DATAFIELD THEN
         UPDATE SET tableE.DATAFIELD = tableS.DATAFIELD ,tableE.UPDATED = CURRENT_TIMESTAMP
  WHEN NOT MATCHED  THEN
         INSERT VALUES (tableS.UID,tableS.DATAFIELD,CURRENT_TIMESTAMP,CURRENT_TIMESTAMP);
```

### Not enough memory
Query throws error - Not enough memory for merge-style join ;Before the query execution, you can disable the Merge Join at the session level to force the optimizer to choose a better plan, as shown in the following example:
`  SET ENABLE_MERGEJOIN = off; <QUERY GOES HERE> ;`

###  To load to a table using External table (the path is /nzscratch/filename.txt)
```
CREATE EXTERNAL TABLE '/nzscratch/TableName.txt'
USING (
                                Y2BASE 2000
                                ENCODING 'internal'
                                --ESCAPECHAR '\'
                                Delimiter   '|'
                                INCLUDEHEADER
                  )
AS --unique records only
SELECT AllCols
FROM (  SELECT *,
        ROW_NUMBER() OVER( PARTITION BY AllCols ORDER BY AllCols ) AS rowNumb
        FROM TableName) AS aliasName
WHERE rowNumb=1;
```

## Netezza nzload
The nzload command is a SQL CLI client tool that allows you to load data from the local or a remote client, on all the supported client platforms (Linux/windows).
The nzload command processes command-line load options to send queries to the host to create an external table definition, run the insert/select query to load data, and when the load completes, and then drops the external table.
nzload  -host <host IP>
- -db <database>
- -u <username>
- -pw <password>
- -t table
- –df ‘/home/dw/data/data_file.csv’
- -bf  ‘/home/dw/data/badRows_file.bad’
- -lf ‘/home/dw/data/log_file.log’
- [optional args]
- nzload exit codes: The nzload command exits with the following codes:
  - 0 – Successful, all input records were inserted.
  - 1 – Failed, no records were inserted due to an error or errors found during the load.
  - 2 – Successful, but errors found during the input did not exceed the error threshold (-maxErrors), good records were inserted.

### NZLoad a delimited txt file to a Netezza Table, go to nz utility location and run cmd
 ` ./nzload -host HostName -u admin -pw 'Password' -db DataBaseName -t SchemaName.TableName -outputDir /tmp -df  /PathName/OutputTextFile.txt -skipRows 1  -delim '|' -fillRecord -ctrlChars `

## TO SEE WHETHER THE SYSTEM IS RUNNING SLOW
```
[nz@titanpda01 ~]$ nzhw -detail -type SPU
Description HW ID Location  Role   State  Security Serial number Version Detail
----------- ----- --------- ------ ------ -------- ------------- ------- -------------------------------------------------------------------
SPU         1007  spa1.spu1 Active Online N/A      Y014UF61B0A8  10.0    40 CPU Cores; 125.90GB Memory; Ip Addr: 10.0.15.108; Designated Spu
SPU         1008  spa1.spu3 Active Online N/A      Y014UF61W080  10.0    40 CPU Cores; 125.90GB Memory; Ip Addr: 10.0.8.127;

[nz@titanpda01 ~]$ nzstats
Field Name           Value
-------------------- -------------------------------------------
Name                 titanpda01
Description          Predictive Customer Analytics
Contact              Mr. B L Narahari
Location             DataCenter_R1A1
IP Addr              172.50.7.244
Up Time              867567 secs
Up Time Text         10 days, 0 hrs, 59 mins, 27 secs
Date                 11-Jun-18, 15:10:14 IST
State                8
State Text           Online
Model                IBM PureData System for Analytics N3001-002
Serial Num           7836733
MTM                  3567-CH8
Num SFIs             0
Num SPAs             1
Num SPUs             2
Num Data Slices      40
Num Hardware Issues  0
Num Dataslice Issues 0

[nz@titanpda01 ~]$ nzds
Data Slice Status  SPU  Partition Size (GiB) % Used Supporting Disks
---------- ------- ---- --------- ---------- ------ ----------------
1          Healthy 1007 18        195        21.81  1075,1120
2          Healthy 1007 19        195        16.40  1075,1120
3          Healthy 1007 8         195        15.93  1070,1115
4          Healthy 1007 9         195        15.99  1070,1115
```

### SPU partitions
` nzspupart - nz spu part `
Is the command to display information about the SPU partitions on an IBM® Netezza® system including status information and the disks that support the partition.

### to see the size of disk
` df -h Mounted on `

###  uses of nohup command
nohup stands for no hang up,When you execute a Unix job in the background ( using &, bg command), and logout from the session, your process will get killed.
Symbol & sends the process to the background but it is still tied to your shell process – if the shell dies it will die. nohup does not background the task, but disconnects its standard in and out from the shell so that it is no longer dependent on it. To safely close your shell you need to both put the process in the background and disconnect it.

## How Netezza updates records 
Netezza does not update records in place, it marks records with delete flag. In fact, each record contains two slots, one for create xid another for delete xid. Delete xid allows us to mark a record with current transaction for deletion, up to 31 transactions are allowed in Netezza for all tables. As noted earlier, only one update at a time allowed on the same table though. Here update refers to transactions that are not committed yet. Coming back to delete xid, this is how Netezza maintains transaction roll back and recovery. Once a record is modified, it’s delete xid is given transaction id; this is changed from previous value of 0, all records when loaded will contain 0 for delete xid. Note that FPGA uses its intelligence to scan data before delivering them to host or applications.

## Zone Map 
Zone Map is a persistent table maintained by the Netezza system that contains information about the data in a table created by a user. Netezza would maintain the maximum and minimum value of the column that is stored in an extent An extent is the smallest unit of disk allocation on a SPU. Zonemaps is internal mapping structures to the extents that take advantage of the internal ordering of data to eliminate extents that do not need to be scanned. Zonemaps transparently avoid scanning of unreferenced rows. Zonemaps are created for every column in the table and contain the minimum and maximum values for every extent.

### How the zonemaps are created and updated?
Zonemaps are created and refreshed for every SPU when you Generate statistics, Nzload operation, Insert, update operations, Nzreclaim operation.

## Materialized view
Materialized view is a database object that contains the results of a query. Materialized views which store data based on remote tables are also known as snapshots. Materialized views are disk based and are updated periodically based upon the query definition where as Views are virtual only and run the query definition each time they are accessed. Means to say other than DB tables in the DISK; this MVIEW also maintain a table by resolving all the joins. So that in the runtime single table access is enough; and no need to query multiple tables for join conditions which is usually done by the normal view.

## Install Netezza
http://dwgeek.com/install-aginity-workbench-netezza.html/

### To run SQL cmds in putty
```
[nz@webbia1 ~]$ nzsql
Welcome to nzsql, the IBM Netezza SQL interactive terminal.
Type:  \h for help with SQL commands
       \? for help on internal slash commands
       \g or terminate with semicolon to execute query
       \q to quit
SYSTEM(ADMIN)=>
--To connect to a database called SPSALES_DDS_NEW
SYSTEM(ADMIN)=> \c SPSALES_DDS_NEW
You are now connected to database SPSALES_DDS_NEW.
--To describe a table in the connected database
SPSALES_DDS_NEW(ADMIN)=> \d WORK_STORE_INV_STAGE
                      Table "WORK_STORE_INV_STAGE"
      Attribute       |         Type          | Modifier | Default Value
----------------------+-----------------------+----------+---------------
LOGICALDATE          | INTEGER               | NOT NULL |
STORE_NUM            | CHARACTER VARYING(4)  | NOT NULL |
EAN                  | CHARACTER VARYING(13) | NOT NULL |
MODEL_QTY            | INTEGER               |          |
ON_HAND_QTY          | INTEGER               |          |
ON_ORDER_QTY         | INTEGER               |          |
RETURN_QTY           | INTEGER               |          |
TBO_QTY              | INTEGER               |          |
TBO_ADJ_QTY          | INTEGER               |          |
LAST_SALE_DATE       | INTEGER               |          |
LAST_MODEL_DATE      | INTEGER               |          |
MRQ_RELEASE_DATE     | INTEGER               |          |
FIRST_ORDER_QTY      | INTEGER               |          |
FIRST_ORDER_DATE     | INTEGER               |          |
FIRST_ORDER_DATE_SET | CHARACTER VARYING(20) |          |
FIRST_RECIEVE_QTY    | INTEGER               |          |
FIRST_RECEIVE_DATE   | INTEGER               |          |
FIRST_RCPT_DATE_SET  | CHARACTER VARYING(20) |          |
UPD_USER_ID          | CHARACTER VARYING(20) |          |
FIRST_SALE_DATE      | INTEGER               |          |
LAST_MODEL_USERID    | CHARACTER VARYING(20) |          |
UPD_DTIME            | BIGINT                |          |
Distributed on hash: "LOGICALDATE"
--To run SQL queries in connected database
SPSALES_STAGE(ADMIN)=> select distinct(LOGICALDATE) from WORK_STORE_INV_STAGE;
LOGICALDATE
-------------
    20180728
    20180729
(2 rows)
http://nz2nz.blogspot.com/2017/03/netezza-general-purpose-scripts-toolkit.html
SYSTEM(ADMIN)=> select * from _v_system_info;
SYSTEM_STATE | SYSTEM_STATE_VERSION |      SYSTEM_SOFTWARE_VERSION      | SYSTEM_STATE_VALUE |   SYSTEM_VERSION_FULL   | SYSTEM_CAT_VERSION
--------------+----------------------+-----------------------------------+--------------------+-------------------------+--------------------
Online       |                    5 | Release 7.0.2.13-P1 [Build 39352] |                  8 | 7.0.2.13-P1-F1-Bld39352 | 3.1459
(1 row)
IBM PureData System for Analytics N1001-010 or              5725-E49
```
## Netezza ODBC Drivers:
Netezza ODBC driver is not publicly available. Therefore, you need to contact Netezza support to get a copy of their ODBC driver.
The Aginity "Get Netezza Drivers" Help menu item takes me to IBM Fix Central. I have done numerous search on Fix Central but cannot find the drivers.
Please make sure that you follow the steps below. I suspect you may not have installed the ODBC or OLEDB drivers properly.
[Read the steps carefully in this article](https://www.linkedin.com/pulse/12-easy-steps-install-ibm-netezza-software-emulator-roman-taluyev)
also [Refer] https://www.ibm.com/developerworks/community/forums/html/topic?id=5d05e7d4-079d-4d53-b512-0ba27ba6caa9&mhq=nz-winclient-v7.2&mhsrc=ibmsearch_a)
After server part let’s download and install client part (nz-winclient-v7.2.0.0.zip), again follow the [link](https://www14.software.ibm.com/webapp/iwm/web/reg/pick.do?source=swg-im-ibmndn&lang=en_US) (choose option: IBM Netezza Client Components V7.2 for Windows 7)
Unpack nz-winclient-v7.2.0.0.zip, and install “IBM Netezza Tools” (nzsetup.exe).
Run Nzoledbsetup64, nzodbcsetup, nzjdbcsetup and nzsetup.exe
https://www.ibm.com/support/knowledgecenter/en/SSULQD_7.0.3/com.ibm.nz.datacon.doc/r_datacon_odbc_data_srce_admin_win.html
https://stackoverflow.com/questions/38560454/unable-to-connect-too-netezza-using-aginity-im001-odbc-driver-manager-error
https://www.linkedin.com/pulse/12-easy-steps-install-ibm-netezza-software-emulator-roman-taluyev

### ODBC/JDBC Explanation:
Flow of Queries : Any Application -> Java API -> JDBC DRIVERS -> Query Language API -> DB Engine -> DATABASE
Similar ODBC rep: Any Application -> ODBC DRIVERS -> DB Engine -> DATABASE
http://databasebestpractices.com/category/netezza/page/42/

Hi,
Please find the steps to setup DBMS viewing tool - Aginity Workbench for PureData System for Analytics.
- Download and copy Putty.exe program to “C:\Program Files (x86)\PuTTY\”, from the link : http://the.earth.li/~sgtatham/putty/latest/x86/putty.exe
We can connect to the Netezza database using the terminal. Please check if you have access through the firewall.
Alternatively:
- Netezza ODBC driver is not publicly available. Therefore, we need to contact Netezza support to get a copy of their ODBC driver. Please find the drivers available in this link: https://www14.software.ibm.com/webapp/iwm/web/reg/pick.do?source=swg-im-ibmndn&lang=en_US
- Unpack nz-winclient-v7.2.0.0.zip, and install “IBM Netezza Tools” (nzsetup.exe). Run all Nzoledbsetup64, nzodbcsetup, nzjdbcsetup and nzsetup.exe
- Run “IBM Netezza Administrator” ("C:\Program Files (x86)\IBM Netezza Tools\Bin\NzAdmin.exe"). Enter Host name, user name & password for the Netezza Database to test the connection.
- Download software "Aginity Workbench for PureData System for Analytics" from link : https://www.aginity.com/main/netezza-download-link/
- Connect using proper credentials and validate the connection.
Please let me know if you need any additional info regarding this.
Thanks,
Amith.

## Stored Procedures
- to call a stored procedure using only select statements:
`EXECUTE SP_ORG_BREAKUP ;`
-- example stored procedure : First Create reference table
```
                CREATE TABLE WEBSERVICES.TAB1
                (
                                ORG_ID INTEGER,
                                ON_HAND_QTY INTEGER
                )
                DISTRIBUTE ON (ORG_ID);
-- Then use the table in Stored Procedure
                CREATE OR REPLACE PROCEDURE SP_ORG_BREAKUP(INTEGER)
                RETURNS REFTABLE(TEST.WEBSERVICES.TAB1)
                LANGUAGE NZPLSQL AS
                BEGIN_PROC
                                DECLARE
                                SnapDate ALIAS FOR $1;
                BEGIN
                                --Use query to insert data
                                EXECUTE IMMEDIATE 'INSERT INTO ' || REFTABLENAME ||' SELECT ORG_ID, SUM(NVL(ON_HAND_QTY,0)) as ON_HAND_QTY FROM TEST.WEBSERVICES.FACT_SUPP_DAILY_INVENTORY_SNAP_EORG WHERE DATE_ID = ' || SnapDate ||' GROUP BY ORG_ID';
                                RETURN REFTABLE;
                END;
                END_PROC;
--END of Stored Procedure
```

### Dates joined with calender dates
SELECT distinct w.myDate, t.date_id
FROM myTable w
FULL JOIN DIM_TIME t
ON w.myDate=t.date_id
WHERE t.date_id BETWEEN 20160101 AND 20161231 ;

### To find a which column(s) a table is organized on you can use one of the following system table queries.
```
                select *
                from _v_odbc_columns1
                where orgseqno is not null
                --OR
                select *
                from _v_table_organize_column
```

### To Find the column(s) a table is distributed on.
```
                select tablename, owner, attname as distribution_column
                from _v_table_dist_map
                --If the table has no value then the table is distributed on random;
```

## Groom in Netezza
http://dwgeek.com/groom-netezza-aginity.html/
--DB size
http://dwgeek.com/get-netezza-databases-allocated-used-available-space.html/

## Database objects count by DB names:
```
SELECT database::nvarchar(64) AS database, 
OBJTYPE, count(OBJNAME) AS Tables_count
FROM (SELECT * FROM _V_OBJ_RELATION_XDB
WHERE database <> 'SYSTEM') a
GROUP BY database, OBJTYPE;
```
[More details here](http://dwgeek.com/netezza-count-database-objects-system-table-query.html/)

## NZ Utilities

### Generate Netezza Table DDL using nz_ddl_table utility
```
cd /nz/support/bin
nz_ddl_table Netezza_DB_name NETEZZA_Table_name >> FileName.txt
```
[DDL Utility Details](http://dwgeek.com/generate-netezza-table-ddl-using-nz_ddl_table.html/)

--Commands to Generate DDL of Users, Groups, Tables, Views & Functions and Procedure in IBM Netezza
```
nz_ddl_user                       --DDL for all user run this from OS.
nz_ddl_group                    --DDL all groups
nz_ddl_grant_group       --DDL for all grant given to groups
nz_ddl_grant_user  
nz_ddl_mview 
nz_ddl_schema  
nz_ddl_view
nz_ddl_ext_table  
nz_ddl_group   
nz_ddl_table      
nz_ddl_function 
nz_ddl_owner
nz_ddl_database 
nz_ddl_grant_group
nz_ddl_procedure 
nz_ddl_synonym
```

## Query Otpimization Techniques
Specifying Not Null on each column in table results in better performance. Netezza tracks the NULL values at rowheader level. Having NULL values results in storing references to NULL values in header. If all columns are NOT NULL, then there is no record header.

## How FPGA can be helpful in improving query performance?
While reading data from the disk, the Field Programmable Gate Array (FPGA) on each SPU filters out unwanted data. This process of data elimination removes IO bottlenecks and frees up downstream components such as the CPU, memory and network from processing extra data

## Importance of right Netezza Distribution key
Let’s first understand how Netezza Processing Sysytem stores the data on disk drives. 
- Each Snippet Processor in the Snippet Processing Unit (SPU) has a dedicated hard drive has its separate 
  [CPU, FPGA, separate RAM memory, hard disks] and the data on stored on drive is called a data slice.
Each snippet processing unit (SPU) disk is divided into three partitions.
- Primary Partition: This partition contains its own set of data.
- Mirror Partition: This partition contains another SPU’s primary partition data. The mirror partition automatically mirrors another SPU’s primary data slice. This process is called mirroring. In multi-rack configuration, a SPU mirror is located in another rack. This enables the Netezza to handle the fault tolerance. In case of racks fails, data is mirrored in other rack and can be recovered. This process is also called replication.
- Swap Partition: Swap partition is used for aggregating, sorting and other query operations. Swap partition is not mirrored in other word it is called temp or intermediate processing data. Netezza makes use of this temp partition to perform the SQL operations.
Tables are split across multiple SPUs, data slices and the data is stored in groups in same or nearby data slices according to rows, while data is compressed according to identical column values i.e. columnar compression.
Data should uniformly distribute across all the data slices, the processing performance is directly dependent on it. The even distribution of the data over all data slices is directly related to key column(s) used in distribution on clause. Bad distribution key(s) can cause the Netezza to place data on same slices and that will cause the data skew, a major performance bottleneck.
A distribution method that distributes data evenly across all data slices is the single most important factor that can increase overall system performance. Bad distribution key(s) can result in uneven distribution of a table across data slices and SPUs that will cause skew, causing data to be redistributed or broadcasted, of course that will hamper the system performance. It is very important to identify the correct and proper distribution key when creating table definition and that require the extremely good knowledge on data and system.

## Distribution on multiple keys
The maximum number of columns that can specify in distribution on clause is four. Keep in mind that you should not use the multiple keys to distribute data if the chosen key provides good data distribution. However, distribution keys should be used in join condition in order to achieve co-location. If we use multiple distribution keys, then those columns must be used in joining conditions. 
- Leaving out ‘Distribution on’ clause in Netezza distribution
If Netezza distribution key is not specified, system will, by default distribute the data on first column (just like Teradata) using hash partition method. This process sometime can cause the skew if the first column is something like flag.  The best method is to distribute data on column or on random if you are not able identify the best column.
Consider the following factors when choosing distribution keys:
- The more distinct the distribution key values, the better.
- The system distributes rows with the same distribution key value to the same data slice.
- Parallel processing is more efficient when you have distributed table rows evenly across the data slices.
- Tables used together should use the same columns for their distribution key. For example, in an order system application, use the Customer ID as the distribution key for both the customer table and the order table.
- If a particular key is used largely in Equi-join clauses, then that key is a good choice for the distribution key.
http://dwgeek.com/distribution-key-netezza.html/

# Teradata V13--------------------------------------------------------------------
- Teradata Architecture & Terminology:-
- PE - Parsing Engine : contains Parser+Optimizer+Generator+Dispatcher+SessionController.
- Bynet - Cables & Bus connecting hardware interfaces/drivers.
- AMP - Access Module Processor : stores their own data that comes from the Parsing Engine and performs Data Manipulation activities. AMPs have shared nothing architecture.
- SMP Nodes contain individual (processors+memory) called AMP(Access Module Processor). Combinations of multiple AMPs are called clusters.
- Clique process : picks up data from 1 node to another node, similar to node level back ups.

## Teradata Space Management Terminology:-
- Permanent Space per node consists of 2 spaces:
  - Spool Space - unused permanent space to run queries for each user. No more spool space in [userspace] :Usually occurs when query data is too big.
  - Temp Space - only used for Global Temporary Table(GTT) where its assigned only during an active session, not assigend to any particular user. GTT gets destroyed when user is logged out and session expires.
Start Teradata Services : Connects to server
Go to Teradata SQL Assistant -> Select Data Source -> Connect to TDuser
View Database Explorer Window: DBC is Database Computer.
SEL HASHAMP() : views number of AMPs starting from 0 index. Eg. 1 means 0 & 1 AMPS.
```
CREATE SET TABLE DBname.TableName, --SET doesnt allow Rowlevel duplicates
NO FALLBACK --FALLBACK keeps backup of data in other AMPS.
NO BEFORE JOURNAL NO AFTER JOURNAL --Keeps copy of data before and after each transaction performed on the table, used mostly for error prone transactions
CHECKSUM=DEFAULT --Checks for any corrupted data in the table (similar to Defragmentation)
```

## Links
https://pt.slideshare.net/RavikumarNandigam/data-loading-and-unloading-in-ibm-netezza?ref=&smtNoRedir=1
http://dwgeek.com/netezza-external-table-examples.html/
http://dwgeek.com/check-netezza-system-configurations-using-system-commands.html/
https://www.slideshare.net/bijugs/netezza-fundamentals-for-developers
https://mindmajix.com/netezza-interview-questions

##To Search:
update working:does nz delete a row or logically
query optimization
types to load data ? how external table to load data
partitioning key
netezza IIAS
UPSERT for SCD Type 1 loading
upsert cmd
how indexes work, triggers, tablespaces, planner,
sequences
tweak queries, making them run as fast as possible and able to tweak tables making them use less space as they can
nz connect from prompt
NZSQL and its options
Netezza Architecture
Zone maps
Nzload
External table
Distribution key
Joins
Basic query like finding duplicates, scenario based and join or schema based questions
How to check the status of Netezza system
Can we insert duplicate data in Netezza table
What are the restriction of Netezza Materialized Views
How to update the Zonemap
Command to generate statistics
How to check the issues with slowly performing Netezza server from host (Linux command prompt)
How to check the mount point usage and what to do as a remedy
SQL
Joins and types
group by
heirachy
selfjoin
types index and their importace
Basic SQL Functions
Introduction
SELECT * (All Columns) in a Table
Fully Qualifying a Database, Schema and Table
SELECT Specific Columns in a Table
Commas in the Front or Back?
Using Good Form
Using the Best Form for Writing SQL
Place your Commas in front for better Debugging Capabilities
Sort the Data with the ORDER BY Keyword
ORDER BY Defaults to Ascending
Use the Name or the Number in your ORDER BY Statement
Two Examples of ORDER BY using Different Techniques
Changing the ORDER BY to Descending Order
NULL Values sort First in Ascending Mode (Default)
NULL Values sort Last in Descending Mode (DESC)
Major Sort vs. Minor Sorts
Multiple Sort Keys using Names vs. Numbers
Sorts are Alphabetical, NOT Logical
Using A CASE Statement to Sort Logically
How to ALIAS a Column Name
A Missing Comma can by Mistake become an Alias
Using Limit to bring back a Sample
Comments using Double Dashes are Single Line Comments
Comments for Multi-Lines
Comments for Multi-Lines As Double Dashes Per Line
A Great Technique for Comments to Look for SQL Errors
The WHERE Clause
The WHERE Clause limits Returning Rows
Using a Column ALIAS throughout the SQL
Double Quoted Aliases are for Reserved Words and Spaces
Character Data needs Single Quotes in the WHERE Clause
Character Data needs Single Quotes, but Numbers Don't
NULL means UNKNOWN DATA so Equal (=) won't Work
Use IS NULL or IS NOT NULL when dealing with NULLs
NULL is UNKNOWN DATA so NOT Equal won't Work
Use IS NULL or IS NOT NULL when dealing with NULLs
Using Greater Than Or Equal To (>=)
AND in the WHERE Clause
Troubleshooting AND
OR in the WHERE Clause
Troubleshooting OR
OR must utilize the Column Name Each Time 4
Troubleshooting Character Data
Using Different Columns in an AND Statement
Quiz – How many rows will return?
Answer to Quiz – How many rows will return?
What is the Order of Precedence?
Using Parentheses to change the Order of Precedence
Using an IN List in place of OR
The IN List is an Excellent Technique
IN List vs. OR brings the same Results
Using a NOT IN List
A Technique for Handling Nulls with a NOT IN List
A Better Technique for Handling Nulls with a NOT IN List
BETWEEN is Inclusive 7
BETWEEN Works for Character Data
LIKE uses Wildcards Percent '%' and Underscore '_'
LIKE command Underscore is Wildcard for one Character
LIKE Command Works Differently on Char Vs Varchar
Troubleshooting LIKE Command on Character Data
Introducing the TRIM Command
Quiz – What Data is Left Justified and What is Right?
Numbers are Right Justified and Character Data is Left
Answer – What Data is Left Justified and What is Right?
An Example of Data with Left and Right Justification
A Visual of CHARACTER Data vs. VARCHAR Data
Use the TRIM command to remove spaces on CHAR Data
TRIM Eliminates Leading and Trailing Spaces
Escape Character in the LIKE Command changes Wildcards
Escape Characters Turn off Wildcards in the LIKE Command
Quiz – Turn off that Wildcard
ANSWER – To Find that Wildcard
Distinct Vs Group By
The Distinct Command
Distinct vs. GROUP BY
Rules of Thumb for DISTINCT Vs GROUP BY
Quiz – How many rows come back from the Distinct?
Answer – How many rows come back from the Distinct?
Review
Testing Your Knowledge 1
Testing Your Knowledge 2
Testing Your Knowledge 3
Testing Your Knowledge 4
Testing Your Knowledge 5
Testing Your Knowledge 6
Testing Your Knowledge 7
Aggregation Function
Quiz – You calculate the Answer Set in your own Mind
Answer – You calculate the Answer Set in your own Mind
The 3 Rules of Aggregation
There are Five Aggregates
Quiz – How many rows come back?
Troubleshooting Aggregates
GROUP BY when Aggregates and Normal Columns Mix
GROUP BY Delivers one row per Group
GROUP BY Dept_No or GROUP BY 1 the same thing
Aggregates and Derived Data
Limiting Rows and Improving Performance with WHERE
WHERE Clause in Aggregation limits unneeded Calculations
Keyword HAVING tests Aggregates after they are Totaled
Keyword HAVING is like an Extra WHERE Clause for Totals
Getting the Average Values per Column
Average Values Per Column For all Columns in a Table
Three types of Advanced Grouping
GROUP BY Grouping Sets
GROUP BY Rollup
GROUP BY Rollup Result Set
GROUP BY Cube
GROUP BY CUBE Result Set
GROUP BY CUBE Result Set
Testing Your Knowledge
Testing Your Knowledge
Testing Your Knowledge
Testing Your Knowledge
Testing Your Knowledge
Final Answer to Test Your Knowledge on Aggregates
Join Functions
A two-table join using Non-ANSI Syntax
A two-table join using Non-ANSI Syntax with Table Alias 5
Aliases and Fully Qualifying Columns
A two-table join using Non-ANSI Syntax 7
Both Queries have the same Results and Performance
Quiz – Can You Finish the Join Syntax?
Answer to Quiz – Can You Finish the Join Syntax?
Quiz – Can You Find the Error?
Answer to Quiz – Can You Find the Error?
Quiz – Which rows from both tables won’t return?
Answer to Quiz – Which rows from both tables Won't Return?
LEFT OUTER JOIN
LEFT OUTER JOIN Example and Results
RIGHT OUTER JOIN
RIGHT OUTER JOIN Example and Results
FULL OUTER JOIN FULL OUTER JOIN Example and Results
Which Tables are the Left and which are the Right?
Answer - Which Tables are the Left and which are the Right?
INNER JOIN with Additional AND Clause
ANSI INNER JOIN with Additional AND Clause
ANSI INNER JOIN with Additional WHERE Clause
OUTER JOIN with Additional WHERE Clause
OUTER JOIN with Additional AND Clause
OUTER JOIN with Additional AND Clause Example
Quiz – Why is this considered an INNER JOIN?
The DREADED Product Join
The DREADED Product Join
The Horrifying Cartesian Product Join 2
The ANSI Cartesian Join will ERROR Quiz – Do these Joins Return the Same Answer Set?
Answer – Do these Joins Return the Same Answer Set?
The CROSS JOIN
The CROSS JOIN Answer Set
The Self Join
The Self Join with ANSI Syntax
Quiz – Will both queries bring back the same Answer Set?
Answer – Will both queries bring back the same Answer Set?
Quiz – Will both queries bring back the same Answer Set?
Answer – Will both queries bring back the same Answer Set?
How would you join these two tables?
How would you join these two tables? You can’t….Yet!
An Associative Table is a Bridge that Joins Two Table
Quiz – Can you write the 3-Table Join?
Answer to Quiz – Can you write the 3-Table Join?
Answer – Can you write the 3-Table Join to ANSI Syntax?
Quiz – Can you Place the ON Clauses at the End?
Answer – Can you Place the ON Clauses at the End?
The 5-Table Join – Logical Insurance Model
Quiz - Write a Five Table Join Using ANSI Syntax
Answer - Write a Five Table Join Using ANSI Syntax
Quiz – Write a Five Table Join Using Non-ANSI Syntax
Answer - Write a Five Table Join Using Non-ANSI Syntax
Quiz –Re-Write this putting the ON clauses at the END
Answer –Re-Write this putting the ON clauses at the END
The Nexus Query Chameleon Writes the SQL for Users.
Date Functions
Date, Time, and Timestamp Keywords
Add or Subtract Days from a date
The to_char command
Conversion Functions
Conversion Function Templates
Conversion Function Templates Continued
Formatting A Date
A Summary of Math Operations on Dates
Using a Math Operation to find your Age in Years
Find What Day of the week you were Born
The ADD_MONTHS Command
Using the ADD_MONTHS Command to Add 1-Year
Using the ADD_MONTHS Command to Add 5-Years
Date Related Functions
The EXTRACT Command
EXTRACT from DATES and TIME
EXTRACT with DATE and TIME Literals
EXTRACT of the Month on Aggregate Queries
A Side Title example with Reserved Words as an Alias 1
Implied Extract of Day, Month, and Year 2
DATE_PART Function
DATE_PART Function using an ALIAS
DATE_TRUNC Function
DATE_TRUNC Function using TIME
MONTHS_BETWEEN Function
MONTHS_BETWEEN Function in Action
ANSI TIME
ANSI TIMESTAMP
Netezza TIMESTAMP Function
Netezza TO_TIMESTAMP Function
Netezza NOW() Function
Netezza TIMEOFDAY Function
Netezza AGE Function
Time Zones
Setting Time Zones
Using Time Zones
Intervals for Date, Time, and Timestamp
Using Intervals
Troubleshooting The Basics of a Simple Interval
Interval Arithmetic Results
A Date Interval Example
A Time Interval Example
A - DATE Interval Example
A Complex Time Interval Example using CAST
A Complex Time Interval Example using CAST
The OVERLAPS Command
An OVERLAPS Example that Returns No Rows
The OVERLAPS Command using TIME
The OVERLAPS Command using a NULL Value
OLAP Functions
How ANSI Moving SUM Handles the Sort
Quiz – How is that Total Calculated?
Answer to Quiz – How is that Total Calculated?
Moving SUM every 3-rows Vs a Continuous Average
Partition By Resets an ANSI OLAP
The ANSI Moving Window is Current Row and Preceding
How ANSI Moving Average Handles the Sort
Quiz – How is that Total Calculated?
Answer to Quiz – How is that Total Calculated?
Quiz – How is that 4th Row Calculated?
Answer to Quiz – How is that 4th Row Calculated?
Moving Average every 3-rows Vs a Continuous Average
Partition By Resets an ANSI OLAP
Moving Difference using ANSI Syntax
Moving Difference using ANSI Syntax with Partition By
RANK using ANSI Syntax Defaults to Ascending Order
Getting RANK using ANSI Syntax to Sort in DESC Order
RANK() OVER and PARTITION BY
RANK() OVER And LIMIT
PERCENT_RANK() OVER
PERCENT_RANK() OVER with 14 rows in Calculation
PERCENT_RANK() OVER with 21 rows in Calculation
Quiz – What Causes the Product_ID to Reset?
Answer to Quiz – What Cause the Product_ID to Reset?
COUNT OVER for a Sequential Number
Troubleshooting COUNT OVER
Quiz – What caused the COUNT OVER to Reset?
Answer to Quiz – What caused the COUNT OVER to Reset?
The MAX OVER Command
MAX OVER with PARTITION BY Reset
Troubleshooting MAX OVER
The MIN OVER Command
Troubleshooting MIN OVER
Quiz – Fill in the Blank
Answer to Quiz – Fill in the Blank
The Row_Number Command
Quiz – How did the Row_Number Reset?
Quiz – How did the Row_Number Reset?
Standard Deviation Functions Using STDDEV / OVER
Standard Deviation Functions and STDDEV / OVER Syntax
STDDEV / OVER Example
Variance Functions Using VARIANCE / OVER
VARIANCE / OVER Syntax
Using VARIANCE with PARTITION BY Example
Using FIRST_VALUE and LAST_VALUE
Using FIRST_VALUE
Using LAST_VALUE
Using LAG and LEAD
Using LEAD
Using LEAD With and Offset of 2 294
Using LAG
Using LAG With an Offset of 2
Temporary Tables
There are Three Types of Temporary Tables
CREATING A Derived Table
Naming the Derived Table
Aliasing the Column Names in The Derived Table
Multiple Ways to Alias the Columns in a Derived Table
CREATING A Derived Table using the WITH Command
Naming the Derived Table Columns using WITH
The Same Derived Query shown Three Different Ways
Most Derived Tables Are Used To Join To Other Tables
Our Join Example With A Different Column Aliasing Style
Column Aliasing Can Default For Normal Columns
Our Join Example With The WITH Syntax
Quiz - Answer the Questions
Answer to Quiz - Answer the Questions
Clever Tricks on Aliasing Columns in a Derived Table
An Example of Two Derived Tables in a Single Query
Syntax For Creating A Temporary Table
Creating and Populating a Temporary Table
A Temporary Table in Action
A Temporary Table Can Be Used Again and Again
Alternative CREATE TEMPORARY TABLE Option
A CTAS Temp Table to Improve Zone Map Selectivity
Creating a Temp Table as a Cluster Based Table (CBT)
What Are External Tables?
External Tables Data Loading Formats
External Table Log Files
External Table Syntax
Exporting Data Off of Netezza into an External Table
Importing Data Into Netezza Using an External Table
What is the Problem Here?
Sub-query Functions
An IN List is much like a Subquery
An IN List Never has Duplicates – Just like a Subquery
An IN List Ignores Duplicates
The Subquery
How a Basic Subquery Works
The Final Answer Set from the Subquery
Quiz- Answer the Difficult Question
Answer to Quiz- Answer the Difficult Question
Should you use a Subquery of a Join?
Quiz- Write the Subquery
Answer to Quiz- Write the Subquery
Quiz- Write the More Difficult Subquery
Answer to Quiz- Write the More Difficult Subquery
Quiz- Write the Subquery with an Aggregate
Answer to Quiz- Write the Subquery with an Aggregate
Quiz- Write the Correlated Subquery
Answer to Quiz- Write the Correlated Subquery
The Basics of a Correlated Subquery
The Top Query always runs first in a Correlated Subquery
The Bottom Query runs Last in a Correlated Subquery
Quiz- Who is coming back in the Final Answer Set?
Answer- Who is coming back in the Final Answer Set?
Correlated Subquery Example vs. a Join with a Derived Table
Quiz- A Second Chance To Write a Correlated Subquery
Answer - A Second Chance to Write a Correlated Subquery
Quiz- A Third Chance To Write a Correlated Subquery
Answer - A Third Chance to Write a Correlated Subquery
Quiz- Last Chance To Write a Correlated Subquery
Answer – Last Chance to Write a Correlated Subquery
Correlated Subquery that Finds Duplicates
Quiz- Write the NOT Subquery
Answer to Quiz- Write the NOT Subquery
Quiz- Write the Subquery using a WHERE Clause
Answer - Write the Subquery using a WHERE Clause
Quiz- Write the Subquery with Two Parameters
Answer to Quiz- Write the Subquery with Two Parameters
How the Double Parameter Subquery Works
More on how the Double Parameter Subquery Works
Quiz – Write the Triple Subquery
Answer to Quiz – Write the Triple Subquery
Quiz – How many rows return on a NOT IN with a NULL?
Answer – How many rows return on a NOT IN with a NULL?
How to handle a NOT IN with Potential NULL Values
IN is equivalent to =ANY Using a Correlated Exists
How a Correlated Exists matches up
The Correlated NOT Exists
The Correlated NOT Exists Answer Set
Quiz – How many rows come back from this NOT Exists?
Answer – How many rows come back from this NOT Exists?
Substrings and Positioning Functions
The LOWER Function
The UPPER Function
CHARACTER_LENGTH
OCTET_LENGTH
TRIM for Troubleshooting the CHARACTERS Command
The TRIM Command trims both Leading and Trailing Spaces
Trim and Trailing is Case Sensitive
Trim and Trailing works if Case right
Trim Combined with the CHARACTERS Command
How to TRIM only the Trailing Spaces
How to TRIM Trailing Letters
How to TRIM Trailing Letters and use CHARACTER_Length
LTRIM Function
RTRIM Function
BTRIM Function
The SUBSTRING Command
How SUBSTRING Works with NO ENDING POSITION
Using SUBSTRING to move Backwards
How SUBSTRING Works with a Starting Position of -1
How SUBSTRING Works with an Ending Position of 0
An Example using SUBSTRING, TRIM, and CHAR Together
SUBSTRING and SUBSTR are equal, but use different syntax
The POSITION Command finds a Letters Position
STRPOS Function
The POSITION And STRPOS Do The Same Thing
SUBSTRING and POSITION Used Together In An UPDATE
The POSITION Command is brilliant with SUBSTRING
Quiz – Name that SUBSTRING Starting and For Length
Answer to Quiz – Name that Starting and For Length
Using the SUBSTRING to Find the Second Word On
Quiz – Why Did only one Row Return
Answer to Quiz – Why Did only one Row Return
Concatenation
Concatenation and SUBSTRING
Four Concatenations Together
Troubleshooting Concatenation
Miscellaneous Character Functions - ASCII
Miscellaneous Character Functions - CHR
Miscellaneous Character Functions - INITCAP
Miscellaneous Character Functions - REPEAT
Miscellaneous Character Functions - TRANSLATE
Character Padding Functions - LPAD Function
Character Padding Functions - RPAD Function
Interrogating the Data
NVL Syntax
NVL Example
NVL Is Often Used With Calculations
Comparisons of NVL
A Real-World NVL Example
NVL2
NVL2 Example
NVL2 Syntax
A Real-World NVL2 Example
DECODE Syntax
DECODE Example
A Real-World DECODE Example
Quiz – Fill in the Answers for the NULLIF Command
Quiz – Fill in the Answers for the NULLIF Command
The COALESCE Command
The COALESCE Answer Set
The Coalesce Quiz
Answer – The Coalesce Quiz
The Basics of CAST (Convert And Store)
Some Great CAST (Convert And Store) Examples
Some Great CAST (Convert And Store) Examples
Some Great CAST (Convert And Store) Examples
Round Function
Round Function Continued
The Basics of the CASE Statements
The Basics of the CASE Statement shown Visually
Valued Case Vs. A Searched Case
Quiz - Valued Case Statement
Answer - Valued Case Statement
Quiz - Searched Case Statement
Answer - Searched Case Statement
Quiz - When NO ELSE is present in CASE Statement
Answer - When NO ELSE is present in CASE Statement
When an ELSE is present in CASE Statement
When NO ELSE is present in CASE Statement
When an Alias is NOT used in a CASE Statement
When an Alias is NOT used in a CASE Statement
Combining Searched Case and Valued Case
A Trick for getting a Horizontal Case
Nested Case
Put a CASE in the ORDER BY
View Functions
Creating a Simple View
Basic Rules for Views
Views sometimes CREATED for Formatting or Row Security
Another Way to Alias Columns in a View CREATE
Resolving Aliasing Problems in a View CREATE
Resolving Aliasing Problems in a View CREATE
Resolving Aliasing Problems in a View CREATE
CREATING Views for Complex SQL such as Joins
WHY certain columns need Aliasing in a View
Using a WHERE Clause When Selecting From a View
Altering A Table
Altering A Table After a View has been Created
A View that Errors After An ALTER
Troubleshooting a View
Set Operators Functions
Rules of Set Operators
INTERSECT Explained Logically
INTERSECT Explained Logically
UNION Explained Logically
UNION Explained Logically
UNION ALL Explained Logically
UNION Explained Logically
EXCEPT Explained Logically EXCEPT Explained Logically
Minus Explained Logically
Minus Explained Logically
Testing Your Knowledge
Testing Your Knowledge
An Equal Amount of Columns in both SELECT List
Columns in the SELECT list should be from the same Domain
The Top Query handles all Aliases
The Bottom Query does the ORDER BY (a Number)
Great Trick: Place your Set Operator in a Derived Table
UNION Vs UNION ALL
Using UNION ALL and Literals
A Great Example of how EXCEPT works
USING Multiple SET Operators in a Single Request
Changing the Order of Precedence with Parentheses
Using UNION to be same as GROUP BY GROUPING SETS
Using UNION to be same as GROUP BY ROLLUP
Using UNION to be the same as GROUP BY Cube
Using UNION to be same as GROUP BY Cube
Data Manipulations
Netezza Transactions
BEGIN Command
COMMIT Command
What Happens on a Transaction Error?
Can I See My Uncommitted Changes?
Until the Commit Others Can't See Your Changes?
ROLLBACK Command
ROLLBACK Command in ACTION INSERT Command
INSERT With Keyword Null
A Different Syntax for the INSERT Statements
These Three Statements are the Same
A Third Way of Doing an INSERT
Netezza Has Implemented the Default Values Clause
INSERT/SELECT
INSERT/SELECT Examples
Another Syntax for the INSERT/SELECT
INSERT/SELECT Used To CREATE A Data Mart
UPDATE
An UPDATE In Action
An UPDATE With Multiple WHERE and AND Clauses
An UPDATE With Multiple WHERE and AND Clauses
UPDATE Using A Subquery
UPDATE Using A Subquery
UPDATE Using A Subquery
UPDATE Using A Join
DELETE
Two DELETE Examples
DELETE Through a Subquery or Join
DELETE Through a Subquery And A Join Examples
Multi-Statement Example
How to Undo A Delete
A Delete Example Query How to Undo a Delete
How to Undo a Delete In Action
Tables, DDL, and Data Types
CREATE TABLE Syntax
Viewing the DDL
Netezza Tables - Distribution Key or Random Distribution
Table CREATE Examples with 4 different Distribution Keys
The Worst Mistake You Can Make For A Distribution Key
Good things to know about Table and Object Names
Netezza Data Types
Netezza Data Types in More Detail
Netezza Data Type Extensions
Reserved Names Within A Table
How To Query and See Non-Active Rows
Column Attributes
Constraints
Constraints
Column Level Constraint Example
Defining Constraints at the Table Level
Utilizing Default Values for a Table
CTAS (Create Table AS)
CTAS Facts
Using the CTAS (Create Table AS) Table For Co-Location
Altering a CTAS Table to Rename It
FPGA Card and Zone Maps – The Netezza Secret Weapon
How A CTAS with ORDER BY Improves Queries
A CTAS Major Sort Benefits over the Minor Sort
Altering A Table
Altering a Table Examples
Drop Table, Truncate, and Delete Compared
Creating and Dropping a Netezza Database
How to Determine the Database you are in?
Netezza Users
Altering a Netezza User
Reserved Words to find out about a User
Statistical Aggregate Functions
The Stats Table
The STDDEV_POP Function
A STDDEV_POP Example
The STDDEV_SAMP Function
A STDDEV_SAMP Example
The VAR_POP Function
A VAR_POP Example
The VAR_SAMP Function
A VAR_SAMP Example
Using GROUP BY
A Great Query Example
Stored Procedure Functions
Netezza Stored Procedures
Creating and Executing a Stored Procedure
Creating a Stored Procedure
Netezza Provides Multiple Ways to Run the Stored Procedure
You Can Have Multiple BEGIN and END statements
How to Declare and Set a Variable
Declaring a Variable With A Value
Input Parameters
Input Parameters Using Character Data
Calling a Procedure With Multiple Input Parameters
CREATE OR REPLACE Procedure : http://www.lexykassan.com/coding-conundrums/netezza-stored-procedures-and-optional-arguments/
IF THEN ELSE IF Techniques
An Easier Way for IF THEN ELSE is ELSIF or ELSEIF
Using Loops in Stored Procedures
Using Loops with Different EXIT strategies
Looping With The WHILE Statement
Stored Procedure Workshop
Using FOR to Loop
Nexus Query Chameleon
The Old Nexus Logo
The New Nexus Logo
Watch the Video on the new Nexus Super Join Builder
How to Customize your System Tree View
Introducing the new Nexus Super Join Builder
Define your Joins and tell Nexus to 'Add and Remember Me'
Nexus knows what Tables Join together
Nexus Presents Tables and their columns in Color
Nexus Builds your SQL Automagically
Nexus can Cube a Table and Join to Everything Possible
Nexus can Cube a Table and Join to Everything Possible
The Cube SQL created Automagically
Manipulate the Columns with the Columns Tab
Single Click and ORDER BY using the Sort Tab
Using the Joins Tab of Nexus
The SQL Tab reflects the changes we make in all other Tabs
WHERE Tab shows Tables Indexes
The Answer Set Tab shows the Results
The Answer Set Tab shows the Results
The Answer Set Tab shows the Results
The Answer Set Tab shows the Results
The Metadata Tab shows Metadata
Nexus Makes a View look like a Table
Nexus Joins Views to other Views in seconds
Nexus can Cube a View and Join to all other related Views
Nexus Cubes Views in Seconds
The Cube SQL created on Views is done Automagically
Views with the Underlying Indexes of the Base Tables
WHERE Tab shows Views Underlying Base Table Indexes
After an Answer Set Returns, you can do many things
After an Answer Set Returns, Perform OLAP Calculations
After an Answer Set Returns, you can Graph and Chart
Custom Joins With Nexus
Users Who Want to Load the Model
Users Who Want to Load the Model (Continued)
How Custom Joins Will Look in the Super Join Builder
Loading an ERwin Mode
Loading an ERwin Model (Continued)
Attaching The ERwin Model
Attaching The ERwin Model (Continued)
Managing The ERwin Model (Continued)
Saving an Answer Set in another Format
Sandbox – How to Create a Sandbox (1 of 5)
Sandbox - Join Answer Sets from different Systems (2 of 5)
Sandbox - Join Answer Sets from different Systems (3 of 5)
Sandbox - Join Answer Sets from different Systems (4 of 5)
Sandbox - Join Answer Sets from different Systems (5 of 5)
Convert Netezza DDL to Another Database Vendor
Replicate Data from One Netezza System to Another
