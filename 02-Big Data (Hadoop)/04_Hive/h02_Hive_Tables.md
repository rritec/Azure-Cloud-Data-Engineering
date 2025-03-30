# Hive Managed Tables
1. Hive Tables are two types
    1. Managed Tables or Internal Tables
    2. External Tables or Non Managed Tables
3. **Create managed table or internal table**
    1. Open terminal ```type hive```
    2. Refer document for more information [help](https://cwiki.apache.org/confluence/display/Hive/LanguageManual+DDL#LanguageManualDDL-Create/Drop/Alter/UseDatabase)
    3. create table using below script
    
        ``` sql 

        Create table if not exists emp (empno int,ename string,sal int) row format delimited fields terminated by ',';

        ```
    4. Observe Schema of table

        ``` sql
        describe emp;
        ```
        ``` sql
        desc emp;
        ```
        ``` sql
        desc formatted emp;
        ```
    5. Observe data
        ``` sql
        select * from emp;
        ```
    6. load data from **file** into **table**
        1. In linux desktop > right click open terminal
        2. create a file `vi emp.txt`
        3. enter data
            ``` 
            101,ram,1000
            102,john,2000
            103,marc,3000
            ```
            
        3. type `:wq!`
        4. Copy file from linux to hadoop `hdfs dfs -put -f /home/cloudera/Desktop/emp.txt /user/cloudera/`
        5. in hive terminal type 
            ``` sql
            load data inpath '/user/cloudera/emp.txt' into table emp;
            ```
        6. the file '/user/cloudera/emp.txt' available in this location?
            - No
        7. observe data
            ``` sql 
            select * from emp limit 2;
            ```
            
        7. to see column titles `set hive.cli.print.header=true;` 
        8. observe data
            ``` sql 
            select * from emp limit 2;
            ```
        9. To see only column names without table names `set hive.resultset.use.unique.column.names=false;`
        10. observe data
            ``` sql 
            select * from emp limit 2;
            ```       
    7. Observe table in **metastore** 
        ``` sql
        mysql -uroot -pcloudera
        ```
        ``` sql
        use metastore;
        ```
        ``` sql
        select * from TBLS
        ```
    8. Observe `emp.txt` in warehouse path `hdfs dfs -ls /user/hive/warehouse/rritecdb.db/emp/`
    9. If we drop **managed_table** hive metastore and hdfs warehouse will be cleared.
        1. drop the table
            ``` sql
            drop table emp;
            ```
        2. Note that metastore table `TBLS` deleted the row of `emp`
        3. Note that `emp.txt` deleted from wareouse path

 # Hive External Tables

1. it is also called as `non-mangaed` table
2. Open terminal ```type hive```
3. create table using below script

``` sql 
Create external table if not exists emp_ext (empno int,ename string,sal int) row format delimited fields terminated by ',';
```
3. Observe Schema of table

``` sql
describe emp_ext;
```
``` sql
desc emp_ext;
```
``` sql
desc formatted emp_ext;
```
4. Observe data

``` sql
select * from emp_ext;
```
5. load data from **file** into **table**

6. In linux desktop > right click open terminal
7. create a file `vi emp_ext.txt`
8. enter data
    ``` 
    101,ram,1000
    102,john,2000
    103,marc,3000
    ```

9. type `:wq!`
10. in hive terminal type 
    ``` sql
    load data local inpath '/home/cloudera/Desktop/emp_ext.txt' into table emp_ext;
    ```
11. the file '/home/cloudera/Desktop/emp.txt' available in this location?
    - Yes

12. observe data

    ``` sql 
    select * from emp_ext limit 2;
    ```

13. to see column titles `set hive.cli.print.header=true;` 
14. observe data
    ``` sql 
    select * from emp_ext limit 2;
    ```
15. To see only column names without table names `set hive.resultset.use.unique.column.names=false;`
16. observe data

    ``` sql 
    select * from emp_ext limit 2;
    ```       
17. Observe table in **metastore**

``` sql
mysql -uroot -pcloudera
```
``` sql
use metastore;
```
``` sql
select * from TBLS
```
18. Observe `emp.txt` in warehouse path `hdfs dfs -ls /user/hive/warehouse/rritecdb.db/emp/`
19. If we drop **external_table** hive metastore will be cleared and hdfs warehouse will **not** be cleared.
20. drop the table

``` sql
drop table emp_ext;
```

21. Note that metastore table `TBLS` deleted the row of `emp`
22. Note that `emp_ext.txt` not deleted from wareouse path
23. To delete type `hdfs dfs -rm -r -f /user/hive/warehose/rritecdb.db/emp_ext`
# Questions
1. Types of tables in hive ?
   
2. If we drop internal table what will happens ?
    
3. If we drop External table what will happens ?
    
4. When to go for external table ?
5.  
# Answers
1. Two types of tables available thoses are manged tables and extrnal tables
2. Data and Metadata both will be removed
3. Metadata only will be removed.
4. If data is not owned by hive application and there are other downstream applications(ex:powerbi,..etc) are using then go for External table.
