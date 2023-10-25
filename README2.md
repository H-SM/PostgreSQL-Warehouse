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

<!-- ## NULLIF -->


```shell

```
```shell

```
```shell

```