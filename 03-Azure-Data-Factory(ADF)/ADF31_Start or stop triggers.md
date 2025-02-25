# ADF31_Start or stop triggers

``` sh
$DataFactoryName="b25janadf"
$ResourceGroupName="b25janrg"
$DataFactoryName
$ResourceGroupName

$triggersADF = Get-AzDataFactoryV2Trigger -DataFactoryName $DataFactoryName -ResourceGroupName $ResourceGroupName

$triggersADF | ForEach-Object { Stop-AzDataFactoryV2Trigger -ResourceGroupName $ResourceGroupName -DataFactoryName $DataFactoryName -Name $_.name -Force }

$triggersADF | ForEach-Object { Start-AzDataFactoryV2Trigger -ResourceGroupName $ResourceGroupName -DataFactoryName $DataFactoryName -Name $_.name -Force }

```

## Refer
- https://learn.microsoft.com/en-us/azure/data-factory/continuous-integration-delivery#manually-promote-a-resource-manager-template-for-each-environment
- https://learn.microsoft.com/en-us/azure/data-factory/continuous-integration-delivery-automate-azure-pipelines#updating-active-triggers
