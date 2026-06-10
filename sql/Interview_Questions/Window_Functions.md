# Find Highest Salary Employee in Each Department

```
WITH RankedEmployees AS
(
    SELECT *,
           ROW_NUMBER()
           OVER(
              PARTITION BY Department
              ORDER BY Salary DESC
           ) AS RN
    FROM Employees
)
SELECT *
FROM RankedEmployees
WHERE RN = 1;
```

# Find Second Highest Salary

```
SELECT *
FROM
(
    SELECT Name,
           Salary,
           DENSE_RANK()
           OVER(ORDER BY Salary DESC) AS DR
    FROM Employees
) X
WHERE DR = 2;
```

# Top 3 Salaries

```
SELECT *
FROM
(
    SELECT Name,
           Salary,
           DENSE_RANK()
           OVER(ORDER BY Salary DESC) AS DR
    FROM Employees
) X
WHERE DR <= 3;
```

# Compare Salary with Previous Employee

```
SELECT Name,
       Salary,
       Salary -
       LAG(Salary)
       OVER(ORDER BY Salary) AS Difference
FROM Employees;
```