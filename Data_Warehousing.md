# Data Warehousing
Last Updated on : 24 May 2019

### Data Warehouse                                                                                                                                                                                                        
                subject oriented                                                                                                                                            
                time-variant                                                                                                                                                                                                                      
                integrated                                                                                                                                                       
                nonvolatile                                                                                                                                                      
                designed to support strategic decision making.
### Operational Database
				multiple diverse sources
                application oriented
				real-time, current
				updateable
				designed primarily to support day to day operations.

## Terminology used to describe various parts of a Data Warehouse:
1. Dimensions - A dimension table typically has two types of columns, primary keys which refers fact tables and textual\descriptive data.
2. Fact table - is a collection of multiple keys and measures, joined with one or more dimension tables.
3. Data Staging Area - is where you hold/dump the data in temporary tables, to perform data cleansing and merging, before loading the data to the data warehouse.
4. Hierarchy  - defines parent-child relationships among various levels within a single dimension.
5. Drill-down vs Roll-up.
6. Aggregation / Summarization / Consolidation
7. Granularity - describes the level below which no supporting details are stored.
* http://dwhlaureate.blogspot.com/2012/07/

## Extraction Procedures
Online Extraction  
- data is extracted directly from the Source for processing in the staging area
Offline Extraction 
- data is taken from another external area(Flat files/some dump) which keeps the copy of source.

## Flat file 
- Flat files are data stored in the form of columns and rows(ASCII format) on our file system to emulate a DBMS table.
* Datawarehouse E books: http://dwhlaureate.blogspot.com/2013/01/datawarehouse-e-books-getting-started.html

## OLTP - Online Transaction Processing.
                It uses normalized tables to quickly record large amounts of transactions.
                OLTP database are designed for recording the daily operations and transactions of a business.
                To make sure Application keeps running with high performance as possible
## OLAP - Online Analytical Processing.
                It uses fact and dimension tables to view/querying large amounts of data to enable multidimensional view & analysis.
                OLAP could provide management with fast answers to complex queries or enable them to analyze their company's historical data for trends and patterns.
                designed to sustain updates/deletes

## Architecture
Data Warehouse
                -> Data Marts
                                - Fact Tables
                                - Dimension Tables
                                - Master Tables

### Data Marts
Data Marts are subsets of a data warehouse meant to meet with particular demands of a specific business unit/group of users.
- Data mart and data warehousing are tools to assist management to come up with relevant information about the organization at any point of time
- While data marts are limited for use of a department only, data warehousing applies to an entire organization
- Data marts are easy to design and use while data warehousing is complex and difficult to manage
- Data warehousing is more useful as it can come up with information from any department

## Types of Dimensions
- Slowly Changing Dimensions : a dimension table that would undergo changes over time, to preserve the history of changes depending on business requirement.
- Rapidly Changing Dimensions : A dimension attribute that changes frequently, Example - patientID,Height,Weight.
- Shrunken Dimensions/Junk Dimensions: is a single table with a combination of different and unrelated attributes to avoid having a large number of foreign keys.
- Conformed Dimensions : to be used with multiple fact tables in a single database, or across multiple data marts or data warehouses.
- Degenerate Dimensions : is when the dimension attribute is stored as part of fact table, and does not have its own separate dimension table
- Role Playing Dimensions: is where the same dimension key, along with its associated attributes, can be joined to more than one foreign key in the fact table.
- Static Dimensions : are not extracted from the original data source, can be generated or loaded manually.
- Inferred Dimensions / Late arriving dimensions / Early-arriving facts : While loading fact records, a dimension record may not yet be ready. One solution is to generate a surrogate key with null for all the other attributes. Send rejected records to source team for further investigation.

## Types of Facts
- Additive facts are facts that can be summed up through all of the dimensions in the fact table.
                Eg: Sales fact
- Semi-additive facts are facts that can be summed up for some of the dimensions in the fact table, but not the others.
                Eg: Daily balances fact can be summed up through the customers dimension but not through the time dimension.
- Non-additive facts are facts that cannot be summed up for any of the dimensions present in the fact table.
                Eg: Facts which have percentages, Ratios calculated.
- Snapshot - describes the state of things in a particular instance of time.
                Eg: Daily balances fact can be summed up through the customers dimension but not through the time dimension.
- Cumulative - describes what has happened over a period of time.
                Eg: Total sales by product by store by day.
- Factless Fact Table - A factless fact table captures the many-to-many relationships between dimensions, but contains no numeric or textual facts.
                                                                                                It contains data only at information level but is not included in the calculations.
                Eg: A fact table which has only product key and date key to get the number products sold over a period of time.
                                Tracking student attendance or registration events
                                Tracking insurance-related accident events
                                Identifying building, facility, and equipment schedules for a hospital or university
* http://dwhlaureate.blogspot.com/2012/08/factless-fact-table.html

## Surrogate key / Surrogate Primary key / Artificial Identity key
Surrogate key is a unique identifier/number for each row that can be used in place of primary key to the table.
It is useful because the natural primary key (i.e. Customer Number in Customer table) can change and this makes updates more difficult.
Some tables have columns such as AIRPORT_NAME or CITY_NAME which are stated as the primary keys (according to the business users) but ,not only can these change, indexing on a numerical value is probably better and you could consider creating a surrogate key called, say, AIRPORT_ID. This would be internal to the system and as far as the client is concerned you may display only the AIRPORT_NAME.

## Business Key – This the key column which is used to compare Facts and dimension table or on which look up is performed.

## Types of SCD.
SCD Type 1 – There is no need to store historical data in the dimension table.
                                                 It overwrites the old dimension value with new one in database.
                                                 It is basically used to correct data in dimension table.
                                                 To delete some characters, to correct spelling.
SCD Type 2 – Dimension table historyis maintained in the database in the format of FLAG, EffectiveDate/Versions.
                                                The update is esentially treated as a new record with an entry in flag/date column.
SCD Type 3 – Only current and previous values of particular column kept in database for the dimension table.
                                                Should be used only when changes will only occur for finite number of time & does not increase the size of the table when new information is updated.
SCD Type 4 – A separate table is used to store the history or to track the change in any attributes of the dimension table.

## Data Warehouse Models to organize data using relational databases
Star Schema - has a single fact table connected to multiple dimension tables, with only one join establishing relationship with each of them, like a star.
Snowflake Schema - extension of the star schema where large dimension tables are normalized into multiple tables.
- Star Schema
                Highly denormalized & Data access latency is less in star schema.
                Schema querying is far simple.
                Huge Size but Better Performance than that of snowflake schema.
- Snowflake Schema
                Memory utilization is better, Snowflake schemas will use less space to store data in dimension tables due to denormalization.
                Aggregation/SCD/Junk Dimensions are prepared which compresses data significantly to be used in reports.
- Hybrid Schema
                Combines star schema structures and snowflake schemas, As additional data marts are added to a data warehouse, the schema of the entire data warehouse hosts a combination of the star and snowflake schema to store the data efficiently, for each data mart. Dimension tables may also be shared across different data marts to provide data consistency

## Ways to load incremental data in ETL :
1: Delta loading using parameter file.
2: Delta loading using control table.
3: Delta loading using max variable where max variable is last updated date.

# Things to Remember:
- Development Phase in Data Warehouse Project Life Cycle :
                ETL development: ETL developers will prepare a data-model with all dimensions and facts from heterogeneous data sources.
                Reporting development: Once the DWH is built,the reporters will configure the repository and generate the reports as per the client's requirement.
* http://dwhlaureate.blogspot.com/2013/01/data-warehouse-project-phases-in-life.html
- In the typical case for a data warehouse, we first check if we have new values for dimension tables, insert them first and then load the fact tables.
- In most cases, the fact tables will be the ones that take most of the space. They’ll probably also grow much faster than dimension tables.

## Links
http://www.jamesserra.com/archive/2013/07/why-you-need-a-data-warehouse/
https://blogs.technet.microsoft.com/dataplatforminsider/2015/05/05/top-reasons-why-enterprises-should-choose-azure-sql-data-warehouse/
https://www.targit.com/en/solutions/by-data-source/sql-server
http://it.toolbox.com/blogs/oracle-guide/designing-the-data-mart-part-1-31694
http://www.1keydata.com/datawarehousing/datawarehouse.html
https://anuradhasrinivas.files.wordpress.com/2013/03/data-warehousing-fundamentals-by-paulraj-ponniah.pdf
http://siddhumehta.blogspot.in/2009/04/delta-detection-techniques-in-etl.html
https://dwbi.org/etl/etl/53-methods-of-incremental-loading-in-data-warehouse
https://www.mssqltips.com/sqlservertip/1442/handle-slowly-changing-dimensions-in-sql-server-integration-services/
http://dwgeek.com/scd-type-2-sql.html/
https://www.sciencedirect.com/science/article/pii/S131915781100019X

Course Topics and corresponding URLs are mentioned in the following. Please go through all the links to understand the topics completely.
1.            Introduction
-             http://www.databaseanswers.org/downloads/data_modeling_by_example_vol_1.pdf
-             https://dwbi.org/data-modelling/dw-design
-             http://www.datanamic.com/support/lt-dez005-introduction-db-modeling.html
-             https://en.wikibooks.org/wiki/Introduction_to_Software_Engineering/Architecture/Design_Patterns
2.            Multi-dimensional analysis
-             http://www.learn.geekinterview.com/data-warehouse/data-analysis/multi-dimensional-analysis.html
-             http://www8.cs.umu.se/education/examina/Rapporter/PerWesterlund.pdf
-             https://www.youtube.com/watch?v=yoE6bgJv08E
3.            BI Architecture
-             ftp://ftp.software.ibm.com/software/dw/dm/db2/dm-0505cullen/INT1ARCHINTRO.pdf
4.            Best Practices and Guidelines for Optimal BI and OLAP Design
-             https://evisions.com/best-practices-for-designing-olap-cubes/
-             https://www.google.co.in/url?sa=t&rct=j&q=&esrc=s&source=web&cd=6&cad=rja&uact=8&ved=0ahUKEwibjbiW7sbTAhWGN48KHSt_CxoQFghLMAU&url=https%3A%2F%2Fwww.ibm.com%2Fdeveloperworks%2Fcommunity%2Fwikis%2Fform%2Fanonymous%2Fapi%2Fwiki%2F0fc2f498-7b3e-4285-8881-2b6c0490ceb9%2Fpage%2F2d6faf27-ee09-455f-b88f-9ac9b4a9c212%2Fattachment%2F23ca37f4-53db-4e2e-b940-bafa2f3476a2%2Fmedia%2FDB2BP_Warehouse_Design_0912.pdf&usg=AFQjCNEDmwkf8sfvz6XzrWlR58AK1CK2IQ
-             http://www.cioinsight.com/it-strategy/big-data/slideshows/11-best-practices-for-business-intelligence.html
-             http://www.diva-portal.org/smash/get/diva2:831050/FULLTEXT01.pdf
-             http://web.cs.wpi.edu/~cs561/s12/Lectures/IntegrationOLAP/OLAPandMining.pdf
-             http://research.ijcaonline.org/volume30/number3/pxc3875061.pdf
5.            Concepts for CDC
-             http://download.101com.com/tdwi/ww29/Attunity_Efficient_and_Real-Time_DI.pdf
6.            Data Load Techniques
-             http://www.inmoncif.com/registration/whitepapers/ttload-1.pdf
7.            Data Integration Architecture
-             http://cdn.ttgtmedia.com/searchDataManagement/downloads/DataIntegrationBluePrint.pdf
-             http://www.oracle.com/us/products/middleware/data-integration/goldengate11g-realtimedw-wp-168215.pdf
8.            Best Practices and Guidelines for Optimal ETL Design
-             http://www.developer.com/db/best-practices-etl-development-for-data-warehouse-projects.html
-             https://www.timmitchell.net/etl-best-practices/

## To search:
What are the processes we should follow to take developed code into production
RCP - no need for explicit col definition
docs in prj - src to tgt, technical design doc, unit test document,
what is the need for schedulers apart from inbuilt datastage scheduling ? : can establish dependency between 2 external servers
if ftp takes longer time - check file size of transferred file.
PIVOT stage
what kinds of error codes have u captured? captured using reject stage and processing them later
joins in a datastage is much faster than join in a database because of large memory capability.
scd 2 implementation in datastage ? change capture and change apply
Direct fix is when it can moved to production directly after testing it in dev
join - inner, left outer, right outer and full outer
merge/lookup - inner & left outer.
Explain to a business client on need of ETL
Data warehouse is a central location where data is collected from multiple different sources for analytical needs to provide different vizulizations
Granularity
Grain of a Sales table will be (SalesID+ProductID)
[Youtube Vids] (https://www.youtube.com/watch?v=9gOw3joU4a8)