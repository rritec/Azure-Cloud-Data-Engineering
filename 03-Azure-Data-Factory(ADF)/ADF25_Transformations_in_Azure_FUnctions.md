# Azure Functions

1. Azure Functions is a cloud service available on-demand that provides all the continually updated infrastructure and resources needed to run your applications. You focus on the code that matters most to you, in the most productive language for you, and Functions handles the rest. Functions provides serverless compute for Azure. You can use Functions to build web APIs, respond to database changes, process IoT streams, manage message queues, and more.

## Install Required Tools

1. Refer [document](https://learn.microsoft.com/en-us/azure/azure-functions/create-first-function-vs-code-python) install python/vs code/python Extension/ Azure Function Extension

## Create Azure Function App

1. open portal.azure.com
2. go to free services
3. click on function **Create**

![image](https://github.com/user-attachments/assets/d537d99f-ca1e-41dd-81ef-e02699b80c49)

4. Click on Select
5. Create new Resource group as **rritecFinApprg**
6. Provide function app name as **rritecFunApp**
7. Select runtime stack as python
8. Click on Review and Create
9. Click on Create
10. Observe all the services under this resource group **rritecFinApprg**

## Create Hello http Trigger

1. Open VS Code
2. Left Side Menu bar > click on **Azure** > Login with azure username and password

![image](https://github.com/user-attachments/assets/d5ee7dba-7811-4bb0-b708-85ccea5a9c73)

3. In Workspace Pane > Click on Azure Functions > Click on create new project

![image](https://github.com/user-attachments/assets/db03b47c-2069-4207-8ebc-199312b4ef87)

4. create a folder anywhere and select it
5. Select language as **Python**
6. python Programming Model as **Model V2**
7. Select python version
8. Select HTTP Trigger
9. provide name as http_trigger > Press Enter
10. Select Authorization level as **Anonymous**
11. Click on **add to workspace** > press Enter
12. Observe all the files created in Explorer window > mainily observe **function_app.py** file
13. Click on Azure Menu > Under workspce pane > inline to Local project > click on **Deploy to Azure**
14. Select function app
15. Click on deploy
16. Go to portal.azure.com > naviagate to function > get function URL
17. paste in browser type **name=ram** > and observe output

## Create http Trigger to do ETL using python

1. In VS Code > In workspace Menu
2. Click on Azure Functions > Click on Create Function
3. Click on HTTP trigger > Add http trigger function to an existing file > name it as etl > Select Anonymous >
4. Change etl code as shown below > below logging.info line delete all the code add below code
5. Note: Change Azure SQL Database Servername/Database Name/UserName/Password
```py
connection_string = "Driver={ODBC Driver 18 for SQL Server};Server=tcp:b2410asdbserver.database.windows.net,1433;Database=b2410asdb;Uid=sadmin;Pwd=RRitec123;Encrypt=yes;TrustServerCertificate=yes;Connection Timeout=30;"
    query = "select * from dbo.emp"  
    conn = pyodbc.connect(connection_string)
    cursor = conn.cursor()
    df = pd.read_sql(query, conn)
    df["COMM"]=df["COMM"].fillna(0)
    df["TOTALSAL"] = df["SAL"]+df["COMM"]
    #df.to_sql("tgtemp20241121", conn, index=False, if_exists='replace')
    for index, row in df.iterrows():
        #print(index)
        #print(row)
        cursor.execute("INSERT INTO dbo.tgt_azure_fun(ENAME,SAL,COMM,TOTALSAL) values(?,?,?,?)", row.ENAME, row.SAL, row.COMM,row.TOTALSAL)
    conn.commit()
    cursor.close()
    conn.close()
    return func.HttpResponse("ETL completed")
```
5. Also add two imports in the top of the file after import logging >> save the file
```py
import pyodbc
import pandas as pd
```
6. add below two modules in requirments.txt file > save the file
```py
pandas
pyodbc
```

7. Under workspace click on deploy to azure
8. Create target table with below DDL
```sql
create table dbo.tgt_azure_fun(
ENAME varchar(100),
SAL int,
COMM int,
TOTALSAL int )
```
10. select etl url paste in the browser > enter
11. notice that few records might be created in the target table **tgt_azure_fun**

## Call Azure Function in ADF

1. create new pipeline
2. drag and drop azure function activity
3. Select Azure Function Activity > Click on New linked service > name it as lsaf > Select Azure subscription > Select Azure Function app URL >> Click on Create
4. Provide function name as etl
5. mehod as post
6. body as {name:ram}
7. click on debug
8. observe output

## Questions
1. how to return list as azure function output
2. how to return dataFrame as azure function output
## Answers
1. use this code
``` py
@app.route(route="return_list", auth_level=func.AuthLevel.ANONYMOUS)
def return_list(req: func.HttpRequest) -> func.HttpResponse:
    logging.info('Python HTTP trigger function processed a request.')
    my_list = [
        {"name": "Alice", "age": 30},
        {"name": "Bob", "age": 25},
        {"name": "Charlie", "age": 35}
    ]

    # Convert the list to a JSON string
    json_data = json.dumps(my_list)    
    return func.HttpResponse(json_data, mimetype="application/json")
```
2. use this code
``` py
@app.route(route="return_df", auth_level=func.AuthLevel.ANONYMOUS)
def return_df(req: func.HttpRequest) -> func.HttpResponse:
    logging.info('Python HTTP trigger function processed a request.')
    # Create a sample DataFrame
    data = {'Name': ['Alice', 'Bob', 'Charlie'],
            'Age': [30, 25, 35]}
    df = pd.DataFrame(data)

    # Convert DataFrame to JSON
    json_data = df.to_json(orient='records')
    return func.HttpResponse(json_data, mimetype="application/json")
    
```
3. 






