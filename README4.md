<p align="center">
    <b><i>-------------- Continued from README3.md --------------</i></b>
</p>

## ADDING RELATIONSHIP BETWEEN 2 TABLES 

- by using up of pairing the reference/foriegn key in person 
- to the primary key in the second table (car) 

```shell
test=# UPDATE person SET car_id = 1 WHERE id=1;
UPDATE 1
test=# UPDATE person SET car_id = 2 WHERE id=1;
UPDATE 1
test=# SELECT * FROM person;
 id | first_name | last_name | gender |           email            | date_of_birth | country_of_birth | car_id
----+------------+-----------+--------+----------------------------+---------------+------------------+--------
  2 | Omar       | Calmore   | Male   |                            | 1921-04-03    | Finland          |
  3 | Adriana    | Matuschek | Female | amatuschek2@feedburner.com | 1953-10-28    | Sweden           |
  1 | Fernanda   | Beardon   | Female | fernandab@is.gb            | 1953-10-28    | Norway           |      2
(3 rows)


test=# UPDATE person SET car_id = 2 WHERE id=2;
ERROR:  duplicate key value violates unique constraint "person_car_id_key"
DETAIL:  Key (car_id)=(2) already exists.
```

## INNER JOIN 

```shell
test=# SELECT * FROM person JOIN car ON person.car_id = car.id;
 id | first_name | last_name | gender |      email      | date_of_birth | country_of_birth | car_id | id |    make    |  model   |  price
----+------------+-----------+--------+-----------------+---------------+------------------+--------+----+------------+----------+----------
  2 | Omar       | Calmore   | Male   |                 | 1921-04-03    | Finland          |      1 |  1 | Land Rover | Sterling | 87665.38
  1 | Fernanda   | Beardon   | Female | fernandab@is.gb | 1953-10-28    | Norway           |      2 |  2 | GMC        | Acadia   | 17662.69
(2 rows)

test=# SELECT * FROM person JOIN car ON person.car_id = car.id;
-[ RECORD 1 ]----+----------------
id               | 2
first_name       | Omar
last_name        | Calmore
gender           | Male
email            |
date_of_birth    | 1921-04-03
country_of_birth | Finland
car_id           | 1
id               | 1
make             | Land Rover
model            | Sterling
price            | 87665.38
-[ RECORD 2 ]----+----------------
id               | 1
first_name       | Fernanda
last_name        | Beardon
gender           | Female
email            | fernandab@is.gb
date_of_birth    | 1953-10-28
country_of_birth | Norway
car_id           | 2
id               | 2
make             | GMC
model            | Acadia
price            | 17662.69
```

```shell
test=# SELECT person.first_name , car.make, car.model, car.price FROM person
test-# JOIN car ON person.car_id = car.id;
-[ RECORD 1 ]----------
first_name | Omar
make       | Land Rover
model      | Sterling
price      | 87665.38
-[ RECORD 2 ]----------
first_name | Fernanda
make       | GMC
model      | Acadia
price      | 17662.69
```

## LEFT JOINS

```shell
test=# SELECT * FROM person
test-# LEFT JOIN car ON car.id = person.car_id;
-[ RECORD 1 ]----+---------------------------
id               | 2
first_name       | Omar
last_name        | Calmore
gender           | Male
email            |
date_of_birth    | 1921-04-03
country_of_birth | Finland
car_id           | 1
id               | 1
make             | Land Rover
model            | Sterling
price            | 87665.38
-[ RECORD 2 ]----+---------------------------
id               | 1
first_name       | Fernanda
last_name        | Beardon
gender           | Female
email            | fernandab@is.gb
date_of_birth    | 1953-10-28
country_of_birth | Norway
car_id           | 2
id               | 2
make             | GMC
model            | Acadia
price            | 17662.69
-[ RECORD 3 ]----+---------------------------
id               | 3
first_name       | Adriana
last_name        | Matuschek
gender           | Female
email            | amatuschek2@feedburner.com
date_of_birth    | 1953-10-28
country_of_birth | Sweden
car_id           |
id               |
make             |
model            |
price            |
```

```shell
test=# SELECT * FROM person
test-# LEFT JOIN car ON car.id = person.car_id
test-# WHERE car.* IS NULL;
 id | first_name | last_name | gender |           email            | date_of_birth | country_of_birth | car_id | id | make | model | price
----+------------+-----------+--------+----------------------------+---------------+------------------+--------+----+------+-------+-------
  3 | Adriana    | Matuschek | Female | amatuschek2@feedburner.com | 1953-10-28    | Sweden           |        |    |      |       |
(1 row)
```

## DELETING RECORDS WITH FORIEGN KEYS 

```shell
test=# DELETE FROM car WHERE id = 13;
ERROR:  update or delete on table "car" violates foreign key constraint "person_car_id_fkey" on table "person"
DETAIL:  Key (id)=(13) is still referenced from table "person".
# we need to delete the FK reference to this particular data/ car in order to delete the actual details (we could convert the FK in person to NULL or remove that data before this reference)
test=# DELETE FROM person WHERE id = 9000;
DELETE 1
test=# DELETE FROM car WHERE id = 13;
DELETE 1
test=# SELECT * FROM car WHERE id = 13;
 id | make | model | price
----+------+-------+-------
(0 rows)
```

## GENERATE A CSV EXPORTING QUERY RESULTS

```shell

test=# SELECT * FROM person
test-# LEFT JOIN car ON car.id = person.car_id;
 id | first_name | last_name | gender |           email            | date_of_birth | country_of_birth | car_id | id |    make    |  model   |  price
----+------------+-----------+--------+----------------------------+---------------+------------------+--------+----+------------+----------+----------
  2 | Omar       | Calmore   | Male   |                            | 1921-04-03    | Finland          |      1 |  1 | Land Rover | Sterling | 87665.38
  1 | Fernanda   | Beardon   | Female | fernandab@is.gb            | 1953-10-28    | Norway           |      2 |  2 | GMC        | Acadia   | 17662.69
  3 | Adriana    | Matuschek | Female | amatuschek2@feedburner.com | 1953-10-28    | Sweden           |        |    |            |          |
(3 rows)

test=# \copy (SELECT * FROM person LEFT JOIN car ON car.id = person.car_id) TO 'e:/coder/PostgreSQL-Warehouse/result.csv' DELIMITER ',' CSV HEADER;
COPY 3
```

## SERIAL & SEQUENCES 

```shell 
test=# SELECT * FROM person_id_seq;
 last_value | log_cnt | is_called
------------+---------+-----------
          3 |      30 | t
(1 row)
# log_cnt -> how many times its been invoked

test=# \d person
                                         Table "public.person"
      Column      |          Type          | Collation | Nullable |              Default               ------------------+------------------------+-----------+----------+------------------------------------ id               | bigint                 |           | not null | nextval('person_id_seq'::regclass)
 first_name       | character varying(50)  |           | not null |
 last_name        | character varying(50)  |           | not null |
 gender           | character varying(7)   |           | not null |
 email            | character varying(150) |           |          |
 date_of_birth    | date                   |           | not null |
 country_of_birth | character varying(50)  |           | not null |
 car_id           | bigint                 |           |          |
Indexes:
    "person_pkey" PRIMARY KEY, btree (id)
    "person_car_id_key" UNIQUE CONSTRAINT, btree (car_id)
Foreign-key constraints:
    "person_car_id_fkey" FOREIGN KEY (car_id) REFERENCES car(id)


test=# SELECT nextval('person_id_seq'::regclass);
 nextval
---------
       4
(1 row)

test=# SELECT * FROM person;
 id | first_name | last_name | gender |           email            | date_of_birth | country_of_birth | car_id
----+------------+-----------+--------+----------------------------+---------------+------------------+--------
  3 | Adriana    | Matuschek | Female | amatuschek2@feedburner.com | 1953-10-28    | Sweden           |
  1 | Fernanda   | Beardon   | Female | fernandab@is.gb            | 1953-10-28    | Norway           |      2
  2 | Omar       | Calmore   | Male   |                            | 1921-04-03    | Finland          |      1
(3 rows)


test=# insert into person (first_name, last_name, email, gender, date_of_birth, country_of_birth) values ('Sidney', 'Yakebovich', 'syakebovichm@berkeley.edu', 'Male', '2023-06-03', 'Philippines');
INSERT 0 1
test=# SELECT * FROM person;
 id | first_name | last_name  | gender |           email            | date_of_birth | country_of_birth | car_id
----+------------+------------+--------+----------------------------+---------------+------------------+--------
  3 | Adriana    | Matuschek  | Female | amatuschek2@feedburner.com | 1953-10-28    | Sweden           |
  1 | Fernanda   | Beardon    | Female | fernandab@is.gb            | 1953-10-28    | Norway           |      2
  2 | Omar       | Calmore    | Male   |                            | 1921-04-03    | Finland          |      1
  8 | Sidney     | Yakebovich | Male   | syakebovichm@berkeley.edu  | 2023-06-03    | Philippines      |
(4 rows)
```

```shell
test=# ALTER SEQUENCE person_id_seq RESTART WITH 9;
ALTER SEQUENCE
test=# SELECT * FROM person_id_seq;
 last_value | log_cnt | is_called
------------+---------+-----------
          9 |       0 | f
(1 row)
```

## EXTENSIONS 

```shell
test=# SELECT * FROM pg_available_extensions;
         name         | default_version | installed_version |                                comment   
----------------------+-----------------+-------------------+------------------------------------------------------------------------
 adminpack            | 2.1             |                   | administrative functions for PostgreSQL
 amcheck              | 1.3             |                   | functions for verifying relation integrity
 autoinc              | 1.0             |                   | functions for autoincrementing fields
 bloom                | 1.0             |                   | bloom access method - signature file based index
 bool_plperl          | 1.0             |                   | transform between bool and plperl
 bool_plperlu         | 1.0             |                   | transform between bool and plperlu
 btree_gin            | 1.3             |                   | support for indexing common datatypes in GIN
 btree_gist           | 1.7             |                   | support for indexing common datatypes in GiST
 citext               | 1.6             |                   | data type for case-insensitive character strings
 cube                 | 1.5             |                   | data type for multidimensional cubes
 dblink               | 1.2             |                   | connect to other PostgreSQL databases from within a database
 dict_int             | 1.0             |                   | text search dictionary template for integers
 dict_xsyn            | 1.0             |                   | text search dictionary template for extended synonym processing
 dummy_index_am       | 1.0             |                   | dummy_index_am - index access method template
 dummy_seclabel       | 1.0             |                   | Test code for SECURITY LABEL feature
 earthdistance        | 1.1             |                   | calculate great-circle distances on the surface of the Earth
 file_fdw             | 1.0             |                   | foreign-data wrapper for flat file access fuzzystrmatch        | 1.2             |                   | determine similarities and distance between strings
 hstore               | 1.8             |                   | data type for storing sets of (key, value) pairs
 hstore_plperl        | 1.0             |                   | transform between hstore and plperl
 hstore_plperlu       | 1.0             |                   | transform between hstore and plperlu
 hstore_plpython3u    | 1.0             |                   | transform between hstore and plpython3u
 insert_username      | 1.0             |                   | functions for tracking who changed a table
 intagg               | 1.1             |                   | integer aggregator and enumerator (obsolete)
 intarray             | 1.5             |                   | functions, operators, and index support for 1-D arrays of integers
 isn                  | 1.2             |                   | data types for international product numbering standards
 jsonb_plperl         | 1.0             |                   | transform between jsonb and plperl
 jsonb_plperlu        | 1.0             |                   | transform between jsonb and plperlu
 jsonb_plpython3u     | 1.0             |                   | transform between jsonb and plpython3u
test=#
```