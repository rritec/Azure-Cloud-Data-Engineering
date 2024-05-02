# Drop the table
``` sql
drop table dates
```
# Create the table
```sql
create table dates(start_date DATE,end_date DATE)
```
# Insert the values
```sql
INSERT INTO dates (Start_date, End_date)
VALUES
('2024-02-01', '2024-03-31'),
('2024-02-02', '2024-03-31'),
('2024-02-03', '2024-03-31'),
('2024-02-04', '2024-03-31'),
('2024-02-05', '2024-03-31'),
('2024-02-06', '2024-03-31'),
('2024-02-07', '2024-03-31'),
('2024-02-08', '2024-03-31'),
('2024-02-09', '2024-03-31'),
('2024-02-10', '2024-03-31'),
('2024-02-11', '2024-03-31'),
('2024-02-12', '2024-03-31'),
('2024-02-13', '2024-03-31'),
('2024-02-14', '2024-03-31'),
('2024-02-15', '2024-03-31'),
('2024-02-16', '2024-03-31'),
('2024-02-17', '2024-03-31'),
('2024-02-18', '2024-03-31'),
('2024-02-19', '2024-03-31'),
('2024-02-20', '2024-03-31'),
('2024-02-21', '2024-03-31'),
('2024-02-22', '2024-03-31'),
('2024-02-23', '2024-03-31'),
('2024-02-24', '2024-03-31'),
('2024-02-25', '2024-03-31'),
('2024-02-26', '2024-03-31'),
('2024-02-27', '2024-03-31'),
('2024-02-28', '2024-03-31'),
('2024-02-29', '2024-03-31'),
('2024-03-01', '2024-03-31'),
('2024-03-02', '2024-03-31'),
('2024-03-03', '2024-03-31'),
('2024-03-04', '2024-03-31'),
('2024-03-05', '2024-03-31'),
('2024-03-06', '2024-03-31'),
('2024-03-07', '2024-03-31'),
('2024-03-08', '2024-03-31'),
('2024-03-09', '2024-03-31'),
('2024-03-10', '2024-03-31'),
('2024-03-11', '2024-03-31'),
('2024-03-12', '2024-03-31'),
('2024-03-13', '2024-03-31'),
('2024-03-14', '2024-03-31'),
('2024-03-15', '2024-03-31'),
('2024-03-16', '2024-03-31'),
('2024-03-17', '2024-03-31'),
('2024-03-18', '2024-03-31'),
('2024-03-19', '2024-03-31'),
('2024-03-20', '2024-03-31'),
('2024-03-21', '2024-03-31'),
('2024-03-22', '2024-03-31'),
('2024-03-23', '2024-03-31'),
('2024-03-24', '2024-03-31'),
('2024-03-25', '2024-03-31'),
('2024-03-26', '2024-03-31'),
('2024-03-27', '2024-03-31'),
('2024-03-28', '2024-03-31'),
('2024-03-29', '2024-03-31'),
('2024-03-30', '2024-03-31'),
('2024-03-31', '2024-03-31'),
('2024-04-01', '2024-03-31'),
('2024-04-02', '2024-03-31'),
('2024-04-03', '2024-03-31'),
('2024-04-04', '2024-03-31'),
('2024-04-05', '2024-03-31'),
('2024-04-06', '2024-03-31'),
('2024-04-07', '2024-03-31'),
('2024-04-08', '2024-03-31'),
('2024-04-09', '2024-03-31'),
('2024-04-10', '2024-03-31'),
('2024-04-11', '2024-03-31'),
('2024-04-12', '2024-03-31'),
('2024-04-13', '2024-03-31'),
('2024-04-14', '2024-03-31'),
('2024-04-15', '2024-03-31'),
('2024-04-16', '2024-03-31'),
('2024-04-17', '2024-03-31'),
('2024-04-18', '2024-03-31'),
('2024-04-19', '2024-03-31'),
('2024-04-20', '2024-03-31'),
('2024-04-21', '2024-03-31'),
('2024-04-22', '2024-03-31'),
('2024-04-23', '2024-03-31'),
('2024-04-24', '2024-03-31'),
('2024-04-25', '2024-03-31'),
('2024-04-26', '2024-03-31'),
('2024-04-27', '2024-03-31'),
('2024-04-28', '2024-03-31'),
('2024-04-29', '2024-03-31'),
('2024-04-30', '2024-03-31');
```
# Drop the function 
```sql
drop function CalculateWorkingDays
```
# Create the function
```sql
CREATE or alter FUNCTION dbo.CalculateWorkingDays
(
    @startDate DATE,
    @endDate DATE
)
RETURNS INT
AS
BEGIN
    DECLARE @workingDays INT = 0;
    DECLARE @currentDate DATE = @startDate;
    DECLARE @targetDate DATE = @endDate;

    WHILE @currentDate <> @targetDate
    BEGIN
        -- Exclude weekends (Saturday and Sunday)
        IF DATEPART(WEEKDAY, @currentDate) NOT IN (1, 7)
        BEGIN
            SET @workingDays = @workingDays + 1;
        END

        -- Move to the next day
        IF @startDate < @endDate
            SET @currentDate = DATEADD(DAY, 1, @currentDate);
        ELSE
            SET @currentDate = DATEADD(DAY, -1, @currentDate);
    END

    -- Include the end date if it's not a weekend
    IF DATEPART(WEEKDAY, @targetDate) NOT IN (1, 7)
    BEGIN
        SET @workingDays = @workingDays + 1;
    END

    RETURN IIF(@startDate <= @endDate, @workingDays, -@workingDays);
END;
```
# Test the function
```sql
SELECT [dbo].[CalculateWorkingDays]('2024-02-01', '2024-02-07') AS working_days 
```
# Create a stored procedure
```sql
CREATE or alter PROCEDURE GetEmployeeList
AS
BEGIN
    IF OBJECT_ID('Final_table', 'U') IS NOT NULL
    BEGIN
        DROP TABLE Final_table;
    END

    EXEC('
        SELECT *,
               CASE
                   WHEN working_days <= 10 THEN 0
                   WHEN working_days <= 30 THEN 1
                   ELSE 2
               END AS bucketing
        INTO Final_table
        FROM (
            SELECT *,
                   [dbo].[CalculateWorkingDays](start_date, end_date) AS working_days
            FROM dates
        ) AS Working_days
    ');
END;
```
# Execute the stored procedure
```sql
EXEC GetEmployeeList;
```