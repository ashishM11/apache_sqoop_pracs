# apache_sqoop_pracs


* Sqoop is a hadoop component build on top of hdfs.

* It is an open source tool that allow user to extract data from a structured data store into hadoop for further processing.

* Structured Data Store ⇔ RDBMS
  * MySQL
  * Oracle
  * Sql Server
  * PostgreSQL

# sqoop lists commands:
---------------------------------------------------
sqoop list-databases \
--connect jdbc:mysql://localhost \
--username root -password cloudera
---------------------------------------------------
sqoop list-tables \
--connect jdbc:mysql://localhost/retail_db \
--username root -password cloudera
---------------------------------------------------

# sqoop Eval Commands: (-e,--query <statement> 	|Execute statement in SQL). 
------------------------------------------------------------------------------------------------------
sqoop-eval \
--connect jdbc:mysql://localhost/retail_db \
--username root -password cloudera \
--query "select * from categories limit 5"
------------------------------------------------------------------------------------------------------
sqoop-eval \
--connect jdbc:mysql://localhost/retail_db \
--username root -password cloudera \
--e "select category_id as 'catID',category_name as 'catName' from categories limit 5"
------------------------------------------------------------------------------------------------------
 
# Sqoop job Commands:
---------------------------------------------------
->>	1.How to create sqoop job
---------------------------------------------------
sqoop job --create listsDB -- list-databases \
--connect "jdbc:mysql://localhost" \
--username retail_dba \
-P

sqoop job --create listsTabs -- list-tables \
--connect "jdbc:mysql://localhost/retail" \
--username retail_dba \
-P
---------------------------------------------------
->>	2.How to run Sqoop Job	
---------------------------------------------------
sqoop job --exec <<Job-Name>>
---------------------------------------------------
->>	3.List all the Sqoop Jobs
---------------------------------------------------
sqoop job --list
---------------------------------------------------
->>	4.how to check SQOOP parameters in Jobs
---------------------------------------------------
sqoop job --show <<Job-Name>>
---------------------------------------------------
->>	5.How to delete SQOOP Jobs
---------------------------------------------------
sqoop job --delete <<Job-Name>>
