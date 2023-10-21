# WELCOME TO THE POSTGRES-WAREHOUSE

**Welcome to our PostgreSQL Learning Repository!** - *Unlock the full potential of PostgreSQL with our comprehensive learning repository, offering in-depth guides and examples. Dive deep into the world of PostgreSQL, from basic concepts to advanced techniques, ensuring you're well-equipped to handle real-world database challenges.*

Ways to connect the databasee using the client 
- GUI client -> DataGrip (licensed), pgAdmin (free)
- Terminal/CMD
- application over the db server 

## Connect the database using psql shell 
when we open our shell these are the details tto fill up (can keep them default) -> 
```shell
Server [localhost]:
Database [postgres]:
Port [5432]:
Username [postgres]:
Password for user postgres:
```

if connecting to an unknown/ghost server -> 
```shell 
Server [localhost]:
Database [postgres]: test
Port [5432]:
Username [postgres]:
Password for user postgres:
psql: error: connection to server at "localhost" (::1), port 5432 failed: FATAL:  database "test" does not exist
Press any key to continue . . .
```

## Creating a database

few commands -> 
```shell 
postgres=# help
You are using psql, the command-line interface to PostgreSQL.
Type:  \copyright for distribution terms
       \h for help with SQL commands
       \? for help with psql commands
       \g or terminate with semicolon to execute query
       \q to quit
```

getting list of all databases -> 
```shell 
postgres=# \l
                                                              List of databases
   Name    |  Owner   | Encoding | Locale Provider |      Collate       |       Ctype        | ICU Locale | ICU Rules |   Access privileges
-----------+----------+----------+-----------------+--------------------+--------------------+------------+-----------+-----------------------
 postgres  | postgres | UTF8     | libc            | English_India.1252 | English_India.1252 |            |           |  template0 | postgres | UTF8     | libc            | English_India.1252 | English_India.1252 |            |           | =c/postgres          +
           |          |          |                 |                    |                    |            |           | postgres=CTc/postgres
 template1 | postgres | UTF8     | libc            | English_India.1252 | English_India.1252 |            |           | =c/postgres          +
           |          |          |                 |                    |                    |            |           | postgres=CTc/postgres
(3 rows)
```

creating a database -> 
```shell 
postgres=# CREATE DATABASE test;
```

for working over cmd in postgreSQL -> 
```
cd \Program Files\PostgreSQL\16\bin
```

```bash
C:\Program Files\PostgreSQL\16\bin>psql -h localhost -p 5432 -U postgres test
Password for user postgres:
psql (16.0)
WARNING: Console code page (437) differs from Windows code page (1252)
         8-bit characters might not work correctly. See psql reference
         page "Notes for Windows users" for details.
Type "help" for help.
```

**OR** 

```bash
C:\Program Files\PostgreSQL\16\bin>psql -U postgres
Password for user postgres:
psql (16.0)
WARNING: Console code page (437) differs from Windows code page (1252)
         8-bit characters might not work correctly. See psql reference
         page "Notes for Windows users" for details.
Type "help" for help.

postgres=# \c test
You are now connected to database "test" as user "postgres".
test=#
```

remove a database (*caution*) -> 
```shell 
postgres=# DROP DATABASE test
```

## Creating tables

creating a table -> 
should have a format as below - 
```
CREATE TABLE table_name (
    Column name + data type + constraints (if any)
)
```
and implementing it is as -> 
```shell 
test=# CREATE TABLE person (
test(# id INT,
test(# first_name VARCHAR(50),
test(# last_name VARCHAR(50),
test(# gender VARCHAR(7),
test(# date_of_birth DATE
test(# );
CREATE TABLE
test=# \d
         List of relations
 Schema |  Name  | Type  |  Owner
--------+--------+-------+----------
 public | person | table | postgres
(1 row)
```
getting insides over the person table -> 
```shell 
test=# \d person
                         Table "public.person"
    Column     |         Type          | Collation | Nullable | Default
---------------+-----------------------+-----------+----------+---------
 id            | integer               |           |          |
 first_name    | character varying(50) |           |          |
 last_name     | character varying(50) |           |          |
 gender        | character varying(7)  |           |          |
 date_of_birth | date                  |           |          |
```

Now implementing up the constraints over the table could be done as -> 

```shell 
 CREATE TABLE person (
    id INT NOT NULL PRIMARY KEY,
    first_name VARCHAR(50) NOT NULL ,
    last_name VARCHAR(50) NOT NULL ,
    gender VARCHAR(7) NOT NULL ,
    date_of_birth DATE NOT NULL 
    );
```

## Types of Data Types -
|Name|Aliases|Description|
|---|---|---|
|`bigint`|`int8`|signed eight-byte integer|
|`bigserial`|`serial8`|autoincrementing eight-byte integer|
|``bit [ (_`n`_) ]``||fixed-length bit string|
|``bit varying [ (_`n`_) ]``|``varbit [ (_`n`_) ]``|variable-length bit string|
|`boolean`|`bool`|logical Boolean (true/false)|
|`box`||rectangular box on a plane|
|`bytea`||binary data (“byte array”)|
|``character [ (_`n`_) ]``|``char [ (_`n`_) ]``|fixed-length character string|
|``character varying [ (_`n`_) ]``|``varchar [ (_`n`_) ]``|variable-length character string|
|`cidr`||IPv4 or IPv6 network address|
|`circle`||circle on a plane|
|`date`||calendar date (year, month, day)|
|`double precision`|`float8`|double precision floating-point number (8 bytes)|
|`inet`||IPv4 or IPv6 host address|
|`integer`|`int`, `int4`|signed four-byte integer|
|``interval [ _`fields`_ ] [ (_`p`_) ]``||time span|
|`json`||textual JSON data|
|`jsonb`||binary JSON data, decomposed|
|`line`||infinite line on a plane|
|`lseg`||line segment on a plane|
|`macaddr`||MAC (Media Access Control) address|
|`macaddr8`||MAC (Media Access Control) address (EUI-64 format)|
|`money`||currency amount|
|``numeric [ (_`p`_, _`s`_) ]``|``decimal [ (_`p`_, _`s`_) ]``|exact numeric of selectable precision|
|`path`||geometric path on a plane|
|`pg_lsn`||PostgreSQL Log Sequence Number|
|`pg_snapshot`||user-level transaction ID snapshot|
|`point`||geometric point on a plane|
|`polygon`||closed geometric path on a plane|
|`real`|`float4`|single precision floating-point number (4 bytes)|
|`smallint`|`int2`|signed two-byte integer|
|`smallserial`|`serial2`|autoincrementing two-byte integer|
|`serial`|`serial4`|autoincrementing four-byte integer|
|`text`||variable-length character string|
|``time [ (_`p`_) ] [ without time zone ]``||time of day (no time zone)|
|``time [ (_`p`_) ] with time zone``|`timetz`|time of day, including time zone|
|``timestamp [ (_`p`_) ] [ without time zone ]``||date and time (no time zone)|
|``timestamp [ (_`p`_) ] with time zone``|`timestamptz`|date and time, including time zone|
|`tsquery`||text search query|
|`tsvector`||text search document|
|`txid_snapshot`||user-level transaction ID snapshot (deprecated; see `pg_snapshot`)|
|`uuid`||universally unique identifier|
|`xml`||XML data|



<!-- creating a database -> 
```shell 
postgres=# CREATE DATABASE test;
```
creating a database -> 
```shell 
postgres=# CREATE DATABASE test;
```
creating a database -> 
```shell 
postgres=# CREATE DATABASE test;
```
creating a database -> 
```shell 
postgres=# CREATE DATABASE test;
``` -->
