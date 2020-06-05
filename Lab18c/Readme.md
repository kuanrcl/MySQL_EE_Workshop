# Manage MySQL with Ansible

## Install Ansible

Make sure you have EPEL repository enabled
```
cat /etc/yum.repos.d/epel-yum-ol7.repo
[ol7_epel]
name=Oracle Linux $releasever EPEL ($basearch)
baseurl=http://yum.oracle.com/repo/OracleLinux/OL7/developer_EPEL/$basearch/
gpgkey=file:///etc/pki/rpm-gpg/RPM-GPG-KEY-oracle
gpgcheck=1
enabled=1

sudo yum install ansible
```


Window and CTEs
## CTEs

```

WITH emp_raleigh AS (
  SELECT * FROM employees
    WHERE office='Raleigh'
)
SELECT * FROM emp_raleigh
  WHERE title != 'salesperson'
  ORDER BY title;
+-----+-----------------+------------+---------+
| id  | name            | title      | office  |
+-----+-----------------+------------+---------+
|  73 | Mark Hamilton   | dba        | Raleigh |
|  77 | Nancy Porter    | dba        | Raleigh |
| 135 | Pauline Neal    | dba        | Raleigh |
|  68 | Edmund Hines    | manager    | Raleigh |
|  28 | Marc Greene     | programmer | Raleigh |
|  96 | Mary Walker     | programmer | Raleigh |
| 100 | Freida Duchesne | programmer | Raleigh |
+-----+-----------------+------------+---------+
7 rows in set (0.00 sec)


SELECT * FROM (
  SELECT * FROM employees
    WHERE office='Raleigh'
) AS emp_raleigh
WHERE title != 'salesperson'
  ORDER BY title;
+-----+-----------------+------------+---------+
| id  | name            | title      | office  |
+-----+-----------------+------------+---------+
|  73 | Mark Hamilton   | dba        | Raleigh |
|  77 | Nancy Porter    | dba        | Raleigh |
| 135 | Pauline Neal    | dba        | Raleigh |
|  68 | Edmund Hines    | manager    | Raleigh |
|  28 | Marc Greene     | programmer | Raleigh |
|  96 | Mary Walker     | programmer | Raleigh |
| 100 | Freida Duchesne | programmer | Raleigh |
+-----+-----------------+------------+---------+
7 rows in set (0.00 sec)


WITH emp_raleigh AS (
  SELECT * FROM employees
    WHERE office='Raleigh'
),
emp_raleigh_dbas AS (
  SELECT * from emp_raleigh
    WHERE title='dba'
)
SELECT * FROM emp_raleigh_dbas;
+-----+---------------+-------+---------+
| id  | name          | title | office  |
+-----+---------------+-------+---------+
|  73 | Mark Hamilton | dba   | Raleigh |
|  77 | Nancy Porter  | dba   | Raleigh |
| 135 | Pauline Neal  | dba   | Raleigh |
+-----+---------------+-------+---------+
3 rows in set (0.00 sec)


WITH dbas AS (
  SELECT * FROM employees
    WHERE title='dba'
)
SELECT * FROM dbas A1
  WHERE NOT EXISTS (
    SELECT 1 FROM dbas A2
      WHERE
        A2.office=A1.office
      AND
        A2.name <> A1.name
  );
+----+---------------+-------+---------+
| id | name          | title | office  |
+----+---------------+-------+---------+
|  6 | Toby Lucas    | dba   | Wichita |
| 16 | Susan Charles | dba   | Nauvoo  |
+----+---------------+-------+---------+
2 rows in set (0.01 sec)


SELECT
  salesperson_id,
  YEAR(commission_date) AS year,
  SUM(commission_amount) AS total
FROM
  commissions
GROUP BY
  salesperson_id, year;

+----------------+------+----------+
| salesperson_id | year | total    |
+----------------+------+----------+
|             19 | 2016 |  2449.01 |
|             38 | 2016 |  5277.46 |
|             42 | 2016 |  7436.57 |
|             74 | 2016 |  8302.50 |
|             79 | 2016 | 12216.46 |
|            106 | 2016 |  6041.18 |
|             44 | 2016 | 12870.31 |
|             69 | 2016 |  8159.49 |
|            104 | 2016 |  9981.57 |
|             49 | 2016 |  9485.64 |
|             53 | 2016 |  5310.08 |
|             64 | 2016 |  6737.23 |
|            109 | 2016 |  7347.17 |
|             18 | 2016 |  5146.93 |
|             27 | 2016 |  6729.25 |
|             29 | 2016 |  8072.71 |
|             33 | 2016 |  8475.14 |
|             34 | 2016 | 10736.72 |
|             95 | 2016 |  5471.26 |
|            117 | 2016 | 10888.29 |
|            129 | 2016 |  8446.11 |
|            136 | 2016 | 12028.57 |
|             40 | 2016 | 10377.72 |
|             66 | 2016 |  9799.21 |
|             78 | 2016 |  9266.08 |
|             89 | 2016 | 13186.68 |
|            119 | 2016 |  3917.15 |
|             10 | 2016 |  9433.58 |
|            116 | 2016 | 10873.30 |
|             31 | 2016 |  7161.30 |
|             56 | 2016 |  7465.51 |
|             75 | 2016 |  8922.70 |
|            118 | 2016 |  7289.67 |
|            126 | 2016 |  7486.75 |
|             47 | 2016 | 12417.51 |
|            105 | 2016 |  8387.52 |
|            114 | 2016 | 11530.40 |
|            138 | 2016 |  9666.24 |
|             32 | 2016 |  7161.17 |
|            132 | 2016 |  6277.41 |
|             41 | 2016 |  8366.91 |
|            103 | 2016 |  6057.86 |
|            128 | 2016 |  7746.91 |
|            137 | 2016 |  2558.30 |
|              3 | 2016 |  2249.93 |
|            131 | 2016 |  7774.82 |
|             61 | 2016 |  7889.91 |
|             98 | 2016 |  8651.65 |
|            139 | 2016 |  5793.49 |
|              8 | 2016 |  4514.73 |
|             63 | 2016 |  4629.89 |
|             20 | 2016 |  6093.17 |
|            121 | 2016 |  8327.77 |
|              7 | 2016 |  1088.32 |
|             71 | 2016 |  3985.55 |
|             41 | 2017 | 10219.33 |
|             47 | 2017 |  7724.59 |
|             53 | 2017 | 10967.64 |
|             74 | 2017 |  6218.34 |
|            116 | 2017 | 13856.74 |
|            128 | 2017 | 12253.44 |
|             27 | 2017 |  5766.24 |
|             34 | 2017 |  9928.69 |
|             75 | 2017 |  7426.66 |
|             79 | 2017 |  9055.54 |
|            131 | 2017 | 11147.38 |
|            139 | 2017 |  6591.31 |
|             19 | 2017 |  5193.44 |
|             42 | 2017 |  6456.68 |
|             71 | 2017 |  8443.95 |
|             33 | 2017 |  6672.30 |
|             38 | 2017 | 12010.91 |
|             61 | 2017 |  7740.47 |
|             64 | 2017 |  6468.15 |
|             10 | 2017 |  8479.05 |
|             40 | 2017 |  8539.44 |
|             56 | 2017 |  7289.49 |
|             63 | 2017 |  6906.14 |
|             69 | 2017 | 12570.95 |
|            126 | 2017 |  7428.74 |
|             29 | 2017 |  5322.36 |
|             49 | 2017 |  8679.84 |
|            106 | 2017 |  9387.77 |
|            118 | 2017 |  9265.96 |
|            138 | 2017 |  7250.48 |
|             44 | 2017 |  8595.13 |
|             89 | 2017 |  8177.24 |
|             31 | 2017 |  8473.84 |
|             95 | 2017 |  6813.64 |
|            119 | 2017 |  4102.66 |
|             32 | 2017 |  4023.82 |
|            104 | 2017 |  7655.84 |
|             98 | 2017 |  7272.29 |
|            132 | 2017 | 10066.97 |
|            109 | 2017 |  5791.50 |
|            117 | 2017 |  8226.05 |
|              7 | 2017 |  3197.25 |
|             66 | 2017 |  9433.56 |
|             18 | 2017 |  6680.49 |
|             78 | 2017 | 11423.48 |
|            114 | 2017 | 10651.10 |
|            103 | 2017 |  6294.81 |
|            121 | 2017 | 10979.07 |
|              3 | 2017 |  3449.67 |
|            129 | 2017 |  5280.57 |
|            136 | 2017 |  8382.18 |
|            105 | 2017 |  9782.69 |
|             20 | 2017 |  8260.88 |
|              8 | 2017 |  5178.19 |
|            137 | 2017 |   698.93 |
+----------------+------+----------+
110 rows in set (0.03 sec)


WITH commissions_year AS (
  SELECT
    salesperson_id,
    YEAR(commission_date) AS year,
    SUM(commission_amount) AS total
  FROM
    commissions
  GROUP BY
    salesperson_id, year
)
SELECT *
FROM
  commissions_year CUR,
  commissions_year PREV
WHERE
  CUR.salesperson_id=PREV.salesperson_id AND
  CUR.year=PREV.year + 1;

+----------------+------+----------+----------------+------+----------+
| salesperson_id | year | total    | salesperson_id | year | total    |
+----------------+------+----------+----------------+------+----------+
|             19 | 2017 |  5193.44 |             19 | 2016 |  2449.01 |
|             38 | 2017 | 12010.91 |             38 | 2016 |  5277.46 |
|             42 | 2017 |  6456.68 |             42 | 2016 |  7436.57 |
|             74 | 2017 |  6218.34 |             74 | 2016 |  8302.50 |
|             79 | 2017 |  9055.54 |             79 | 2016 | 12216.46 |
|            106 | 2017 |  9387.77 |            106 | 2016 |  6041.18 |
|             44 | 2017 |  8595.13 |             44 | 2016 | 12870.31 |
|             69 | 2017 | 12570.95 |             69 | 2016 |  8159.49 |
|            104 | 2017 |  7655.84 |            104 | 2016 |  9981.57 |
|             49 | 2017 |  8679.84 |             49 | 2016 |  9485.64 |
|             53 | 2017 | 10967.64 |             53 | 2016 |  5310.08 |
|             64 | 2017 |  6468.15 |             64 | 2016 |  6737.23 |
|            109 | 2017 |  5791.50 |            109 | 2016 |  7347.17 |
|             18 | 2017 |  6680.49 |             18 | 2016 |  5146.93 |
|             27 | 2017 |  5766.24 |             27 | 2016 |  6729.25 |
|             29 | 2017 |  5322.36 |             29 | 2016 |  8072.71 |
|             33 | 2017 |  6672.30 |             33 | 2016 |  8475.14 |
|             34 | 2017 |  9928.69 |             34 | 2016 | 10736.72 |
|             95 | 2017 |  6813.64 |             95 | 2016 |  5471.26 |
|            117 | 2017 |  8226.05 |            117 | 2016 | 10888.29 |
|            129 | 2017 |  5280.57 |            129 | 2016 |  8446.11 |
|            136 | 2017 |  8382.18 |            136 | 2016 | 12028.57 |
|             40 | 2017 |  8539.44 |             40 | 2016 | 10377.72 |
|             66 | 2017 |  9433.56 |             66 | 2016 |  9799.21 |
|             78 | 2017 | 11423.48 |             78 | 2016 |  9266.08 |
|             89 | 2017 |  8177.24 |             89 | 2016 | 13186.68 |
|            119 | 2017 |  4102.66 |            119 | 2016 |  3917.15 |
|             10 | 2017 |  8479.05 |             10 | 2016 |  9433.58 |
|            116 | 2017 | 13856.74 |            116 | 2016 | 10873.30 |
|             31 | 2017 |  8473.84 |             31 | 2016 |  7161.30 |
|             56 | 2017 |  7289.49 |             56 | 2016 |  7465.51 |
|             75 | 2017 |  7426.66 |             75 | 2016 |  8922.70 |
|            118 | 2017 |  9265.96 |            118 | 2016 |  7289.67 |
|            126 | 2017 |  7428.74 |            126 | 2016 |  7486.75 |
|             47 | 2017 |  7724.59 |             47 | 2016 | 12417.51 |
|            105 | 2017 |  9782.69 |            105 | 2016 |  8387.52 |
|            114 | 2017 | 10651.10 |            114 | 2016 | 11530.40 |
|            138 | 2017 |  7250.48 |            138 | 2016 |  9666.24 |
|             32 | 2017 |  4023.82 |             32 | 2016 |  7161.17 |
|            132 | 2017 | 10066.97 |            132 | 2016 |  6277.41 |
|             41 | 2017 | 10219.33 |             41 | 2016 |  8366.91 |
|            103 | 2017 |  6294.81 |            103 | 2016 |  6057.86 |
|            128 | 2017 | 12253.44 |            128 | 2016 |  7746.91 |
|            137 | 2017 |   698.93 |            137 | 2016 |  2558.30 |
|              3 | 2017 |  3449.67 |              3 | 2016 |  2249.93 |
|            131 | 2017 | 11147.38 |            131 | 2016 |  7774.82 |
|             61 | 2017 |  7740.47 |             61 | 2016 |  7889.91 |
|             98 | 2017 |  7272.29 |             98 | 2016 |  8651.65 |
|            139 | 2017 |  6591.31 |            139 | 2016 |  5793.49 |
|              8 | 2017 |  5178.19 |              8 | 2016 |  4514.73 |
|             63 | 2017 |  6906.14 |             63 | 2016 |  4629.89 |
|             20 | 2017 |  8260.88 |             20 | 2016 |  6093.17 |
|            121 | 2017 | 10979.07 |            121 | 2016 |  8327.77 |
|              7 | 2017 |  3197.25 |              7 | 2016 |  1088.32 |
|             71 | 2017 |  8443.95 |             71 | 2016 |  3985.55 |
+----------------+------+----------+----------------+------+----------+
55 rows in set (0.01 sec)


WITH commissions_year AS (
  SELECT
    employees.id AS sp_id,
    employees.name AS salesperson,
    YEAR(commission_date) AS year,
    SUM(commission_amount) AS total
  FROM
    commissions LEFT JOIN employees
      ON commissions.salesperson_id = employees.id
  GROUP BY
    sp_id, year
)
SELECT CUR.sp_id, CUR.salesperson, PREV.year, PREV.total, CUR.year, CUR.total
FROM
  commissions_year CUR,
  commissions_year PREV
WHERE
  CUR.sp_id=PREV.sp_id AND
  CUR.year=PREV.year + 1;

+-------+---------------------+------+----------+------+----------+
| sp_id | salesperson         | year | total    | year | total    |
+-------+---------------------+------+----------+------+----------+
|    19 | Tina Jefferson      | 2016 |  2449.01 | 2017 |  5193.44 |
|    38 | Dorothy Anderson    | 2016 |  5277.46 | 2017 | 12010.91 |
|    42 | Marian Patrick      | 2016 |  7436.57 | 2017 |  6456.68 |
|    74 | Deanna Hayes        | 2016 |  8302.50 | 2017 |  6218.34 |
|    79 | Rafael Sandoval     | 2016 | 12216.46 | 2017 |  9055.54 |
|   106 | Bradley Black       | 2016 |  6041.18 | 2017 |  9387.77 |
|    44 | Randal Hogan        | 2016 | 12870.31 | 2017 |  8595.13 |
|    69 | Luis Vaughn         | 2016 |  8159.49 | 2017 | 12570.95 |
|   104 | Steve Woods         | 2016 |  9981.57 | 2017 |  7655.84 |
|    49 | Kent Jones          | 2016 |  9485.64 | 2017 |  8679.84 |
|    53 | Jennifer Moore      | 2016 |  5310.08 | 2017 | 10967.64 |
|    64 | Carol Rivera        | 2016 |  6737.23 | 2017 |  6468.15 |
|   109 | Ruby Boyd           | 2016 |  7347.17 | 2017 |  5791.50 |
|    18 | Doris Wilson        | 2016 |  5146.93 | 2017 |  6680.49 |
|    27 | Donald Carter       | 2016 |  6729.25 | 2017 |  5766.24 |
|    29 | Winifred Walsh      | 2016 |  8072.71 | 2017 |  5322.36 |
|    33 | Frances Griffin     | 2016 |  8475.14 | 2017 |  6672.30 |
|    34 | Bobby French        | 2016 | 10736.72 | 2017 |  9928.69 |
|    95 | Virginia Butler     | 2016 |  5471.26 | 2017 |  6813.64 |
|   117 | Russell Rios        | 2016 | 10888.29 | 2017 |  8226.05 |
|   129 | Sandra Fleming      | 2016 |  8446.11 | 2017 |  5280.57 |
|   136 | Jack Green          | 2016 | 12028.57 | 2017 |  8382.18 |
|    40 | Joyce Beck          | 2016 | 10377.72 | 2017 |  8539.44 |
|    66 | Kathryn Barnes      | 2016 |  9799.21 | 2017 |  9433.56 |
|    78 | Louis Santiago      | 2016 |  9266.08 | 2017 | 11423.48 |
|    89 | Tyler Copeland      | 2016 | 13186.68 | 2017 |  8177.24 |
|   119 | Amos Miles          | 2016 |  3917.15 | 2017 |  4102.66 |
|    10 | Ryan Fletcher       | 2016 |  9433.58 | 2017 |  8479.05 |
|   116 | Christian Reeves    | 2016 | 10873.30 | 2017 | 13856.74 |
|    31 | Julius Ramos        | 2016 |  7161.30 | 2017 |  8473.84 |
|    56 | Raquel Greene       | 2016 |  7465.51 | 2017 |  7289.49 |
|    75 | Timmy Henderson     | 2016 |  8922.70 | 2017 |  7426.66 |
|   118 | Deborah Peterson    | 2016 |  7289.67 | 2017 |  9265.96 |
|   126 | Santiago Mclaughlin | 2016 |  7486.75 | 2017 |  7428.74 |
|    47 | Joann Smith         | 2016 | 12417.51 | 2017 |  7724.59 |
|   105 | Alonzo Page         | 2016 |  8387.52 | 2017 |  9782.69 |
|   114 | Veronica Boone      | 2016 | 11530.40 | 2017 | 10651.10 |
|   138 | Jason Wright        | 2016 |  9666.24 | 2017 |  7250.48 |
|    32 | Josefina Fernandez  | 2016 |  7161.17 | 2017 |  4023.82 |
|   132 | Alan Carroll        | 2016 |  6277.41 | 2017 | 10066.97 |
|    41 | Terrance Reese      | 2016 |  8366.91 | 2017 | 10219.33 |
|   103 | Emily Wilkerson     | 2016 |  6057.86 | 2017 |  6294.81 |
|   128 | Stephanie Dawson    | 2016 |  7746.91 | 2017 | 12253.44 |
|   137 | Adrian Neal         | 2016 |  2558.30 | 2017 |   698.93 |
|     3 | Evelyn Alexander    | 2016 |  2249.93 | 2017 |  3449.67 |
|   131 | Rene Gibbs          | 2016 |  7774.82 | 2017 | 11147.38 |
|    61 | Patti Harper        | 2016 |  7889.91 | 2017 |  7740.47 |
|    98 | Claude Powell       | 2016 |  8651.65 | 2017 |  7272.29 |
|   139 | Kathy Garcia        | 2016 |  5793.49 | 2017 |  6591.31 |
|     8 | Leo Gutierrez       | 2016 |  4514.73 | 2017 |  5178.19 |
|    63 | Tammy Castro        | 2016 |  4629.89 | 2017 |  6906.14 |
|    20 | Vernon Pittman      | 2016 |  6093.17 | 2017 |  8260.88 |
|   121 | Christina Terry     | 2016 |  8327.77 | 2017 | 10979.07 |
|     7 | John Conner         | 2016 |  1088.32 | 2017 |  3197.25 |
|    71 | Cecil Adkins        | 2016 |  3985.55 | 2017 |  8443.95 |
+-------+---------------------+------+----------+------+----------+
55 rows in set (0.04 sec)


WITH commissions_year AS (
  SELECT
    employees.id AS sp_id,
    employees.name AS salesperson,
    YEAR(commission_date) AS year,
    SUM(commission_amount) AS total
  FROM
    commissions LEFT JOIN employees
      ON commissions.salesperson_id = employees.id
  GROUP BY
    sp_id, year
)
SELECT *
FROM
  commissions_year C1
WHERE
  total > ( SELECT
              0.02*SUM(total)
            FROM
              commissions_year C2
            WHERE
              C2.year = C1.year
              AND C2.year = 2017)
ORDER BY
  total DESC;

+-------+------------------+------+----------+
| sp_id | salesperson      | year | total    |
+-------+------------------+------+----------+
|   116 | Christian Reeves | 2017 | 13856.74 |
|    69 | Luis Vaughn      | 2017 | 12570.95 |
|   128 | Stephanie Dawson | 2017 | 12253.44 |
|    38 | Dorothy Anderson | 2017 | 12010.91 |
|    78 | Louis Santiago   | 2017 | 11423.48 |
|   131 | Rene Gibbs       | 2017 | 11147.38 |
|   121 | Christina Terry  | 2017 | 10979.07 |
|    53 | Jennifer Moore   | 2017 | 10967.64 |
|   114 | Veronica Boone   | 2017 | 10651.10 |
|    41 | Terrance Reese   | 2017 | 10219.33 |
|   132 | Alan Carroll     | 2017 | 10066.97 |
|    34 | Bobby French     | 2017 |  9928.69 |
|   105 | Alonzo Page      | 2017 |  9782.69 |
|    66 | Kathryn Barnes   | 2017 |  9433.56 |
|   106 | Bradley Black    | 2017 |  9387.77 |
|   118 | Deborah Peterson | 2017 |  9265.96 |
|    79 | Rafael Sandoval  | 2017 |  9055.54 |
+-------+------------------+------+----------+
17 rows in set (0.03 sec)



SELECT *
FROM (
  SELECT
    employees.id AS sp_id,
    employees.name AS salesperson,
    YEAR(commission_date) AS year,
    SUM(commission_amount) AS total
  FROM
    commissions LEFT JOIN employees
      ON commissions.salesperson_id = employees.id
  GROUP BY
    sp_id, year
   ) AS C1
WHERE
  total > ( SELECT
              0.02*SUM(total)
            FROM (
              SELECT
                employees.id AS sp_id,
                employees.name AS salesperson,
                YEAR(commission_date) AS year,
                SUM(commission_amount) AS total
              FROM
                commissions LEFT JOIN employees
                  ON commissions.salesperson_id = employees.id
              GROUP BY
                sp_id, year
              ) AS C2
            WHERE
              C2.year = C1.year
              AND C2.year = 2017)
ORDER BY
  total DESC;

+-------+------------------+------+----------+
| sp_id | salesperson      | year | total    |
+-------+------------------+------+----------+
|   116 | Christian Reeves | 2017 | 13856.74 |
|    69 | Luis Vaughn      | 2017 | 12570.95 |
|   128 | Stephanie Dawson | 2017 | 12253.44 |
|    38 | Dorothy Anderson | 2017 | 12010.91 |
|    78 | Louis Santiago   | 2017 | 11423.48 |
|   131 | Rene Gibbs       | 2017 | 11147.38 |
|   121 | Christina Terry  | 2017 | 10979.07 |
|    53 | Jennifer Moore   | 2017 | 10967.64 |
|   114 | Veronica Boone   | 2017 | 10651.10 |
|    41 | Terrance Reese   | 2017 | 10219.33 |
|   132 | Alan Carroll     | 2017 | 10066.97 |
|    34 | Bobby French     | 2017 |  9928.69 |
|   105 | Alonzo Page      | 2017 |  9782.69 |
|    66 | Kathryn Barnes   | 2017 |  9433.56 |
|   106 | Bradley Black    | 2017 |  9387.77 |
|   118 | Deborah Peterson | 2017 |  9265.96 |
|    79 | Rafael Sandoval  | 2017 |  9055.54 |
+-------+------------------+------+----------+
17 rows in set (0.05 sec)

```

## Recursive CTEs

```

WITH RECURSIVE TotalSum AS (
  SELECT
    0 AS Count,
    0 AS Total
  UNION ALL
  SELECT
    Count + 1,
    Total + Count + 1
  FROM TotalSum
  WHERE Count < 100
)
SELECT * FROM TotalSum;

+-------+-------+
| Count | Total |
+-------+-------+
|     0 |     0 |
|     1 |     1 |
|     2 |     3 |
|     3 |     6 |
|     4 |    10 |
|     5 |    15 |
|     6 |    21 |
|     7 |    28 |
|     8 |    36 |
|     9 |    45 |
|    10 |    55 |
|    11 |    66 |
|    12 |    78 |
|    13 |    91 |
|    14 |   105 |
|    15 |   120 |
|    16 |   136 |
|    17 |   153 |
|    18 |   171 |
|    19 |   190 |
|    20 |   210 |
|    21 |   231 |
|    22 |   253 |
|    23 |   276 |
|    24 |   300 |
|    25 |   325 |
|    26 |   351 |
|    27 |   378 |
|    28 |   406 |
|    29 |   435 |
|    30 |   465 |
|    31 |   496 |
|    32 |   528 |
|    33 |   561 |
|    34 |   595 |
|    35 |   630 |
|    36 |   666 |
|    37 |   703 |
|    38 |   741 |
|    39 |   780 |
|    40 |   820 |
|    41 |   861 |
|    42 |   903 |
|    43 |   946 |
|    44 |   990 |
|    45 |  1035 |
|    46 |  1081 |
|    47 |  1128 |
|    48 |  1176 |
|    49 |  1225 |
|    50 |  1275 |
|    51 |  1326 |
|    52 |  1378 |
|    53 |  1431 |
|    54 |  1485 |
|    55 |  1540 |
|    56 |  1596 |
|    57 |  1653 |
|    58 |  1711 |
|    59 |  1770 |
|    60 |  1830 |
|    61 |  1891 |
|    62 |  1953 |
|    63 |  2016 |
|    64 |  2080 |
|    65 |  2145 |
|    66 |  2211 |
|    67 |  2278 |
|    68 |  2346 |
|    69 |  2415 |
|    70 |  2485 |
|    71 |  2556 |
|    72 |  2628 |
|    73 |  2701 |
|    74 |  2775 |
|    75 |  2850 |
|    76 |  2926 |
|    77 |  3003 |
|    78 |  3081 |
|    79 |  3160 |
|    80 |  3240 |
|    81 |  3321 |
|    82 |  3403 |
|    83 |  3486 |
|    84 |  3570 |
|    85 |  3655 |
|    86 |  3741 |
|    87 |  3828 |
|    88 |  3916 |
|    89 |  4005 |
|    90 |  4095 |
|    91 |  4186 |
|    92 |  4278 |
|    93 |  4371 |
|    94 |  4465 |
|    95 |  4560 |
|    96 |  4656 |
|    97 |  4753 |
|    98 |  4851 |
|    99 |  4950 |
|   100 |  5050 |
+-------+-------+
101 rows in set (0.00 sec)


WITH RECURSIVE fibonacci AS (
  SELECT
    0 AS Current,
    1 AS Next
  UNION ALL
  SELECT
    Next AS Current,
    Current + Next AS Next
  FROM fibonacci
  WHERE Next < 1000
)
SELECT * FROM fibonacci;

+---------+------+
| Current | Next |
+---------+------+
|       0 |    1 |
|       1 |    1 |
|       1 |    2 |
|       2 |    3 |
|       3 |    5 |
|       5 |    8 |
|       8 |   13 |
|      13 |   21 |
|      21 |   34 |
|      34 |   55 |
|      55 |   89 |
|      89 |  144 |
|     144 |  233 |
|     233 |  377 |
|     377 |  610 |
|     610 |  987 |
|     987 | 1597 |
+---------+------+
17 rows in set (0.00 sec)


WITH RECURSIVE fibonacci AS (
  SELECT
    0 AS Current,
    1 AS Next
  UNION ALL
  SELECT
    Next AS Current,
    Current + Next AS Next
  FROM fibonacci
  WHERE Next < 1000
)
SELECT Current AS fibonacci_series FROM fibonacci;
+------------------+
| fibonacci_series |
+------------------+
|                0 |
|                1 |
|                1 |
|                2 |
|                3 |
|                5 |
|                8 |
|               13 |
|               21 |
|               34 |
|               55 |
|               89 |
|              144 |
|              233 |
|              377 |
|              610 |
|              987 |
+------------------+
17 rows in set (0.00 sec)

This table **tudors** contains data on the Tudor monarchs of England— you know, Henry VIII, Elizabeth I, Bloody Mary, those guys. 


WITH RECURSIVE elizabeth AS (
  SELECT * FROM tudors
  WHERE id = 1
UNION
  SELECT tudors.*
  FROM tudors, elizabeth
  WHERE
    tudors.id = elizabeth.father OR
    tudors.id = elizabeth.mother
)
SELECT * FROM elizabeth;

+------+---------------------------------------+--------+--------+
| id   | name                                  | father | mother |
+------+---------------------------------------+--------+--------+
|    1 | Elizabeth I of England                |      2 |      3 |
|    2 | Henry VIII of England                 |      4 |      5 |
|    3 | Anne Boleyn                           |      6 |      7 |
|    4 | Henry VII of England                  |      8 |      9 |
|    5 | Elizabeth of York                     |     10 |     11 |
|    6 | Thomas Boleyn, 1st Earl of Wiltshire  |     12 |     13 |
|    7 | Elizabeth Howard                      |     14 |     15 |
|    8 | Edmund Tudor, 1st Earl of Richmond    |     16 |     17 |
|    9 | Margaret Beaufort                     |     18 |     19 |
|   10 | Edward IV of England                  |     20 |     21 |
|   11 | Elizabeth Woodville                   |     22 |     23 |
|   12 | William Boleyn                        |     24 |     25 |
|   13 | Margaret Butler                       |     26 |     27 |
|   14 | Thomas Howard, 2nd Duke of Norfolk    |     28 |     29 |
|   15 | Elizabeth Tilney                      |     30 |     31 |
|   16 | Owen Tudor                            |   NULL |   NULL |
|   17 | Catherine of Valois                   |   NULL |   NULL |
|   18 | John Beaufort, 1st Duke of Somerset   |   NULL |   NULL |
|   19 | Margaret Beauchamp of Bletso          |   NULL |   NULL |
|   20 | Richard Plantagenet, 3rd Duke of York |   NULL |   NULL |
|   21 | Cecily Neville                        |   NULL |   NULL |
|   22 | Richard Woodville, 1st Earl Rivers    |   NULL |   NULL |
|   23 | Jacquetta of Luxembourg               |   NULL |   NULL |
|   24 | Geoffrey Boleyn                       |   NULL |   NULL |
|   25 | Anne Hoo                              |   NULL |   NULL |
|   26 | Thomas Butler, 7th Earl of Ormonde    |   NULL |   NULL |
|   27 | Anne Hankford                         |   NULL |   NULL |
|   28 | John Howard, 1st Duke of Norfolk      |   NULL |   NULL |
|   29 | Catherine Moleyns                     |   NULL |   NULL |
|   30 | Frederick Tilney                      |   NULL |   NULL |
|   31 | Elizabeth Cheney                      |   NULL |   NULL |
+------+---------------------------------------+--------+--------+
31 rows in set (0.00 sec)


WITH RECURSIVE destinations AS (
    SELECT arriving
    FROM routes
    WHERE departing='Raleigh'
  UNION
    SELECT routes.arriving
    FROM destinations, routes
    WHERE
      destinations.arriving=routes.departing
)
SELECT * FROM destinations;

+------------+
| arriving   |
+------------+
| Washington |
| Atlanta    |
| Miami      |
| Chicago    |
| Raleigh    |
| New York   |
| Toronto    |
+------------+
7 rows in set (0.00 sec)


WITH RECURSIVE destinations AS (
    SELECT departing AS arriving
    FROM routes
    WHERE departing='Raleigh'
  UNION
    SELECT routes.arriving
    FROM destinations, routes
    WHERE
      destinations.arriving=routes.departing
)
SELECT * FROM destinations;

+------------+
| arriving   |
+------------+
| Raleigh    |
| Washington |
| Atlanta    |
| Miami      |
| Chicago    |
| New York   |
| Toronto    |
+------------+
7 rows in set (0.01 sec)


WITH RECURSIVE full_routes AS (
    SELECT departing AS path, arriving
    FROM routes
    WHERE departing='Raleigh'
  UNION
    SELECT
      CONCAT(full_routes.path, ' > ',
             routes.arriving),
      routes.arriving
    FROM full_routes, routes
    WHERE
      full_routes.arriving=routes.departing
      AND
      LOCATE(routes.arriving, full_routes.path)=0
) SELECT * FROM full_routes;

+-------------------------------------------+------------+
| path                                      | arriving   |
+-------------------------------------------+------------+
| Raleigh                                   | Washington |
| Raleigh                                   | Atlanta    |
| Raleigh                                   | Miami      |
| Raleigh > Chicago                         | Chicago    |
| Raleigh > New York                        | New York   |
| Raleigh > Miami                           | Miami      |
| Raleigh > Chicago > New York              | New York   |
| Raleigh > New York > Washington           | Washington |
| Raleigh > New York > Toronto              | Toronto    |
| Raleigh > Chicago > New York > Washington | Washington |
| Raleigh > Chicago > New York > Toronto    | Toronto    |
+-------------------------------------------+------------+
11 rows in set (0.01 sec)


WITH RECURSIVE full_routes AS (
    SELECT departing AS path, departing AS arriving
    FROM routes
    WHERE departing='Raleigh'
  UNION
    SELECT
      CONCAT(full_routes.path, ' > ',
             routes.arriving),
      routes.arriving
    FROM full_routes, routes
    WHERE
      full_routes.arriving=routes.departing
      AND
      LOCATE(routes.arriving, full_routes.path)=0
) SELECT * FROM full_routes;

+-----------------------------------------------------+------------+
| path                                                | arriving   |
+-----------------------------------------------------+------------+
| Raleigh                                             | Raleigh    |
| Raleigh > Washington                                | Washington |
| Raleigh > Atlanta                                   | Atlanta    |
| Raleigh > Miami                                     | Miami      |
| Raleigh > Atlanta > Chicago                         | Chicago    |
| Raleigh > Washington > New York                     | New York   |
| Raleigh > Atlanta > Miami                           | Miami      |
| Raleigh > Atlanta > Chicago > New York              | New York   |
| Raleigh > Washington > New York > Toronto           | Toronto    |
| Raleigh > Atlanta > Chicago > New York > Washington | Washington |
| Raleigh > Atlanta > Chicago > New York > Toronto    | Toronto    |
+-----------------------------------------------------+------------+
11 rows in set (0.00 sec)


SELECT * FROM (
  SELECT
    commissions.commission_date AS date,
    commissions.commission_id AS cid,
    employees.name AS salesperson,
    employees.office AS office,
    commissions.commission_amount AS amount
  FROM commissions LEFT JOIN employees
    ON (commissions.salesperson_id = employees.id)
) AS c1
WHERE (
  SELECT count(c2.amount)
  FROM (
    SELECT
      commissions.commission_id AS cid,
      commissions.salesperson_id AS sp_id,
      employees.office AS office,
      commissions.commission_amount AS amount
    FROM commissions LEFT JOIN employees
      ON (commissions.salesperson_id = employees.id)
    ) AS c2
  WHERE
    c1.cid != c2.cid
    AND
    c1.office = c2.office
    AND
    c2.amount > c1.amount) < 2
ORDER BY office,amount desc;

+------------+-------+------------------+-------------+--------+
| date       | cid   | salesperson      | office      | amount |
+------------+-------+------------------+-------------+--------+
| 2017-09-22 | 17776 | Jack Green       | Chicago     | 497.83 |
| 2017-05-12 | 17215 | Deborah Peterson | Chicago     | 497.29 |
| 2017-06-15 | 17340 | Alonzo Page      | Cleveland   | 499.90 |
| 2017-05-25 | 17261 | Alonzo Page      | Cleveland   | 499.29 |
| 2017-03-22 | 16987 | Rene Gibbs       | Dallas      | 499.55 |
| 2017-09-18 | 17756 | Kathryn Barnes   | Dallas      | 499.43 |
| 2017-01-16 | 16688 | Joyce Beck       | Memphis     | 499.41 |
| 2017-03-02 | 16890 | Tammy Castro     | Memphis     | 497.58 |
| 2017-12-11 | 18116 | Terrance Reese   | Minneapolis | 499.31 |
| 2017-04-05 | 17047 | Ruby Boyd        | Minneapolis | 495.98 |
| 2017-08-30 | 17674 | Dorothy Anderson | Nauvoo      | 497.91 |
| 2017-12-11 | 18115 | Dorothy Anderson | Nauvoo      | 468.99 |
| 2017-01-20 | 16706 | John Conner      | Raleigh     | 499.33 |
| 2016-08-26 | 16082 | Randal Hogan     | Raleigh     | 499.07 |
| 2017-11-03 | 17970 | Christina Terry  | Wichita     | 499.97 |
| 2016-10-31 | 16345 | Christina Terry  | Wichita     | 497.83 |
+------------+-------+------------------+-------------+--------+
16 rows in set (18.88 sec)


SELECT
  RANK() OVER (
    PARTITION BY employees.office
    ORDER BY commissions.commission_amount DESC
  ) AS rnk,
  commissions.commission_date AS date,
  commissions.commission_id AS cid,
  employees.name AS salesperson,
  employees.office AS office,
  commissions.commission_amount AS amount
FROM commissions LEFT JOIN employees
  ON (commissions.salesperson_id = employees.id)
ORDER BY office,rnk limit 20;

+-----+------------+-------+------------------+---------+--------+
| rnk | date       | cid   | salesperson      | office  | amount |
+-----+------------+-------+------------------+---------+--------+
|   1 | 2017-09-22 | 17776 | Jack Green       | Chicago | 497.83 |
|   2 | 2017-05-12 | 17215 | Deborah Peterson | Chicago | 497.29 |
|   3 | 2016-11-09 | 16394 | Jack Green       | Chicago | 496.97 |
|   4 | 2016-04-05 | 15486 | Donald Carter    | Chicago | 496.60 |
|   5 | 2017-08-03 | 17566 | Jason Wright     | Chicago | 494.83 |
|   6 | 2016-04-19 | 15549 | Deborah Peterson | Chicago | 494.42 |
|   7 | 2016-10-18 | 16285 | Jack Green       | Chicago | 493.74 |
|   8 | 2017-06-13 | 17324 | Jason Wright     | Chicago | 490.94 |
|   9 | 2016-03-24 | 15441 | Frances Griffin  | Chicago | 488.31 |
|  10 | 2017-10-19 | 17900 | Evelyn Alexander | Chicago | 488.17 |
|  11 | 2016-07-05 | 15877 | Jack Green       | Chicago | 487.95 |
|  12 | 2016-02-15 | 15267 | Jack Green       | Chicago | 485.35 |
|  13 | 2016-06-01 | 15735 | Jack Green       | Chicago | 480.87 |
|  14 | 2017-09-06 | 17700 | Jason Wright     | Chicago | 478.98 |
|  14 | 2017-12-21 | 18149 | Carol Rivera     | Chicago | 478.98 |
|  16 | 2016-12-12 | 16533 | Jason Wright     | Chicago | 477.93 |
|  17 | 2017-04-14 | 17097 | Jason Wright     | Chicago | 477.77 |
|  18 | 2016-09-13 | 16155 | Jason Wright     | Chicago | 476.89 |
|  19 | 2017-07-17 | 17480 | Jack Green       | Chicago | 476.29 |
|  20 | 2016-12-30 | 16613 | Frances Griffin  | Chicago | 475.27 |
+-----+------------+-------+------------------+---------+--------+
20 rows in set (0.03 sec)


SELECT * FROM (
  SELECT
    RANK() OVER (
      PARTITION BY employees.office
      ORDER BY commissions.commission_amount DESC
    ) AS rnk,
    commissions.commission_date AS date,
    commissions.commission_id AS cid,
    employees.name AS salesperson,
    employees.office AS office,
    commissions.commission_amount AS amount
  FROM commissions LEFT JOIN employees
    ON (commissions.salesperson_id = employees.id)
) AS ranks
WHERE rnk <= 2
ORDER BY office,rnk;

+-----+------------+-------+------------------+-------------+--------+
| rnk | date       | cid   | salesperson      | office      | amount |
+-----+------------+-------+------------------+-------------+--------+
|   1 | 2017-09-22 | 17776 | Jack Green       | Chicago     | 497.83 |
|   2 | 2017-05-12 | 17215 | Deborah Peterson | Chicago     | 497.29 |
|   1 | 2017-06-15 | 17340 | Alonzo Page      | Cleveland   | 499.90 |
|   2 | 2017-05-25 | 17261 | Alonzo Page      | Cleveland   | 499.29 |
|   1 | 2017-03-22 | 16987 | Rene Gibbs       | Dallas      | 499.55 |
|   2 | 2017-09-18 | 17756 | Kathryn Barnes   | Dallas      | 499.43 |
|   1 | 2017-01-16 | 16688 | Joyce Beck       | Memphis     | 499.41 |
|   2 | 2017-03-02 | 16890 | Tammy Castro     | Memphis     | 497.58 |
|   1 | 2017-12-11 | 18116 | Terrance Reese   | Minneapolis | 499.31 |
|   2 | 2017-04-05 | 17047 | Ruby Boyd        | Minneapolis | 495.98 |
|   1 | 2017-08-30 | 17674 | Dorothy Anderson | Nauvoo      | 497.91 |
|   2 | 2017-12-11 | 18115 | Dorothy Anderson | Nauvoo      | 468.99 |
|   1 | 2017-01-20 | 16706 | John Conner      | Raleigh     | 499.33 |
|   2 | 2016-08-26 | 16082 | Randal Hogan     | Raleigh     | 499.07 |
|   1 | 2017-11-03 | 17970 | Christina Terry  | Wichita     | 499.97 |
|   2 | 2016-10-31 | 16345 | Christina Terry  | Wichita     | 497.83 |
+-----+------------+-------+------------------+-------------+--------+
16 rows in set (0.05 sec)
```
## Window Function
```
use olap;

SELECT day, hour, temp
FROM beach
WHERE day BETWEEN '1984-06-01' AND '1984-06-06'
ORDER BY day,hour;

+------------+----------+------+
| day        | hour     | temp |
+------------+----------+------+
| 1984-06-01 | 00:00:00 | 19.4 |
| 1984-06-01 | 06:00:00 | 13.3 |
| 1984-06-01 | 12:00:00 | 12.7 |
| 1984-06-02 | 00:00:00 | 24.9 |
| 1984-06-02 | 18:00:00 | 27.2 |
| 1984-06-03 | 12:00:00 | 21.6 |
| 1984-06-03 | 18:00:00 | 29.4 |
| 1984-06-04 | 06:00:00 | 23.3 |
| 1984-06-04 | 12:00:00 | 23.3 |
| 1984-06-04 | 18:00:00 | 28.8 |
| 1984-06-05 | 00:00:00 | 25.5 |
| 1984-06-05 | 06:00:00 | 22.2 |
| 1984-06-05 | 12:00:00 | 23.3 |
| 1984-06-05 | 18:00:00 | 28.8 |
| 1984-06-06 | 06:00:00 | 23.3 |
| 1984-06-06 | 12:00:00 | 23.3 |
+------------+----------+------+
16 rows in set (0.01 sec)



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

+------------+----------+------+--------------------+--------------------+--------------------+
| day        | hour     | temp | day_avg            | day_min            | day_max            |
+------------+----------+------+--------------------+--------------------+--------------------+
| 1984-06-01 | 00:00:00 | 19.4 | 15.133333206176758 | 12.699999809265137 | 19.399999618530273 |
| 1984-06-01 | 06:00:00 | 13.3 | 15.133333206176758 | 12.699999809265137 | 19.399999618530273 |
| 1984-06-01 | 12:00:00 | 12.7 | 15.133333206176758 | 12.699999809265137 | 19.399999618530273 |
| 1984-06-02 | 00:00:00 | 24.9 | 26.050000190734863 | 24.899999618530273 | 27.200000762939453 |
| 1984-06-02 | 18:00:00 | 27.2 | 26.050000190734863 | 24.899999618530273 | 27.200000762939453 |
| 1984-06-03 | 12:00:00 | 21.6 |               25.5 | 21.600000381469727 | 29.399999618530273 |
| 1984-06-03 | 18:00:00 | 29.4 |               25.5 | 21.600000381469727 | 29.399999618530273 |
| 1984-06-04 | 06:00:00 | 23.3 |  25.13333257039388 | 23.299999237060547 | 28.799999237060547 |
| 1984-06-04 | 12:00:00 | 23.3 |  25.13333257039388 | 23.299999237060547 | 28.799999237060547 |
| 1984-06-04 | 18:00:00 | 28.8 |  25.13333257039388 | 23.299999237060547 | 28.799999237060547 |
| 1984-06-05 | 00:00:00 | 25.5 | 24.949999809265137 | 22.200000762939453 | 28.799999237060547 |
| 1984-06-05 | 06:00:00 | 22.2 | 24.949999809265137 | 22.200000762939453 | 28.799999237060547 |
| 1984-06-05 | 12:00:00 | 23.3 | 24.949999809265137 | 22.200000762939453 | 28.799999237060547 |
| 1984-06-05 | 18:00:00 | 28.8 | 24.949999809265137 | 22.200000762939453 | 28.799999237060547 |
| 1984-06-06 | 06:00:00 | 23.3 | 23.299999237060547 | 23.299999237060547 | 23.299999237060547 |
| 1984-06-06 | 12:00:00 | 23.3 | 23.299999237060547 | 23.299999237060547 | 23.299999237060547 |
+------------+----------+------+--------------------+--------------------+--------------------+
16 rows in set (0.00 sec)



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

+------------+----------+------+---------+---------+---------+
| day        | hour     | temp | day_avg | day_min | day_max |
+------------+----------+------+---------+---------+---------+
| 1984-06-01 | 00:00:00 | 19.4 |    15.1 |    12.7 |    19.4 |
| 1984-06-01 | 06:00:00 | 13.3 |    15.1 |    12.7 |    19.4 |
| 1984-06-01 | 12:00:00 | 12.7 |    15.1 |    12.7 |    19.4 |
| 1984-06-02 | 00:00:00 | 24.9 |    26.1 |    24.9 |    27.2 |
| 1984-06-02 | 18:00:00 | 27.2 |    26.1 |    24.9 |    27.2 |
| 1984-06-03 | 12:00:00 | 21.6 |    25.5 |    21.6 |    29.4 |
| 1984-06-03 | 18:00:00 | 29.4 |    25.5 |    21.6 |    29.4 |
| 1984-06-04 | 06:00:00 | 23.3 |    25.1 |    23.3 |    28.8 |
| 1984-06-04 | 12:00:00 | 23.3 |    25.1 |    23.3 |    28.8 |
| 1984-06-04 | 18:00:00 | 28.8 |    25.1 |    23.3 |    28.8 |
| 1984-06-05 | 00:00:00 | 25.5 |    24.9 |    22.2 |    28.8 |
| 1984-06-05 | 06:00:00 | 22.2 |    24.9 |    22.2 |    28.8 |
| 1984-06-05 | 12:00:00 | 23.3 |    24.9 |    22.2 |    28.8 |
| 1984-06-05 | 18:00:00 | 28.8 |    24.9 |    22.2 |    28.8 |
| 1984-06-06 | 06:00:00 | 23.3 |    23.3 |    23.3 |    23.3 |
| 1984-06-06 | 12:00:00 | 23.3 |    23.3 |    23.3 |    23.3 |
+------------+----------+------+---------+---------+---------+
16 rows in set (0.00 sec)



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

+------------+----------+------+---------+---------+---------+
| day        | hour     | temp | day_avg | day_min | day_max |
+------------+----------+------+---------+---------+---------+
| 1984-06-01 | 00:00:00 | 19.4 |    15.1 |    12.7 |    19.4 |
| 1984-06-01 | 06:00:00 | 13.3 |    15.1 |    12.7 |    19.4 |
| 1984-06-01 | 12:00:00 | 12.7 |    15.1 |    12.7 |    19.4 |
| 1984-06-02 | 00:00:00 | 24.9 |    26.1 |    24.9 |    27.2 |
| 1984-06-02 | 18:00:00 | 27.2 |    26.1 |    24.9 |    27.2 |
| 1984-06-03 | 12:00:00 | 21.6 |    25.5 |    21.6 |    29.4 |
| 1984-06-03 | 18:00:00 | 29.4 |    25.5 |    21.6 |    29.4 |
| 1984-06-04 | 06:00:00 | 23.3 |    25.1 |    23.3 |    28.8 |
| 1984-06-04 | 12:00:00 | 23.3 |    25.1 |    23.3 |    28.8 |
| 1984-06-04 | 18:00:00 | 28.8 |    25.1 |    23.3 |    28.8 |
| 1984-06-05 | 00:00:00 | 25.5 |    24.9 |    22.2 |    28.8 |
| 1984-06-05 | 06:00:00 | 22.2 |    24.9 |    22.2 |    28.8 |
| 1984-06-05 | 12:00:00 | 23.3 |    24.9 |    22.2 |    28.8 |
| 1984-06-05 | 18:00:00 | 28.8 |    24.9 |    22.2 |    28.8 |
| 1984-06-06 | 06:00:00 | 23.3 |    23.3 |    23.3 |    23.3 |
| 1984-06-06 | 12:00:00 | 23.3 |    23.3 |    23.3 |    23.3 |
+------------+----------+------+---------+---------+---------+
16 rows in set (0.00 sec)


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
+------------+----------+------+---------+---------+---------+---------+---------+---------+
| day        | hour     | temp | day_avg | day_min | day_max | all_avg | all_max | all_min |
+------------+----------+------+---------+---------+---------+---------+---------+---------+
| 1984-06-01 | 00:00:00 | 19.4 |    15.1 |    12.7 |    19.4 |    23.1 |    29.4 |    12.7 |
| 1984-06-01 | 06:00:00 | 13.3 |    15.1 |    12.7 |    19.4 |    23.1 |    29.4 |    12.7 |
| 1984-06-01 | 12:00:00 | 12.7 |    15.1 |    12.7 |    19.4 |    23.1 |    29.4 |    12.7 |
| 1984-06-02 | 00:00:00 | 24.9 |    26.1 |    24.9 |    27.2 |    23.1 |    29.4 |    12.7 |
| 1984-06-02 | 18:00:00 | 27.2 |    26.1 |    24.9 |    27.2 |    23.1 |    29.4 |    12.7 |
| 1984-06-03 | 12:00:00 | 21.6 |    25.5 |    21.6 |    29.4 |    23.1 |    29.4 |    12.7 |
| 1984-06-03 | 18:00:00 | 29.4 |    25.5 |    21.6 |    29.4 |    23.1 |    29.4 |    12.7 |
| 1984-06-04 | 06:00:00 | 23.3 |    25.1 |    23.3 |    28.8 |    23.1 |    29.4 |    12.7 |
| 1984-06-04 | 12:00:00 | 23.3 |    25.1 |    23.3 |    28.8 |    23.1 |    29.4 |    12.7 |
| 1984-06-04 | 18:00:00 | 28.8 |    25.1 |    23.3 |    28.8 |    23.1 |    29.4 |    12.7 |
| 1984-06-05 | 00:00:00 | 25.5 |    24.9 |    22.2 |    28.8 |    23.1 |    29.4 |    12.7 |
| 1984-06-05 | 06:00:00 | 22.2 |    24.9 |    22.2 |    28.8 |    23.1 |    29.4 |    12.7 |
| 1984-06-05 | 12:00:00 | 23.3 |    24.9 |    22.2 |    28.8 |    23.1 |    29.4 |    12.7 |
| 1984-06-05 | 18:00:00 | 28.8 |    24.9 |    22.2 |    28.8 |    23.1 |    29.4 |    12.7 |
| 1984-06-06 | 06:00:00 | 23.3 |    23.3 |    23.3 |    23.3 |    23.1 |    29.4 |    12.7 |
| 1984-06-06 | 12:00:00 | 23.3 |    23.3 |    23.3 |    23.3 |    23.1 |    29.4 |    12.7 |
+------------+----------+------+---------+---------+---------+---------+---------+---------+
16 rows in set (0.01 sec)

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

## Window + CTEs

```
SELECT
  p1.location,
  p1.day, MAX(p2.day),
  DATEDIFF(p1.day, MAX(p2.day)) AS diff
FROM precip AS p1,
     precip AS p2
WHERE
  p1.location = p2.location
  AND
  p2.day < p1.day
GROUP BY p1.day,p1.location
ORDER BY location;

+------------+------------+-------------+------+
| location   | day        | MAX(p2.day) | diff |
+------------+------------+-------------+------+
| Asheville  | 1976-05-28 | 1976-05-25  |    3 |
| Asheville  | 1976-04-21 | 1976-04-04  |   17 |
| Asheville  | 1976-04-29 | 1976-04-21  |    8 |
| Asheville  | 1976-04-30 | 1976-04-29  |    1 |
| Asheville  | 1976-05-01 | 1976-04-30  |    1 |
| Asheville  | 1976-05-06 | 1976-05-01  |    5 |
| Asheville  | 1976-05-07 | 1976-05-06  |    1 |
| Asheville  | 1976-05-11 | 1976-05-07  |    4 |
| Asheville  | 1976-05-13 | 1976-05-11  |    2 |
| Asheville  | 1976-05-14 | 1976-05-13  |    1 |
| Asheville  | 1976-05-15 | 1976-05-14  |    1 |
| Asheville  | 1976-05-16 | 1976-05-15  |    1 |
| Asheville  | 1976-05-18 | 1976-05-16  |    2 |
| Asheville  | 1976-05-25 | 1976-05-18  |    7 |
| Asheville  | 1976-04-04 | 1976-03-31  |    4 |
| Asheville  | 1976-05-29 | 1976-05-28  |    1 |
| Asheville  | 1976-05-31 | 1976-05-29  |    2 |
| Asheville  | 1976-06-01 | 1976-05-31  |    1 |
| Asheville  | 1976-06-02 | 1976-06-01  |    1 |
| Asheville  | 1976-06-03 | 1976-06-02  |    1 |
| Asheville  | 1976-06-08 | 1976-06-03  |    5 |
| Asheville  | 1976-06-16 | 1976-06-08  |    8 |
| Asheville  | 1976-06-17 | 1976-06-16  |    1 |
| Asheville  | 1976-06-19 | 1976-06-17  |    2 |
| Asheville  | 1976-06-20 | 1976-06-19  |    1 |
| Asheville  | 1976-06-23 | 1976-06-20  |    3 |
| Asheville  | 1976-06-25 | 1976-06-23  |    2 |
| Asheville  | 1976-06-26 | 1976-06-25  |    1 |
| Asheville  | 1976-03-05 | 1976-02-22  |   12 |
| Asheville  | 1976-12-31 | 1976-12-30  |    1 |
| Asheville  | 1976-01-07 | 1976-01-03  |    4 |
| Asheville  | 1976-01-13 | 1976-01-07  |    6 |



WITH precip_avg AS (
  SELECT
    p1.location,
    p1.day, MAX(p2.day),
    DATEDIFF(p1.day, MAX(p2.day)) AS diff
  FROM precip AS p1,
       precip AS p2
  WHERE
    p1.location = p2.location
    AND
    p2.day < p1.day
  GROUP BY p1.day,p1.location
  ORDER BY location
)
SELECT
  location,
  AVG(diff) AS avg_days
FROM precip_avg
GROUP BY location
ORDER BY location;

+------------+----------+
| location   | avg_days |
+------------+----------+
| Asheville  |   3.3611 |
| Raleigh    |   3.7423 |
| Wilmington |   3.3645 |
+------------+----------+
3 rows in set (0.10 sec)


SELECT
  location,day,
  LAG(day) OVER (
    PARTITION BY location
    ORDER BY day
  ) AS prev_day
FROM precip;

+------------+------------+------------+
| location   | day        | prev_day   |
+------------+------------+------------+
| Asheville  | 1976-01-03 | NULL       |
| Asheville  | 1976-01-07 | 1976-01-03 |
| Asheville  | 1976-01-13 | 1976-01-07 |
| Asheville  | 1976-01-16 | 1976-01-13 |
| Asheville  | 1976-01-17 | 1976-01-16 |
| Asheville  | 1976-01-26 | 1976-01-17 |
| Asheville  | 1976-01-27 | 1976-01-26 |
| Asheville  | 1976-02-01 | 1976-01-27 |
| Asheville  | 1976-02-02 | 1976-02-01 |
| Asheville  | 1976-02-15 | 1976-02-02 |
| Asheville  | 1976-02-18 | 1976-02-15 |
| Asheville  | 1976-02-21 | 1976-02-18 |


WITH precip_avg AS (
  SELECT
    location,day,
    LAG(day) OVER (
      PARTITION BY location
      ORDER BY day
    ) AS prev_day
  FROM precip
)
SELECT location,
  AVG(DATEDIFF(day, prev_day)) AS avg_day
FROM precip_avg
GROUP BY location;

+------------+---------+
| location   | avg_day |
+------------+---------+
| Asheville  |  3.3611 |
| Raleigh    |  3.7423 |
| Wilmington |  3.3645 |
+------------+---------+
3 rows in set (0.01 sec)

```



