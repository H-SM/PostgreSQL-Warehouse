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

test=# ALTER TABLE person ADD UNIQUE(email);
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
    "person_email_key" UNIQUE CONSTRAINT, btree (email)

# here the name for the constraint is decided by postgres 
```

## Check constraint 
```shell 
# add constraint based on a boolean condition over it 
test=# ALTER TABLE person ADD constraint gender_constraint CHECK( gender = 'Female' OR gender = 'Male');
ALTER TABLE
test=# \d person
                                         Table "public.person"
      Column      |          Type          | Collation | Nullable |              Default               ------------------+------------------------+-----------+----------+------------------------------------ id               | bigint                 |           | not null | nextval('person_id_seq'::regclass)
 first_name       | character varying(50)  |           | not null |
 last_name        | character varying(50)  |           | not null |
 email            | character varying(150) |           |          |
 gender           | character varying(50)  |           | not null |
 date_of_birth    | date                   |           | not null |
 country_of_birth | character varying(50)  |           |          |
Indexes:
    "person_pkey" PRIMARY KEY, btree (id)
Check constraints:
    "gender_constraint" CHECK (gender::text = 'Female'::text OR gender::text = 'Male'::text)


test=# insert into person (first_name, last_name, email, gender, date_of_birth, country_of_birth) values ('Ab', 'Bowater', 'abowat0@toplist.cz', 'Hello', '2022-11-04', 'Ukraine');
ERROR:  new row for relation "person" violates check constraint "gender_constraint"
DETAIL:  Failing row contains (1002, Ab, Bowater, abowat0@toplist.cz, Hello, 2022-11-04, Ukraine).
```
## DELETE 

```shell 
DELETE FROM person WHERE gender = 'Female' AND country_of_birth = 'Hungary';
DELETE 1

test=# DELETE FROM person WHERE id = 3;
DELETE 1

test=# DELETE FROM person WHERE gender NOT IN ('Male', 'Female');
DELETE 99
```

## UPDATE 

```shell 
test=# UPDATE person SET email = 'ommar@gmail.com' WHERE id = 13;
UPDATE 1

test=# SELECT * FROM person WHERE id = 13;
 id | first_name | last_name |      email      | gender | date_of_birth | country_of_birth
----+------------+-----------+-----------------+--------+---------------+------------------
 13 | Tamiko     | Minty     | ommar@gmail.com | Female | 2022-11-19    | Jordan
(1 row)

```

## ON CONFLICT DO NOTHING

```shell
test=# INSERT INTO person (id, first_name, last_name, gender, email, date_of_birth, country_of_birth )
test-# VALUES (14, 'Arman', 'Gartenfeild', 'agartenfeldd@gmail.com', 'Male', DATE '2023-04-09', 'France');
ERROR:  duplicate key value violates unique constraint "person_pkey"
DETAIL:  Key (id)=(14) already exists.
test=# INSERT INTO person (id, first_name, last_name, gender, email, date_of_birth, country_of_birth )
test-# VALUES (14, 'Arman', 'Gartenfeild', 'agartenfeldd@gmail.com', 'Male', DATE '2023-04-09', 'France')
test-# ON CONFLICT (id) DO NOTHING;
INSERT 0 0
```

## ON CONFLICT DO UPDATE (UPSERT)

```shell
test=# INSERT INTO person (id, first_name, last_name, email, gender, date_of_birth, country_of_birth )
test-# VALUES (14, 'Arman', 'Gartenfeild', 'agartenfeldd@gmail.gov.in', 'Male', DATE '2023-04-09', 'France')
test-# ON CONFLICT (id) DO UPDATE SET email = EXCLUDED.email;
INSERT 0 1
test=# SELECT * FROM person WHERE id = 14;
 id | first_name | last_name  |           email           | gender | date_of_birth | country_of_birth
----+------------+------------+---------------------------+--------+---------------+------------------
 14 | Arman      | Gartenfeld | agartenfeldd@gmail.gov.in | Male   | 2023-04-09    | France
(1 row)
# update up only the email if there is any change done by the user else do nothing
```
<p align="center">
    <b><i>-------------- Continued in README4.md --------------</i></b>
</p>