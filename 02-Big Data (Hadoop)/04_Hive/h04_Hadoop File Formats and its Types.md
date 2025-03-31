# Hadoop File Formats and its Types

1. Hadoop file formats are
    1. text file
    2. orc file [read more](https://cwiki.apache.org/confluence/display/hive/languagemanual+orc)
    3. parquet file
    4. avro file
2. Load data from text file to orc file storage table
    1. load data from text file to dept_text;
        ``` sql
        create table dept_text(deptno int,dname string,loc string)
         row format delimited
         fields terminated by ',';
        ```
        ``` sql
        load data local inpath '/home/cloudera/Desktop/b2023jand/dept.txt' into table dept_text;

        ```
    2. load data from table to orc storage tale
        ``` sql
        create table dept_orc(deptno int,dname string,loc string)
        row format delimited
        fields terminated by ','
        stored as orc;
        ```
        ``` sql 
        insert into dept_orc select * from dept_text;
        ```
3.  load data from table to parquet storage tale

    ``` sql
    create table dept_parquet(deptno int,dname string,loc string)
    row format delimited
    fields terminated by ','
    stored as parquet;
    ```
    ``` sql 
    insert into dept_parquet select * from dept_text;
    ```
4. Create table directly from orc file

    ``` sql
    create external table dept2(deptno int,dname string,loc string)
    stored as orc location "/user/hive/warehouse/b2210.db/dept_orc/";    
    ```
5. Create table directly from parquet file

    ``` sql
    create external table dept3(deptno int,dname string,loc string)
     stored as parquet
     location "/user/hive/warehouse/b2210.db/dept_parquet/";
    ```
6. 
