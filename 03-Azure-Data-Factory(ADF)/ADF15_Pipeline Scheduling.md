# Triggers

1. A pipeline run in Azure Data Factory and Azure Synapse defines an instance of a pipeline execution.
2. For example, say you have a pipeline that executes at 8:00 AM, 9:00 AM, and 10:00 AM. In this case, there are three separate runs of the pipeline. Each pipeline run has a unique pipeline run ID.
3. A run ID is a GUID that uniquely defines that particular pipeline run
4. types of triggers
    1. Scheduled
    2. Tumbling window
    3. Storage event
    4. Custom event
5. Pipelines and triggers have a many-to-many relationship (except for the tumbling window trigger). 
6. Multiple triggers can kick off a single pipeline, or a single trigger can kick off multiple pipelines

## 1. Schedule Trigger
1. Click on **Manage** > Click on **Triggers** > Click on **New** > Provide Below Information

![image](https://github.com/rritec/Cloud-Data-Engineering/assets/20516321/b4bcf012-2a6e-43f5-a401-b4b2fbaa725b)

2. Click on ok

![image](https://github.com/rritec/Cloud-Data-Engineering/assets/20516321/04843300-3ee2-4620-a0ca-622770ac9571)

### Map Schedule Trigger to Pipeline
1. Open any pipeline > Click on **Trigger** > Click on **New/Edit** > From the drop down Select above trigger > Click on **ok**
## 2. Tumbling Window Trigger
1. Click on **Manage** > Click on **Triggers** > Click on **New** > Provide Below Information

![image](https://github.com/rritec/Cloud-Data-Engineering/assets/20516321/1af0f58d-270a-4675-9fd7-436eab358082)

2. Click on ok

![image](https://github.com/rritec/Cloud-Data-Engineering/assets/20516321/7121fa9e-92c8-4ad9-9a25-e1c2e642936c)

3. Map this trigger to any pipeline.

## 3. Storage Event
### Register EventGrid
1. Click on **Subscriptions** > Click on **Azure For Students** > Click on **Resource Providers** > Select **Microsoft.EventGrid** > Click on **Register**

![image](https://github.com/rritec/Cloud-Data-Engineering/assets/20516321/5693bcad-bcf9-4d67-9296-09f233bb0518)


### Create Storage Event
1.  Click on **Manage** > Click on **Triggers** > Click on **New** > Provide Below Information

![image](https://github.com/rritec/Cloud-Data-Engineering/assets/20516321/6d2d8bb0-03bc-4ad0-8c45-f046249b6012)


2. Click on **Continue**

![image](https://github.com/rritec/Cloud-Data-Engineering/assets/20516321/4d003ee9-a3b3-44a6-aab6-696a628c40d7)

3. Click on **ok**

![image](https://github.com/rritec/Cloud-Data-Engineering/assets/20516321/e8c60179-9c49-4c6b-9e95-3a7868e5618f)

4. Map this trigger to any pipeline.
## Questions
1. Difference between tumbling window trigger and the schedule trigger
2. How to start or stop triggers using script ?
## Answers
1. [Trigger type comparison](https://learn.microsoft.com/en-us/azure/data-factory/concepts-pipeline-execution-triggers#trigger-type-comparison)
2. we can use power shell script

``` sh
$DataFactoryName="b25janadf"
$ResourceGroupName="b25janrg"
$DataFactoryName
$ResourceGroupName

$triggersADF = Get-AzDataFactoryV2Trigger -DataFactoryName $DataFactoryName -ResourceGroupName $ResourceGroupName

$triggersADF | ForEach-Object { Stop-AzDataFactoryV2Trigger -ResourceGroupName $ResourceGroupName -DataFactoryName $DataFactoryName -Name $_.name -Force }

$triggersADF | ForEach-Object { Start-AzDataFactoryV2Trigger -ResourceGroupName $ResourceGroupName -DataFactoryName $DataFactoryName -Name $_.name -Force }

```

    - Refer
    - https://learn.microsoft.com/en-us/azure/data-factory/continuous-integration-delivery#manually-promote-a-resource-manager-template-for-each-environment
    - https://learn.microsoft.com/en-us/azure/data-factory/continuous-integration-delivery-automate-azure-pipelines#updating-active-triggers


