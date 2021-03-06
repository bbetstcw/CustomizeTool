<properties
   pageTitle="Custom scripts on Windows VMs using templates | Microsoft Azure"
   description="Automate Windows VM configuration tasks by using the Custom Script extension with Resource Manager templates"
   services="virtual-machines-windows"
   documentationCenter=""
   authors="kundanap"
   manager="timlt"
   editor=""
   tags="azure-resource-manager"/>

<tags
	ms.service="virtual-machines-windows"
	ms.date="03/29/2016"
	wacn.date=""/>

# Using the Custom Script extension for Windows VMs With Azure Resource Manager templates


> [AZURE.NOTE] Azure has two different deployment models for creating and working with resources:  [Resource Manager and classic](/documentation/articles/resource-manager-deployment-model/).  This article covers using the Resource Manager deployment model, which Azure recommends for most new deployments instead of the [classic deployment model](/documentation/articles/virtual-machines-windows-classic-extensions-customscript/).


[AZURE.INCLUDE [virtual-machines-common-extensions-customscript](../includes/virtual-machines-common-extensions-customscript.md)]

## Template example for a Windows VM

Define the following resource in the Resource section of the template

       {
       "type": "Microsoft.Compute/virtualMachines/extensions",
       "name": "MyCustomScriptExtension",
       "apiVersion": "2015-05-01-preview",
       "location": "[parameters('location')]",
       "dependsOn": [
           "[concat('Microsoft.Compute/virtualMachines/',parameters('vmName'))]"
       ],
       "properties": {
           "publisher": "Microsoft.Compute",
           "type": "CustomScriptExtension",
           "typeHandlerVersion": "1.7",
           "autoUpgradeMinorVersion":true,
           "settings": {
               "fileUris": [

               "http://Yourstorageaccount.blob.core.windows.net/customscriptfiles/start.ps1"


               "http://Yourstorageaccount.blob.core.chinacloudapi.cn/customscriptfiles/start.ps1"

           ],
           "commandToExecute": "powershell.exe -ExecutionPolicy Unrestricted -File start.ps1"
         }
       }
     }

In the example above, replace the file URL and the file name with your own settings.

After authoring the template, you can deploy it using Azure Powershell.

Please refer to the example below for a complete samples of configuring applications on a VM using Custom Script extension.

* [Custom Script extension on a Windows VM](https://github.com/Azure/azure-quickstart-templates/blob/b1908e74259da56a92800cace97350af1f1fc32b/201-list-storage-keys-windows-vm/azuredeploy.json/)
