# Logic Apps

**Step 1: Create email workflow endpoint using Gmail**

![image](https://user-images.githubusercontent.com/20516321/208333982-d43d8332-1a6d-44af-809d-d26f25204ce2.png)

1. In search box type **Logic Apps** > Under **services** > Click on **Logic Apps**
2. Click on **Add** > Under **Consumption** > Select **Multi-tenant** > Click on **Select**
3. Provide required information as for your subscription
4. ![image](https://user-images.githubusercontent.com/20516321/209438015-aaf01251-0a7c-4f75-93f8-5ac4e009cd2d.png)
3. Click on **review&create**
4. After Deployment completed > click on **Goto resource** > Click on **Edit**
5. Click on **Add a Trigger** > Search for  **When a HTTP request is received**

    ![image](https://github.com/user-attachments/assets/679623fd-925f-4ffb-8dd4-dc9993569343)
 
    
6. Under **Request body json Schema** > Copy and paste below **json** code 

``` json 
{
    "properties": {
        "dataFactoryName": {
            "type": "string"
        },
        "pipelineName": {
            "type": "string"
        },
        "status": {
            "type": "string"
        },
        "message": {
            "type": "string"
        }
    },
    "type": "object"
}
 
```
7. At the top of the Logic Apps Designer > Click on **Save** 
8. You can now see the URL of your HTTP request
9. Select the copy icon to copy it 
10. Paste in your **notepad.**
11. We will use this URL later in ADF pipeline web activity
12. ![image](https://user-images.githubusercontent.com/20516321/209442383-618a03ed-462e-46df-a3bb-53a217319f16.png)
13. Click on **New step** > type **Gmail** in the actions search box > Search and select **Send email (V2)** 
14. Type connection name as **sendstatusmail** as per below dialog box
15. Click on **sign in** > Give you **gmail userid and password**.
16. Type TargetGmail in **To** address 
17. click on **Add parameter** and in drop down select **Body** and **subject**.
18. ![image](https://user-images.githubusercontent.com/20516321/211259603-dfa11d6f-b986-41ca-9c1d-4e91b64fcd29.png)

**Step 2: Call Logic app inside the Pipeline to send Succeeded email**
   
1. Go to **Azure Data Factory** > click **Launch Studio** > Click on **Author** 
2. Open the pipeline **p01_Create Pipenine from Blob to Azure Sql Database**
3. In Activities Search for **Web** and drag into workflow.
4. ![image](https://user-images.githubusercontent.com/20516321/209440556-3604c90e-3284-4f8a-a610-2ab37e09378f.png)
5. Click on **web** > In **General** tab give name of Activity **successmail** > open settings tab past **URL** from notepad
6. Select **Method** as **POST**
7. Click on **Body** > Click on **Add dynamic expression**. > Copy and paste below json code > Click on **ok**
``` json
{
    "dataFactoryName":"@{pipeline().DataFactory}",
    "pipelineName":"@{pipeline().Pipeline}",
    "status":"@{activity('Copy data from blob to azuresqldb').output.executionDetails[0].status}",
    "message":"The Number of rows Read from Source is:  @{activity('Copy data from blob to azuresqldb').output.rowsRead} <br/>
    The Number of rows Copied to target is: @{activity('Copy data from blob to azuresqldb').output.rowsCopied}"
}
```
7.  Click on **Headers** **+ New** > Provide **Name** as **Content-Type** > Provide **Value** as **application/json**
14. Click on **Debug**
15. Observe Mail.
16. ![image](https://user-images.githubusercontent.com/20516321/211261313-781433ed-c256-46aa-a912-889f0ed0e4f3.png)


**Step 3: Call Logic app inside the Pipeline to send Failed email**

1. Inside the pipeline click on web activity **clone**
2. In **General** tab give name of Activity **Failedmail** 
7. Click on **Body** > Click on **Add dynamic expression**. > Copy and paste below json code > Click on **ok**
``` json
{
    "dataFactoryName": "@{pipeline().DataFactory}",
    "pipelineName": "@{pipeline().Pipeline}",
    "status": "@{activity('Copy data from blob to azuresqldb').output.executionDetails[0].status}",
    "message": "The Error Message is : @{activity('Copy data from blob to azuresqldb').output.errors[0].Message}"
    
}

```
7.  Select **Copy Data** activity > Click on **Source** > Click on **open**
8.  Change file name as **emp1111111.csv**(Note: this file is not availble inside our blob)
9.  Click on **Debug**
15. Observe Mail.
16. ![image](https://user-images.githubusercontent.com/20516321/211262341-951c70f4-6676-43cb-bcea-2dac4562eb6c.png)

Refer for more information [msft doc](https://learn.microsoft.com/en-us/azure/data-factory/tutorial-control-flow-portal)
