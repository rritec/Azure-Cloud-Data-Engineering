# Transformations in Stored Procedure

1. If you are good with coding do transformations inside stored procedure, that means we do not required dataflows.

## create schama and table 
```sql
drop table if exists emp

CREATE TABLE [dbo].[EMP]
(EMPNO INT PRIMARY KEY,
ENAME VARCHAR(20),
JOB VARCHAR(20),
MGR INT,
HIREDATE DATE,
SAL MONEY,
COMM MONEY,
DEPTNO INT );



INSERT INTO [dbo].[EMP] VALUES (7369, 'SMITH', 'CLERK', 7902, '17-DEC-1980', 800, NULL, 20);

INSERT INTO [dbo].[EMP] VALUES (7499, 'ALLEN', 'SALESMAN', 7698, '20-FEB-1981', 1600, 300, 30);

INSERT INTO [dbo].[EMP] VALUES (7521, 'WARD', 'SALESMAN', 7698, '22-FEB-1981', 1250, 500, 30);

INSERT INTO [dbo].[EMP] VALUES (7566, 'JONES', 'MANAGER', 7839, '2-APR-1981', 2975, NULL, 20);

INSERT INTO [dbo].[EMP] VALUES (7654, 'MARTIN', 'SALESMAN', 7698, '28-SEP-1981', 1250, 1400, 30);

INSERT INTO [dbo].[EMP] VALUES (7698, 'BLAKE', 'MANAGER', 7839, '1-MAY-1981', 2850, NULL, 30);

INSERT INTO [dbo].[EMP] VALUES (7782, 'CLARK', 'MANAGER', 7839, '9-JUN-1981', 2450, NULL, 10);

INSERT INTO [dbo].[EMP] VALUES (7788, 'SCOTT', 'ANALYST', 7566, '09-DEC-1982', 3000, NULL, 20);

INSERT INTO [dbo].[EMP] VALUES (7839, 'KING', 'PRESIDENT', NULL, '17-NOV-1981', 5000, NULL, 10);

INSERT INTO [dbo].[EMP] VALUES (7844, 'TURNER', 'SALESMAN', 7698, '8-SEP-1981', 1500, 0, 30);

INSERT INTO [dbo].[EMP] VALUES (7876, 'ADAMS', 'CLERK', 7788, '12-JAN-1983', 1100, NULL, 20);

INSERT INTO [dbo].[EMP] VALUES (7900, 'JAMES', 'CLERK', 7698, '3-DEC-1981', 950, NULL, 30);

INSERT INTO [dbo].[EMP] VALUES (7902, 'FORD', 'ANALYST', 7566, '3-DEC-1981', 3000, NULL, 20);

INSERT INTO [dbo].[EMP] VALUES (7934, 'MILLER', 'CLERK', 7782, '23-JAN-1982', 1300, NULL, 10);

INSERT INTO [dbo].[EMP] VALUES (1234, 'Ram', 'CLERK', 7782, '23-JAN-1982', 1400, NULL, 50);
```
## Create Stored Procedure

1. create Stored procedure inside database


```sql
-- Drop procedure if it exists
IF OBJECT_ID('dbo.emp_total_sal', 'P') IS NOT NULL
    DROP PROCEDURE dbo.emp_total_sal;
GO

-- create procedure
CREATE PROCEDURE emp_total_sal
AS
BEGIN
-- Drop ResultTable if it exists
IF OBJECT_ID('dbo.ResultTable', 'U') IS NOT NULL
  DROP TABLE dbo.ResultTable;
select empno,ename,sal,comm,sal+isnull(comm,0) as total_sal 
into ResultTable
from emp
END
```
## Call this Stored procedure and verify results
``` sql
-- Call the stored procedure
EXEC emp_total_sal;

-- Check the contents of the ResultTable
SELECT * FROM ResultTable;

```
## Map this stored procedure in ADF

  ![image](https://github.com/rritec/Cloud-Data-Engineering/assets/20516321/34935fa4-9b74-4d0e-ac16-be5def3e95a9)

4. Run and observe it.
   ![image](https://github.com/rritec/Cloud-Data-Engineering/assets/20516321/5833de7f-ac19-4845-803b-82161e952769)

5. 
## Questions
## Answers
