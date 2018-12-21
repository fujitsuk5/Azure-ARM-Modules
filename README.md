# Azure-ARM-Modules
A respository for the creation of Microsoft Azure Arm Modules to be used in Nested Arm Templates



Requirements:
All modules will automatically add customTags and suffixes to the resources they create, so setup your initial arm files as shown below. 
If you do not want to add a suffix, then simply set the value to null as shown in the last entry.

#azuredeploy.parameters.json

"$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentParameters.json#",
    "contentVersion": "1.0.0.0",
    "parameters": {

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
                        "Customer": "ITSytems",
                        "Project": "Alpha",
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
                        "applicationGatewaySuffix": "-agw",
                        "availabilitySetSuffix": "-avset",
                        "extLoadbalancerSuffix": "-extlb",
                        "intLoadbalancerSuffix": null
                    }
                }
            }
        }
    }

#azuredeploy.json
{
    "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
    "contentVersion": "1.0.0.1",
    "parameters": {

        "suffixes" : {
            "type" : "object"
        },
        "customTages": {
            "type": "object"
        }
    },
        "outputs": {
    }
}






## moduleCreateApplicationSecurityGroups
#Module to create one or more ASGs

#azuredeploy.parameters.json
1. Specify a parameter, with value Yes to enable or No to disable:

"$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentParameters.json#",
    "contentVersion": "1.0.0.0",
    "parameters": {
        
        "requireCreateApplicationSecurityGroups": {
            "value" : "[Yes or No]" 
        },

2. Specify a parameter for moduleCreateApplicationSecurityGroups that includes the GroupNames you require as shown below. Default location can be changed from that of the Security Group by changing the location from null to a valid azure region i.e. "location" : "westuk".

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

#azuredeploy.json
1. Add the following to your azuredeploy.json to call the module:
{
    "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
    "contentVersion": "1.0.0.1",
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
        },
    },
    "variables": {
        "moduleStoreURL": "https://raw.githubusercontent.com/fujitsuk5/Azure-ARM-Modules/master/modules/",
        "module-CreateApplicationSecurityGroups-URL": "[concat(variables('moduleStoreFBURL' 'virtualNetworks/', 'az-createApplicationSecurityGroups.json')]",
    },
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
        }
     ],
    "outputs": {
    }
}










# moduleCreateApplicationSecurityGroups
#Module to create one or more ASGs

#azuredeploy.parameters.json
1. Specify a parameter, with value Yes to enable or No to disable:

"$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentParameters.json#",
    "contentVersion": "1.0.0.0",
    "parameters": {
        
        "requireCreateApplicationSecurityGroups": {
            "value" : "[Yes or No]" 
        },

2. Specify a parameter for moduleCreateApplicationSecurityGroups that includes the GroupNames you require as shown below. Default location can be changed from that of the Security Group by changing the location from null to a valid azure region i.e. "location" : "westuk".

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

#azuredeploy.json
1. Add the following to your azuredeploy.json to call the module:
{
    "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
    "contentVersion": "1.0.0.1",
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
        },
    },
    "variables": {
        "moduleStoreURL": "https://raw.githubusercontent.com/fujitsuk5/Azure-ARM-Modules/master/modules/",
        "module-CreateApplicationSecurityGroups-URL": "[concat(variables('moduleStoreFBURL' 'virtualNetworks/', 'az-createApplicationSecurityGroups.json')]",
    },
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
        }
     ],
    "outputs": {
    }
}














# moduleCreateApplicationSecurityGroups
#Module to create one or more ASGs

#azuredeploy.parameters.json
1. Specify a parameter, with value Yes to enable or No to disable:

"$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentParameters.json#",
    "contentVersion": "1.0.0.0",
    "parameters": {
        
        "requireCreateApplicationSecurityGroups": {
            "value" : "[Yes or No]" 
        }
    }

2. Specify a parameter for moduleCreateApplicationSecurityGroups that includes the GroupNames you require as shown below. Default location can be changed from that of the Security Group by changing the location from null to a valid azure region i.e. "location" : "westuk".

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

#azuredeploy.json
1. Add the following to your azuredeploy.json to call the module:
{
    "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
    "contentVersion": "1.0.0.1",
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
        },
    },
    "variables": {
        "moduleStoreURL": "https://raw.githubusercontent.com/fujitsuk5/Azure-ARM-Modules/master/modules/",
        "module-CreateApplicationSecurityGroups-URL": "[concat(variables('moduleStoreFBURL' 'virtualNetworks/', 'az-createApplicationSecurityGroups.json')]",
    },
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
        }
     ],
    "outputs": {
    }
}












# moduleCreateNetworkSecurityGroupWithASGBasedRules
# Module to create one or more Network Security Groups with ASG Based Rules
# Dependencies: moduleCreateApplicationSecurityGroups

#azuredeploy.parameters.json
1. Specify a parameter, with value Yes to enable or No to disable:

"$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentParameters.json#",
    "contentVersion": "1.0.0.0",
    "parameters": {
        
        "requireCreateNetworkSecurityGroupWithASGBasedRules": {
            "value" : "Yes" 
        }
    }

2. Specify a nsgName object for each NSG required as shown below. Multiple rule can be specifed by adding additional Rules to the object. destinationApplicationSecurityGroups should reflect the name of an existing ASG created in the previous module. Default location can be changed from that of the Security Group by changing the location from null to a valid azure region i.e. "location" : "westuk".

        "moduleCreateNetworkSecurityGroupWithASGBasedRules": {
            "value": {
                "location" : null,
                "nsgNames" : [
                    {
                        "nsgName": "inboundWebTrafficasg1",
                        "rules": [
                            {
                                "name": "AllowHttpAll",
                                "description": "Allow http traffic to web servers",
                                "sourceAddressPrefix": "*",
                                "sourcePortRange": "*",
                                "direction": "Inbound",
                                "access": "Allow",
                                "priority": 100,
                                "protocol": "Tcp",
                                "destinationPortRange": "80",
                                "destinationApplicationSecurityGroups" : "tier1ASG"
                            },
                            {
                                "name": "AllowRDP",
                                "description": "Allow RDP traffic to web servers",
                                "sourceAddressPrefix": "192.168.1.1",
                                "sourcePortRange": "*",
                                "direction": "Inbound",
                                "access": "Allow",
                                "priority": 101,
                                "protocol": "Tcp",
                                "destinationPortRange": "3389",
                                "destinationApplicationSecurityGroups" : "tier2ASG"
                            },
                            {
                                "name": "AllowSSHl",
                                "description": "Allow SSH traffic to web servers",
                                "sourceAddressPrefix": "192.168.1.1",
                                "sourcePortRange": "*",
                                "direction": "Inbound",
                                "access": "Allow",
                                "priority": 102,
                                "protocol": "Tcp",
                                "destinationPortRange": "22",
                                "destinationApplicationSecurityGroups" : "tier1ASG"
                            }
                        ]
                    },
                    {
                        "nsgName": "inboundWebTrafficasg2",
                        "rules": [
                            {
                                "name": "AllowHttpAll",
                                "description": "Allow http traffic to web servers",
                                "sourceAddressPrefix": "*",
                                "sourcePortRange": "*",
                                "direction": "Inbound",
                                "access": "Allow",
                                "priority": 100,
                                "protocol": "Tcp",
                                "destinationPortRange": "80",
                                "destinationApplicationSecurityGroups" : "tier1ASG"
                            },
                            {
                                "name": "AllowRDP",
                                "description": "Allow RDP traffic to web servers",
                                "sourceAddressPrefix": "192.168.1.1",
                                "sourcePortRange": "*",
                                "direction": "Inbound",
                                "access": "Allow",
                                "priority": 101,
                                "protocol": "Tcp",
                                "destinationPortRange": "3389",
                                "destinationApplicationSecurityGroups" : "tier2ASG"
                            },
                            {
                                "name": "AllowSSHl",
                                "description": "Allow SSH traffic to web servers",
                                "sourceAddressPrefix": "192.168.1.1",
                                "sourcePortRange": "*",
                                "direction": "Inbound",
                                "access": "Allow",
                                "priority": 102,
                                "protocol": "Tcp",
                                "destinationPortRange": "22",
                                "destinationApplicationSecurityGroups" : "tier1ASG"
                            }
                        ]
                    }

                ]
            }
        },

#azuredeploy.json
1. Add the following to your azuredeploy.json to call the module:
{
    "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
    "contentVersion": "1.0.0.1",
    "parameters": {

         "requireCreateNetworkSecurityGroupWithASGBasedRules" : {
            "type" : "string",
            "allowedValues": [
                "Yes",
                "No"
            ]
        }

        "moduleCreateNetworkSecurityGroupWithASGBasedRules" : {
            "type" : "object"
        },
    },
    "variables": {
        "moduleStoreURL": "https://raw.githubusercontent.com/fujitsuk5/Azure-ARM-Modules/master/modules/",
         "module-CreateNetworkSecurityGroupWithASGBasedRules-URL": "[concat(variables('moduleStoreFBURL'),'virtualNetworks/', 'az-createNetworkSecurityGroupWithASGBasedRules.json')]"
    },
     "resources": [
        {
            "apiVersion": "2018-05-01",
            "name": "AllASGBasedNSGForResourceGroup",
            "comments": "Calling Module to deploy a multiple Application Security Group based Network Security Groups-v1.00",
            "type": "Microsoft.Resources/deployments",
            "condition": "[equals(parameters('requireCreateNetworkSecurityGroupWithASGBasedRules'), 'Yes')]",
            "dependsOn": [
                "AllASGSForResourceGroup"
            ],
            "properties" : {
                "mode": "Incremental",
                "templateLink": {
                    "uri": "[variables('module-CreateNetworkSecurityGroupWithASGBasedRules-URL')]",
                    "contentVersion": "1.0.0.1"
                },
                "parameters": {
                    "moduleCreateNetworkSecurityGroupWithASGBasedRules": { "value": "[parameters('moduleCreateNetworkSecurityGroupWithASGBasedRules')]"},
                    "customTages": {
                        "value": "[parameters('customTages')]"
                    },
                    "suffixes" : {
                        "value": "[parameters('suffixes')]"
                    }
                }
            }
        },
     ],
    "outputs": {
    }
}














# moduleCreateStorageAccounts
#Module to create one or more Storage Accounts

#azuredeploy.parameters.json
1. Specify a parameter, with value Yes to enable or No to disable:

"$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentParameters.json#",
    "contentVersion": "1.0.0.0",
    "parameters": {
        
        "requireCreateStorageAccounts": {
            "value" : "Yes" 
        }

2. Define a new accounts object for each StorageAccount Required as shown below. Default location can be changed from that of the Security Group by changing the location from null to a valid azure region i.e. "location" : "westuk".

        "moduleCreateStorageAccounts": {
            "value": {
                "location" : null,
                "accounts": [
                    {
                        "saName": "mystoragegroupisp01",
                        "description": "this is a storage group",
                        "storageAccountType": "Standard_LRS",
                        "storageAccountTier": "Standard",
                        "kind": "StorageV2",
                        "accessTier": "Cool"
                    },
                    {
                        "saName": "mystoragegroupisp02",
                        "description": "this is a storage group",
                        "storageAccountType": "Standard_LRS",
                        "storageAccountTier": "Standard",
                        "kind": "StorageV2",
                        "accessTier": "Cool"
                    }
                ]
            }
        }
        

#azuredeploy.json
1. Add the following to your azuredeploy.json to call the module:
{
    "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
    "contentVersion": "1.0.0.1",
    "parameters": {

        "requireCreateStorageAccounts" : {
            "type" : "string",
            "allowedValues": [
                "Yes",
                "No"
            ]
        },

         "moduleCreateStorageAccounts" : {
            "type" : "object"
        }
    },
    "variables": {
        "moduleStoreURL": "https://raw.githubusercontent.com/fujitsuk5/Azure-ARM-Modules/master/modules/",
         "module-CreateStorageAccounts-URL": "[concat(variables('moduleStoreFBURL'),'virtualNetworks/', 'az-createStorageAccounts.json')]"
    },
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
        }
     ],
    "outputs": {
    }
}










# moduleCreatePublicIPAddresses
# Module to create one or more public ip address

#azuredeploy.parameters.json
1. Specify a parameter, with value Yes to enable or No to disable:

"$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentParameters.json#",
    "contentVersion": "1.0.0.0",
    "parameters": {
        
        "requireCreatePublicIPAddresses": {
            "value" : "Yes" 
        }

2. Define a pips array object for each public ip address required as shown below. Default location can be changed from that of the Security Group by changing the location from null to a valid azure region i.e. "location" : "westuk".

        "moduleCreatePublicIPAddresses": {
            "value": {
                "location" : null,
                "pips": [
                    {
                        "vmName":  "myVM",
                        "pipName": "myVM",
                        "description": "this is a storage group",
                        "publicIPAllocationMethod": "Dynamic",
                        "domainNameLabelprefix": "myrdpserver"
                    },
                    {
                        "vmName":  "myVM2",
                        "pipName": "myVM2",
                        "description": "this is a storage group",
                        "publicIPAllocationMethod": "Dynamic",
                        "domainNameLabelprefix": "myrdpserver2F"
                    },
                    {
                        "vmName":  "myLB1",
                        "pipName": "myLB1",
                        "description": "this is a storage group",
                        "publicIPAllocationMethod": "Dynamic",
                        "domainNameLabelprefix": "myLoadBalancer1"
                    },
                    {
                        "vmName":  "myLB2",
                        "pipName": "myLB2",
                        "description": "this is a storage group",
                        "publicIPAllocationMethod": "Dynamic",
                        "domainNameLabelprefix": "myLoadBalancer2"
                    },
                    {
                        "vmName":  "myVM3",
                        "pipName": "myVM3",
                        "description": "this is a storage group",
                        "publicIPAllocationMethod": "Dynamic",
                        "domainNameLabelprefix": "myrdpserver2d"
                    }
                ]
            }
        }

#azuredeploy.json
1. Add the following to your azuredeploy.json to call the module:
{
    "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
    "contentVersion": "1.0.0.1",
    "parameters": {

        "requireCreatePublicIPAddresses" : {
            "type" : "string",
            "allowedValues": [
                "Yes",
                "No"
            ]
        },

        "moduleCreatePublicIPAddresses" : {
            "type" : "object"
        }
    },
    "variables": {
        "moduleStoreURL": "https://raw.githubusercontent.com/fujitsuk5/Azure-ARM-Modules/master/modules/",
        "module-CreatePublicIPAddresses-URL": "[concat(variables('moduleStoreFBURL'),'virtualNetworks/', 'az-createPublicIPAddresses.json')]",
    },
     "resources": [
        {
            "apiVersion": "2018-05-01",
            "name": "AllPublicIPAddressesForResourceGroup",
            "comments": "Calling Module to deploy one or more Public IPs",
            "type": "Microsoft.Resources/deployments",
            "condition": "[equals(parameters('requireCreatePublicIPAddresses'), 'Yes')]",
            "dependsOn": [
                "AllStorageGroupsForResourceGroup"],
            "properties" : {
                "mode": "Incremental",
                "templateLink": {
                    "uri": "[variables('module-CreatePublicIPAddresses-URL')]",
                    "contentVersion": "1.0.0.1"
                },
                "parameters": {
                    "moduleCreatePublicIPAddresses": { "value": "[parameters('moduleCreatePublicIPAddresses')]"},
                    "customTages": {
                        "value": "[parameters('customTages')]"
                    },
                    "suffixes" : {
                        "value": "[parameters('suffixes')]"
                    }
                }
            }
        }
     ],
    "outputs": {
    }
}















# moduleCreateVNetsAndSubnets
# Module to create one or more Virtual Networks and subnets
# Dependencies: Network Security Groups Module

#azuredeploy.parameters.json
1. Specify a parameter, with value Yes to enable or No to disable:

"$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentParameters.json#",
    "contentVersion": "1.0.0.0",
    "parameters": {
        
        "requireCreateVNetsAndSubnets": {
            "value" : "Yes" 
        },

2. Define a vNets array object for each virtual network required, including one or more subnets array objects as shown below. The nsgName specifed should match one of the values in the name defined in the Network Security Group Module. Default location can be changed from that of the Security Group by changing the location from null to a valid azure region i.e. "location" : "westuk".

        "moduleCreateVNetsAndSubnets": {
            "value": {
                "location" : null,
                "vNets" : [
                    {
                        "vnetName" : "myvnet",
                        "vnetCIDR" : "10.0.0.0/16",
                        "subnets": [
                            {
                                "name": "GatewaySubnet",
                                "addressPrefix": "10.0.0.0/24",
                                "nsgName": "inboundWebTrafficasg1"
                            },
                            {
                                "name":"prod-ne1-vnet-subnet1",
                                "addressPrefix":"10.0.1.0/24",
                                "nsgName": "inboundWebTrafficasg1"
                            },
                            {
                                "name":"prod-ne1-vnet-subnet2",
                                "addressPrefix":"10.0.2.0/24",
                                "nsgName": "inboundWebTrafficasg1"
                            }
                        ]
                    },
                    {
                        "vnetName" : "myvnet2",
                        "vnetCIDR" : "10.0.0.0/16",
                        "subnets": [
                            {
                                "name": "GatewaySubnet",
                                "addressPrefix": "10.0.0.0/24",
                                "nsgName": "inboundWebTrafficasg1"
                            },
                            {
                                "name":"prod-ne1-vnet-subnet1",
                                "addressPrefix":"10.0.1.0/24",
                                "nsgName": "inboundWebTrafficasg1"
                            },
                            {
                                "name":"prod-ne1-vnet-subnet2",
                                "addressPrefix":"10.0.2.0/24",
                                "nsgName": "inboundWebTrafficasg1"
                            }
                        ]
                    }
                ]
            }
        },

#azuredeploy.json
1. Add the following to your azuredeploy.json to call the module:
{
    "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
    "contentVersion": "1.0.0.1",
    "parameters": {

        "requireCreateVNetsAndSubnets" : {
            "type" : "string",
            "allowedValues": [
                "Yes",
                "No"
            ]
        },

         "moduleCreateVNetsAndSubnets" : {
            "type" : "object"
        }
    },
    "variables": {
        "moduleStoreURL": "https://raw.githubusercontent.com/fujitsuk5/Azure-ARM-Modules/master/modules/",
         "module-CreateVirtualNetworks-URL": "[concat(variables('moduleStoreFBURL'),'virtualNetworks/', 'az-createVNetsWithSubnets.json')]",
    },
     "resources": [
        {
            "apiVersion": "2018-05-01",
            "name": "AllVNetsAndSubnetsForResourceGroup",
            "comments": "Calling Module to deploy a Vnets with subnets",
            "type": "Microsoft.Resources/deployments",
            "condition": "[equals(parameters('requireCreateVNetsAndSubnets'), 'Yes')]",
            "dependsOn": [
                "AllASGBasedNSGForResourceGroup"
            ],
            "properties" : {
                "mode": "Incremental",
                "templateLink": {
                    "uri": "[variables('Module-CreateVirtualNetworks-URL')]",
                    "contentVersion": "1.0.0.1"
                },
                "parameters": {
                    "moduleCreateVNetsAndSubnets": { "value": "[parameters('moduleCreateVNetsAndSubnets')]"},
                    "customTages": {
                        "value": "[parameters('customTages')]"
                    },
                    "suffixes" : {
                        "value": "[parameters('suffixes')]"
                    }
                }
            }
        }
     ],
    "outputs": {
    }
}














# moduleCreateNetworkInterfaces
# Module to create one or more Network Interfaces
# Dependenceies: ASG, Public IP Addresses, Vnets & Subnets and Load Balance Modules required

#azuredeploy.parameters.json
1. Specify a parameter, with value Yes to enable or No to disable:

"$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentParameters.json#",
    "contentVersion": "1.0.0.0",
    "parameters": {
        
        "requireCreateApplicationSecurityGroups": {
            "value" : "[Yes or No]" 
        },

2. Define a nics array object for each network interface required as shown below. The values of vmName, vnetName, subnetName, aSGName and lbName should be match an existing resource created using the apporiate module. If asg or lb not required, then specify aSGName, lbName, backendAddressPoolName & inboundNatRulesname as null.  Default location can be changed from that of the Security Group by changing the location from null to a valid azure region i.e. "location" : "westuk".

        "moduleCreateNetworkInterfaces": {
            "value": {
                "location" : null,
                "nics": [
                    {
                        "vmName":  "myVM",
                        "nicName": "myNic",
                        "ipConfigName": "ipconfigdmz",
                        "privateIPAllocationMethod": "Dynamic",
                        "pipName": "myVM",
                        "vnetName": "myvnet",
                        "subnetName": "prod-ne1-vnet-subnet1",
                        "aSGName": "tier1ASG",
                        "lbName": "appLB",
                        "backendAddressPoolName": "backendpool",
                        "inboundNatRulesname": "rdp-vm-one"
                    },
                    {
                        "vmName":  "myVM2",
                        "nicName": "myNic2",
                        "ipConfigName": "ipconfigdmz",
                        "privateIPAllocationMethod": "Dynamic",
                        "pipName": "myVM2",
                        "vnetName": "myvnet",
                        "subnetName": "prod-ne1-vnet-subnet1",
                        "aSGName": "tier1ASG",
                        "lbName": "appLB",
                        "backendAddressPoolName": "backendpool",
                        "inboundNatRulesname": "rdp-vm-two"
                    },
                    {
                        "vmName":  "myVM3",
                        "nicName": "myNic3",
                        "ipConfigName": "ipconfigdmz",
                        "privateIPAllocationMethod": "Dynamic",
                        "pipName": "myVM3",
                        "vnetName": "myvnet",
                        "subnetName": "prod-ne1-vnet-subnet1",
                        "aSGName": null,
                        "lbName": null,
                        "backendAddressPoolName": null,
                        "inboundNatRulesname": null
                    }
                ]
            }
        }

#azuredeploy.json
1. Add the following to your azuredeploy.json to call the module:
{
    "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
    "contentVersion": "1.0.0.1",
    "parameters": {

        "requireCreateNetworkInterfaces" : {
            "type" : "string",
            "allowedValues": [
                "Yes",
                "No"
            ]
        },,

        "moduleCreateNetworkInterfaces" : {
            "type" : "object"
        },
    },
    "variables": {
        "moduleStoreURL": "https://raw.githubusercontent.com/fujitsuk5/Azure-ARM-Modules/master/modules/",
        "module-CreateVirtualNetworks-URL": "[concat(variables('moduleStoreFBURL'),'virtualNetworks/', 'az-createVNetsWithSubnets.json')]",
    },
     "resources": [
        {
            "apiVersion": "2018-05-01",
            "name": "AllNetworkInterfacesForResourceGroup",
            "comments": "Calling Module to deploy one or more Nics",
            "type": "Microsoft.Resources/deployments",
            "condition": "[equals(parameters('requireCreateNetworkInterfaces'), 'Yes')]",
            "dependsOn": [
                "AllASGSForResourceGroup",
                "AllPublicIPAddressesForResourceGroup",
                "AllVNetsAndSubnetsForResourceGroup",
                "AllLoadBalancersForResourceGroup-inbound"

            ],
            "properties" : {
                "mode": "Incremental",
                "templateLink": {
                    "uri": "[variables('module-CreateNetworkInterfaces-URL')]",
                    "contentVersion": "1.0.0.1"
                },
                "parameters": {
                    "moduleCreateNetworkInterfaces": { "value": "[parameters('moduleCreateNetworkInterfaces')]"},
                    "customTages": {
                        "value": "[parameters('customTages')]"
                    },
                    "suffixes" : {
                        "value": "[parameters('suffixes')]"
                    }
                }
            }
        }
     ],
    "outputs": {
    }
}














# moduleCreateVirtualMachinesPerVNet
# Module to create one or more VMs for a specific subnet
# Dependencies : ASG, PIP, NIC, VNET and Availability Set Modules

#azuredeploy.parameters.json
1. Specify a parameter, with value Yes to enable or No to disable:

"$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentParameters.json#",
    "contentVersion": "1.0.0.0",
    "parameters": {
        
        "requireCreateVirtualMachinesPerVNet": {
            "value" : "Yes" 
        }
    }

2. Define a vms array object for each required VM as shown, including otherDisks array object for any datadisks requried. Parameters nicName, availabilitySetName should match existing resource created using appropriate dependecies. Default location can be changed from that of the Security Group by changing the location from null to a valid azure region i.e. "location" : "westuk".

        "moduleCreateVirtualMachinesPerVNet": {
            "value": {
                "location" : null,
                "vms": [
                    {
                        "vmName":  "myVM1",
                        "storageAccountName": "mystoragegroupisp01",
                        "nicName": "myNic",
                        "vmSize": "Standard_A1",
                        "availabilitySetName" : "myAvSet01",
                        "adminUsername": "ian",
                        "adminPassword": "Qwertyu1",
                        "publisher": "MicrosoftWindowsServer",
                        "offer": "WindowsServer",
                        "sku": "2016-Datacenter",
                        "version": "latest",
                        "osCreateOption": "FromImage",
                        "otherDisks": [
                            {
                                "name": "myVM101",
                                "diskSizeGB": 1023,
                                "createOption": "Empty"
                            },
                            {
                                "name": "myVM102",
                                "diskSizeGB": 1023,
                                "createOption": "Empty"
                            }
                        ]
                    },
                    {
                        "vmName":  "myVM2",
                        "storageAccountName": "mystoragegroupisp01",
                        "nicName": "myNic2",
                        "vmSize": "Standard_A1",
                        "availabilitySetName" : "myAvSet01",
                        "adminUsername": "ian",
                        "adminPassword": "Qwertyu1",
                        "publisher": "MicrosoftWindowsServer",
                        "offer": "WindowsServer",
                        "sku": "2016-Datacenter",
                        "version": "latest",
                        "osCreateOption": "FromImage",
                        "otherDisks": [
                            {
                                "name": "myVM201",
                                "diskSizeGB": 1023,
                                "createOption": "Empty"
                            },
                            {
                                "name": "myVM202",
                                "diskSizeGB": 1023,
                                "createOption": "Empty"
                            }
                        ]
                    }
                ]
            }
        },

#azuredeploy.json
1. Add the following to your azuredeploy.json to call the module:
{
    "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
    "contentVersion": "1.0.0.1",
    "parameters": {

        "requireCreateVirtualMachinesPerVNet" : {
            "type" : "string",
            "allowedValues": [
                "Yes",
                "No"
            ]
        },

        "moduleCreateVirtualMachinesPerVNet" : {
            "type" : "object"
        },
    },
    "variables": {
        "moduleStoreURL": "https://raw.githubusercontent.com/fujitsuk5/Azure-ARM-Modules/master/modules/",
        "module-CreateVirtualMachines-URL": "[concat(variables('moduleStoreFBURL'),'virtualMachines/', 'az-createVirtualMachines.json')]"
    },
     "resources": [
        {
            "apiVersion": "2018-05-01",
            "name": "AllVirtualMachinesForVNetAndResourceGroup",
            "comments": "Calling Module to deploy one or more VMs per VNet and Resource Group",
            "type": "Microsoft.Resources/deployments",
            "condition": "[equals(parameters('requireCreateVirtualMachinesPerVNet'), 'Yes')]",
            "dependsOn": [
                "AllASGSForResourceGroup",
                "AllPublicIPAddressesForResourceGroup",
                "AllNetworkInterfacesForResourceGroup",
                "AllVNetsAndSubnetsForResourceGroup",
                "AllAvailabilitySetsPerResourceGroup"

            ],
            "properties" : {
                "mode": "Incremental",
                "templateLink": {
                    "uri": "[variables('module-CreateVirtualMachines-URL')]",
                    "contentVersion": "1.0.0.1"
                },
                "parameters": {
                    "moduleCreateVirtualMachinesPerVNet": { "value": "[parameters('moduleCreateVirtualMachinesPerVNet')]"},
                    "customTages": {
                        "value": "[parameters('customTages')]"
                    },
                    "suffixes" : {
                        "value": "[parameters('suffixes')]"
                    }
                }
            }
        }
     ],
    "outputs": {
    }
}











# moduleCreateApplicationGateway
# Module to create one or more Application Gatewats

#azuredeploy.parameters.json
1. Specify a parameter, with value Yes to enable or No to disable:

"$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentParameters.json#",
    "contentVersion": "1.0.0.0",
    "parameters": {
        
        "requireCreateApplicationGateway": {
            "value" : "No" 
        },

2. Define appGWs array object for each required Application Gateway, including the required backendIPAddress as shown below. Parameters vnetName and subnetName should match existing objects created using the appropriate modules. Default location can be changed from that of the Security Group by changing the location from null to a valid azure region i.e. "location" : "westuk".

        "moduleCreateApplicationGateway": {
            "value": {
                "location" : null,
                "appGWs": [
                    {
                        "applicationGatewayName" : "myfirstapplicationgatewayisp001",
                        "vnetName": "myvnet",
                        "subnetName": "GatewaySubnet",
                        "tier": "Standard",
                        "applicationGatewaySize":  "Standard_small",
                        "applicationGatewayInstanceCount": 1,
                        "frontendPort": "80",
                        "backendPort": "80",
                        "backendIPAddresses" : [
                            {
                                "IpAddress": "10.0.0.7"
                            },
                            {
                                "IpAddress": "10.0.0.8"
                            },
                            {
                                "IpAddress": "10.0.0.9"
                            }
                        ],
                        "cookieBasedAffinity": "Disabled",
                        "wafEnabled" : false,
                        "wafMode" : "Detection",
                        "wafRuleSetType" : "OWASP",
                        "wafRuleSetVersion" : "3.0"
                    },
                    {
                        "applicationGatewayName" : "myfirstapplicationgatewayisp002",
                        "vnetName": "myvnet2",
                        "subnetName": "GatewaySubnet",
                        "tier": "Standard",
                        "applicationGatewaySize":  "Standard_small",
                        "applicationGatewayInstanceCount": 1,
                        "frontendPort": "80",
                        "backendPort": "80",
                        "backendIPAddresses" : [
                            {
                                "IpAddress": "10.0.0.7"
                            },
                            {
                                "IpAddress": "10.0.0.8"
                            },
                            {
                                "IpAddress": "10.0.0.9"
                            }
                        ],
                        "cookieBasedAffinity": "Disabled",
                        "wafEnabled" : false,
                        "wafMode" : "Detection",
                        "wafRuleSetType" : "OWASP",
                        "wafRuleSetVersion" : "3.0"
                        
                    }
                ]
            }
        },

#azuredeploy.json
1. Add the following to your azuredeploy.json to call the module:
{
    "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
    "contentVersion": "1.0.0.1",
    "parameters": {

        "requireCreateApplicationGateway" : {
            "type" : "string",
            "allowedValues": [
                "Yes",
                "No"
            ]
        },,

        "moduleCreateApplicationGateway" : {
            "type" : "object"
        },
    },
    "variables": {
        "moduleStoreURL": "https://raw.githubusercontent.com/fujitsuk5/Azure-ARM-Modules/master/modules/",
        "module-CreateApplicationGateway-URL": "[concat(variables('moduleStoreFBURL'),'virtualNetworks/', 'az-createApplicationGateways.json')]"
    },
     "resources": [
        {
            "apiVersion": "2018-05-01",
            "name": "AllApplicationGatewaysForResourceGroup",
            "comments": "Calling Module to deploy one or more Application Gateway for the Resource Group",
            "type": "Microsoft.Resources/deployments",
            "condition": "[equals(parameters('requireCreateApplicationGateway'), 'Yes')]",
            "dependsOn": [
                "AllVNetsAndSubnetsForResourceGroup"
            ],
            "properties" : {
                "mode": "Incremental",
                "templateLink": {
                    "uri": "[variables('module-CreateApplicationGateway-URL')]",
                    "contentVersion": "1.0.0.1"
                },
                "parameters": {
                    "moduleCreateApplicationGateway": { "value": "[parameters('moduleCreateApplicationGateway')]"},
                    "customTages": {
                        "value": "[parameters('customTages')]"
                    },
                    "suffixes" : {
                        "value": "[parameters('suffixes')]"
                    }
                }
            }
        }
     ],
    "outputs": {
    }
}













# moduleCreateWAFApplicationGateway
# Module to create one or more WAF enabled Gateways

#azuredeploy.parameters.json
1. Specify a parameter, with value Yes to enable or No to disable:

"$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentParameters.json#",
    "contentVersion": "1.0.0.0",
    "parameters": {
        
        "requireCreateWAFApplicationGateway": {
            "value" : "No" 
        },,

2. Define appGWs array object for each required Application Gateway, including the required backendIPAddress as shown below. Parameters vnetName and subnetName should match existing objects created using the appropriate modules. Default location can be changed from that of the Security Group by changing the location from null to a valid azure region i.e. "location" : "westuk".

        "moduleCreateWAFApplicationGateway": {
            "value": {
                "location" : null,
                "appGWs": [
                    {
                        "applicationGatewayName" : "myfirstwafapplicationgatewayisp001",
                        "vnetName": "myvnet",
                        "subnetName": "GatewaySubnet",
                        "tier": "WAF",
                        "applicationGatewaySize":  "WAF_Medium",
                        "applicationGatewayInstanceCount": 2,
                        "frontendPort": "80",
                        "backendPort": "80",
                        "backendIPAddresses" : [
                            {
                                "IpAddress": "10.0.0.17"
                            },
                            {
                                "IpAddress": "10.0.0.18"
                            },
                            {
                                "IpAddress": "10.0.0.19"
                            }
                        ],
                        "cookieBasedAffinity": "Disabled",
                        "wafEnabled" : true,
                        "wafMode" : "Detection",
                        "wafRuleSetType" : "OWASP",
                        "wafRuleSetVersion" : "3.0"
                    }
                ]
            }
        }

#azuredeploy.json
1. Add the following to your azuredeploy.json to call the module:
{
    "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
    "contentVersion": "1.0.0.1",
    "parameters": {

        "requireCreateWAFApplicationGateway" : {
            "type" : "string",
            "allowedValues": [
                "Yes",
                "No"
            ]
        },

        "moduleCreateWAFApplicationGateway" : {
            "type" : "object"
        },
    },
    "variables": {
        "moduleStoreURL": "https://raw.githubusercontent.com/fujitsuk5/Azure-ARM-Modules/master/modules/",
         "module-CreateWAFApplicationGateway-URL": "[concat(variables('moduleStoreFBURL'),'virtualNetworks/', 'az-createWAFApplicationGateways.json')]",
    },
     "resources": [
        {
            "apiVersion": "2018-05-01",
            "name": "AllWAFApplicationGatewaysForResourceGroup",
            "comments": "Calling Module to deploy one or more WAF Application Gateway for the Resource Group",
            "type": "Microsoft.Resources/deployments",
            "condition": "[equals(parameters('requireCreateWAFApplicationGateway'), 'Yes')]",
            "dependsOn": [
                "AllVNetsAndSubnetsForResourceGroup"
            ],
            "properties" : {
                "mode": "Incremental",
                "templateLink": {
                    "uri": "[variables('module-CreateWAFApplicationGateway-URL')]",
                    "contentVersion": "1.0.0.1"
                },
                "parameters": {
                    "moduleCreateWAFApplicationGateway": { "value": "[parameters('moduleCreateWAFApplicationGateway')]"},
                    "customTages": {
                        "value": "[parameters('customTages')]"
                    },
                    "suffixes" : {
                        "value": "[parameters('suffixes')]"
                    }
                }
            }
        }
     ],
    "outputs": {
    }
}















# moduleCreateAvailabilitySets
# Module to create one or more AvailabilitySet

#azuredeploy.parameters.json
1. Specify a parameter, with value Yes to enable or No to disable:

"$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentParameters.json#",
    "contentVersion": "1.0.0.0",
    "parameters": {
        
        "requireCreateAvailabilitySets": {
            "value" : "Yes" 
        }

2. Definem  availabilitySets array object or each AVset required as shown below. Default location can be changed from that of the Security Group by changing the location from null to a valid azure region i.e. "location" : "westuk".

        "moduleCreateAvailabilitySets": {
            "value": {
                "location" : null,
                "availabilitySets": [
                    {
                        "availabilitySetName" : "myAvSet01",
                        "platformFaultDomainCount": "2",
                        "platformUpdateDomainCount": "2",
                        "skuName" : "Aligned"
                    },
                    {
                        "availabilitySetName" : "myAvSet02",
                        "platformFaultDomainCount": "2",
                        "platformUpdateDomainCount": "2",
                        "skuName" : "Aligned"
                    }
                ]
            }
        },
#azuredeploy.json
1. Add the following to your azuredeploy.json to call the module:
{
    "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
    "contentVersion": "1.0.0.1",
    "parameters": {

        "requireCreateAvailabilitySets" : {
            "type" : "string",
            "allowedValues": [
                "Yes",
                "No"
            ]
        },

        "moduleCreateAvailabilitySets" : {
            "type" : "object"
        }
    },
    "variables": {
        "moduleStoreURL": "https://raw.githubusercontent.com/fujitsuk5/Azure-ARM-Modules/master/modules/",
        "module-CreateAvailabilitySets-URL": "[concat(variables('moduleStoreFBURL'),'virtualMachines/', 'az-createAvailabilitySets.json')]",
    },
     "resources": [
        {
            "apiVersion": "2018-05-01",
            "name": "AllAvailabilitySetsPerResourceGroup",
            "comments": "Calling Module to deploy one or more availability sets per Resource Group",
            "type": "Microsoft.Resources/deployments",
            "condition": "[equals(parameters('requireCreateAvailabilitySets'), 'Yes')]",
            "dependsOn": [

            ],
            "properties" : {
                "mode": "Incremental",
                "templateLink": {
                    "uri": "[variables('module-CreateAvailabilitySets-URL')]",
                    "contentVersion": "1.0.0.1"
                },
                "parameters": {
                    "moduleCreateAvailabilitySets": { "value": "[parameters('moduleCreateAvailabilitySets')]"},
                    "customTages": {
                        "value": "[parameters('customTages')]"
                    },
                    "suffixes" : {
                        "value": "[parameters('suffixes')]"
                    }
                }
            }
        }
     ],
    "outputs": {
    }
}















# moduleCreateLoadBalancers
 #Module to create one or more Loadbalancers

#azuredeploy.parameters.json
1. Specify a parameter, with value Yes to enable or No to disable:

"$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentParameters.json#",
    "contentVersion": "1.0.0.0",
    "parameters": {
        
        "requireCreateLoadBalancers": {
            "value" : "Yes" 
        },

2. Define a loadblancers object array for each required load balancer as shown below including multiple probes, inboundNatRules and loadBalancingRules. The value of nameOfExistingPublicIPAddress attribute should match existing Public IP Address created by the appropriate module. Default location can be changed from that of the Security Group by changing the location from null to a valid azure region i.e. "location" : "westuk".

        "moduleCreateLoadBalancers": {
            "value": {
                "location" : null,
                "loadbalancers": [
                    {
                        "lbName": "appLB",
                        "frontendIPConfigurationsName": "LoadBalancerFrontEnd",
                        "nameOfExistingPublicIPAddress": "myLB1",
                        "backendAddressPoolName": "backendpool",
                        "probes": [
                            {
                                "probeName": "tcpProbe1",
                                "protocol": "tcp",
                                "port": 80,
                                "intervalInSeconds": 5,
                                "numberOfProbes": 2
                                    
                            },
                            {
                                "probeName": "tcpProbe2",
                                "protocol": "tcp",
                                "port": 80,
                                "intervalInSeconds": 5,
                                "numberOfProbes": 2
                                    
                            }
                        ],
                        "inboundNatRules": [
                            {
                                "inboundNatRulesname": "rdp-vm-one",
                                "protocol": "tcp",
                                "frontendPort": 50001,
                                "backendPort": 3389,
                                "enableFloatingIP": false
                            },
                            {
                                "inboundNatRulesname": "rdp-vm-two",
                                "protocol": "tcp",
                                "frontendPort": 50002,
                                "backendPort": 3388,
                                "enableFloatingIP": false
                            }
                        ],
                        "loadBalancingRules": [
                            {
                                "name": "LBRule",
                                "protocol": "tcp",
                                "frontendPort": 80,
                                "backendPort": 80,
                                "enableFloatingIP": false,
                                "idleTimeoutInMinutes": 5,
                                "probeName" : "tcpProbe1"
                            },
                            {
                                "name": "LBRule2",
                                "protocol": "tcp",
                                "frontendPort": 81,
                                "backendPort": 81,
                                "enableFloatingIP": false,
                                "idleTimeoutInMinutes": 5,
                                "probeName" : "tcpProbe2"
                            }
                        ]
                    }
                ]   
            }
        },

#azuredeploy.json
1. Add the following to your azuredeploy.json to call the module. Note the resource is currently called 3 times, due to a bug with azure arm templates not adding all the required multiple probes, inboundNatRules and loadBalancingRules to the lb in one go:
{
    "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
    "contentVersion": "1.0.0.1",
    "parameters": {

        "requireCreateLoadBalancers" : {
            "type" : "string",
            "allowedValues": [
                "Yes",
                "No"
            ]
        },

        "moduleCreateLoadBalancers" : {
            "type" : "object"
        },
    },
    "variables": {
        "moduleStoreURL": "https://raw.githubusercontent.com/fujitsuk5/Azure-ARM-Modules/master/modules/",
        "module-CreateLoadBalancers-URL": "[concat(variables('moduleStoreFBURL'),'virtualNetworks/', 'az-createLoadBalancers.json')]",
        "module-CreateLoadBalancers-URL2": "[concat(variables('moduleStoreFBURL'),'virtualNetworks/', 'az-createLoadBalancers-probes.json')]",
        "module-CreateLoadBalancers-URL3": "[concat(variables('moduleStoreFBURL'),'virtualNetworks/', 'az-createLoadBalancers-inbound.json')]"
    },
     "resources": [
        {
            "apiVersion": "2018-05-01",
            "name": "AllLoadBalancersForResourceGroup",
            "comments": "Calling Module to deploy one or more Loadbalancers for the Resource Group",
            "type": "Microsoft.Resources/deployments",
            "condition": "[equals(parameters('requireCreateLoadbalancers'), 'Yes')]",
            "dependsOn": [
                "AllVNetsAndSubnetsForResourceGroup",
                "AllPublicIPAddressesForResourceGroup",
                "AllLoadBalancersForResourceGroup-probes"
            ],
            "properties" : {
                "mode": "Incremental",
                "templateLink": {
                    "uri": "[variables('module-CreateLoadbalancers-URL')]",
                    "contentVersion": "1.0.0.1"
                },
                "parameters": {
                    "moduleCreateLoadBalancers": { "value": "[parameters('moduleCreateLoadBalancers')]"},
                    "customTages": {
                        "value": "[parameters('customTages')]"
                    },
                    "suffixes" : {
                        "value": "[parameters('suffixes')]"
                    }
                }
            }
        },
        {
            "apiVersion": "2018-05-01",
            "name": "AllLoadBalancersForResourceGroup-probes",
            "comments": "Calling Module to deploy one or more Loadbalancers for the Resource Group",
            "type": "Microsoft.Resources/deployments",
            "condition": "[equals(parameters('requireCreateLoadbalancers'), 'Yes')]",
            "dependsOn": [
                "AllVNetsAndSubnetsForResourceGroup",
                "AllPublicIPAddressesForResourceGroup"
            ],
            "properties" : {
                "mode": "Incremental",
                "templateLink": {
                    "uri": "[variables('module-CreateLoadbalancers-URL2')]",
                    "contentVersion": "1.0.0.1"
                },
                "parameters": {
                    "moduleCreateLoadBalancers": { "value": "[parameters('moduleCreateLoadBalancers')]"},
                    "customTages": {
                        "value": "[parameters('customTages')]"
                    },
                    "suffixes" : {
                        "value": "[parameters('suffixes')]"
                    }
                }
            }
        },
        {
            "apiVersion": "2018-05-01",
            "name": "AllLoadBalancersForResourceGroup-inbound",
            "comments": "Calling Module to deploy one or more Loadbalancers for the Resource Group",
            "type": "Microsoft.Resources/deployments",
            "condition": "[equals(parameters('requireCreateLoadbalancers'), 'Yes')]",
            "dependsOn": [
                "AllVNetsAndSubnetsForResourceGroup",
                "AllPublicIPAddressesForResourceGroup",
                "AllLoadBalancersForResourceGroup-probes",
                "AllLoadBalancersForResourceGroup"
            ],
            "properties" : {
                "mode": "Incremental",
                "templateLink": {
                    "uri": "[variables('module-CreateLoadbalancers-URL3')]",
                    "contentVersion": "1.0.0.1"
                },
                "parameters": {
                    "moduleCreateLoadBalancers": { "value": "[parameters('moduleCreateLoadBalancers')]"},
                    "customTages": {
                        "value": "[parameters('customTages')]"
                    },
                    "suffixes" : {
                        "value": "[parameters('suffixes')]"
                    }
                }
            }
        }
     ],
    "outputs": {
    }
}















