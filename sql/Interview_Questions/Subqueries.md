# Employee Table
| EmpID | Name  | Department | Salary |
| ----- | ----- | ---------- | ------ |
| 1     | Ravi  | HR         | 50000  |
| 2     | Priya | HR         | 70000  |
| 3     | Arun  | IT         | 60000  |
| 4     | Kiran | IT         | 80000  |
| 5     | Meena | IT         | 60000  |

# 1. Find Employees Earning Above Average Salary

```
SELECT *
FROM Employees
WHERE Salary >
(
    SELECT AVG(Salary)
    FROM Employees
);
```
Step1:
```
SELECT AVG(Salary)
FROM Employees;
```

# 2. Find Employee(s) with Highest Salary

```
SELECT *
FROM Employees
WHERE Salary =
(
    SELECT MAX(Salary)
    FROM Employees
);
```

# 3. Find Employee(s) with Lowest Salary

```
SELECT *
FROM Employees
WHERE Salary =
(
    SELECT MIN(Salary)
    FROM Employees
);
```

# 4. Find Second Highest Salary

```
SELECT MAX(Salary)
FROM Employees
WHERE Salary <
(
    SELECT MAX(Salary)
    FROM Employees
);
```

# 5. Find Third Highest Salary

```
SELECT MAX(Salary)
FROM Employees
WHERE Salary <
(
    SELECT MAX(Salary)
    FROM Employees
    WHERE Salary <
    (
        SELECT MAX(Salary)
        FROM Employees
    )
);
```

# 6. Find Employees Earning More Than Department Average

Correlated Subquery

```
SELECT *
FROM Employees E
WHERE Salary >
(
    SELECT AVG(Salary)
    FROM Employees
    WHERE Department = E.Department
);
```

# 7. Find Departments Having More Than One Employee

```
SELECT Department
FROM Employees
GROUP BY Department
HAVING COUNT(*) >
(
    SELECT 1
);
```

# 8. Find Employees Belonging to Department with Highest Average Salary

```
SELECT *
FROM Employees
WHERE Department =
(
    SELECT Department
    FROM Employees
    GROUP BY Department
    ORDER BY AVG(Salary) DESC
    LIMIT 1
);
```

# 9. Find Employees Not Earning Maximum Salary

```
SELECT *
FROM Employees
WHERE Salary <>
(
    SELECT MAX(Salary)
    FROM Employees
);
```

# 10. Find Employees Earning Same Salary as Another Employee

```
SELECT *
FROM Employees
WHERE Salary IN
(
    SELECT Salary
    FROM Employees
    GROUP BY Salary
    HAVING COUNT(*) > 1
);
```

# 11. Find Duplicate Salaries

```
SELECT Salary
FROM Employees
GROUP BY Salary
HAVING COUNT(*) > 1;
```

# 12. Find Employees Working in HR Department

Employees:
| EmpID | Name  | DeptID |
| ----- | ----- | ------ |
| 1     | Ravi  | 101    |
| 2     | Priya | 102    |
| 3     | Arun  | 101    |

Departments:
| DeptID | DeptName |
| ------ | -------- |
| 101    | HR       |
| 102    | IT       |

```
SELECT *
FROM Employees
WHERE DeptID =
(
    SELECT DeptID
    FROM Departments
    WHERE DeptName = 'HR'
);
```

# 13. Find Customers Who Have Orders

customers:
| CustomerID | Name  |
| ---------- | ----- |
| 1          | Ravi  |
| 2          | Priya |
| 3          | Arun  |

orders:
| OrderID | CustomerID |
| ------- | ---------- |
| 101     | 1          |
| 102     | 2          |

```
SELECT *
FROM Customers C
WHERE EXISTS
(
    SELECT 1
    FROM Orders O
    WHERE O.CustomerID = C.CustomerID
);
```

# 14. Find Customers Without Orders
```
SELECT *
FROM Customers C
WHERE NOT EXISTS
(
    SELECT 1
    FROM Orders O
    WHERE O.CustomerID = C.CustomerID
);
```

# 15. Find Employees Earning More Than Ravi
```
SELECT *
FROM Employees
WHERE Salary >
(
    SELECT Salary
    FROM Employees
    WHERE Name = 'Ravi'
);
```

# 16. Find Department with Maximum Employees
```
SELECT Department
FROM Employees
GROUP BY Department
HAVING COUNT(*) =
(
    SELECT MAX(EmployeeCount)
    FROM
    (
        SELECT COUNT(*) AS EmployeeCount
        FROM Employees
        GROUP BY Department
    ) X
);
```

# 17. Find Employee(s) with Salary Greater Than All HR Employees
```
SELECT *
FROM Employees
WHERE Salary >
ALL
(
    SELECT Salary
    FROM Employees
    WHERE Department = 'HR'
);
```

# 18. Find Employee(s) with Salary Greater Than Any HR Employee
```
SELECT *
FROM Employees
WHERE Salary >
ANY
(
    SELECT Salary
    FROM Employees
    WHERE Department = 'HR'
);
```

# 19. Find Employees Whose Salary Exists in Another Department
```
SELECT *
FROM Employees E1
WHERE EXISTS
(
    SELECT 1
    FROM Employees E2
    WHERE E1.Salary = E2.Salary
    AND E1.Department <> E2.Department
);
```

# 20. Find Nth Highest Salary
Generic Pattern
```
SELECT Salary
FROM
(
    SELECT DISTINCT Salary
    FROM Employees
    ORDER BY Salary DESC
    LIMIT N
) X
ORDER BY Salary
LIMIT 1;
```

# difference
| EXISTS                    | IN                        |
| ------------------------- | ------------------------- |
| Checks existence          | Compares values           |
| Stops after first match   | Checks complete result    |
| Better for large datasets | Better for small datasets |
| Often faster              | Can be slower             |


# Top 10 Subquery Interview Questions
| Question                  | Concept             |
| ------------------------- | ------------------- |
| Highest Salary            | MAX()               |
| Second Highest Salary     | Nested Subquery     |
| Third Highest Salary      | Nested Subquery     |
| Nth Highest Salary        | Nested Query        |
| Above Average Salary      | AVG()               |
| Department Average Salary | Correlated Subquery |
| Customers With Orders     | EXISTS              |
| Customers Without Orders  | NOT EXISTS          |
| Salary Greater Than Any   | ANY                 |
| Salary Greater Than All   | ALL                 |

# Quick Revision
| Keyword             | Purpose                |
| ------------------- | ---------------------- |
| IN                  | Match multiple values  |
| EXISTS              | Check existence        |
| NOT EXISTS          | Check absence          |
| ANY                 | At least one match     |
| ALL                 | Every value match      |
| Correlated Subquery | Depends on outer query |
| Nested Subquery     | Query inside query     |
| Scalar Subquery     | Returns one value      |
| Multi-row Subquery  | Returns many values    |
