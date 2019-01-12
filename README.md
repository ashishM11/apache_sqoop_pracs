# apache_sqoop_pracs


* Sqoop is a hadoop component build on top of hdfs.

* It is an open source tool that allow user to extract data from a structured data store into hadoop for further processing.

* Structured Data Store ⇔ RDBMS
  * MySQL
  * Oracle
  * Sql Server
  * PostgreSQL
  
#	sqoop list-databases

	sqoop list-databases \
	--connect "jdbc:mysql://localhost" \
	--username root -P

#	sqoop list-tables

	sqoop list-tables \
	--connect "jdbc:mysql://localhost/classicmodels" \
	--username root -P



#	Sqoop eval using -e {execute} & --query performing {CRUD}
* sqoop using --query

		sqoop eval \
		--connect "jdbc:mysql://localhost/classicmodels" \
		--username root -P \
		--query "insert into employees values(1101,'Ashish','tiwari','abc@gmail.com',1,1002,'S/W Developer')"

		sqoop eval \
		--connect "jdbc:mysql://localhost/classicmodels" \
		--username root -P \
		-e "select * from employees where employeeNumber=1101"

		sqoop eval \
		--connect "jdbc:mysql://localhost/classicmodels" \
		--username root -P \
		--query "update employees set email='ashish.ace11@gmail.com' where employeeNumber=1101"

		sqoop eval \
		--connect "jdbc:mysql://localhost/classicmodels" \
		--username root -P \
		-e "delete from employees where employeeNumber=1101"

#	Sqoop import:
 * 4.1 => Sqoop Selecting the Data to Import:

		sqoop import \
		--connect jdbc:mysql://localhost/classicmodels \
		--username root -P \
		--table employees -m 2 \
		--columns "employeeNumber,lastName,firstName,email,jobTitle" \
		--where "jobTitle='Sales Rep'" \
		--fields-terminated-by "|" \
		--as-textfile \
		--target-dir /sqoop/nye/classicmodels/empl-select

	*	4.2 => Sqoop Free-form Query Imports:

  *	Note: The facility of using free-form query in the current version of Sqoop 1.x is limited to simple queries where there are no ambiguous projections and no OR conditions in the WHERE clause.
		* Use of complex queries such as queries that have sub-queries or joins leading to ambiguous projections can lead to unexpected results.

		sqoop import \
		--connect jdbc:mysql://localhost/classicmodels \
		--username root -P \
		--query "select * from employees where jobTitle='Sales Rep' AND \$CONDITIONS" -m 3 \
		--split-by employees.employeeNumber \
		--target-dir /sqoop/nye/classicmodels/empl-freeform2 \
		--as-avrodatafile

*	Note: --split-by is used to distribute the values from table across the mappers uniformly i.e. say u have 100 unique records(primary key) and if there are 4 mappers, --split-by (primary key column) will help to distribute you data-set evenly among the mappers.$CONDITIONS is used by Sqoop process, it will replace with a unique condition expression internally to get the data-set. If you run a parallel import, the map tasks will execute your query with different values substituted in for $CONDITIONS. e.g., one mapper may execute "select bla from foo WHERE (id >=0 AND id < 10000)", and the next mapper may execute "select bla from foo WHERE (id >= 10000 AND id < 20000)" and so on.

# sqoop lists commands:

	  sqoop list-databases \
	  --connect jdbc:mysql://localhost \
	  --username root -password cloudera

	  sqoop list-tables \
	  --connect jdbc:mysql://localhost/retail_db \
	  --username root -password cloudera

# sqoop Eval Commands:

* (-e,--query <statement> 	|Execute statement in SQL). 

	    sqoop-eval \
	    --connect jdbc:mysql://localhost/retail_db \
	    --username root -password cloudera \
	    --query "select * from categories limit 5"

	    sqoop-eval \
	    --connect jdbc:mysql://localhost/retail_db \
	    --username root -password cloudera \
	    --e "select category_id as 'catID',category_name as 'catName' from categories limit 5"

# Sqoop job Commands:

 * How to create sqoop job

	   sqoop job --create listsDB -- list-databases \
	   --connect "jdbc:mysql://localhost" \
	   --username retail_dba \
	   -P

	   sqoop job --create listsTabs -- list-tables \
	   --connect "jdbc:mysql://localhost/retail" \
	   --username retail_dba \
	   -P

 * How to run Sqoop Job	

 	 sqoop job --exec [Job-Name]

* List all the Sqoop Jobs

  	sqoop job --list

* how to check SQOOP parameters in Jobs

  	sqoop job --show [Job-Name]
  
* How to delete SQOOP Jobs

  	sqoop job --delete [Job-Name]
