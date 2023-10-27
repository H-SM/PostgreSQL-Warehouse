<p align="center">
    <b><i>-------------- Continued from README2.md --------------</i></b>
</p>

## Primary Key 

```shell    
test=# \d person
                                         Table "public.person"
      Column      |          Type          | Collation | Nullable |              Default
------------------+------------------------+-----------+----------+------------------------------------
 id               | bigint                 |           | not null | nextval('person_id_seq'::regclass)
 first_name       | character varying(50)  |           | not null |
 last_name        | character varying(50)  |           | not null |
 email            | character varying(150) |           |          |
 gender           | character varying(50)  |           | not null |
 date_of_birth    | date                   |           | not null |
 country_of_birth | character varying(50)  |           |          |
Indexes:
    "person_pkey" PRIMARY KEY, btree (id)

test=# insert into person (id, first_name, last_name, email, gender, date_of_birth, country_of_birth) values (1, 'Ab', 'Bowater', 'abowater0@toplist.cz', 'Male', '2022-11-04', 'Ukraine');
ERROR:  duplicate key value violates unique constraint "person_pkey"
DETAIL:  Key (id)=(1) already exists.

# DROPPING THE CONSTRAINT FOR THE PRIMARY KEY 
test=# ALTER TABLE person DROP CONSTRAINT person_pkey;
ALTER TABLE
test=# \d person
                                         Table "public.person"
      Column      |          Type          | Collation | Nullable |              Default
------------------+------------------------+-----------+----------+------------------------------------
 id               | bigint                 |           | not null | nextval('person_id_seq'::regclass)
 first_name       | character varying(50)  |           | not null |
 last_name        | character varying(50)  |           | not null |
 email            | character varying(150) |           |          |
 gender           | character varying(50)  |           | not null |
 date_of_birth    | date                   |           | not null |
 country_of_birth | character varying(50)  |           |          |

test=# insert into person (id, first_name, last_name, email, gender, date_of_birth, country_of_birth) values (1, 'Ab', 'Bowater', 'abowater0@toplist.cz', 'Male', '2022-11-04', 'Ukraine');
INSERT 0 1

test=# SELECT * FROM person WHERE id=1 ;
 id | first_name | last_name |        email         | gender | date_of_birth | country_of_birth
----+------------+-----------+----------------------+--------+---------------+------------------
  1 | Ab         | Bowater   | abowater0@toplist.cz | Male   | 2022-11-04    | Ukraine
  1 | Ab         | Bowater   | abowater0@toplist.cz | Male   | 2022-11-04    | Ukraine
(2 rows)
```

```shell 
# ADDING BACK THE PRIMARY KEY 
test=# DELETE FROM person WHERE id = 1;
DELETE 2
test=# insert into person (id, first_name, last_name, email, gender, date_of_birth, country_of_birth) values (1, 'Ab', 'Bowater', 'abowater0@toplist.cz', 'Male', '2022-11-04', 'Ukraine');
INSERT 0 1
test=# SELECT * FROM person WHERE id=1 ;
 id | first_name | last_name |        email         | gender | date_of_birth | country_of_birth
----+------------+-----------+----------------------+--------+---------------+------------------
  1 | Ab         | Bowater   | abowater0@toplist.cz | Male   | 2022-11-04    | Ukraine
(1 row)


test=# ALTER TABLE person ADD PRIMARY KEY (id);
ALTER TABLE
test=# \d person
                                         Table "public.person"
      Column      |          Type          | Collation | Nullable |              Default
------------------+------------------------+-----------+----------+------------------------------------
 id               | bigint                 |           | not null | nextval('person_id_seq'::regclass)
 first_name       | character varying(50)  |           | not null |
 last_name        | character varying(50)  |           | not null |
 email            | character varying(150) |           |          |
 gender           | character varying(50)  |           | not null |
 date_of_birth    | date                   |           | not null |
 country_of_birth | character varying(50)  |           |          |
Indexes:
    "person_pkey" PRIMARY KEY, btree (id)

```

## UNIQUE CONSTRAINTS 

```shell 
test=# SELECT email, COUNT(*) FROM person GROUP BY email;
                email                | count
-------------------------------------+-------
 dhiggonetec@meetup.com              |     1
 wbourne7@boston.com                 |     1
 vstoylesrp@cocolog-nifty.com        |     1
                                     |   301
 oeakenbz@mit.edu                    |     1
 nritchingsh@goodreads.com           |     1
 dbodenql@bloomberg.com              |     1
 awanka5m@cisco.com                  |     1
 bpydcock8n@rambler.ru               |     1
 bburehillkm@wikia.com               |     1
 rahrensen@home.pl                   |     1
 rovendalehj@imageshack.us           |     1
 gsnoadah@jiathis.com                |     1
 bmallenfe@bandcamp.com              |     1
 scomellinix@yelp.com                |     1
 dsedgwick76@ucoz.com                |     1
 njellings75@fotki.com               |     1
 dbertm0@bandcamp.com                |     1
 njensenfv@craigslist.org            |     1
 lstollenhoffi@cornell.edu           |     1
 mpavlitschekpf@dagondesign.com      |     1
 pjannyoz@ucsd.edu                   |     1
 oautinfk@dailymail.co.uk            |     1
 bdoonican61@independent.co.uk       |     1
 jmccroryn5@uol.com.br               |     1
 gbalazot7o@opera.com                |     1
 dmccooleok@shutterfly.com           |     1
 sclougher1g@wisc.edu                |     1
 evalentineme@cbsnews.com            |     1

test=# ALTER TABLE person ADD CONSTRAINT unique_email UNIQUE(email);
ALTER TABLE
test=# \d person
                                         Table "public.person"
      Column      |          Type          | Collation | Nullable |              Default
------------------+------------------------+-----------+----------+------------------------------------
 id               | bigint                 |           | not null | nextval('person_id_seq'::regclass)
 first_name       | character varying(50)  |           | not null |
 last_name        | character varying(50)  |           | not null |
 email            | character varying(150) |           |          |
 gender           | character varying(50)  |           | not null |
 date_of_birth    | date                   |           | not null |
 country_of_birth | character varying(50)  |           |          |
Indexes:
    "person_pkey" PRIMARY KEY, btree (id)
    "unique_email" UNIQUE CONSTRAINT, btree (email)

test=# insert into person (first_name, last_name, email, gender, date_of_birth, country_of_birth) values ( 'Ab', 'Bowater', 'abowater0@toplist.cz', 'Male', '2022-11-04', 'Ukraine');
ERROR:  duplicate key value violates unique constraint "unique_email"
DETAIL:  Key (email)=(abowater0@toplist.cz) already exists.
```
```shell 

```
```shell 

```
```shell 

```