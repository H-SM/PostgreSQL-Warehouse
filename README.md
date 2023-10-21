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
    id BIGSERIAL NOT NULL PRIMARY KEY,
    first_name VARCHAR(50) NOT NULL,
    last_name VARCHAR(50) NOT NULL,
    gender VARCHAR(7) NOT NULL,
    date_of_birth DATE NOT NULL,
    email VARCHAR(150)
);
test=# \d person
                                       Table "public.person"
    Column     |          Type          | Collation | Nullable |              Default
---------------+------------------------+-----------+----------+------------------------------------
 id            | bigint                 |           | not null | nextval('person_id_seq'::regclass)
 first_name    | character varying(50)  |           | not null |
 last_name     | character varying(50)  |           | not null |
 gender        | character varying(7)   |           | not null |
 date_of_birth | date                   |           | not null |
 email         | character varying(150) |           |          |
Indexes:
    "person_pkey" PRIMARY KEY, btree (id)


test=# \d
              List of relations
 Schema |     Name      |   Type   |  Owner
--------+---------------+----------+----------
 public | person        | table    | postgres
 public | person_id_seq | sequence | postgres
(2 rows)
```
we have `person_id_seq` due to the `BIGSERIAL` for its property for `autoincrementing eight-byte integer`. 

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

## Insert into records

```shell 
INSERT INTO person ( 
    first_name, 
    last_name, 
    gender, 
    date_of_birth
) VALUES ('Anne', 'Smith', 'FEMALE', DATE '1988-01-09'); 
# yyyy-mm-dd

INSERT INTO person ( 
    first_name, 
    last_name, 
    gender, 
    date_of_birth,
    email
) VALUES ('Harman', 'Singh', 'MALE', DATE '2023-01-01','smthinghere@gmail.com'); 
```

we get the resulttant as -> 
```bash
test=# SELECT * FROM person;
 id | first_name | last_name | gender | date_of_birth |         email
----+------------+-----------+--------+---------------+-----------------------
  1 | Anne       | Smith     | FEMALE | 1988-01-09    |
  2 | Harman     | Singh     | MALE   | 2023-01-01    | smthinghere@gmail.com
(2 rows)
```

we could use website as [mockaroo](https://www.mockaroo.com) to generate random datas to test on as we generatted our `person.sql` file -> 

![Alt text](https://i.imgur.com/6w4pA8Q.png)


getting the file in the database -> 
```
test=# \i e:/Project/PostgreSQL-Warehouse/person(1).sql
CREATE TABLE
INSERT 0 1
INSERT 0 1
INSERT 0 1
INSERT 0 1
INSERT 0 1
```

## SELECT 
select specific characteristicvs from the table -> 
```shell 
test=# SELECT  FROM person;
--
(1000 rows)


test=# SELECT first_name,last_name FROM person;
 first_name  |     last_name
-------------+--------------------
 Ab          | Bowater
 Mirella     | Hane
 Rosanne     | Frantsev
 Loleta      | Pattesall
 Thurston    | Weatherby
 Dev         | De Stoop
 Ambrosi     | Pepper
 Wynnie      | Bourne
 Penny       | Hutt
 Tanitansy   | Gann
 Reba        | Borsnall
 Frank       | Staniland
 Tamiko      | Minty
 Arman       | Gartenfeld
 Giselle     | Titchmarsh
 Raddy       | Parkhouse
 Amandie     | Yurinov
 Neil        | Ritchings
 Wyndham     | Bugge
 Bink        | D'eye
```
sand getting out a nullable value is like -> 
```shell 
test=# SELECT email FROM person;
                email
-------------------------------------
 abowater0@toplist.cz
 mhane1@wikipedia.org

 lpattesall3@vistaprint.com
 tweatherby4@senate.gov
 ddestoop5@last.fm
 apepper6@google.co.jp
 wbourne7@boston.com
 phutt8@twitter.com


 fstanilandb@nature.com
```

## ORDER BY 

### Using ascending [ASC] / descending [DESC]
```shell 
test=# SELECT * FROM person ORDER BY country_of_birth ASC;
  id  | first_name  |     last_name      |                email                |   gender    | date_of_birth |     country_of_birth

------+-------------+--------------------+-------------------------------------+-------------+---------------+--------------------------
  728 | Wandis      | Ilsley             |                                     | Female      | 2022-11-29    | Afghanistan
   27 | Saundra     | Habgood            | shabgoodq@noaa.gov                  | Male        | 2023-09-17    | Afghanistan
  306 | Barnebas    | Sheringham         | bsheringham8h@phpbb.com             | Male        | 2022-12-18    | Afghanistan
  723 | Federica    | Avieson            |                                     | Female      | 2023-06-14    | Afghanistan
   19 | Wyndham     | Bugge              |                                     | Male        | 2023-07-17    | Albania
  775 | Orion       | Covely             | ocovelyli@fc2.com                   | Polygender  | 2023-09-07    | Albania
  795 | Colin       | Paliser            | cpaliserm2@sourceforge.net          | Male        | 2023-01-13    | Albania
  378 | Gaspar      | Snoad              | gsnoadah@jiathis.com                | Male        | 2023-06-29    | Albania
  609 | Virgina     | Kelby              | vkelbygw@parallels.com              | Female      | 2023-10-11    | Algeria
  779 | Dillon      | Leale              |                                     | Male        | 2023-01-17    | Angola
  229 | Rhys        | Midghall           |                                     | Male        | 2023-09-27    | Angola
  429 | Idalina     | Oxnam              | ioxnambw@hao123.com                 | Agender     | 2022-12-20    | Argentina
  305 | Wendi       | Tanguy             | wtanguy8g@qq.com                    | Female      | 2022-12-29    | Argentina
   75 | Mirilla     | Zanini             |                                     | Female      | 2023-01-28    | Argentina
```

### using multiple columns 
```shell 
test=# SELECT * FROM person ORDER BY id, email;
  id  | first_name  |     last_name      |                email                |   gender    | date_of_birth |     country_of_birth

------+-------------+--------------------+-------------------------------------+-------------+---------------+--------------------------
    1 | Ab          | Bowater            | abowater0@toplist.cz                | Male        | 2022-11-04    | Ukraine
    2 | Mirella     | Hane               | mhane1@wikipedia.org                | Female      | 2023-07-12    | Thailand
    3 | Rosanne     | Frantsev           |                                     | Female      | 2022-11-18    | Zambia
    4 | Loleta      | Pattesall          | lpattesall3@vistaprint.com          | Female      | 2023-03-27    | Sweden
    5 | Thurston    | Weatherby          | tweatherby4@senate.gov              | Male        | 2023-08-03    | Egypt
    6 | Dev         | De Stoop           | ddestoop5@last.fm                   | Polygender  | 2023-04-10    | China
    7 | Ambrosi     | Pepper             | apepper6@google.co.jp               | Male        | 2022-12-10    | South Korea
    8 | Wynnie      | Bourne             | wbourne7@boston.com                 | Female      | 2023-03-10    | China
    9 | Penny       | Hutt               | phutt8@twitter.com                  | Male        | 2023-05-18    | Peru
   10 | Tanitansy   | Gann               |                                     | Female      | 2023-08-27    | Japan
   11 | Reba        | Borsnall           |                                     | Female      | 2022-11-27    | China
   12 | Frank       | Staniland          | fstanilandb@nature.com              | Male        | 2022-12-27    | Czech Republic
   13 | Tamiko      | Minty              |                                     | Female      | 2022-11-19    | Jordan
   14 | Arman       | Gartenfeld         | agartenfeldd@arstechnica.com        | Male        | 2023-04-09    | France
   15 | Giselle     | Titchmarsh         | gtitchmarshe@netscape.com           | Female      | 2023-07-03    | Indonesia
   16 | Raddy       | Parkhouse          | rparkhousef@illinois.edu            | Male        | 2022-11-30    | Czech Republic
```

### using DOB (ASC/DESC)

```shell
test=# SELECT * FROM person ORDER BY date_of_birth;
  id  | first_name  |     last_name      |                email                |   gender    | date_of_birth |     country_of_birth

------+-------------+--------------------+-------------------------------------+-------------+---------------+--------------------------
   52 | Alick       | Bownde             | abownde1f@sciencedaily.com          | Male        | 2022-10-22    | France
  393 | Vale        | Colegrove          |                                     | Bigender    | 2022-10-22    | Germany
  682 | Aldous      | Pattie             | apattieix@vimeo.com                 | Male        | 2022-10-23    | Russia
  961 | Debera      | Dougary            | ddougaryqo@intel.com                | Female      | 2022-10-23    | Greece
  193 | Josefa      | Stilldale          |                                     | Female      | 2022-10-24    | Ukraine
  182 | Jean        | Lindley            | jlindley51@live.com                 | Female      | 2022-10-26    | Russia
  825 | Nannette    | Shyre              |                                     | Female      | 2022-10-27    | China
  465 | Tedman      | Loveguard          |                                     | Male        | 2022-10-27    | Philippines
  798 | Ermina      | Rowlatt            |                                     | Female      | 2022-10-27    | China
  851 | Shelden     | Blackly            | sblacklynm@springer.com             | Male        | 2022-10-28    | China
  991 | Jessica     | Christoffersen     |                                     | Female      | 2022-10-29    | Brazil
   61 | Camala      | Minger             | cminger1o@cbc.ca                    | Female      | 2022-10-30    | Slovenia
```



<!-- creating a database -> 

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
