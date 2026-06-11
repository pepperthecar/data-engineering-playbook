# Stored Procedure
Stored Procedures are one of the most important SQL database objects used in real-world applications and ETL pipelines.

A Stored Procedure is a precompiled collection of SQL statements stored inside the database and executed as a single unit.

Instead of writing the same SQL repeatedly, you create a procedure once and execute it whenever needed.

# Why Use Stored Procedures?

Stored Procedures help you:
- Reuse SQL code
- Centralize business logic
- Improve security
- Reduce network traffic
- Automate ETL processes
- Improve maintainability
- Execute multiple SQL statements together

Employees:
| EmpID | Name  | Department | Salary |
| ----- | ----- | ---------- | ------ |
| 1     | Ravi  | HR         | 50000  |
| 2     | Priya | HR         | 70000  |
| 3     | Arun  | IT         | 60000  |
| 4     | Kiran | IT         | 80000  |
| 5     | Meena | IT         | 60000  |

# Stored Procedures Syntax
```
CREATE PROCEDURE procedure_name
AS
BEGIN

    SQL Statements

END;
```
Execute Procedure
```
EXEC procedure_name;
```

1. Simple Stored Procedure

Display all employees
```
CREATE PROCEDURE GetEmployees
AS
BEGIN

    SELECT *
    FROM Employees;

END;
```
Execute
```
EXEC GetEmployees;
```
Output:
| EmpID | Name  | Department | Salary |
| ----- | ----- | ---------- | ------ |
| 1     | Ravi  | HR         | 50000  |
| 2     | Priya | HR         | 70000  |
| 3     | Arun  | IT         | 60000  |
| 4     | Kiran | IT         | 80000  |
| 5     | Meena | IT         | 60000  |

2. Stored Procedure With Parameters

Parameters make procedures reusable.
```
CREATE PROCEDURE GetEmployeesByDept
    @Department VARCHAR(20)
AS
BEGIN

    SELECT *
    FROM Employees
    WHERE Department = @Department;

END;
```
Execute
```
EXEC GetEmployeesByDept 'IT';
```
Output:
| EmpID | Name  | Department | Salary |
| ----- | ----- | ---------- | ------ |
| 3     | Arun  | IT         | 60000  |
| 4     | Kiran | IT         | 80000  |
| 5     | Meena | IT         | 60000  |

3. Multiple Parameters
```
CREATE PROCEDURE GetEmployeeSalary
(
    @Department VARCHAR(20),
    @MinSalary INT
)
AS
BEGIN

    SELECT *
    FROM Employees
    WHERE Department = @Department
      AND Salary >= @MinSalary;

END;
```
Execute:
```
EXEC GetEmployeeSalary 'IT', 70000;
```
Output:
| Name  | Salary |
| ----- | ------ |
| Kiran | 80000  |

4. INSERT Using Stored Procedure

Add a new employee.

```
CREATE PROCEDURE AddEmployee
(
    @Name VARCHAR(50),
    @Department VARCHAR(20),
    @Salary INT
)
AS
BEGIN

    INSERT INTO Employees
    (
        Name,
        Department,
        Salary
    )
    VALUES
    (
        @Name,
        @Department,
        @Salary
    );

END;
```
Execute
```
EXEC AddEmployee
     'Vijay',
     'Finance',
     65000;
```

5. UPDATE Using Stored Procedure

Update salary.
```
CREATE PROCEDURE UpdateSalary
(
    @EmpID INT,
    @NewSalary INT
)
AS
BEGIN

    UPDATE Employees
    SET Salary = @NewSalary
    WHERE EmpID = @EmpID;

END;
```
Execute
```
EXEC UpdateSalary
     3,
     75000;
```

6. DELETE Using Stored Procedure

```
CREATE PROCEDURE DeleteEmployee
(
    @EmpID INT
)
AS
BEGIN

    DELETE FROM Employees
    WHERE EmpID = @EmpID;

END;
```
Execute
```
EXEC DeleteEmployee 5;
```

7. IF ELSE in Stored Procedure

Business logic can be added.

```
CREATE PROCEDURE CheckSalary
(
    @Salary INT
)
AS
BEGIN

    IF @Salary >= 70000
        PRINT 'High Salary';
    ELSE
        PRINT 'Normal Salary';

END;
```
Execute:
```
EXEC CheckSalary 80000;
```
Output:

High Salary

8. Variables Inside Procedure

```
CREATE PROCEDURE EmployeeCount
AS
BEGIN

    DECLARE @TotalEmployees INT;

    SELECT @TotalEmployees =
           COUNT(*)
    FROM Employees;

    PRINT @TotalEmployees;

END;
```
Output:

5

9. Stored Procedure with Output Parameter

Return values to caller.

```
CREATE PROCEDURE GetEmployeeCount
(
    @TotalCount INT OUTPUT
)
AS
BEGIN

    SELECT @TotalCount =
           COUNT(*)
    FROM Employees;

END;
```
Execute
```
DECLARE @Count INT;

EXEC GetEmployeeCount
     @Count OUTPUT;

PRINT @Count;
```
Output:

5

10. Transactions in Stored Procedures

Useful for banking and ETL.

```
CREATE PROCEDURE TransferMoney
AS
BEGIN

    BEGIN TRANSACTION;

    UPDATE Accounts
    SET Balance = Balance - 1000
    WHERE AccountID = 1;

    UPDATE Accounts
    SET Balance = Balance + 1000
    WHERE AccountID = 2;

    COMMIT;

END;
```
Ensures both updates succeed together.

11. Error Handling

```
CREATE PROCEDURE SafeInsert
AS
BEGIN

    BEGIN TRY

        INSERT INTO Employees
        VALUES
        (
            6,
            'Ramesh',
            'HR',
            55000
        );

    END TRY

    BEGIN CATCH

        PRINT ERROR_MESSAGE();

    END CATCH

END;
```

12. ETL Example

Load staging data into warehouse.
```
CREATE PROCEDURE LoadOrders
AS
BEGIN

    INSERT INTO WarehouseOrders
    SELECT *
    FROM StagingOrders;

END;
```
Execute
```
EXEC LoadOrders;
```

# Advantages
| Advantage         | Description                    |
| ----------------- | ------------------------------ |
| Reusable          | Write once, execute many times |
| Secure            | Hide table access              |
| Faster            | Precompiled execution plans    |
| Centralized Logic | One place for business rules   |
| Easy Automation   | ETL tools can call procedures  |

# Disadvantages
| Disadvantage           | Description                      |
| ---------------------- | -------------------------------- |
| Harder Debugging       | Errors can be difficult to trace |
| Vendor Specific        | Syntax differs by database       |
| Version Control Issues | Logic stored inside database     |
| Complex Procedures     | Can become difficult to maintain |

# Functions vs Stored Procedures
| Function              | Stored Procedure                  |
| --------------------- | --------------------------------- |
| Returns a value       | May or may not return             |
| Can be used in SELECT | Cannot be used in SELECT directly |
| Usually read-only     | Can modify data                   |
| Lightweight           | Can contain complex logic         |

# Quick Revision Table
| Concept              | Purpose                 |
| -------------------- | ----------------------- |
| CREATE PROCEDURE     | Create procedure        |
| EXEC                 | Execute procedure       |
| Parameters           | Accept inputs           |
| OUTPUT               | Return values           |
| Variables            | Store temporary values  |
| IF ELSE              | Conditional logic       |
| TRY CATCH            | Error handling          |
| TRANSACTION          | Ensure data consistency |
| INSERT/UPDATE/DELETE | Modify data             |
| SELECT               | Retrieve data           |

# Interview Rule of Thumb
| Requirement                 | Best Choice          |
| --------------------------- | -------------------- |
| Reusable SQL Logic          | Stored Procedure     |
| Return Single Value         | Function             |
| ETL Pipeline Logic          | Stored Procedure     |
| Error Handling              | TRY-CATCH            |
| Data Consistency            | Transaction          |
| Reusable Report Query       | Stored Procedure     |
| Input Parameters            | Procedure Parameters |
| Return Multiple Result Sets | Stored Procedure     |
