# DataStage 
Last Updated on : 24 May 2019

Three levels of Parallelism in Datastage :
- Pipeline Parallelism
- Data Parallelism
- Component Parallelism

## Stages : 
A stage is categorized into two types i.e. active or passive. A passive stage allows access to databases for the mining or scripting of data. Active stages defines the movement of data and offer mechanisms for merging collecting data, data streams, and transforming data from one data type to another type.

## Server Components 
DataStage is divided into three server components:
- Repository : A central store that contains all the information required to build a data mart or data warehouse.
- DataStage Server : Runs executable jobs, under the control of the DataStage Director, that extract, transform, and load data into a data warehouse.
- DataStage Package Installer : A user interface used to install packaged DataStage jobs and plug-ins.
DataStage is available to be installed in different variants:
                Server edition runs on a single server, and has no inherent parallelism. It generates code in a language called DataStage BASIC.
                Enterprise edition runs on an architecture that permits automatic parallelism in either an SMP or cluster (MPP) environment. It generates OSH (Orchestrate shell) scripts.
                Enterprise MVS edition is used for mainframe execution. DataStage generates COBOL and JCL which are transferred to the mainframe and executed there.

## Types of Parallel Processing:
Symmetric Processing (SMP)
                All the CPUs share the same memory.
                SMP systems create a bottleneck problem in which all CPUs attempt to access same memory at once.
Massively Parallel Processing (MPP)
                MPP architecture consists of multiple servers with each node/server is autonomous to process and store up data.
                High volumes of data in data warehouse systems are made into smaller and more manageable blocks, which are then distributed to multiple processors.
                In MPP systems, data is distributed/processed across many servers (nodes) in CPUs, each of which has its own memory and disk thus sharing the load.
                Although there is no disk-level sharing, all communication happen through network interconnect.
                MPPs are high in cost and complexity,
https://career.guru99.com/top-50-datastage-interview-questions/
https://mindmajix.com/datastage-interview-questions-answers
http://datastage4you.blogspot.com/2014/06/aptconfigfile-configuration-file.html?m=1
http://www.geekinterview.com/question_details/77921
http://mydatastage-notes.blogspot.com/2013/04/configuration-file.html?m=1

Parallel jobs has lots of stages and uses MPP where as server jobs do not.
Pipeline Parallelism - Consider three CPU connected in series. When data is being fed into the first one, it start processing, simultaneously is being transferred into the second CPU and so on. U can compare with 3 section of pipe. As water enters s the pipe it start moving into all the section of pipe.
Partition Pipeline Parallelism- consider 3 CPU connected in parallel and being fed with data at same time thus reduces the load and efficiency. You can compare a single big pipe having 3 inbuilt pipes. As water is being fed to them it consumes large quantity in less time.

## What is the difference between Symmetrically parallel processing, Massively parallel processing?
Symmetric Multiprocessing (SMP) - Some Hardware resources may be shared by processor. Processor communicates via shared memory and has single operating system.
Cluster or Massively Parallel Processing (MPP) - Known as shared nothing in which each processor have exclusive access to hardware resources. Cluster systems can be physically dispoersed. The processor have their own operations system and communicate via high speed network

## What is the difference between sequential file and a dataset? When to use the copy stage?
Sequential file stores small amount of the data with any extension .txt where as Dataset stores huge amount of the data and opens the file only with an extension .ds. Sequential Stage stores small amount of the data with any extension in order to access the file where as Dataset is used to store huge amount of the data and it opens only with an extension (.ds ) .The Copy stage copies a single input data set to a number of output datasets. Each record of the input data set is copied to every output data set. Records can be copied without modification or you can drop or change the order of columns. Main difference b/w sequential file and dataset is: Sequential stores small amount of data and stores normally. But dataset loads the data like ansi format.
http://evergreendatastage.blogspot.com/
Read cache size (MB): Specify the size of the read cache in Megabytes. The default is 128 MB.
Write cache size (MB): Specify the size of the write cache in Megabytes. The default is 128 MB.
Buffer size: Specifies the size of the buffer used by in-process or inter-process row buffering. Defaults to 128 Kilobytes.
https://datastage-vishnu.blogspot.com/

## CHANGE CAPTURE
It captures the change between two input data(Before/Existing Data link & After/Newly Arriving Data Link) by comparing them based on key column and is mentioned in output in the form of code in separate column as:
0 = If the data is Copied as it is from ‘Before’ Link to ‘After’ Link.
1 = If the data is newly Inserted in ‘After’ link
2 = If the data is Deleted from ‘Before’ link
3 = If the data is Edited in ‘After’ link from ‘Before’ link
*http://www.databaseetl.com/change-capture-stage-datastage/

## To get job names in a project
```
. `cat /.dshome`/dsenv
dsjob -ljobs ProjectName
-sh-4.2$ dsjob
```
Command Syntax:
        dsjob [-authfile <authfile> | -file <file> <domain> <DataStage server> |  [-domain <domain> | -url <url>] -user <user> -password <password> -server <DataStage server>] | -domain <domain> [-user <user>] -server <DataStage server>
                        <primary command> [<arguments>]

## Status code = -9999 DSJE_DSJOB_ERROR
Transaction Count [Default - 2000] in DB connector stage
Do not make 0, as table will be locked. It specifies after how many records, commit needs to happen.

## Iteration
@ITERATION – System Variable to set the while loop condition. It starts from 1 and increments by one as the loop progresses. Once while loop breaks, it gets back to 1.

## Surrogate Key
Surrogate key generator state file: is the source for surrogate key generator stage, the state file (essentially a flat file in the file system) persists data that contains the next value and properties for creation of keys between job runs utilising this source.
To create a new state file, start by creating a new parallel job and add a surrogate key generator stage, do not add any input or output links. On the Properties tab, define the Key Source properties:
                a. Set the Key Source Action property to Create.
                b. Source Name, browse to or type location and file name for this new state file..
                c. Select the source type as Flat File.
                d. Leave View State File as False

## UPSERTs
Update happens first and then inserts.
http://data-engineer.in/2014/04/27/slowing-changing-dimension-using-scd-stage-in-datastage/

## SCD - Slowly Changing Dimensions
3 ways to construct the scd2 in DataStage
- using SCD stage in processing stage
- using change capture and change apply stages
- using source file,lookup,transformers,filters,surrogate key generator or stored procedures,target tables
[Video Explanation] https://www.youtube.com/watch?v=WZiVrgF3uPU&t=3151s

## Why is hash file is faster than sequential file n odbc stage?
Hash file is indexed & works under hashing algorithm. That's why the search is faster in hash file.
Hash file is used to eliminate the duplicate rows based on hash key, and also used for lookups. The primary use of the hash file is to do a look up. When you generate a hash file, it indexes the key by an inbuilt hashing algorithm. So when a look up is made, it is much faster. These files are stored in the memory hence faster performance than from a sequential file.

## Routines:
Routine is user-defined functions that can be reusable with in the project. Routines are stored in the Routines branch of the DataStage Repository, where you can create, view or edit. The following are different types of routines:
1) Transform functions
2) Before-after job subroutines
3) Job Control routines

## DS Engine
There are three processes start when the DataStage engine starts:
1. DSRPC
2. DataStage Engine Resources
3. DataStage telnet Services

## What is Runtime Column Propagation and how to use it?
If your job has more columns which are not defined in metadata if runtime propagation is enabled it will propagate those extra columns to the rest of the job

## How to remove duplicates without using remove duplicate stage?
- In the target make the column as the key column and run the job.
- Using a sort stage, set property: ALLOW DUPLICATES: false
- Just do a hash partion of the input data and check the options Sort and Unique

## Pivot stage 
is used to make the horizontal rows into vertical and vice versa. Pivot stage supports only horizontal pivoting – columns into rows. Pivot stage doesn’t supports vertical pivoting – rows into columns

## Link Collector algorithims:
- Sort/Merge : Using the sort/merge method the stage reads multiple sorted inputs and writes one sorted output.
- Hash : How to improve the performance of hash file?
You can improve performance of hashed file by
1. Preloading hash file into memory -->this can be done by enabling preloading options in hash file output stage
2. Write caching options -->.It makes data written into cache before being flushed to disk. You can enable this to ensure that hash files are written in order onto cash before flushed to disk instead of order in which individual rows are written
3. Preallocating--> estimating the approx size of the hash file so that file needs not to be splitted to often after write operation

## Interview prep Links:
http://www.softwaretestinghelp.com/datastage-interview-questions/

## Links:
https://www.ibm.com/support/knowledgecenter/en/SSZJPZ_9.1.0/com.ibm.swg.im.iis.ds.nav.doc/topics/dshold_developing_parallel_ds_and_qs_jobs.html
https://intellipaat.com/tutorial/datastage-tutorial/datastage-parallel-stages-group-and-designing-jobs-in-datastage-palette/
http://etl-tools.info/infosphere-datastage-ee/tutorial/parallel-stages.htm
https://shreedhar.wikispaces.com/file/view/parjdev.pdf
http://www.geekinterview.com/Interview-Questions/Data-Warehouse/DataStage
http://datastage4you.blogspot.in/2014/02/datastage-coding-checklist.html
https://tekslate.com/different-types-of-partition-datastage/
http://talentain.com/resources/datastage-slowly-changing-dimensions
http://www.dsxchange.com/viewtopic.php?p=283362
http://datastage4you.blogspot.in/2012/12/dataset-in-datastage.html
http://mydatastage-notes.blogspot.in/2013/04/configuration-file.html
http://datastage4you.blogspot.in/2014/06/aptconfigfile-configuration-file.html
http://datastage4you.blogspot.in/2013/11/conductor-node-in-datastage.html
To Search for implementations:
stage valiables
how tranformer works
loop valiables
configuration files
all stages
diff join lookup and merge
join types
sparse lookup
remove duplicates using stage valiables
cumilative sum
max min salary
group by dept salary
(@INROWNUM-1)*NUMPARTITIONS+@PARTITIONNUM+1
To get Cumulative Sum
Ex:
                a 1
                a 2
                a 3
                b 1
                b 2
                c 1
Ans:
sv1=sv2
sv2=in_lnk
sv3= if sv1=sv2 then sv3+1 else 1
                                sv1=sv2
                                sv2=in_lnk
                                sv3=if sv1=sv2 then csum+sv2 else sv2
                                csum=if sv1<>sv2 then 0 else csum+sv2
                a 1
sv1=null
sv2=1
sv3=1
csum=1
                a 1
                a 2
sv1=1
sv2=2
sv3=3
csum=3
                a 1
                a 2
                a 3
sv1=2
sv2=3
sv3=6
csum=6
                a 1
                a 2
                a 3
                b 1
sv1=3
sv2=1
sv3=1
csum=0
                a 1
                a 2
                a 3
                b 1
                b 2
sv1=1
sv2=2
sv3=2
http://www.databaseetl.com/user-variable-stage-in-datastage/

## DataStage best practices:-
Select suitable configurations file (nodes depending on data volume)
Select buffer memory correctly and select proper partition
Turn off Run time Column propagation wherever it’s not required
Taking care about sorting of the data.
Handling null values (use modify instead of transformer)
Try to decrease the use of transformer. (Use copy, filter, modify)
Use data-set instead of sequential file in the middle of the vast jobs
Take maximum 20 stages for a job for best performance.
Select Join or Lookup or Merge (depending on data volume)
Stop propagation of unnecessary metadata between the stages.
Points we need to consider:
Transformer vs Merge, Copy, Aggregator etc   each stage in Datastage is optimized for specific purpose. Stages like Merge, Copy are light Weight compared with Transformer stage. Because transformer stage is compiled to C++ whereas other stages are compiled into datastage native OSH code and for every transformer function, we need to call all C++ function library from native OSH code. Therefore avoid using Transformation stage where other stages could be used
Staged the data coming from ODBC/OCI/DB2UDB stages or any database on the server using Hash/Sequential files for optimum performance
Tuned the OCI stage for ‘Array Size’ and ‘Rows per Transaction’ numerical values for faster inserts, updates and selects.
Tuned the ‘Project Tunable’ in Administrator for better performance
Use sorted data for Aggregator.
Sorted the data as much as possible in DB and reduced the use of DS-Sort for better performance of jobs.
Removed the data and columns not used from the source as early as possible in the job.
Worked with DB-admin to create appropriate Indexes on tables for better performance of DS queries.
Converted some of the complex joins/business in DS to Stored Procedures on DS for faster execution of the jobs.
If an input file has an excessive number of rows and can be split-up then use standard logic to run jobs in parallel.
Before writing a routine or a transform, make sure that there is not the functionality required in one of the standard routines supplied in the sdk or ds utilities categories. Constraints are generally CPU intensive and take a significant amount of time to process. This may be the case if the constraint calls routines or external macros but if it is inline code then the overhead will be minimal.
Try to have the constraints in the ‘Selection’ criteria of the jobs itself. This will eliminate the unnecessary records even getting in before joins are made.
Tuning should occur on a job-by-job basis.
Use the power of DBMS.
Try not to use a sort stage when you can use an ORDER BY clause in the database.
Using a constraint to filter a record set is much slower than performing a SELECT … WHERE….
Make every attempt to use the bulk loader for your particular database. Bulk loaders are generally faster than using ODBC or OLE.
Minimize the usage of Transformer (Instead of this use Copy modify Filter Row Generator
Use SQL Code while extracting the data
Handle the nulls using modify stage.
Minimize the warnings
Reduce the number of lookup in a job design.
Try not to use more than 20 stages in a job if expected data volume is too high.
Use IPC stage between two passive stages Reduces processing time
Drop indexes before data loading and recreate after loading data into tables
Check the write cache of Hash file. If the same hash file is used for Look up and as well as target disable this Option.
http://itexcerpts.blogspot.com/2016/04/ibm-infosphere-datastage-job-design.html
Understanding the datastage configuration file:
In Datastage, the degree of parallelism, resources being used, etc. are all determined during the run time based entirely on the configuration provided in the APT CONFIGURATION FILE. This is one of the biggest strengths of Datastage. For cases in which you have changed your processing configurations, or changed servers or platform, you will never have to worry about it affecting your jobs since  all the jobs depend on this configuration file for execution. Datastage jobs determine which node to run the process on, where to store the temporary data , where to store the dataset data, based on the entries provide in the configuration file. There is a default configuration file available whenever the server is installed.  You can typically find it under the <>\IBM\InformationServer\Server\Configurations  folder with the name default.apt. Bear in mind that you will have to optimise these configurations for your server based on your resources.
Basically the configuration file contains the different processing nodes and also specifies the disk space provided for each processing node. Now when we talk about processing nodes you have to remember that these can are logical processing nodes that are specified in the configuration file. So if you have more than one CPU this does not mean the nodes in your configuration file correspond to these CPUs. It is possible to have more than one logical node on a single physical node. However you should be wise in configuring the number of logical nodes on a single physical node. Increasing nodes, increases the degree of parallelism but it does not necessarily mean better performance because it results in more number of processes. If your underlying system should have the capability to handle these loads then you will be having a very inefficient configuration on your hands.
Now lets try our hand in interpreting a configuration file. Lets try the below sample.
```
{
                node “node1″
                {
                                fastname “SVR1″
                                pools “”
                                resource disk “C:/IBM/InformationServer/Server/Datasets/Node1″ {pools “”}
                                resource scratchdisk “C:/IBM/InformationServer/Server/Scratch/Node1″ {pools “}
                }
                node “node2″
                {
                                fastname “SVR1″
                                pools “”
                                resource disk “C:/IBM/InformationServer/Server/Datasets/Node1″ {pools “”}
                                resource scratchdisk “C:/IBM/InformationServer/Server/Scratch/Node1″ {pools “”}
                }
                node “node3″
                {
                                fastname “SVR2″
                                pools “” “sort”
                                resource disk “C:/IBM/InformationServer/Server/Datasets/Node1″ {pools “”}
                                resource scratchdisk “C:/IBM/InformationServer/Server/Scratch/Node1″ {pools  ”" }
                }
}
```
This is a 3 node configuration file. Lets go through the basic entries and what it represents.
Fastname – This refers to the node name on a fast network. From this we can imply that the nodes node1 and node2 are on the same physical node. However if we look at node3 we can see that it is on a different physical node (identified by SVR2). So basically in node1 and node2 , all the resources are shared. This means that the disk and scratch disk specified is actually shared between those two logical nodes. Node3 on the other hand has its own disk and scratch disk space.
Pools – Pools allow us to associate different processing nodes based on their functions and characteristics. So if you see an entry other  entry like “node0” or other reserved node pools like “sort”,”db2”,etc.. Then it means that this node is part of the specified pool.  A node will be by default associated to the default pool which is indicated by “”. Now if you look at node3 can see that this node is associated to the sort pool. This will ensure that that the sort stage will run only on nodes part of the sort pool.
Resource disk  - This will specify Specifies the location on your server where the processing node will write all the data set files. As you might know when Datastage creates a dataset, the file you see will not contain the actual data. The dataset file will actually point to the place where the actual data is stored. Now where the dataset data is stored is specified in this line.
Resource scratchdisk – The location of temporary files created during Datastage processes, like lookups and sorts will be specified here. If the node is part of the sort pool then the scratch disk can also be made part of the sort scratch disk pool. This will ensure that the temporary files created during sort are stored only in this location. If such a pool is not specified then Datastage determines if there are any scratch disk resources that belong to the default scratch disk pool on the nodes  that sort is specified to run on. If this is the case then this space will be used.
https://sites.google.com/site/jmdstips/system/app/pages/subPages?path=/datastage

## To count number of rows on the input link of a transformer stage.
DataStage Transformer stage provides a handy set of Last Row Handling functions. Steps:
                Add an output link lnk_Count to the Transformer stage. The number of rows will be sent to this link.
                Add an integer stage variable vNumRows. Set initial value to 0. Set derivation to: vNumRows + 1.
                Define a num_rows integer column in the lnk_Count output link. Set derivation to vNumRows
                Edit the constraint of the lnk_Count output link. Set to LastRow().
                Add a Peek stage to your DataStage job and connect it to the lnk_Count output link of the transformer

## To assign row numbers using a DataStage Parallel Transformer stage.
Step 1. Add a transformer stage to your data flow
Step 2. Define a ROW_NUMBER column to the transformer output
Step 3. Modify the ROW_NUMBER derivation. You need to enter the following expression as a derivation for the row number column:
(@INROWNUM - 1) * @NUMPARTITIONS + @PARTITIONNUM + 1
Discussion
The proposed solution uses three DataStage system variables:
                @INROWNUM – this DataStage system variable contains the row number within the partition. For each partition this variable starts from 1: 1, 2, 3, …
                @NUMPARTITIONS – this DataStage system variable contains the number of partitions (1, 2, 3, …) the stage is running on.
                @PARTITIONNUM – this DataStage system variable contains the number (0, 1, 2,…) of the partition that is processing the particular row.
                To understand the derivation expression better, let’s see an example. If our DataStage job runs in two partitions (@NUMPARTITIONS), than the transformer stage for the first partition (@PARTITIONNUM = 0) will assign following numbers:
Stages in Datastage: https://datastage4u.wordpress.com/category/datastage-stages/
Funnel Stage: Funnel stage is used to combine multiple input datasets into a single input dataset.This stage can have any number of input links and single output link.
                reference link    None
                input link                                             Multiple (Note:Metadata for all inputs must be identical)
                output link                          1
                reject link                                            1
                3 types: Continuous Funnel combines records as they arrive (i.e. no particular order);
                                                Sort Funnel combines the input records in the order defined by one or more key fields; (requires data must be sorted and partitioned by the same key columns)
                                                Sequence copies all records from the first input data set to the output data set, then all the records from the second input data set, etc.
Filter Stage: Filter stage is a processing stage used to filter database based on filter condition.(like a where clause). ex:Where CUSTOMER_NAME=’UMA’ AND CUSTOMER_ID=’1′
                reference link    none
                input link                                             1
                output link                          multiple
                reject link                                            1
Lookup Stage: does not require input/reference link to be sorted.
  Lookup stage is a in-memory processing stage. Large look up table will result in the job failure if DataStage engine server runs out of memory.
                reference link    Multiple
                input link                                             1
                output link                          1
                reject link                                            1
Supports only equivalents of Inner & Left Outer joins.
Lookup vs Join : prefer join when data is huge over lookup
Join Stage: key columns must be the same name, Four join options: inner join, left outer join, right outer join and full outer join
                reference link    none
                input link                                             multiple
                output link                          1
                reject link                                            1
Merge Stage: To minimise memory requirements, we can ensure that rows with the same key column values are located in the same partition and is processed in the same node by partitioning. However, the ‘auto’ option for partitioning usually works fine.
  As part of preprocessing, duplicate records need to be removed from the master. If there are more than one update data sets, it only updates the first record
                reference link    none
                input link                                             multiple (*same)
                output link                          multiple
                reject link                                            multiple (*same)
Supports only equivalents of Inner & Left Outer joins.
https://www.mydatahack.com/datastage-join-vs-lookup-vs-merge/
Normal Lookup :-
                Normal Lookup data needs to be in memory
                Normal might provide poor performance if the reference data is huge as it has to put all the data in memory.
                Normal Lookup can have more than one reference link.
                Normal lookup can be used with any database
Sparse Lookup :- If you open the Oracle_Enterprise/DB2 stage you can find it in "Look Up Type" drop-down.
                Sparse Lookup directly hits the database.
                If the input stream data is less and reference data is more like 1:100 or more in such cases sparse lookup is better. That is if you have input records 100 times less that the lookup data
                Sparse Lookup,we can only have one reference link.
                Sparse lookup,we can only use for Oracle and DB2.
                Sparse lookup sends individual sql statements for every incoming row.(Imagine if the reference data is  huge).
                This Lookup type option can be found in Oracle or DB2 stages.Default is Normal.

###  Trim Functions
Trim(Link.Col)
Trim(convert(char(10):char(13),'',EREPLACE(Link.Col,'|','')))
###  Loop with delim '||'
Loop while                          : @ITERATION+ Number +1 <= DCount(Link.Col,'||')
Loop variable     : Field(Link.Col,'||', @ITERATION+ Number) - lp_var_name
###  Replace Commands - EREPLACE(expression, string, replacement, occurrence)--occurrence(how many occurence to replace)
                REPLACE '|' WITH ''                                                                           : Ereplace(Trim(Link.Col),'|','')
                Convert Syntax                                                                                                   : convert(characters_to_replace, substitute_chars, Link.Col)
                Replacing new line(LF) and CR with space : convert(char(10):char(13),'',Trim(Link.Col,'|','A'))
                Replacing CR, LF and '|' with space            : convert(char(10):char(13),'',EREPLACE(Link.Col,'|',''))
###  NUll Handling Functions
                Replaces nulls with zero : NullToZero(Link.Col)
                IsNull,IsNotNull,NullToEmpty,NullToZero,NullToValue,setnull()
###  Field Picker Function - Field(%Link.Col%,%delimiter%,%occurrence%,[%number%])
                Field(Link.Col,' ',1)
                Field(Link.Col,'||',1)='3'
                Field(Link.Col,'/',7)
###  Date from String Conversion: DS default is yyyy-mm-dd hh:mm:ss
                2018-12-03 to Default                    : StringToDate(Link.Col,"%yyyy-%mm-%dd")
                03 12 2018 to Default                     : StringToDate(Link.Col,"%dd %mmm %yyyy")
                3-Dec-18 to Default                         : StringToDate(Link.Col,"%d-%mmm-%2000yy")
                09/01/2017 to Default                   : StringToDate(Link.Col,"%dd/%mm/%yyyy")
                Excel Seconds to Default               : TimestampToDate(TimestampFromSecondsSince2((Link.Col-25569)*86400,"1970-01-01 00:00:00"))
                FileDate format reader in Sequence Job : Change(Oconv(@DATE-1,"DYMD[4,2,2]"),"/","")
###  Timestamp from String Conversion
                Timestamp to Date                                                          : TimestampToDate(Link.Col)
                03 Dec 2018 12:53 to Default      : StringToTimestamp(Link.Col,"%dd %mmm %yyyy %(h,s):%(n,s)")
                2018-12-31 12:25:53 to Default  : StringToTimestamp(Link.Col,"%yyyy-%mm-%dd %hh:%nn:%ss")
                Dec-03-2018 12:25 to Default     : StringToTimestamp(Link.Col,"%mmm-%dd-%yyyy %hh:%nn")
                29-12-18 1:13 to Default                               : StringToTimestamp(Link,Col,"%dd-%mm-%2000yy %h:%nn")
                20090707123100 to Default                        : StringToTimestamp(Link.Col,"%yyyy%mm%dd%hh%nn%ss")
###  Time from String Conversions
                StringToTime(Link,Col,"%hh:%nn") --HH:MM
                StringToTime(Link,Col,"%h:%nn")
                StringToTime(Link,Col,"%(h,s):%(n,s)")
                If (Link,Col='24:00:00') Then StringToTime('00:00:00',"%h:%nn:%ss")  Else StringToTime(Link,Col,"%h:%nn")
###  Decimal from String
                StringToDecimal(Trim(Link,Col))
###  Dcount - Count number of delimited fields in a string.
                DCount(Link.Col,%delimiter%)
                DCount(“I_am_Data_stage_Developer”,”_”) = 5
###  Case Conversions
                DownCase(string)
                UpCase(string)
###  Len - Length of string
                Len("abcd")=4 


## To search
Import & Export Jobs
Number of Nodes in Dev & Prod
.DSX files are sent to admins to pass through the Versioning tool
RCP - When u have a straight push in source cols to target can be handled without modifying the jobs by enabling RCP
Need for third party schedulers: can be helpful for handling dependencies on multiple servers without harming the jobs
If file is not completely downloaded due to nw issues, we need to check changes in file sizes if it remains constant for few mins, then we can go ahead and download it.
Skip First and last record: http://datastagebox.com/sequential-file-stage-datastage.html
The First and last records can be skipped using sequential stage Filter property as follows.
Go to Sequential properties ? Filter
Paste the UNIX command sed ‘1d;$d’ in the filter property
*Don’t change any partitioning Setting
Can edit stage neames in dsx jobs by opening in xml mode by opening it as a text file.