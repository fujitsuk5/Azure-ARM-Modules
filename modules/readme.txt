# Azure-ARM-Modules
A respository for the creation of Micorosoft Azure Arm Modules to be used in Nested Arm Templates


## Command to deploy arm template

    az group deployment create --parameters azuredeploy.parameters.json --template-file azuredeploy.json --resource-group $rg --name $job

    where $rg environment variable is the name of your Azure resource group and the $job environment variable is the name of the job


The location used by each module defaults to the same region as the specified resource group. This can be overriden on an individual
module basis, by changing the location value from 'null' to your region of choice, within the parameters.

If your naming convention requires the inclusion of a suffix to identity each type of Azure resource, then rather than specify this 
suffix in the name of each component, simply update the suffixes section of the parameters file, with the own naming standard. 
Note: the arm template does not perform any kind of validation so it's your responsiblity to ensure your naming convention aderes to Azure
naming standards

If required, duplicate modules to create multiple tasks if requried. update this later



## Virtual Network Modules


#az-createApplicationSecurityGroups Module
 
    Description: Creates a list of 

    Dependencies:
        none

    azuredeploy.json:

        "parameters": {

        "requireCreateApplicationSecurityGroups" : {
            "type" : "string",
            "allowedValues": [
                "Yes",
                "No"
            ]
        },
        "moduleCreateApplicationSecurityGroups" : {
            "type" : "object"
        }
        "suffixes" : {
            "type" : "object"
        },
        "customTages": {
            "type": "object"
        }
        "variables": {
            "moduleStoreFBURL": "https://raw.githubusercontent.com/fujitsuk5/Azure-ARM-Modules/TidyUp/modules/",
            "module-CreateApplicationSecurityGroups-URL": "[concat(variables('moduleStoreFBURL'),'virtualNetworks/', 'az-createApplicationSecurityGroups.json')]",
        "resources": [
        {
            "apiVersion": "2018-05-01",
            "name": "AllASGSForResourceGroup",
            "comments": "Calling Module to deploy multiple Application Security Groups-v1.00",
            "type": "Microsoft.Resources/deployments",
            "condition": "[equals(parameters('requireCreateApplicationSecurityGroups'), 'Yes')]",
            "properties" : {
                "mode": "Incremental",
                "templateLink": {
                    "uri": "[variables('module-CreateApplicationSecurityGroups-URL')]",
                    "contentVersion": "1.0.0.1"
                },
                "parameters": {
                    "moduleCreateApplicationSecurityGroups": { "value": "[parameters('moduleCreateApplicationSecurityGroups')]"},
                    "customTages": {
                        "value": "[parameters('customTages')]"
                    },
                    "suffixes" : {
                        "value": "[parameters('suffixes')]"
                    }
                }
            }
        },

    azuredeploy.parameters.json:

        "parameters": {
        
        "requireCreateApplicationSecurityGroups": {
            "value" : "Yes" 
        },
        "moduleCreateApplicationSecurityGroups": { 
            "value" : {
                "location" : null,
                "groupNames" : [
                    {
                        "name": "tier1ASG"
                    },
                    {
                        "name": "tier2ASG"
                    },
                    {
                        "name": "tier3ASG"
                    },
                    {
                        "name": "tier4ASG"
                    }
                ]
            } 
        },
        "customTages": {
            "value": {
                "Application": "Finance",
                "Environment": "Production",
                "Version": "1.0.1",
                "Creation Date": "30/10/2018",
                "Creation By": "Ian Purvis",
                "End Date": "01/11/2018",
                "Owner": "Steve Owens",
                "Cost Centre": "CNS",
                "Customer": "MAB",
                "Project": "Khamelon",
                "Data Classificiation":"Secret"
            }
        },
        "suffixes": {
            "value": { 
                "asgSuffix": "-asg",
                "nsgSuffix": "-nsg",
                "publicIPSuffix": "-pip",
                "nicSuffix": "-nic",
                "storageAccountSuffix": "stg",
                "vnetSuffix": "-vnet",
                "subnetSuffix": "-snet",
                "vmSuffix": "-vm",
                "osDiskSuffix": "-osdisk",
                "osDataDiskSuffix": "-datadisk",
                "applicationGatewaySuffix": "-agw"
            }
        }

az-createNetworkSecurityGroupWithASGBasedRules
az-createNetworkSecurityGroupWithIPBasedRules
az-createPublicIPAddresses
az-createNetworkInterfaces
az-createVNetsWithSubnets
az-createApplicationGateways
az-createWAFApplicationGateways

## Storage Modules
az-createStorageAccounts
az-createBlobContainers