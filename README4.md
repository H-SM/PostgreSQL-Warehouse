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