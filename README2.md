<p align="center">
    <b><i>-------------- Continued from README.md --------------</i></b>
</p>

## Basics of Arthmetric Operators

### ROUND 

```shell
# A 10 percent discount 

test=# SELECT id, make, model, price, price * .10 FROM car;
  id  |     make      |        model         |   price   |  ?column?
------+---------------+----------------------+-----------+------------
    1 | Jeep          | Grand Cherokee       | 434177.29 | 43417.7290
    2 | GMC           | Vandura G3500        | 476903.06 | 47690.3060
    3 | Subaru        | Impreza              | 409093.64 | 40909.3640
    4 | MINI          | Cooper               | 232981.88 | 23298.1880
    5 | Nissan        | Quest                | 870477.44 | 87047.7440
    6 | Lexus         | GS                   | 321818.23 | 32181.8230
    7 | Lincoln       | MKT                  | 600861.19 | 60086.1190
    8 | Chevrolet     | Suburban 2500        | 630196.90 | 63019.6900
    9 | Pontiac       | Bonneville           | 709936.89 | 70993.6890
   10 | Cadillac      | STS                  | 535423.51 | 53542.3510
   11 | Dodge         | Daytona              | 699009.95 | 69900.9950
   12 | Ford          | Mustang              | 983209.15 | 98320.9150
   13 | Acura         | Legend               | 180805.18 | 18080.5180
   14 | Dodge         | Ram Van 2500         | 895435.81 | 89543.5810
   15 | Saab          | 900                  | 678017.10 | 67801.7100
   16 | Mitsubishi    | GTO                  | 290617.49 | 29061.7490
   17 | BMW           | 1 Series             | 348247.55 | 34824.7550
```

```shell
# Using ROUND to round up the values for our discount
test=# SELECT id, make, model, price, ROUND(price * .10) FROM car;
  id  |     make      |        model         |   price   | round
------+---------------+----------------------+-----------+-------
    1 | Jeep          | Grand Cherokee       | 434177.29 | 43418
    2 | GMC           | Vandura G3500        | 476903.06 | 47690
    3 | Subaru        | Impreza              | 409093.64 | 40909
    4 | MINI          | Cooper               | 232981.88 | 23298
    5 | Nissan        | Quest                | 870477.44 | 87048
    6 | Lexus         | GS                   | 321818.23 | 32182
    7 | Lincoln       | MKT                  | 600861.19 | 60086
    8 | Chevrolet     | Suburban 2500        | 630196.90 | 63020
    9 | Pontiac       | Bonneville           | 709936.89 | 70994
   10 | Cadillac      | STS                  | 535423.51 | 53542
   11 | Dodge         | Daytona              | 699009.95 | 69901
   12 | Ford          | Mustang              | 983209.15 | 98321
   13 | Acura         | Legend               | 180805.18 | 18081
   14 | Dodge         | Ram Van 2500         | 895435.81 | 89544
   15 | Saab          | 900                  | 678017.10 | 67802

test=# SELECT id, make, model, price, ROUND(price * .10,2) FROM car;
  id  |     make      |        model         |   price   |  round
------+---------------+----------------------+-----------+----------
    1 | Jeep          | Grand Cherokee       | 434177.29 | 43417.73
    2 | GMC           | Vandura G3500        | 476903.06 | 47690.31
    3 | Subaru        | Impreza              | 409093.64 | 40909.36
    4 | MINI          | Cooper               | 232981.88 | 23298.19
    5 | Nissan        | Quest                | 870477.44 | 87047.74
    6 | Lexus         | GS                   | 321818.23 | 32181.82
    7 | Lincoln       | MKT                  | 600861.19 | 60086.12
    8 | Chevrolet     | Suburban 2500        | 630196.90 | 63019.69
    9 | Pontiac       | Bonneville           | 709936.89 | 70993.69
   10 | Cadillac      | STS                  | 535423.51 | 53542.35
   11 | Dodge         | Daytona              | 699009.95 | 69901.00
   12 | Ford          | Mustang              | 983209.15 | 98320.92
   13 | Acura         | Legend               | 180805.18 | 18080.52
   14 | Dodge         | Ram Van 2500         | 895435.81 | 89543.58
   15 | Saab          | 900                  | 678017.10 | 67801.71
   16 | Mitsubishi    | GTO                  | 290617.49 | 29061.75
   17 | BMW           | 1 Series             | 348247.55 | 34824.76
```

```shell
test=# SELECT id, make, model, price, ROUND(price * .10,2),ROUND(price -(price * .10), 2) FROM car;
  id  |     make      |        model         |   price   |  round   |   round
------+---------------+----------------------+-----------+----------+-----------
    1 | Jeep          | Grand Cherokee       | 434177.29 | 43417.73 | 390759.56
    2 | GMC           | Vandura G3500        | 476903.06 | 47690.31 | 429212.75
    3 | Subaru        | Impreza              | 409093.64 | 40909.36 | 368184.28
    4 | MINI          | Cooper               | 232981.88 | 23298.19 | 209683.69
    5 | Nissan        | Quest                | 870477.44 | 87047.74 | 783429.70
    6 | Lexus         | GS                   | 321818.23 | 32181.82 | 289636.41
    7 | Lincoln       | MKT                  | 600861.19 | 60086.12 | 540775.07
    8 | Chevrolet     | Suburban 2500        | 630196.90 | 63019.69 | 567177.21
    9 | Pontiac       | Bonneville           | 709936.89 | 70993.69 | 638943.20
   10 | Cadillac      | STS                  | 535423.51 | 53542.35 | 481881.16
   11 | Dodge         | Daytona              | 699009.95 | 69901.00 | 629108.96
   12 | Ford          | Mustang              | 983209.15 | 98320.92 | 884888.24
   13 | Acura         | Legend               | 180805.18 | 18080.52 | 162724.66
   14 | Dodge         | Ram Van 2500         | 895435.81 | 89543.58 | 805892.23
   15 | Saab          | 900                  | 678017.10 | 67801.71 | 610215.39
   16 | Mitsubishi    | GTO                  | 290617.49 | 29061.75 | 261555.74
   17 | BMW           | 1 Series             | 348247.55 | 34824.76 | 313422.80
```

## Alias (AS) 

```shell
test=# SELECT id, make, model, price AS original_price, ROUND(price * .10,2) AS ten_percent ,ROUND(price -(price * .10), 2) AS discount_after_10_percent FROM car;'
  id  |     make      |        model         | original_price | ten_percent | discount_after_10_percent ------+---------------+----------------------+----------------+-------------+---------------------------    1 | Jeep          | Grand Cherokee       |      434177.29 |    43417.73 |                 390759.56
    2 | GMC           | Vandura G3500        |      476903.06 |    47690.31 |                 429212.75
    3 | Subaru        | Impreza              |      409093.64 |    40909.36 |                 368184.28
    4 | MINI          | Cooper               |      232981.88 |    23298.19 |                 209683.69
    5 | Nissan        | Quest                |      870477.44 |    87047.74 |                 783429.70
    6 | Lexus         | GS                   |      321818.23 |    32181.82 |                 289636.41
    7 | Lincoln       | MKT                  |      600861.19 |    60086.12 |                 540775.07
    8 | Chevrolet     | Suburban 2500        |      630196.90 |    63019.69 |                 567177.21
    9 | Pontiac       | Bonneville           |      709936.89 |    70993.69 |                 638943.20
   10 | Cadillac      | STS                  |      535423.51 |    53542.35 |                 481881.16
   11 | Dodge         | Daytona              |      699009.95 |    69901.00 |                 629108.96
   12 | Ford          | Mustang              |      983209.15 |    98320.92 |                 884888.24
   13 | Acura         | Legend               |      180805.18 |    18080.52 |                 162724.66
   14 | Dodge         | Ram Van 2500         |      895435.81 |    89543.58 |                 805892.23
   15 | Saab          | 900                  |      678017.10 |    67801.71 |                 610215.39
   16 | Mitsubishi    | GTO                  |      290617.49 |    29061.75 |                 261555.74
   17 | BMW           | 1 Series             |      348247.55 |    34824.76 |                 313422.80
```

## Coalesce 

```shell
# handling nulls (have a default value in place of NULL)
test=# SELECT first_name,last_name,COALESCE(email,'-Email not provided-') AS email FROM person;
 first_name  |     last_name      |                email
-------------+--------------------+-------------------------------------
 Ab          | Bowater            | abowater0@toplist.cz
 Mirella     | Hane               | mhane1@wikipedia.org
 Rosanne     | Frantsev           | -Email not provided-
 Loleta      | Pattesall          | lpattesall3@vistaprint.com
 Thurston    | Weatherby          | tweatherby4@senate.gov
 Dev         | De Stoop           | ddestoop5@last.fm
 Ambrosi     | Pepper             | apepper6@google.co.jp
 Wynnie      | Bourne             | wbourne7@boston.com
 Penny       | Hutt               | phutt8@twitter.com
 Tanitansy   | Gann               | -Email not provided-
 Reba        | Borsnall           | -Email not provided-
 Frank       | Staniland          | fstanilandb@nature.com
 Tamiko      | Minty              | -Email not provided-
 Arman       | Gartenfeld         | agartenfeldd@arstechnica.com
 Giselle     | Titchmarsh         | gtitchmarshe@netscape.com
 Raddy       | Parkhouse          | rparkhousef@illinois.edu
 Amandie     | Yurinov            | ayurinovg@cbslocal.com
 Neil        | Ritchings          | nritchingsh@goodreads.com
 Wyndham     | Bugge              | -Email not provided-
```

## NULL-IF

```shell
# returns the first argument, if not equal to the second argument
test=# SELECT 10 / NULLIF(2,10);
 ?column?
----------
        5
(1 row)

test=# SELECT 10 / NULLIF(10,10);
 ?column?
----------

(1 row)
```

```shell
test=# SELECT COALESCE(10 / NULLIF(0,0), 0);
 coalesce
----------
        0
(1 row)

test=# SELECT COALESCE(10 / NULLIF(0,1), 0);
ERROR:  division by zero
```

## TimeStamps and Dates

```shell
test=# SELECT NOW();
               now
----------------------------------
 2023-10-27 16:27:47.806372+05:30
(1 row)

test=# SELECT NOW()::DATE;
    now
------------
 2023-10-27
(1 row)


test=# SELECT NOW()::TIME;
       now
-----------------
 16:28:22.657357
(1 row)
```

```shell
# Addition and subtraction in the dates and timestamps
test=# SELECT NOW() - INTERVAL '1 YEAR';
             ?column?
----------------------------------
 2022-10-27 16:29:40.435401+05:30
(1 row)


test=# SELECT NOW() - INTERVAL '10 YEARS';
             ?column?
----------------------------------
 2013-10-27 16:29:51.462778+05:30
(1 row)


test=# SELECT NOW() - INTERVAL '10 MONTHS';
             ?column?
----------------------------------
 2022-12-27 16:30:05.553674+05:30
(1 row)


test=# SELECT NOW() - INTERVAL '10 DAYS';
             ?column?
----------------------------------
 2023-10-17 16:30:15.177061+05:30
(1 row)


test=# SELECT NOW() + INTERVAL '10 DAYS';
             ?column?
----------------------------------
 2023-11-06 16:30:23.887727+05:30
(1 row)


test=# SELECT NOW()::DATE + INTERVAL '10 DAYS';
      ?column?
---------------------
 2023-11-06 00:00:00
(1 row)


test=# SELECT (NOW()::DATE + INTERVAL '10 DAYS')::DATE;
    date
------------
 2023-11-06
(1 row)
```

## EXTRACTING DATES and ACTUAL TIMESTAMP
```shell
test=# SELECT EXTRACT (YEAR FROM now());
 extract
---------
    2023
(1 row)


test=# SELECT EXTRACT (MONTH FROM now());
 extract
---------
      10
(1 row)

test=# SELECT EXTRACT (DAY FROM now());
 extract
---------
      27
(1 row)


test=# SELECT EXTRACT (DOW FROM now());
# date of the week
 extract
---------
       5
(1 row)
```
```shell
test=# SELECT first_name, last_name, gender, country_of_birth, date_of_birth, AGE(NOW(), date_of_birth) AS Age FROM person;
 first_name  |     last_name      |   gender    |     country_of_birth     | date_of_birth |               age
-------------+--------------------+-------------+--------------------------+---------------+---------------------------------
 Ab          | Bowater            | Male        | Ukraine                  | 2022-11-04    | 11 mons 23 days 16:37:31.320109
 Mirella     | Hane               | Female      | Thailand                 | 2023-07-12    | 3 mons 15 days 16:37:31.320109
 Rosanne     | Frantsev           | Female      | Zambia                   | 2022-11-18    | 11 mons 9 days 16:37:31.320109
 Loleta      | Pattesall          | Female      | Sweden                   | 2023-03-27    | 7 mons 16:37:31.320109
 Thurston    | Weatherby          | Male        | Egypt                    | 2023-08-03    | 2 mons 24 days 16:37:31.320109
 Dev         | De Stoop           | Polygender  | China                    | 2023-04-10    | 6 mons 17 days 16:37:31.320109
 Ambrosi     | Pepper             | Male        | South Korea              | 2022-12-10    | 10 mons 17 days 16:37:31.320109
 Wynnie      | Bourne             | Female      | China                    | 2023-03-10    | 7 mons 17 days 16:37:31.320109
 Penny       | Hutt               | Male        | Peru                     | 2023-05-18    | 5 mons 9 days 16:37:31.320109
```
<p align="center">
    <b><i>-------------- Continued in README3.md --------------</i></b>
</p>
