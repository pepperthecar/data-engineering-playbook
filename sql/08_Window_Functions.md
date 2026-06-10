# Window Functions in SQL

Window Functions are one of the most important advanced SQL concepts and are frequently asked in interviews.

A Window Function performs calculations across a set of rows related to the current row without grouping the rows into a single result.

Unlike `GROUP BY`, window functions keep all rows and add calculated values alongside them.

## Why Use Window Functions?

Window Functions help you:
- Calculate rankings
- Find top N records
- Running totals
- Moving averages
- Compare current row with previous/next row
- Calculate percentages
- Analyze trends

Employees:
| EmpID | Name  | Department | Salary |
| ----- | ----- | ---------- | ------ |
| 1     | Ravi  | HR         | 50000  |
| 2     | Priya | HR         | 70000  |
| 3     | Arun  | IT         | 60000  |
| 4     | Kiran | IT         | 80000  |
| 5     | Meena | IT         | 60000  |

## Window Function Syntax

```
Function_Name()
OVER
(
    PARTITION BY column_name
    ORDER BY column_name
)
```
Components:

`OVER()`

Defines the window.

`PARTITION BY`

Divides data into groups.

`ORDER BY`

Orders rows within each partition.

## GROUP BY vs WINDOW FUNCTION

GROUP BY
```
SELECT Department,
       AVG(Salary)
FROM Employees
GROUP BY Department;
```
Output:
| Department | AvgSalary |
| ---------- | --------- |
| HR         | 60000     |
| IT         | 66666.67  |

Notice:

Individual employee rows disappear.

Window Function
```
SELECT Name,
       Department,
       Salary,
       AVG(Salary)
       OVER(PARTITION BY Department) AS AvgSalary
FROM Employees;
```
Output:
| Name  | Department | Salary | AvgSalary |
| ----- | ---------- | ------ | --------- |
| Ravi  | HR         | 50000  | 60000     |
| Priya | HR         | 70000  | 60000     |
| Arun  | IT         | 60000  | 66666.67  |
| Kiran | IT         | 80000  | 66666.67  |
| Meena | IT         | 60000  | 66666.67  |

Notice:

- Every row remains visible.
- Aggregate value is added.

## Types of Window Functions

1. ROW_NUMBER()

Assigns a unique sequential number.

```
SELECT Name,
       Salary,
       ROW_NUMBER()
       OVER(ORDER BY Salary DESC) AS RowNum
FROM Employees;
```
Output:
| Name  | Salary | RowNum |
| ----- | ------ | ------ |
| Kiran | 80000  | 1      |
| Priya | 70000  | 2      |
| Arun  | 60000  | 3      |
| Meena | 60000  | 4      |
| Ravi  | 50000  | 5      |

2. RANK()

Assigns rank.

Same values receive same rank.

Gaps appear after ties.

```
SELECT Name,
       Salary,
       RANK()
       OVER(ORDER BY Salary DESC) AS RankNo
FROM Employees;
```
Output:
| Name  | Salary | Rank |
| ----- | ------ | ---- |
| Kiran | 80000  | 1    |
| Priya | 70000  | 2    |
| Arun  | 60000  | 3    |
| Meena | 60000  | 3    |
| Ravi  | 50000  | 5    |

Notice:

Rank 4 is skipped.

3. DENSE_RANK()

Same as RANK but no gaps.

```
SELECT Name,
       Salary,
       DENSE_RANK()
       OVER(ORDER BY Salary DESC) AS DenseRank
FROM Employees;
```
Output:
| Name  | Salary | DenseRank |
| ----- | ------ | --------- |
| Kiran | 80000  | 1         |
| Priya | 70000  | 2         |
| Arun  | 60000  | 3         |
| Meena | 60000  | 3         |
| Ravi  | 50000  | 4         |

## ROW_NUMBER vs RANK vs DENSE_RANK
| Salary | ROW_NUMBER | RANK | DENSE_RANK |
| ------ | ---------- | ---- | ---------- |
| 80000  | 1          | 1    | 1          |
| 70000  | 2          | 2    | 2          |
| 60000  | 3          | 3    | 3          |
| 60000  | 4          | 3    | 3          |
| 50000  | 5          | 5    | 4          |

4. NTILE()

Divides rows into groups.

Split employees into 3 salary groups.
```
SELECT Name,
       Salary,
       NTILE(3)
       OVER(ORDER BY Salary DESC) AS Bucket
FROM Employees;
```
Output:
| Name  | Salary | Bucket |
| ----- | ------ | ------ |
| Kiran | 80000  | 1      |
| Priya | 70000  | 1      |
| Arun  | 60000  | 2      |
| Meena | 60000  | 2      |
| Ravi  | 50000  | 3      |

## Aggregate Window Functions

1. SUM()

Running Total
```
SELECT Name,
       Salary,
       SUM(Salary)
       OVER(ORDER BY Salary) AS RunningTotal
FROM Employees;
```
Output:
| Name  | Salary | RunningTotal |
| ----- | ------ | ------------ |
| Ravi  | 50000  | 50000        |
| Arun  | 60000  | 110000       |
| Meena | 60000  | 170000       |
| Priya | 70000  | 240000       |
| Kiran | 80000  | 320000       |

2. AVG()

Department-wise Average
```
SELECT Name,
       Department,
       Salary,
       AVG(Salary)
       OVER(PARTITION BY Department) AS DeptAvg
FROM Employees;
```
Output:
| Name  | Department | DeptAvg  |
| ----- | ---------- | -------- |
| Ravi  | HR         | 60000    |
| Priya | HR         | 60000    |
| Arun  | IT         | 66666.67 |
| Kiran | IT         | 66666.67 |
| Meena | IT         | 66666.67 |

3. MAX()
```
SELECT Name,
       Department,
       Salary,
       MAX(Salary)
       OVER(PARTITION BY Department) AS HighestSalary
FROM Employees;
```
Output:
| Name  | Department | HighestSalary |
| ----- | ---------- | ------------- |
| Ravi  | HR         | 70000         |
| Priya | HR         | 70000         |
| Arun  | IT         | 80000         |
| Kiran | IT         | 80000         |
| Meena | IT         | 80000         |

4. LEAD()

Access next row value.
```
SELECT Name,
       Salary,
       LEAD(Salary)
       OVER(ORDER BY Salary) AS NextSalary
FROM Employees;
```
Output:
| Name  | Salary | NextSalary |
| ----- | ------ | ---------- |
| Ravi  | 50000  | 60000      |
| Arun  | 60000  | 60000      |
| Meena | 60000  | 70000      |
| Priya | 70000  | 80000      |
| Kiran | 80000  | NULL       |

5. LAG()
```
SELECT Name,
       Salary,
       LAG(Salary)
       OVER(ORDER BY Salary) AS PreviousSalary
FROM Employees;
```
Output:
| Name  | Salary | PreviousSalary |
| ----- | ------ | -------------- |
| Ravi  | 50000  | NULL           |
| Arun  | 60000  | 50000          |
| Meena | 60000  | 60000          |
| Priya | 70000  | 60000          |
| Kiran | 80000  | 70000          |

6. FIRST_VALUE()

Returns first value in partition.
```
SELECT Name,
       Department,
       Salary,
       FIRST_VALUE(Salary)
       OVER(
            PARTITION BY Department
            ORDER BY Salary DESC
       ) AS HighestSalary
FROM Employees;
```
Output:
| Name  | Department | HighestSalary |
| ----- | ---------- | ------------- |
| Ravi  | HR         | 70000         |
| Priya | HR         | 70000         |
| Arun  | IT         | 80000         |
| Kiran | IT         | 80000         |
| Meena | IT         | 80000         |

7. LAST_VALUE()

Returns last value in partition.
```
SELECT Name,
       Department,
       Salary,
       LAST_VALUE(Salary)
       OVER(
            PARTITION BY Department
            ORDER BY Salary
            ROWS BETWEEN UNBOUNDED PRECEDING
            AND UNBOUNDED FOLLOWING
       ) AS LastSalary
FROM Employees;
```
Output:
| Name  | Department | LastSalary |
| ----- | ---------- | ---------- |
| Ravi  | HR         | 70000      |
| Priya | HR         | 70000      |
| Arun  | IT         | 80000      |
| Kiran | IT         | 80000      |
| Meena | IT         | 80000      |

8. PARTITION BY

Acts like GROUP BY but does not collapse rows.
```
SELECT Name,
       Department,
       Salary,
       COUNT(*)
       OVER(PARTITION BY Department) AS DeptCount
FROM Employees;
```
Output:
| Name  | Department | DeptCount |
| ----- | ---------- | --------- |
| Ravi  | HR         | 2         |
| Priya | HR         | 2         |
| Arun  | IT         | 3         |
| Kiran | IT         | 3         |
| Meena | IT         | 3         |

## Window Frame Clause

Controls how many rows participate.

Running Total
```
SELECT Name,
       Salary,
       SUM(Salary)
       OVER(
          ORDER BY Salary
          ROWS BETWEEN UNBOUNDED PRECEDING
          AND CURRENT ROW
       ) AS RunningTotal
FROM Employees;
```
Explanation:

- UNBOUNDED PRECEDING = First row
- CURRENT ROW         = Current row

Execution Order
```
SELECT Name,
       Salary,
       RANK()
       OVER(ORDER BY Salary DESC)
FROM Employees;
```
Actual Execution:

1. FROM Employees
2. SELECT Rows
3. Apply Window Function
4. Return Results

## Quick Revision Table
| Function       | Purpose                               |
| -------------- | ------------------------------------- |
| ROW_NUMBER()   | Unique sequence number                |
| RANK()         | Ranking with gaps                     |
| DENSE_RANK()   | Ranking without gaps                  |
| NTILE()        | Divide rows into buckets              |
| SUM() OVER()   | Running total                         |
| AVG() OVER()   | Moving/Aggregate average              |
| COUNT() OVER() | Count rows in partition               |
| MAX() OVER()   | Maximum value                         |
| MIN() OVER()   | Minimum value                         |
| LEAD()         | Next row value                        |
| LAG()          | Previous row value                    |
| FIRST_VALUE()  | First value in partition              |
| LAST_VALUE()   | Last value in partition               |
| PARTITION BY   | Create groups without collapsing rows |

## Interview Rule of Thumb
| Requirement           | Best Function               |
| --------------------- | --------------------------- |
| Unique Row Number     | ROW_NUMBER()                |
| Rank with Ties        | RANK()                      |
| Rank without Gaps     | DENSE_RANK()                |
| Previous Row          | LAG()                       |
| Next Row              | LEAD()                      |
| Running Total         | SUM() OVER()                |
| Top N Per Group       | ROW_NUMBER() + PARTITION BY |
| Second Highest Salary | DENSE_RANK()                |
