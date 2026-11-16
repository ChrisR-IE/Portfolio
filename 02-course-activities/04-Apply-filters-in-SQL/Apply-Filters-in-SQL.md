> **Note on File Formats:** For the best visual formatting and layout, you can download the original formal Word document version of this report here: [`Apply-filters-to-SQL-queries.docx`](./Apply-filters-to-SQL-queries.docx). This Markdown version is provided for quick, convenient in-browser reading, with terminal screenshots formatted as clean, skimmable log blocks.

---

# Apply filters to SQL queries

## Project description
This project uses SQL to investigate potential security issues involving login attempts and employee machines at a large organization. I applied filters with the AND, OR, NOT, and LIKE operators to retrieve only the records each investigation needed. These included failed logins after business hours, login activity around a suspicious event, logins from outside Mexico, and employee records grouped by department and office so that security updates could be applied to the right machines.

---

## Retrieve after hours failed login attempts
A potential security incident occurred after business hours, so I needed to review failed login attempts made after 18:00. The following query retrieves them from the log_in_attempts table:

```sql
SELECT *
FROM log_in_attempts
WHERE login_time > '18:00' AND success = 0;
```

```text
MariaDB [organization]> SELECT *
    -> 
    -> FROM log_in_attempts
    -> 
    -> WHERE login_time > '18:00' AND success = 0;
+----------+-----------+------------+------------+---------+-----------------+---------+
| event_id | username  | login_date | login_time | country | ip_address      | success |
+----------+-----------+------------+------------+---------+-----------------+---------+
|        2 | apatel    | 2022-05-10 | 20:27:27   | CAN     | 192.168.205.12  |       0 |
|       18 | pwashing  | 2022-05-11 | 19:28:50   | US      | 192.168.66.142  |       0 |
|       20 | tshah     | 2022-05-12 | 18:56:36   | MEXICO  | 192.168.109.50  |       0 |
|       28 | aestrada  | 2022-05-09 | 19:28:12   | MEXICO  | 192.168.27.57   |       0 |
|       34 | drosas    | 2022-05-11 | 21:02:04   | US      | 192.168.45.93   |       0 |
|       42 | cgriffin  | 2022-05-09 | 23:04:05   | US      | 192.168.4.157   |       0 |
|       52 | cjackson  | 2022-05-10 | 22:07:07   | CAN     | 192.168.58.57   |       0 |
|       69 | wjaffrey  | 2022-05-11 | 19:55:15   | USA     | 192.168.100.17  |       0 |
|       82 | abernard  | 2022-05-12 | 23:38:46   | MEX     | 192.168.234.49  |       0 |
|       87 | apatel    | 2022-05-08 | 22:38:31   | CANADA  | 192.168.132.153 |       0 |
|       96 | ivelasco  | 2022-05-09 | 22:36:36   | CAN     | 192.168.84.194  |       0 |
|      104 | asundara  | 2022-05-11 | 18:38:07   | US      | 192.168.96.200  |       0 |
|      107 | bisles    | 2022-05-12 | 20:25:57   | USA     | 192.168.116.187 |       0 |
|      111 | aestrada  | 2022-05-10 | 22:00:26   | MEXICO  | 192.168.76.27   |       0 |
|      127 | abellmas  | 2022-05-09 | 21:20:51   | CANADA  | 192.168.70.122  |       0 |
|      131 | bisles    | 2022-05-09 | 20:03:55   | US      | 192.168.113.171 |       0 |
|      155 | cgriffin  | 2022-05-12 | 22:18:42   | USA     | 192.168.236.176 |       0 |
|      160 | jclark    | 2022-05-10 | 20:49:00   | CANADA  | 192.168.214.49  |       0 |
|      199 | yappiah   | 2022-05-11 | 19:34:48   | MEXICO  | 192.168.44.232  |       0 |
+----------+-----------+------------+------------+---------+-----------------+---------+
19 rows in set (0.002 sec)
```

SELECT * returns every column, and FROM log_in_attempts names the table being searched. The WHERE clause applies the filters. Because the two conditions are joined with AND, a row is returned only when both are true.

The first condition, login_time > '18:00', keeps attempts made after the end of business hours. The second, success = 0, keeps only failed attempts. MariaDB stores Boolean values as 1 for TRUE and 0 for FALSE, and Boolean values are written without quotes because they are not strings.

The query returned 19 rows. Every row has a success value of 0 and a login_time after 18:00.

---

## Retrieve login attempts on specific dates
A suspicious event occurred on 2022-05-09, so I needed all login attempts from that day and the day before, 2022-05-08. The query is:

```sql
SELECT *
FROM log_in_attempts
WHERE login_date = '2022-05-09' OR login_date = '2022-05-08';
```

<details>
<summary><strong>View Terminal Output (75 rows in set) — Click to expand</strong></summary>

```text
MariaDB [organization]> SELECT *
    -> 
    -> FROM log_in_attempts
    -> 
    -> WHERE login_date = '2022-05-09' OR login_date = '2022-05-08';
+----------+-----------+------------+------------+---------+-----------------+---------+
| event_id | username  | login_date | login_time | country | ip_address      | success |
+----------+-----------+------------+------------+---------+-----------------+---------+
|        1 | jrafael   | 2022-05-09 | 04:56:27   | CAN     | 192.168.243.140 |       1 |
|        3 | dkot      | 2022-05-09 | 06:47:41   | USA     | 192.168.151.162 |       1 |
|        4 | dkot      | 2022-05-08 | 02:00:39   | USA     | 192.168.178.71  |       0 |
|        8 | bisles    | 2022-05-08 | 01:30:17   | US      | 192.168.119.173 |       0 |
|       12 | dkot      | 2022-05-08 | 09:11:34   | USA     | 192.168.100.158 |       1 |
|       15 | lyamamot  | 2022-05-09 | 17:17:26   | USA     | 192.168.183.51  |       0 |
|       24 | arusso    | 2022-05-09 | 06:49:39   | MEXICO  | 192.168.171.192 |       1 |
|       25 | sbaelish  | 2022-05-09 | 07:04:02   | US      | 192.168.33.137  |       1 |
|       26 | apatel    | 2022-05-08 | 17:27:00   | CANADA  | 192.168.123.105 |       1 |
|       28 | aestrada  | 2022-05-09 | 19:28:12   | MEXICO  | 192.168.27.57   |       0 |
...
|      190 | jsoto     | 2022-05-09 | 05:09:21   | USA     | 192.168.25.60   |       0 |
|      191 | cjackson  | 2022-05-08 | 06:46:07   | CANADA  | 192.168.7.187   |       0 |
|      193 | lrodriqu  | 2022-05-08 | 07:11:29   | US      | 192.168.125.240 |       0 |
|      197 | jsoto     | 2022-05-08 | 09:05:09   | US      | 192.168.36.21   |       0 |
+----------+-----------+------------+------------+---------+-----------------+---------+
75 rows in set (0.065 sec)
```

</details>

*Query and the start of the output.*  
*End of the output: 75 rows in set.*

This query filters on the login_date column. OR returns a row when either condition is true. A single login attempt can only have one date, so no row could satisfy both conditions at once. Using AND here would return no results.

Both dates are written as strings in 'YYYY-MM-DD' format, matching how dates are stored in the table. The query returned 75 rows, and the output includes both successful (1) and failed (0) attempts because the query does not filter on the success column.

---

## Retrieve login attempts outside of Mexico
The team determined that the suspicious login activity did not originate in Mexico, so I needed every login attempt from outside the country. The query is:

```sql
SELECT *
FROM log_in_attempts
WHERE NOT country LIKE 'MEX%';
```

<details>
<summary><strong>View Terminal Output (144 rows in set) — Click to expand</strong></summary>

```text
MariaDB [organization]> SELECT *
    -> 
    -> FROM log_in_attempts
    -> 
    -> WHERE NOT country LIKE 'MEX%';
+----------+-----------+------------+------------+---------+-----------------+---------+
| event_id | username  | login_date | login_time | country | ip_address      | success |
+----------+-----------+------------+------------+---------+-----------------+---------+
|        1 | jrafael   | 2022-05-09 | 04:56:27   | CAN     | 192.168.243.140 |       1 |
|        2 | apatel    | 2022-05-10 | 20:27:27   | CAN     | 192.168.205.12  |       0 |
|        3 | dkot      | 2022-05-09 | 06:47:41   | USA     | 192.168.151.162 |       1 |
|        4 | dkot      | 2022-05-08 | 02:00:39   | USA     | 192.168.178.71  |       0 |
|        5 | jrafael   | 2022-05-11 | 03:05:59   | CANADA  | 192.168.86.232  |       0 |
|        7 | eraab     | 2022-05-11 | 01:45:14   | CAN     | 192.168.170.243 |       1 |
|        8 | bisles    | 2022-05-08 | 01:30:17   | US      | 192.168.119.173 |       0 |
...
|      196 | acook     | 2022-05-10 | 09:56:48   | CAN     | 192.168.52.90   |       0 |
|      197 | jsoto     | 2022-05-08 | 09:05:09   | US      | 192.168.36.21   |       0 |
|      200 | jclark    | 2022-05-12 | 01:11:45   | CANADA  | 192.168.91.103  |       1 |
+----------+-----------+------------+------------+---------+-----------------+---------+
144 rows in set (0.084 sec)
```

</details>

*Query and the start of the output.*  
*End of the output: 144 rows in set.*

The country column contains both 'MEX' and 'MEXICO' for Mexico, so filtering with = 'MEXICO' alone would have missed some records. The LIKE operator searches for a pattern, and the percent sign (%) is a wildcard that stands for any number of characters. The pattern 'MEX%' therefore matches any value that starts with MEX, which covers both spellings.

NOT reverses the condition, so the query returns only rows where the country does not match that pattern. The query returned 144 rows, and the country values in the output include CAN, CANADA, US, and USA.

---

## Retrieve employees in Marketing
The team is updating machines for employees in the Marketing department who are located in the East building, so I needed to query the employees table. The query is:

```sql
SELECT *
FROM employees
WHERE department = 'Marketing' AND office LIKE 'East%';
```

```text
MariaDB [organization]> SELECT * FROM employees WHERE department = 'Marketing' AND office LIKE 'East%';
+-------------+--------------+-----------+------------+----------+
| employee_id | device_id    | username  | department | office   |
+-------------+--------------+-----------+------------+----------+
|        1000 | a320b137c219 | elarson   | Marketing  | East-170 |
|        1052 | a192b174c940 | jdarosa   | Marketing  | East-195 |
|        1075 | x573y883z772 | fbautist  | Marketing  | East-267 |
|        1088 | k865l965m233 | rgosh     | Marketing  | East-157 |
|        1103 | NULL         | randerss  | Marketing  | East-460 |
|        1156 | a184b775c707 | dellery   | Marketing  | East-417 |
|        1163 | h679i515j339 | cwilliam  | Marketing  | East-216 |
+-------------+--------------+-----------+------------+----------+
7 rows in set (0.038 sec)
```

*Query and output: 7 Marketing employees in the East building.*

AND requires both conditions to be true. The first, department = 'Marketing', is an exact match on the department column. The second, office LIKE 'East%', uses the % wildcard to match any office value beginning with East, such as East-170 or East-267, without listing each office separately.

The query returned 7 rows. One of the returned records (username randerss) has a NULL value in the device_id column.

---

## Retrieve employees in Finance or Sales
A different security update was needed on machines for employees in the Finance or Sales departments. The query is:

```sql
SELECT *
FROM employees
WHERE department = 'Finance' OR department = 'Sales';
```

<details>
<summary><strong>View Terminal Output (71 rows in set) — Click to expand</strong></summary>

```text
MariaDB [organization]> SELECT *
    -> 
    -> FROM employees
    -> 
    -> WHERE department = 'Finance' OR department = 'Sales';
+-------------+--------------+-----------+------------+-------------+
| employee_id | device_id    | username  | department | office      |
+-------------+--------------+-----------+------------+-------------+
|        1003 | d394e816f943 | sgilmore  | Finance    | South-153   |
|        1007 | h174i497j413 | wjaffrey  | Finance    | North-406   |
|        1008 | i858j583k571 | abernard  | Finance    | South-170   |
|        1009 | NULL         | lrodriqu  | Sales      | South-134   |
|        1010 | k242l212m542 | jlansky   | Finance    | South-109   |
...
|        1187 | f963g637h851 | bbode     | Finance    | East-351    |
|        1188 | g164h566i795 | noshiro   | Finance    | West-252    |
|        1195 | n516o853p957 | orainier  | Finance    | East-346    |
+-------------+--------------+-----------+------------+-------------+
71 rows in set (0.001 sec)
```

</details>

*Query and the start of the output.*  
*End of the output: 71 rows in set.*

OR returns a row when either condition is true, so employees in either department are included. Although both conditions use the same column, the full condition has to be written out each time (department = 'Finance' OR department = 'Sales'). Writing department = 'Finance' OR 'Sales' would not give the intended result, because 'Sales' on its own is not a comparison.

The query returned 71 rows containing employees from both departments.

---

## Retrieve all employees not in IT
One more update was needed on employee machines. Employees in the Information Technology department already had it, so I needed everyone else. The query is:

```sql
SELECT *
FROM employees
WHERE NOT department = 'Information Technology';
```

<details>
<summary><strong>View Terminal Output (161 rows in set) — Click to expand</strong></summary>

```text
MariaDB [organization]> SELECT *
    -> 
    -> FROM employees
    -> 
    -> WHERE NOT department = 'Information Technology';
+-------------+--------------+-----------+-----------------+-------------+
| employee_id | device_id    | username  | department      | office      |
+-------------+--------------+-----------+-----------------+-------------+
|        1000 | a320b137c219 | elarson   | Marketing       | East-170    |
|        1001 | b239c825d303 | bmoreno   | Marketing       | Central-276 |
|        1002 | c116d593e558 | tshah     | Human Resources | North-434   |
|        1003 | d394e816f943 | sgilmore  | Finance         | South-153   |
|        1004 | e218f877g788 | eraab     | Human Resources | South-127   |
...
|        1195 | n516o853p957 | orainier  | Finance         | East-346    |
|        1198 | q308r573s459 | jmartine  | Marketing       | South-117   |
|        1199 | r520s571t459 | areyes    | Human Resources | East-100    |
+-------------+--------------+-----------+-----------------+-------------+
161 rows in set (0.001 sec)
```

</details>

*Query and the start of the output.*  
*End of the output: 161 rows in set.*

NOT reverses the condition that follows it, so the query returns every row where the department is anything other than 'Information Technology'. The same filter could also be written with the not-equal operator, department != 'Information Technology'.

The query returned 161 rows, which covers employees in departments such as Marketing, Finance, Sales, and Human Resources.

---

## Summary
I used SQL filters to narrow the log_in_attempts and employees tables down to the records needed for each part of the investigation. AND combined conditions on time and login outcome and on department and office, OR matched either of two dates or departments, and NOT excluded Mexico-based logins and the IT department. LIKE with the % wildcard made it possible to match country values and office locations by pattern, and filtering on login_time and login_date focused the review on specific times and days. Together these queries help identify suspicious login activity and the employee machines that need updates.
