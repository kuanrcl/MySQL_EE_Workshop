# Window and CTEs

```

SELECT day, hour, temp
FROM beach
WHERE day BETWEEN '1984-06-01' AND '1984-06-06'
ORDER BY day,hour;


SELECT day, hour, temp,
  AVG(temp) OVER (
    PARTITION BY day
    ORDER BY hour
    ROWS BETWEEN
      UNBOUNDED PRECEDING
      AND
      UNBOUNDED FOLLOWING
  ) AS day_avg,
  MIN(temp) OVER (
    PARTITION BY day
    ORDER BY hour
    ROWS BETWEEN
      UNBOUNDED PRECEDING
      AND
      UNBOUNDED FOLLOWING
  ) AS day_min,
  MAX(temp) OVER (
    PARTITION BY day
    ORDER BY hour
    ROWS BETWEEN
      UNBOUNDED PRECEDING
      AND
      UNBOUNDED FOLLOWING
  ) AS day_max
FROM beach
WHERE day BETWEEN '1984-06-01' AND '1984-06-06'
ORDER BY day,hour;


SELECT day, hour, temp,
  ROUND (AVG(temp) OVER (
    PARTITION BY day
    ORDER BY hour
    ROWS BETWEEN
      UNBOUNDED PRECEDING
      AND
      UNBOUNDED FOLLOWING
  ),1) AS day_avg,
  ROUND (MIN(temp) OVER (
    PARTITION BY day
    ORDER BY hour
    ROWS BETWEEN
      UNBOUNDED PRECEDING
      AND
      UNBOUNDED FOLLOWING
  ),1) AS day_min,
  ROUND (MAX(temp) OVER (
    PARTITION BY day
    ORDER BY hour
    ROWS BETWEEN
      UNBOUNDED PRECEDING
      AND
      UNBOUNDED FOLLOWING
  ),1) AS day_max
FROM beach
WHERE day BETWEEN '1984-06-01' AND '1984-06-06'
ORDER BY day,hour;


SELECT day, hour, temp,
  ROUND (AVG(temp) OVER w1,1) AS day_avg,
  ROUND (MIN(temp) OVER w1,1) AS day_min,
  ROUND (MAX(temp) OVER w1,1) AS day_max
FROM beach
WHERE day BETWEEN '1984-06-01' AND '1984-06-06'
WINDOW
  w1 AS (
    PARTITION BY day
    ORDER BY day,hour
    ROWS BETWEEN
      UNBOUNDED PRECEDING
      AND
      UNBOUNDED FOLLOWING
  )
ORDER BY day,hour;


SELECT day, hour, temp,
  ROUND (AVG(temp) OVER w1,1) AS day_avg,
  ROUND (MIN(temp) OVER w1,1) AS day_min,
  ROUND (MAX(temp) OVER w1,1) AS day_max,
  ROUND (AVG(temp) OVER w2,1) AS all_avg,
  ROUND (MAX(temp) OVER w2,1) AS all_max,
  ROUND (MIN(temp) OVER w2,1) AS all_min
FROM beach
WHERE day BETWEEN '1984-06-01' AND '1984-06-06'
WINDOW
  w1 AS (
    PARTITION BY day
    ORDER BY day,hour
    ROWS BETWEEN
      UNBOUNDED PRECEDING
      AND
      UNBOUNDED FOLLOWING
  ),
  w2 AS (
    ORDER BY day,hour
    ROWS BETWEEN
      UNBOUNDED PRECEDING
      AND
      UNBOUNDED FOLLOWING
  )
ORDER BY day,hour;
```

# Fruitmart table
```

SELECT store, month, fruit, sold,
  LAG(sold)  OVER w1 AS prev,
  LEAD(sold) OVER w1 AS next
FROM fruitmart
WHERE fruit = 'oranges'
  AND store = 'Durham'
WINDOW w1 AS (
  PARTITION BY fruit,store
  ORDER BY store,month
)
ORDER BY fruit,store;

+--------+------------+---------+------+------+------+
| store  | month      | fruit   | sold | prev | next |
+--------+------------+---------+------+------+------+
| Durham | 2016-01-01 | oranges |  244 | NULL |   29 |
| Durham | 2016-02-01 | oranges |   29 |  244 |  153 |
| Durham | 2016-03-01 | oranges |  153 |   29 |  297 |
| Durham | 2016-04-01 | oranges |  297 |  153 |   16 |
| Durham | 2016-05-01 | oranges |   16 |  297 |  930 |
| Durham | 2016-06-01 | oranges |  930 |   16 |  320 |
| Durham | 2016-07-01 | oranges |  320 |  930 |  393 |
| Durham | 2016-08-01 | oranges |  393 |  320 |  292 |
| Durham | 2016-09-01 | oranges |  292 |  393 |  396 |
| Durham | 2016-10-01 | oranges |  396 |  292 |  379 |
| Durham | 2016-11-01 | oranges |  379 |  396 |  432 |
| Durham | 2016-12-01 | oranges |  432 |  379 | NULL |
+--------+------------+---------+------+------+------+
12 rows in set (0.01 sec)


SELECT store, month, fruit, sold,
  LAG(sold)       OVER w1  AS prev,
  LEAD(sold)      OVER w1  AS next,
  ROUND(AVG(sold) OVER w3) AS avg,
  RANK()          OVER w2  AS rnk
FROM fruitmart
WHERE fruit = 'oranges'
  AND store = 'Durham'
WINDOW
  w1 AS (
    PARTITION BY fruit,store
    ORDER BY store,month
  ),
  w2 AS (
    PARTITION BY fruit,store
    ORDER BY sold DESC
  ),
  w3 AS (w2
    ROWS BETWEEN
      UNBOUNDED PRECEDING
      AND
      UNBOUNDED FOLLOWING
  )
ORDER BY fruit,store,rnk;

+--------+------------+---------+------+------+------+------+-----+
| store  | month      | fruit   | sold | prev | next | avg  | rnk |
+--------+------------+---------+------+------+------+------+-----+
| Durham | 2016-06-01 | oranges |  930 |   16 |  320 |  323 |   1 |
| Durham | 2016-12-01 | oranges |  432 |  379 | NULL |  323 |   2 |
| Durham | 2016-10-01 | oranges |  396 |  292 |  379 |  323 |   3 |
| Durham | 2016-08-01 | oranges |  393 |  320 |  292 |  323 |   4 |
| Durham | 2016-11-01 | oranges |  379 |  396 |  432 |  323 |   5 |
| Durham | 2016-07-01 | oranges |  320 |  930 |  393 |  323 |   6 |
| Durham | 2016-04-01 | oranges |  297 |  153 |   16 |  323 |   7 |
| Durham | 2016-09-01 | oranges |  292 |  393 |  396 |  323 |   8 |
| Durham | 2016-01-01 | oranges |  244 | NULL |   29 |  323 |   9 |
| Durham | 2016-03-01 | oranges |  153 |   29 |  297 |  323 |  10 |
| Durham | 2016-02-01 | oranges |   29 |  244 |  153 |  323 |  11 |
| Durham | 2016-05-01 | oranges |   16 |  297 |  930 |  323 |  12 |
+--------+------------+---------+------+------+------+------+-----+
12 rows in set (0.00 sec)


SELECT store, month, fruit, sold,
  LAG(sold)       OVER w1  AS prev,
  LEAD(sold)      OVER w1  AS next,
  ROUND(AVG(sold) OVER w4) AS s_avg,
  ROUND(AVG(sold) OVER w3) AS a_avg,
  RANK()          OVER w2  AS rnk
FROM fruitmart
WHERE fruit = 'oranges'
WINDOW
  w1 AS (
    PARTITION BY fruit,store
    ORDER BY store,month
  ),
  w2 AS (
    PARTITION BY fruit
    ORDER BY sold DESC
  ),
  w3 AS (w2
    ROWS BETWEEN
      UNBOUNDED PRECEDING
      AND
      UNBOUNDED FOLLOWING
  ),
  w4 AS (
    PARTITION BY fruit,store
    ORDER BY sold DESC
    ROWS BETWEEN
      UNBOUNDED PRECEDING
      AND
      UNBOUNDED FOLLOWING
  )
ORDER BY fruit,rnk;

+------------+------------+---------+------+------+------+-------+-------+-----+
| store      | month      | fruit   | sold | prev | next | s_avg | a_avg | rnk |
+------------+------------+---------+------+------+------+-------+-------+-----+
| Greensboro | 2016-04-01 | oranges |  998 |  613 |    4 |   443 |   498 |   1 |
| Hickory    | 2016-04-01 | oranges |  968 |  749 |  452 |   661 |   498 |   2 |
| Gastonia   | 2016-09-01 | oranges |  967 |   32 |  226 |   376 |   498 |   3 |
| Kinston    | 2016-05-01 | oranges |  967 |  100 |  789 |   562 |   498 |   3 |
| Kinston    | 2016-12-01 | oranges |  949 |  206 | NULL |   562 |   498 |   5 |
| Wilmington | 2016-07-01 | oranges |  936 |  812 |  306 |   635 |   498 |   6 |
| Asheville  | 2016-04-01 | oranges |  932 |  285 |  456 |   613 |   498 |   7 |
| Durham     | 2016-06-01 | oranges |  930 |   16 |  320 |   323 |   498 |   8 |
| Hickory    | 2016-12-01 | oranges |  930 |  874 | NULL |   661 |   498 |   8 |
| Charlotte  | 2016-05-01 | oranges |  917 |  157 |  251 |   510 |   498 |  10 |
| Hickory    | 2016-01-01 | oranges |  917 | NULL |  540 |   661 |   498 |  10 |
| Hickory    | 2016-10-01 | oranges |  917 |  171 |  874 |   661 |   498 |  10 |
| Wilmington | 2016-09-01 | oranges |  912 |  306 |  857 |   635 |   498 |  13 |
| Gastonia   | 2016-04-01 | oranges |  891 |  440 |   20 |   376 |   498 |  14 |
| Raleigh    | 2016-11-01 | oranges |  889 |   31 |  538 |   434 |   498 |  15 |
| Kinston    | 2016-08-01 | oranges |  883 |  165 |  674 |   562 |   498 |  16 |
| Greensboro | 2016-09-01 | oranges |  880 |  204 |  152 |   443 |   498 |  17 |
| Hickory    | 2016-11-01 | oranges |  874 |  917 |  930 |   661 |   498 |  18 |
| Wilmington | 2016-11-01 | oranges |  859 |  857 |  642 |   635 |   498 |  19 |
| Wilmington | 2016-10-01 | oranges |  857 |  912 |  859 |   635 |   498 |  20 |
| Charlotte  | 2016-02-01 | oranges |  846 |  735 |  584 |   510 |   498 |  21 |
| Asheville  | 2016-11-01 | oranges |  830 |  756 |  129 |   613 |   498 |  22 |
| Wilmington | 2016-06-01 | oranges |  812 |  670 |  936 |   635 |   498 |  23 |
| Kinston    | 2016-06-01 | oranges |  789 |  967 |  165 |   562 |   498 |  24 |
| Asheville  | 2016-07-01 | oranges |  776 |  437 |  747 |   613 |   498 |  25 |
| Asheville  | 2016-01-01 | oranges |  759 | NULL |  710 |   613 |   498 |  26 |
| Asheville  | 2016-10-01 | oranges |  756 |  534 |  830 |   613 |   498 |  27 |
| Greensboro | 2016-11-01 | oranges |  755 |  152 |  445 |   443 |   498 |  28 |
| Charlotte  | 2016-09-01 | oranges |  752 |  324 |  611 |   510 |   498 |  29 |
| Hickory    | 2016-03-01 | oranges |  749 |  540 |  968 |   661 |   498 |  30 |
| Asheville  | 2016-08-01 | oranges |  747 |  776 |  534 |   613 |   498 |  31 |
| Goldsboro  | 2016-05-01 | oranges |  746 |  301 |  258 |   427 |   498 |  32 |
| Raleigh    | 2016-02-01 | oranges |  744 |  187 |  739 |   434 |   498 |  33 |
| Kinston    | 2016-02-01 | oranges |  742 |  498 |  431 |   562 |   498 |  34 |
| Raleigh    | 2016-03-01 | oranges |  739 |  744 |  591 |   434 |   498 |  35 |
| Gastonia   | 2016-07-01 | oranges |  738 |   68 |   32 |   376 |   498 |  36 |
| Charlotte  | 2016-01-01 | oranges |  735 | NULL |  846 |   510 |   498 |  37 |
| Asheville  | 2016-02-01 | oranges |  710 |  759 |  285 |   613 |   498 |  38 |
| Greensboro | 2016-01-01 | oranges |  692 | NULL |  170 |   443 |   498 |  39 |
| Raleigh    | 2016-07-01 | oranges |  692 |   62 |  180 |   434 |   498 |  39 |
| Kinston    | 2016-09-01 | oranges |  674 |  883 |  345 |   562 |   498 |  41 |
| Wilmington | 2016-02-01 | oranges |  673 |  604 |   33 |   635 |   498 |  42 |
| Wilmington | 2016-05-01 | oranges |  670 |  319 |  812 |   635 |   498 |  43 |
| Goldsboro  | 2016-03-01 | oranges |  667 |   56 |  301 |   427 |   498 |  44 |
| Gastonia   | 2016-12-01 | oranges |  657 |  230 | NULL |   376 |   498 |  45 |
| Goldsboro  | 2016-09-01 | oranges |  643 |  565 |  268 |   427 |   498 |  46 |
| Wilmington | 2016-12-01 | oranges |  642 |  859 | NULL |   635 |   498 |  47 |
| Greensboro | 2016-03-01 | oranges |  613 |  170 |  998 |   443 |   498 |  48 |
| Charlotte  | 2016-10-01 | oranges |  611 |  752 |  175 |   510 |   498 |  49 |
| Wilmington | 2016-01-01 | oranges |  604 | NULL |  673 |   635 |   498 |  50 |
| Raleigh    | 2016-04-01 | oranges |  591 |  739 |  264 |   434 |   498 |  51 |
| Hickory    | 2016-07-01 | oranges |  587 |  266 |  562 |   661 |   498 |  52 |
| Charlotte  | 2016-03-01 | oranges |  584 |  846 |  157 |   510 |   498 |  53 |
| Goldsboro  | 2016-08-01 | oranges |  565 |  541 |  643 |   427 |   498 |  54 |
| Hickory    | 2016-08-01 | oranges |  562 |  587 |  171 |   661 |   498 |  55 |
| Goldsboro  | 2016-01-01 | oranges |  561 | NULL |   56 |   427 |   498 |  56 |
| Goldsboro  | 2016-07-01 | oranges |  541 |  258 |  565 |   427 |   498 |  57 |
| Hickory    | 2016-02-01 | oranges |  540 |  917 |  749 |   661 |   498 |  58 |
| Raleigh    | 2016-12-01 | oranges |  538 |  889 | NULL |   434 |   498 |  59 |
| Asheville  | 2016-09-01 | oranges |  534 |  747 |  756 |   613 |   498 |  60 |
| Kinston    | 2016-01-01 | oranges |  498 | NULL |  742 |   562 |   498 |  61 |
| Charlotte  | 2016-12-01 | oranges |  482 |  175 | NULL |   510 |   498 |  62 |
| Asheville  | 2016-05-01 | oranges |  456 |  932 |  437 |   613 |   498 |  63 |
| Hickory    | 2016-05-01 | oranges |  452 |  968 |  266 |   661 |   498 |  64 |
| Greensboro | 2016-12-01 | oranges |  445 |  755 | NULL |   443 |   498 |  65 |
| Gastonia   | 2016-03-01 | oranges |  440 |  144 |  891 |   376 |   498 |  66 |
| Asheville  | 2016-06-01 | oranges |  437 |  456 |  776 |   613 |   498 |  67 |
| Durham     | 2016-12-01 | oranges |  432 |  379 | NULL |   323 |   498 |  68 |
| Kinston    | 2016-03-01 | oranges |  431 |  742 |  100 |   562 |   498 |  69 |
| Durham     | 2016-10-01 | oranges |  396 |  292 |  379 |   323 |   498 |  70 |
| Durham     | 2016-08-01 | oranges |  393 |  320 |  292 |   323 |   498 |  71 |
| Goldsboro  | 2016-12-01 | oranges |  382 |  134 | NULL |   427 |   498 |  72 |
| Durham     | 2016-11-01 | oranges |  379 |  396 |  432 |   323 |   498 |  73 |
| Greensboro | 2016-06-01 | oranges |  359 |    4 |   41 |   443 |   498 |  74 |
| Kinston    | 2016-10-01 | oranges |  345 |  674 |  206 |   562 |   498 |  75 |
| Charlotte  | 2016-08-01 | oranges |  324 |  287 |  752 |   510 |   498 |  76 |
| Durham     | 2016-07-01 | oranges |  320 |  930 |  393 |   323 |   498 |  77 |
| Wilmington | 2016-04-01 | oranges |  319 |   33 |  670 |   635 |   498 |  78 |
| Wilmington | 2016-08-01 | oranges |  306 |  936 |  912 |   635 |   498 |  79 |
| Goldsboro  | 2016-04-01 | oranges |  301 |  667 |  746 |   427 |   498 |  80 |
| Durham     | 2016-04-01 | oranges |  297 |  153 |   16 |   323 |   498 |  81 |
| Raleigh    | 2016-09-01 | oranges |  296 |  180 |   31 |   434 |   498 |  82 |
| Durham     | 2016-09-01 | oranges |  292 |  393 |  396 |   323 |   498 |  83 |
| Charlotte  | 2016-07-01 | oranges |  287 |  251 |  324 |   510 |   498 |  84 |
| Asheville  | 2016-03-01 | oranges |  285 |  710 |  932 |   613 |   498 |  85 |
| Goldsboro  | 2016-10-01 | oranges |  268 |  643 |  134 |   427 |   498 |  86 |
| Hickory    | 2016-06-01 | oranges |  266 |  452 |  587 |   661 |   498 |  87 |
| Raleigh    | 2016-05-01 | oranges |  264 |  591 |   62 |   434 |   498 |  88 |
| Goldsboro  | 2016-06-01 | oranges |  258 |  746 |  541 |   427 |   498 |  89 |
| Charlotte  | 2016-06-01 | oranges |  251 |  917 |  287 |   510 |   498 |  90 |
| Durham     | 2016-01-01 | oranges |  244 | NULL |   29 |   323 |   498 |  91 |
| Gastonia   | 2016-11-01 | oranges |  230 |  226 |  657 |   376 |   498 |  92 |
| Gastonia   | 2016-10-01 | oranges |  226 |  967 |  230 |   376 |   498 |  93 |
| Kinston    | 2016-11-01 | oranges |  206 |  345 |  949 |   562 |   498 |  94 |
| Greensboro | 2016-08-01 | oranges |  204 |   41 |  880 |   443 |   498 |  95 |
| Raleigh    | 2016-01-01 | oranges |  187 | NULL |  744 |   434 |   498 |  96 |
| Raleigh    | 2016-08-01 | oranges |  180 |  692 |  296 |   434 |   498 |  97 |
| Charlotte  | 2016-11-01 | oranges |  175 |  611 |  482 |   510 |   498 |  98 |
| Hickory    | 2016-09-01 | oranges |  171 |  562 |  917 |   661 |   498 |  99 |
| Greensboro | 2016-02-01 | oranges |  170 |  692 |  613 |   443 |   498 | 100 |
| Kinston    | 2016-07-01 | oranges |  165 |  789 |  883 |   562 |   498 | 101 |
| Charlotte  | 2016-04-01 | oranges |  157 |  584 |  917 |   510 |   498 | 102 |
| Durham     | 2016-03-01 | oranges |  153 |   29 |  297 |   323 |   498 | 103 |
| Greensboro | 2016-10-01 | oranges |  152 |  880 |  755 |   443 |   498 | 104 |
| Gastonia   | 2016-02-01 | oranges |  144 |   99 |  440 |   376 |   498 | 105 |
| Goldsboro  | 2016-11-01 | oranges |  134 |  268 |  382 |   427 |   498 | 106 |
| Asheville  | 2016-12-01 | oranges |  129 |  830 | NULL |   613 |   498 | 107 |
| Kinston    | 2016-04-01 | oranges |  100 |  431 |  967 |   562 |   498 | 108 |
| Gastonia   | 2016-01-01 | oranges |   99 | NULL |  144 |   376 |   498 | 109 |
| Gastonia   | 2016-06-01 | oranges |   68 |   20 |  738 |   376 |   498 | 110 |
| Raleigh    | 2016-06-01 | oranges |   62 |  264 |  692 |   434 |   498 | 111 |
| Goldsboro  | 2016-02-01 | oranges |   56 |  561 |  667 |   427 |   498 | 112 |
| Greensboro | 2016-07-01 | oranges |   41 |  359 |  204 |   443 |   498 | 113 |
| Wilmington | 2016-03-01 | oranges |   33 |  673 |  319 |   635 |   498 | 114 |
| Gastonia   | 2016-08-01 | oranges |   32 |  738 |  967 |   376 |   498 | 115 |
| Raleigh    | 2016-10-01 | oranges |   31 |  296 |  889 |   434 |   498 | 116 |
| Durham     | 2016-02-01 | oranges |   29 |  244 |  153 |   323 |   498 | 117 |
| Gastonia   | 2016-05-01 | oranges |   20 |  891 |   68 |   376 |   498 | 118 |
| Durham     | 2016-05-01 | oranges |   16 |  297 |  930 |   323 |   498 | 119 |
| Greensboro | 2016-05-01 | oranges |    4 |  998 |  359 |   443 |   498 | 120 |
+------------+------------+---------+------+------+------+-------+-------+-----+
120 rows in set (0.02 sec)

```



