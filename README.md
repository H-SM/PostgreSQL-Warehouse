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

## DISTINCT 

```shell
test=# SELECT country_of_birth FROM person ORDER BY country_of_birth;
     country_of_birth
--------------------------
 Afghanistan
 Afghanistan
 Afghanistan
 Afghanistan
 Albania
 Albania
 Albania
 Albania
 Algeria
 Angola
 Angola
 Argentina
 Argentina
 Argentina
 Argentina

test=# SELECT DISTINCT country_of_birth FROM person ORDER BY country_of_birth;
     country_of_birth
--------------------------
 Afghanistan
 Albania
 Algeria
 Angola
 Argentina
 Armenia
 Azerbaijan
 Bahamas
 Bangladesh
 Belarus
 Bhutan
 Bolivia
 Bosnia and Herzegovina
 Botswana
 Brazil
 Bulgaria
 Cameroon
 Canada
 Central African Republic
 Chad
 Chile
 China
 Colombia
```

## WHERE clause 

filter the data over our conditions.  as -> 
```shell
test=# SELECT * FROM person WHERE gender = 'Female';
  id  | first_name  |   last_name    |               email                | gender | date_of_birth |     country_of_birth
------+-------------+----------------+------------------------------------+--------+---------------+--------------------------
    2 | Mirella     | Hane           | mhane1@wikipedia.org               | Female | 2023-07-12    | Thailand
    3 | Rosanne     | Frantsev       |                                    | Female | 2022-11-18    | Zambia
    4 | Loleta      | Pattesall      | lpattesall3@vistaprint.com         | Female | 2023-03-27    | Sweden
    8 | Wynnie      | Bourne         | wbourne7@boston.com                | Female | 2023-03-10    | China
   10 | Tanitansy   | Gann           |                                    | Female | 2023-08-27    | Japan
   11 | Reba        | Borsnall       |                                    | Female | 2022-11-27    | China
   13 | Tamiko      | Minty          |                                    | Female | 2022-11-19    | Jordan
   15 | Giselle     | Titchmarsh     | gtitchmarshe@netscape.com          | Female | 2023-07-03    | Indonesia
   17 | Amandie     | Yurinov        | ayurinovg@cbslocal.com             | Female | 2023-04-26    | Hungary
   26 | Elna        | Dumelow        | edumelowp@utexas.edu               | Female | 2023-09-21    | Kazakhstan
   28 | Pauly       | Johnston       | pjohnstonr@wordpress.com           | Female | 2023-01-31    | Malaysia
   30 | Arabele     | Orringe        | aorringet@yahoo.co.jp              | Female | 2022-12-14    | China
   32 | Nicholle    | Joanic         |                                    | Female | 2023-04-14    | Yemen
   33 | Britteny    | Bourne         |                                    | Female | 2023-01-05    | China
   34 | Sibella     | Comellini      | scomellinix@yelp.com               | Female | 2023-06-19    | China
   35 | Mureil      | Ogilvie        | mogilviey@ask.com                  | Female | 2023-06-21    | Russia
   36 | Sonnie      | McKnockiter    | smcknockiterz@answers.com          | Female | 2023-06-11    | Uganda
   38 | Nadine      | Lewton         | nlewton11@parallels.com            | Female | 2023-02-13    | Bosnia and Herzegovina
   39 | Mattie      | Durante        | mdurante12@ycombinator.com         | Female | 2022-11-02    | China
   41 | Alidia      | Klimochkin     | aklimochkin14@redcross.org         | Female | 2023-01-09    | Croatia
```

```shell
test=# SELECT * FROM person WHERE gender = 'Male' AND country_of_birth = 'Poland';
 id  | first_name | last_name  |             email             | gender | date_of_birth | country_of_birth
-----+------------+------------+-------------------------------+--------+---------------+------------------
 191 | Towny      | Antognoni  | tantognoni5a@nasa.gov         | Male   | 2022-12-25    | Poland
 234 | Silvester  | Cranston   | scranston6h@hp.com            | Male   | 2023-04-13    | Poland
 236 | Federico   | Caselli    | fcaselli6j@histats.com        | Male   | 2023-05-02    | Poland
 301 | Seamus     | Fairlie    | sfairlie8c@arizona.edu        | Male   | 2023-10-02    | Poland
 341 | Dall       | Vennings   | dvennings9g@howstuffworks.com | Male   | 2023-10-08    | Poland
 363 | Rollins    | Heasley    | rheasleya2@artisteer.com      | Male   | 2023-07-20    | Poland
 365 | Thedrick   | Colnet     | tcolneta4@yandex.ru           | Male   | 2023-09-21    | Poland
 373 | Knox       | Owens      |                               | Male   | 2023-04-27    | Poland
 390 | Vidovic    | Chimenti   | vchimentiat@freewebs.com      | Male   | 2023-08-30    | Poland
 399 | Berkeley   | Cubbit     |                               | Male   | 2023-03-26    | Poland
 470 | Waylan     | MacMechan  |                               | Male   | 2023-06-24    | Poland
 479 | Kimbell    | Tindley    | ktindleyda@digg.com           | Male   | 2023-09-08    | Poland
 531 | Nikolos    | Pawson     | npawsoneq@ucoz.ru             | Male   | 2023-01-02    | Poland
 573 | Meredith   | Kensitt    | mkensittfw@phpbb.com          | Male   | 2023-02-06    | Poland
 849 | Stephanus  | Isles      |                               | Male   | 2023-09-30    | Poland
 867 | Dominik    | Pettican   |                               | Male   | 2022-12-09    | Poland
 889 | Mayne      | Boddington | mboddingtonoo@rakuten.co.jp   | Male   | 2023-05-18    | Poland
 899 | Grant      | Gaishson   | ggaishsonoy@wired.com         | Male   | 2023-06-23    | Poland
 952 | Grant      | Newband    |                               | Male   | 2023-09-20    | Poland
 953 | Dilly      | Zanre      |                               | Male   | 2023-08-22    | Poland
(20 rows)
```

## COMPARISON OPERATIONS 

```shell
test=# SELECT 1=1;
 ?column?
----------
 t
(1 row)
```

```shell
test=# SELECT 1<>2;
# not equal to
 ?column?
----------
 t
(1 row)

test=# SELECT 'hsm'<>'HSM';
 ?column?
----------
 t
(1 row)


test=# SELECT 'hsm'<>'hsm';
 ?column?
----------
 f
(1 row)


test=# SELECT 'hsm'<>'hsm1';
 ?column?
----------
 t
(1 row)
```
other comparisons are simple -> `> < >= <=`
## Limit, Offset & fetch

```bash
test=# SELECT * FROM person LIMIT 10;
 id | first_name | last_name |           email            |   gender   | date_of_birth | country_of_birth
----+------------+-----------+----------------------------+------------+---------------+------------------
  1 | Ab         | Bowater   | abowater0@toplist.cz       | Male       | 2022-11-04    | Ukraine
  2 | Mirella    | Hane      | mhane1@wikipedia.org       | Female     | 2023-07-12    | Thailand
  3 | Rosanne    | Frantsev  |                            | Female     | 2022-11-18    | Zambia
  4 | Loleta     | Pattesall | lpattesall3@vistaprint.com | Female     | 2023-03-27    | Sweden
  5 | Thurston   | Weatherby | tweatherby4@senate.gov     | Male       | 2023-08-03    | Egypt
  6 | Dev        | De Stoop  | ddestoop5@last.fm          | Polygender | 2023-04-10    | China
  7 | Ambrosi    | Pepper    | apepper6@google.co.jp      | Male       | 2022-12-10    | South Korea
  8 | Wynnie     | Bourne    | wbourne7@boston.com        | Female     | 2023-03-10    | China
  9 | Penny      | Hutt      | phutt8@twitter.com         | Male       | 2023-05-18    | Peru
 10 | Tanitansy  | Gann      |                            | Female     | 2023-08-27    | Japan
(10 rows)
```

```bash
test=# SELECT * FROM person OFFSET 5 LIMIT 5 ;
 id | first_name | last_name |         email         |   gender   | date_of_birth | country_of_birth
----+------------+-----------+-----------------------+------------+---------------+------------------
  6 | Dev        | De Stoop  | ddestoop5@last.fm     | Polygender | 2023-04-10    | China
  7 | Ambrosi    | Pepper    | apepper6@google.co.jp | Male       | 2022-12-10    | South Korea
  8 | Wynnie     | Bourne    | wbourne7@boston.com   | Female     | 2023-03-10    | China
  9 | Penny      | Hutt      | phutt8@twitter.com    | Male       | 2023-05-18    | Peru
 10 | Tanitansy  | Gann      |                       | Female     | 2023-08-27    | Japan
(5 rows)
```

the better way for limiting over the viw on the table -> 
```bash
test=# SELECT * FROM person OFFSET 5 FETCH NEXT 23 ROW ONLY;
 id | first_name |  last_name  |            email             |   gender   | date_of_birth | country_of_birth
----+------------+-------------+------------------------------+------------+---------------+------------------
  6 | Dev        | De Stoop    | ddestoop5@last.fm            | Polygender | 2023-04-10    | China
  7 | Ambrosi    | Pepper      | apepper6@google.co.jp        | Male       | 2022-12-10    | South Korea
  8 | Wynnie     | Bourne      | wbourne7@boston.com          | Female     | 2023-03-10    | China
  9 | Penny      | Hutt        | phutt8@twitter.com           | Male       | 2023-05-18    | Peru
 10 | Tanitansy  | Gann        |                              | Female     | 2023-08-27    | Japan
 11 | Reba       | Borsnall    |                              | Female     | 2022-11-27    | China
 12 | Frank      | Staniland   | fstanilandb@nature.com       | Male       | 2022-12-27    | Czech Republic
 13 | Tamiko     | Minty       |                              | Female     | 2022-11-19    | Jordan
 14 | Arman      | Gartenfeld  | agartenfeldd@arstechnica.com | Male       | 2023-04-09    | France
 15 | Giselle    | Titchmarsh  | gtitchmarshe@netscape.com    | Female     | 2023-07-03    | Indonesia
 16 | Raddy      | Parkhouse   | rparkhousef@illinois.edu     | Male       | 2022-11-30    | Czech Republic
 17 | Amandie    | Yurinov     | ayurinovg@cbslocal.com       | Female     | 2023-04-26    | Hungary
 18 | Neil       | Ritchings   | nritchingsh@goodreads.com    | Male       | 2023-08-22    | Venezuela
 19 | Wyndham    | Bugge       |                              | Male       | 2023-07-17    | Albania
 20 | Bink       | D'eye       | bdeyej@booking.com           | Male       | 2023-08-12    | United States
 21 | Esme       | Ondracek    | eondracekk@google.fr         | Non-binary | 2023-01-23    | Belarus
 22 | Lester     | Postlewhite | lpostlewhitel@indiatimes.com | Male       | 2023-03-27    | Russia
 23 | Sidney     | Yakebovich  | syakebovichm@berkeley.edu    | Male       | 2023-06-03    | Philippines
 24 | Harv       | Noirel      | hnoireln@ameblo.jp           | Male       | 2023-06-07    | France
 25 | Kelly      | Hackforth   | khackfortho@wufoo.com        | Polygender | 2023-09-03    | Greece
 26 | Elna       | Dumelow     | edumelowp@utexas.edu         | Female     | 2023-09-21    | Kazakhstan
 27 | Saundra    | Habgood     | shabgoodq@noaa.gov           | Male       | 2023-09-17    | Afghanistan
 28 | Pauly      | Johnston    | pjohnstonr@wordpress.com     | Female     | 2023-01-31    | Malaysia
(23 rows)


test=# SELECT * FROM person FETCH FIRST 4 ROW ONLY;
 id | first_name | last_name |           email            | gender | date_of_birth | country_of_birth
----+------------+-----------+----------------------------+--------+---------------+------------------
  1 | Ab         | Bowater   | abowater0@toplist.cz       | Male   | 2022-11-04    | Ukraine
  2 | Mirella    | Hane      | mhane1@wikipedia.org       | Female | 2023-07-12    | Thailand
  3 | Rosanne    | Frantsev  |                            | Female | 2022-11-18    | Zambia
  4 | Loleta     | Pattesall | lpattesall3@vistaprint.com | Female | 2023-03-27    | Sweden
(4 rows)

```

## IN 

Removes iterable multiple OR commands and introduces an array of contentt selection from.
```shell
test=# SELECT * FROM person WHERE country_of_birth IN ('China','Brazil','France');
 id  | first_name  |     last_name      |                email                |   gender    | date_of_birth | country_of_birth
-----+-------------+--------------------+-------------------------------------+-------------+---------------+------------------
   6 | Dev         | De Stoop           | ddestoop5@last.fm                   | Polygender  | 2023-04-10    | China
   8 | Wynnie      | Bourne             | wbourne7@boston.com                 | Female      | 2023-03-10    | China
  11 | Reba        | Borsnall           |                                     | Female      | 2022-11-27    | China
  14 | Arman       | Gartenfeld         | agartenfeldd@arstechnica.com        | Male        | 2023-04-09    | France
  24 | Harv        | Noirel             | hnoireln@ameblo.jp                  | Male        | 2023-06-07    | France
  30 | Arabele     | Orringe            | aorringet@yahoo.co.jp               | Female      | 2022-12-14    | China
  31 | Angelo      | Riolfo             | ariolfou@privacy.gov.au             | Male        | 2022-11-10    | China
  33 | Britteny    | Bourne             |                                     | Female      | 2023-01-05    | China
  34 | Sibella     | Comellini          | scomellinix@yelp.com                | Female      | 2023-06-19    | China
  39 | Mattie      | Durante            | mdurante12@ycombinator.com          | Female      | 2022-11-02    | China
  45 | Chaim       | Mishow             |                                     | Male        | 2023-01-12    | France
  51 | Leandra     | Serris             | lserris1e@about.com                 | Agender     | 2023-01-28    | China
  52 | Alick       | Bownde             | abownde1f@sciencedaily.com          | Male        | 2022-10-22    | France
  54 | Laurice     | Lillee             | llillee1h@msu.edu                   | Female      | 2022-12-30    | China
  55 | Donia       | Sanderson          | dsanderson1i@google.co.uk           | Agender     | 2023-05-11    | China
  62 | Amalita     | Fulep              | afulep1p@bing.com                   | Female      | 2023-06-13    | France
  63 | Myrtice     | Addams             | maddams1q@timesonline.co.uk         | Female      | 2023-06-29    | China
  65 | Taffy       | Harman             |                                     | Female      | 2023-09-16    | France
  66 | Ralina      | Peete              | rpeete1t@cisco.com                  | Female      | 2022-11-03    | China
  79 | Alameda     | Annion             | aannion26@fc2.com                   | Female      | 2023-08-08    | China
  80 | Moore       | Powrie             |                                     | Male        | 2023-09-09    | China
  89 | Shalom      | Pallaske           | spallaske2g@cyberchimps.com         | Male        | 2023-06-03    | China
  94 | Grazia      | Bendon             | gbendon2l@godaddy.com               | Female      | 2023-03-15    | China
  97 | Jefferson   | Roset              |                                     | Male        | 2023-04-22    | France
 105 | Raphaela    | Pate               | rpate2w@reuters.com                 | Genderfluid | 2023-01-31    | Brazil
 107 | Dru         | Seniour            |                                     | Female      | 2022-11-20    | China
```

## BETWEEN 

```shell 
test=# SELECT * FROM person WHERE date_of_birth BETWEEN '2015-01-01' AND '2023-01-01';
 id  | first_name  |     last_name      |               email                |   gender    | date_of_birth |  country_of_birth
-----+-------------+--------------------+------------------------------------+-------------+---------------+--------------------
   1 | Ab          | Bowater            | abowater0@toplist.cz               | Male        | 2022-11-04    | Ukraine
   3 | Rosanne     | Frantsev           |                                    | Female      | 2022-11-18    | Zambia
   7 | Ambrosi     | Pepper             | apepper6@google.co.jp              | Male        | 2022-12-10    | South Korea
  11 | Reba        | Borsnall           |                                    | Female      | 2022-11-27    | China
  12 | Frank       | Staniland          | fstanilandb@nature.com             | Male        | 2022-12-27    | Czech Republic
  13 | Tamiko      | Minty              |                                    | Female      | 2022-11-19    | Jordan
  16 | Raddy       | Parkhouse          | rparkhousef@illinois.edu           | Male        | 2022-11-30    | Czech Republic
  30 | Arabele     | Orringe            | aorringet@yahoo.co.jp              | Female      | 2022-12-14    | China
  31 | Angelo      | Riolfo             | ariolfou@privacy.gov.au            | Male        | 2022-11-10    | China
  39 | Mattie      | Durante            | mdurante12@ycombinator.com         | Female      | 2022-11-02    | China
  40 | Keen        | Boutell            |                                    | Male        | 2022-12-09    | Indonesia
  50 | Jehu        | Simkovich          |                                    | Male        | 2022-12-07    | Tanzania
  52 | Alick       | Bownde             | abownde1f@sciencedaily.com         | Male        | 2022-10-22    | France
  54 | Laurice     | Lillee             | llillee1h@msu.edu                  | Female      | 2022-12-30    | China
  58 | Braden      | Deares             | bdeares1l@sbwire.com               | Male        | 2022-12-19    | Indonesia
  61 | Camala      | Minger             | cminger1o@cbc.ca                   | Female      | 2022-10-30    | Slovenia
  66 | Ralina      | Peete              | rpeete1t@cisco.com                 | Female      | 2022-11-03    | China
  68 | Josias      | Haslin             | jhaslin1v@wikipedia.org            | Male        | 2022-12-24    | Japan
  70 | Rebecca     | Crowne             |                                    | Female      | 2022-12-09    | Philippines
  73 | Bronnie     | Bignall            | bbignall20@bloglines.com           | Male        | 2022-11-08    | Morocco
  86 | Pasquale    | Corsar             |                                    | Male        | 2022-12-03    | Venezuela
  90 | Jordan      | Gallihawk          | jgallihawk2h@bizjournals.com       | Female      | 2022-12-10    | Russia
  91 | Berrie      | Giraldo            | bgiraldo2i@ucoz.com                | Female      | 2022-12-22    | South Korea
  99 | Patric      | Pratley            | ppratley2q@shop-pro.jp             | Male        | 2022-12-25    | Indonesia
 103 | Adey        | Appleyard          |                                    | Female      | 2022-12-29    | Canada
 107 | Dru         | Seniour            |                                    | Female      | 2022-11-20    | China
 121 | Janessa     | McAllaster         | jmcallaster3c@1und1.de             | Female      | 2022-11-28    | Indonesia
 126 | Clayborn    | Lyddiard           |                                    | Male        | 2022-12-09    | China
 ```

## LIKE 

```shell
test=# SELECT * FROM person WHERE email LIKE '%.com';
  id  | first_name  |  last_name   |              email               |   gender    | date_of_birth |     country_of_birth
------+-------------+--------------+----------------------------------+-------------+---------------+--------------------------
    4 | Loleta      | Pattesall    | lpattesall3@vistaprint.com       | Female      | 2023-03-27    | Sweden
    8 | Wynnie      | Bourne       | wbourne7@boston.com              | Female      | 2023-03-10    | China
    9 | Penny       | Hutt         | phutt8@twitter.com               | Male        | 2023-05-18    | Peru
   12 | Frank       | Staniland    | fstanilandb@nature.com           | Male        | 2022-12-27    | Czech Republic
   14 | Arman       | Gartenfeld   | agartenfeldd@arstechnica.com     | Male        | 2023-04-09    | France
   15 | Giselle     | Titchmarsh   | gtitchmarshe@netscape.com        | Female      | 2023-07-03    | Indonesia
   17 | Amandie     | Yurinov      | ayurinovg@cbslocal.com           | Female      | 2023-04-26    | Hungary
   18 | Neil        | Ritchings    | nritchingsh@goodreads.com        | Male        | 2023-08-22    | Venezuela
   20 | Bink        | D'eye        | bdeyej@booking.com               | Male        | 2023-08-12    | United States
   22 | Lester      | Postlewhite  | lpostlewhitel@indiatimes.com     | Male        | 2023-03-27    | Russia
   25 | Kelly       | Hackforth    | khackfortho@wufoo.com            | Polygender  | 2023-09-03    | Greece
   28 | Pauly       | Johnston     | pjohnstonr@wordpress.com         | Female      | 2023-01-31    | Malaysia
   34 | Sibella     | Comellini    | scomellinix@yelp.com             | Female      | 2023-06-19    | China
   35 | Mureil      | Ogilvie      | mogilviey@ask.com                | Female      | 2023-06-21    | Russia
   36 | Sonnie      | McKnockiter  | smcknockiterz@answers.com        | Female      | 2023-06-11    | Uganda
   38 | Nadine      | Lewton       | nlewton11@parallels.com          | Female      | 2023-02-13    | Bosnia and Herzegovina
   39 | Mattie      | Durante      | mdurante12@ycombinator.com       | Female      | 2022-11-02    | China
   42 | Ancell      | Cundict      | acundict15@elpais.com            | Male        | 2023-01-11    | Guatemala
   47 | Liesa       | Diwell       | ldiwell1a@xrea.com               | Female      | 2023-08-17    | Russia
   48 | Philis      | Mulhall      | pmulhall1b@mozilla.com           | Female      | 2023-09-26    | Yemen
   51 | Leandra     | Serris       | lserris1e@about.com              | Agender     | 2023-01-28    | China
   52 | Alick       | Bownde       | abownde1f@sciencedaily.com       | Male        | 2022-10-22    | France
   56 | Dom         | Scawn        | dscawn1j@blinklist.com           | Male        | 2023-05-09    | Indonesia

test=# SELECT * FROM person WHERE email LIKE '%indiatimes.com';
 id | first_name |  last_name  |            email             | gender | date_of_birth | country_of_birth
----+------------+-------------+------------------------------+--------+---------------+------------------
 22 | Lester     | Postlewhite | lpostlewhitel@indiatimes.com | Male   | 2023-03-27    | Russia
(1 row)

test=# SELECT * FROM person WHERE email LIKE '%google.%';
 id  | first_name | last_name |           email           |   gender   | date_of_birth | country_of_birth
-----+------------+-----------+---------------------------+------------+---------------+------------------
   7 | Ambrosi    | Pepper    | apepper6@google.co.jp     | Male       | 2022-12-10    | South Korea
  21 | Esme       | Ondracek  | eondracekk@google.fr      | Non-binary | 2023-01-23    | Belarus
  43 | May        | Maggi     | mmaggi16@google.com.br    | Female     | 2023-08-31    | Portugal
  49 | Jeannette  | Anglim    | janglim1c@google.co.uk    | Female     | 2023-01-21    | Sweden
  55 | Donia      | Sanderson | dsanderson1i@google.co.uk | Agender    | 2023-05-11    | China
  67 | Milli      | Birley    | mbirley1u@google.co.uk    | Female     | 2023-10-18    | Malaysia
  84 | Payton     | Frame     | pframe2b@google.cn        | Male       | 2023-05-15    | Ethiopia
 125 | Phillie    | Tickel    | ptickel3g@google.co.uk    | Female     | 2023-03-16    | Poland
 167 | Jayme      | Lavery    | jlavery4m@google.co.uk    | Male       | 2023-06-23    | France
 214 | Nolana     | Normant   | nnormant5x@google.ru      | Female     | 2022-11-05    | China
 247 | Dominick   | Prosch    | dprosch6u@google.com.au   | Male       | 2022-12-20    | Peru
 254 | Karia      | Blackborn | kblackborn71@google.it    | Female     | 2022-12-23    | Syria
 408 | Lay        | Ruddock   | lruddockbb@google.co.jp   | Male       | 2023-06-05    | Senegal
 459 | Jerry      | Origan    | jorigancq@google.it       | Polygender | 2023-04-13    | Colombia
 526 | Jock       | Perrin    | jperrinel@google.co.uk    | Male       | 2023-03-17    | Argentina
 611 | Kelcy      | Mitchall  | kmitchallgy@google.it     | Female     | 2022-11-27    | Philippines
 731 | Abran      | Fateley   | afateleyka@google.es      | Male       | 2023-06-27    | Luxembourg
 791 | Emelia     | Bim       | ebimly@google.com         | Female     | 2023-07-25    | Russia
 806 | Livvy      | Barkus    | lbarkusmd@google.ca       | Female     | 2023-03-29    | China
 858 | Leigh      | Velasquez | lvelasqueznt@google.com   | Female     | 2022-11-04    | Philippines
 872 | Alleyn     | Broadbent | abroadbento7@google.it    | Male       | 2023-05-04    | Indonesia
 936 | Arabella   | Elsip     | aelsippz@google.com.hk    | Female     | 2023-02-05    | France
(22 rows)


test=# SELECT * FROM person WHERE email LIKE '%google.__.__';
 id  | first_name | last_name |           email           | gender  | date_of_birth | country_of_birth
-----+------------+-----------+---------------------------+---------+---------------+------------------
   7 | Ambrosi    | Pepper    | apepper6@google.co.jp     | Male    | 2022-12-10    | South Korea
  49 | Jeannette  | Anglim    | janglim1c@google.co.uk    | Female  | 2023-01-21    | Sweden
  55 | Donia      | Sanderson | dsanderson1i@google.co.uk | Agender | 2023-05-11    | China
  67 | Milli      | Birley    | mbirley1u@google.co.uk    | Female  | 2023-10-18    | Malaysia
 125 | Phillie    | Tickel    | ptickel3g@google.co.uk    | Female  | 2023-03-16    | Poland
 167 | Jayme      | Lavery    | jlavery4m@google.co.uk    | Male    | 2023-06-23    | France
 408 | Lay        | Ruddock   | lruddockbb@google.co.jp   | Male    | 2023-06-05    | Senegal
 526 | Jock       | Perrin    | jperrinel@google.co.uk    | Male    | 2023-03-17    | Argentina
(8 rows)



test=# SELECT * FROM person WHERE country_of_birth LIKE 'P%';
 id  | first_name |  last_name  |               email               |   gender    | date_of_birth |   country_of_birth
-----+------------+-------------+-----------------------------------+-------------+---------------+-----------------------
   9 | Penny      | Hutt        | phutt8@twitter.com                | Male        | 2023-05-18    | Peru
  23 | Sidney     | Yakebovich  | syakebovichm@berkeley.edu         | Male        | 2023-06-03    | Philippines
  43 | May        | Maggi       | mmaggi16@google.com.br            | Female      | 2023-08-31    | Portugal
  44 | Xenia      | Shireff     |                                   | Female      | 2023-09-25    | Poland
  53 | Standford  | Clougher    | sclougher1g@wisc.edu              | Male        | 2023-10-13    | Philippines
  64 | Heath      | Phette      | hphette1r@slideshare.net          | Male        | 2023-06-10    | Peru
  70 | Rebecca    | Crowne      |                                   | Female      | 2022-12-09    | Philippines
  81 | Ramon      | Maddie      |                                   | Male        | 2023-09-06    | Philippines
  87 | Tanitansy  | Brotheridge | tbrotheridge2e@mit.edu            | Female      | 2023-07-14    | Philippines
  98 | Adina      | Downing     | adowning2p@army.mil               | Female      | 2023-04-13    | Philippines
 125 | Phillie    | Tickel      | ptickel3g@google.co.uk            | Female      | 2023-03-16    | Poland
 129 | Al         | Shackleford | ashackleford3k@marketwatch.com    | Male        | 2023-08-02    | Philippines
 132 | Lenard     | Manion      | lmanion3n@smugmug.com             | Male        | 2023-03-22    | Peru
 133 | Henrie     | Duigenan    | hduigenan3o@wikia.com             | Female      | 2023-02-02    | Philippines
 134 | Ronica     | Dandie      | rdandie3p@ed.gov                  | Female      | 2023-06-24    | Poland
 179 | Margeaux   | Storrie     |                                   | Female      | 2023-08-13    | Philippines

  532 | Shayna     | Hadgraft    |                                   | Female      | 2022-11-10    | Poland
test=# SELECT * FROM person WHERE country_of_birth ILIKE 'p%';
 id  | first_name |  last_name  |               email               |   gender    | date_of_birth |   country_of_birth
-----+------------+-------------+-----------------------------------+-------------+---------------+-----------------------
   9 | Penny      | Hutt        | phutt8@twitter.com                | Male        | 2023-05-18    | Peru
  23 | Sidney     | Yakebovich  | syakebovichm@berkeley.edu         | Male        | 2023-06-03    | Philippines
  43 | May        | Maggi       | mmaggi16@google.com.br            | Female      | 2023-08-31    | Portugal
  44 | Xenia      | Shireff     |                                   | Female      | 2023-09-25    | Poland
  53 | Standford  | Clougher    | sclougher1g@wisc.edu              | Male        | 2023-10-13    | Philippines
  64 | Heath      | Phette      | hphette1r@slideshare.net          | Male        | 2023-06-10    | Peru
  70 | Rebecca    | Crowne      |                                   | Female      | 2022-12-09    | Philippines
  81 | Ramon      | Maddie      |                                   | Male        | 2023-09-06    | Philippines
  87 | Tanitansy  | Brotheridge | tbrotheridge2e@mit.edu            | Female      | 2023-07-14    | Philippines
  98 | Adina      | Downing     | adowning2p@army.mil               | Female      | 2023-04-13    | Philippines
 125 | Phillie    | Tickel      | ptickel3g@google.co.uk            | Female      | 2023-03-16    | Poland
 129 | Al         | Shackleford | ashackleford3k@marketwatch.com    | Male        | 2023-08-02    | Philippines
 132 | Lenard     | Manion      | lmanion3n@smugmug.com             | Male        | 2023-03-22    | Peru
 133 | Henrie     | Duigenan    | hduigenan3o@wikia.com             | Female      | 2023-02-02    | Philippines
 134 | Ronica     | Dandie      | rdandie3p@ed.gov                  | Female      | 2023-06-24    | Poland
 179 | Margeaux   | Storrie     |                                   | Female      | 2023-08-13    | Philippines
 191 | Towny      | Antognoni   | tantognoni5a@nasa.gov             | Male        | 2022-12-25    | Poland
 201 | Aland      | Pautard     | apautard5k@wired.com              | Male        | 2022-12-04    | Philippines
 205 | Stanleigh  | Somner      | ssomner5o@4shared.com             | Bigender    | 2023-06-17    | Philippines
 215 | Shandie    | Chastang    | schastang5y@yahoo.com             | Non-binary  | 2023-07-09    | Philippines

```

## GROUP BY 

```shell
test=# SELECT country_of_birth, COUNT(*) FROM person GROUP BY country_of_birth;
     country_of_birth     | count
--------------------------+-------
 Bangladesh               |     2
 Indonesia                |   100
 Venezuela                |     9
 Cameroon                 |     1
 Luxembourg               |     2
 Czech Republic           |    18
 Sweden                   |    17
 Uganda                   |     3
 Jordan                   |     2
 Dominican Republic       |     3
 Ireland                  |     7
 Macedonia                |     5
 Sri Lanka                |     1
 Laos                     |     1
 Finland                  |     4
 Portugal                 |    22
 Malta                    |     2
 Colombia                 |    13
 Albania                  |     4
 Saudi Arabia             |     3
 Ukraine                  |    14
 Cuba                     |     4

test=# SELECT country_of_birth, COUNT(*) FROM person GROUP BY country_of_birth ORDER BY COUNT DESC;
     country_of_birth     | count
--------------------------+-------
 China                    |   196
 Indonesia                |   100
 Russia                   |    59
 Philippines              |    49
 France                   |    42
 Poland                   |    39
 Brazil                   |    36
 Portugal                 |    22
 United States            |    18
 Czech Republic           |    18
 Japan                    |    18
 Sweden                   |    17
 Peru                     |    17
 Thailand                 |    15
 Nigeria                  |    15
 Ukraine                  |    14
 Colombia                 |    13
 Argentina                |    13
 Greece                   |    11
 Kazakhstan               |    10
 Mexico                   |    10
 Vietnam                  |     9
 Venezuela                |     9
 Canada                   |     9
 Croatia                  |     9
 Malaysia                 |     8
 Ireland                  |     7
 Pakistan                 |     6
 Yemen                    |     5
 Tanzania                 |     5
```

## GROUP BY HAVING 

```shell
test=# SELECT country_of_birth, COUNT(*) FROM person GROUP BY country_of_birth HAVING COUNT(*)>40 ORDER BY COUNT DESC;
 country_of_birth | count
------------------+-------
 China            |   196
 Indonesia        |   100
 Russia           |    59
 Philippines      |    49
 France           |    42
(5 rows)
```

## **[General-Purpose Aggregate Functions](https://www.postgresql.org/docs/9.5/functions-aggregate.html)**

   
|Function|Argument Type(s)|Return Type|Description|
|---|---|---|---|
|`array_agg(expression)`|any non-array type|array of the argument type|input values, including nulls, concatenated into an array|
|`array_agg(expression)`|any array type|same as argument data type|input arrays concatenated into array of one higher dimension (inputs must all have same dimensionality, and cannot be empty or null)|
|`avg(expression)`|smallint, int, bigint, real, double precision, numeric, or interval|numeric for any integer-type argument, double precision for a floating-point argument, otherwise the same as the argument data type|the average (arithmetic mean) of all non-null input values|
|`bit_and(expression)`|smallint, int, bigint, or bit|same as argument data type|the bitwise AND of all non-null input values, or null if none|
|`bit_or(expression)`|smallint, int, bigint, or bit|same as argument data type|the bitwise OR of all non-null input values, or null if none|
|`bool_and(expression)`|bool|bool|true if all input values are true, otherwise false|
|`bool_or(expression)`|bool|bool|true if at least one input value is true, otherwise false|
|`count(*)`||bigint|number of input rows|
|`count(expression)`|any|bigint|number of input rows for which the value of expression is not null|
|`every(expression)`|bool|bool|equivalent to `bool_and`|
|`json_agg(expression)`|any|json|aggregates values, including nulls, as a JSON array|
|`jsonb_agg(expression)`|any|jsonb|aggregates values, including nulls, as a JSON array|
|`json_object_agg(name, value)`|(any, any)|json|aggregates name/value pairs as a JSON object; values can be null, but not names|
|`jsonb_object_agg(name, value)`|(any, any)|jsonb|aggregates name/value pairs as a JSON object; values can be null, but not names|
|`max(expression)`|any numeric, string, date/time, network, or enum type, or arrays of these types|same as argument type|maximum value of expression across all non-null input values|
|`min(expression)`|any numeric, string, date/time, network, or enum type, or arrays of these types|same as argument type|minimum value of expression across all non-null input values|
|`string_agg(expression, delimiter)`|(text, text) or (bytea, bytea)|same as argument types|non-null input values concatenated into a string, separated by delimiter|
|`sum(expression)`|smallint, int, bigint, real, double precision, numeric, interval, or money|bigint for smallint or int arguments, numeric for bigint arguments, otherwise the same as the argument data type|sum of expression across all non-null input values|
|`xmlagg(expression)`|xml|xml|concatenation of non-null XML values (see also [Section 9.14.1.7](https://www.postgresql.org/docs/9.5/functions-xml.html#FUNCTIONS-XML-XMLAGG))|

It should be noted that except for `count`, these functions return a null value when no rows are selected. In particular, `sum` of no rows returns null, not zero as one might expect, and `array_agg` returns null rather than an empty array when there are no input rows. The `coalesce` function can be used to substitute zero or an empty array for null when necessary.


## MAX 

```shell
test=# SELECT MAX(price) FROM car;
    max
-----------
 999444.30
(1 row)
```

## AVERAGE

```shell
test=# SELECT AVG(price) FROM car;
         avg
---------------------
 492790.419160000000
(1 row)

test=# SELECT ROUND(AVG(price)) FROM car;
 round
--------
 492790
(1 row)
```

## MIN 

q) minimun price for each make and model -> 
```shell
test=# SELECT make, model, MIN(price) FROM car GROUP BY make, model;
     make      |        model         |    min
---------------+----------------------+-----------
 Saturn        | Aura                 | 405701.80
 Oldsmobile    | Silhouette           | 135610.09
 Daewoo        | Lanos                | 866165.72
 Ford          | Econoline E350       |  79830.19
 Jeep          | Grand Cherokee       | 279212.35
 Pontiac       | Monterey             | 611416.77
 Hyundai       | Accent               | 223193.72
 Dodge         | D350                 | 452620.18
 Mazda         | CX-5                 | 244663.46
 Infiniti      | G                    | 430626.59
 Buick         | Electra              | 935000.73
 Nissan        | 300ZX                | 107717.34
 Nissan        | Xterra               | 312617.90
 Chevrolet     | Cavalier             | 787834.34
 Acura         | ZDX                  | 881903.16
 Isuzu         | Axiom                | 367294.32
 Audi          | 4000                 | 487209.19
 Audi          | A8                   | 709109.57
 Hyundai       | Sonata               | 433920.33
 Lamborghini   | Countach             | 693258.67
```

```shell
test=# SELECT make, MAX(price) FROM car GROUP BY make;
     make      |    max
---------------+-----------
 Lincoln       | 970475.69
 Honda         | 929335.28
 Ram           | 902428.59
 Daewoo        | 866165.72
 Morgan        | 922903.52
 Ford          | 985829.44
 Smart         | 469848.62
 Scion         | 789149.33
 Aptera        | 489541.96
 Maserati      | 707960.32
 Dodge         | 979684.93
 Chevrolet     | 927116.45
 Infiniti      | 679216.68
 Saturn        | 998691.18
 MINI          | 820550.52
 Jensen        | 149843.02
 Bentley       | 373437.14
 Pontiac       | 993771.45
 Porsche       | 996496.51
 Plymouth      | 787985.88
 Audi          | 949269.95
 Panoz         | 670140.02
 Tesla         | 377541.75
 Jaguar        | 944300.87
 Renault       | 617073.46
 Lexus         | 879417.90
 Lotus         | 987945.62
```

## SUM 

```shell
test=# SELECT SUM(price) FROM car;
     sum
--------------
 492790419.16
(1 row)

test=# SELECT make, SUM(price) FROM car GROUP BY make;
     make      |     sum
---------------+-------------
 Lincoln       | 15983190.11
 Honda         | 14008370.46
 Ram           |   902428.59
 Daewoo        |  2104801.98
 Morgan        |   922903.52
 Ford          | 45791140.69
 Smart         |   749551.35
 Scion         |  1306352.44
 Aptera        |   489541.96
 Maserati      |  3481176.45
 Dodge         | 29685602.73
 Chevrolet     | 37017690.60
 Infiniti      |  3107084.94
 Saturn        |  5036912.33
 MINI          |  2505426.40
 Jensen        |   149843.02
 Bentley       |   711277.41
 Pontiac       | 18535943.66
 Porsche       |  3397817.77
 Plymouth      |  2470257.14
 Audi          | 11193563.30
 Panoz         |   706520.51
 Tesla         |   377541.75
 Jaguar        |  3287762.86
 Renault       |   617073.46
 Lexus         |  9748126.10
 Lotus         |  3789728.35


 test=# SELECT make, SUM(price) FROM car GROUP BY make ORDER BY SUM(price) DESC;
     make      |     sum
---------------+-------------
 Ford          | 45791140.69
 Chevrolet     | 37017690.60
 Dodge         | 29685602.73
 GMC           | 26154848.08
 Toyota        | 25540240.94
 Mitsubishi    | 20493432.04
 Mazda         | 20386615.28
 Pontiac       | 18535943.66
 Buick         | 16794478.93
 Lincoln       | 15983190.11
 Mercedes-Benz | 15770904.45
 Volkswagen    | 15587170.92
 Honda         | 14008370.46
 Nissan        | 12247657.43
 Chrysler      | 11657087.27
 Subaru        | 11292256.64
 Audi          | 11193563.30
 Hyundai       | 10555593.92
 BMW           | 10027517.86
 Lexus         |  9748126.10
 Suzuki        |  8928944.44
 Cadillac      |  8465624.21
 Mercury       |  7863496.03
 Volvo         |  7725594.92
 Land Rover    |  7513304.56
 Saab          |  5492475.97
```

<p align="center">
    <b><i>-------------- Continued in README2.md --------------</i></b>
</p>