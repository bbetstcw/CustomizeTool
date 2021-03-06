<properties
	pageTitle="Configure Azure Key Vault Integration for SQL Server on Azure VMs (Classic)"
	description="Learn how to automate the configuration of SQL Server encryption for use with Azure Key Vault. This topic explains how to use Azure Key Vault Integration with SQL Server virtual machines create in the classic deployment model."
	services="virtual-machines-windows"
	documentationCenter=""
	authors="rothja"
	manager="jhubbard"
	editor=""
	tags="azure-service-management"/>

<tags
	ms.service="virtual-machines-windows"
	ms.date="04/08/2016"
	wacn.date=""/>

# Configure Azure Key Vault Integration for SQL Server on Azure VMs (Classic)

> [AZURE.SELECTOR]
- [Resource Manager](/documentation/articles/virtual-machines-windows-ps-sql-keyvault/)
- [Classic](/documentation/articles/virtual-machines-windows-classic-ps-sql-keyvault/)

## Overview
There are multiple SQL Server encryption features, such as [transparent data encryption  (TDE)](https://msdn.microsoft.com/library/bb934049.aspx)  (TDE)](https://msdn.microsoft.com/zh-cn/library/bb934049.aspx) , [column level encryption  (CLE)](https://msdn.microsoft.com/library/ms173744.aspx)  (CLE)](https://msdn.microsoft.com/zh-cn/library/ms173744.aspx) , and [backup  encryption](https://msdn.microsoft.com/library/dn449489.aspx)  encryption](https://msdn.microsoft.com/zh-cn/library/dn449489.aspx) . These forms of encryption require you to manage and store the cryptographic keys you use for encryption. The Azure Key Vault (AKV) service is designed to improve the security and management of these keys in a secure and highly available location. The [SQL Server Connector](http://www.microsoft.com/download/details.aspx?id=45344) enables SQL Server to use these keys from Azure Key Vault.


[AZURE.INCLUDE [learn-about-deployment-models](../includes/learn-about-deployment-models-classic-include.md)] Resource Manager model.


> [AZURE.IMPORTANT] Azure has two different deployment models for creating and working with resources:  [Resource Manager and classic](/documentation/articles/resource-manager-deployment-model/).  This article covers using the classic deployment model. Azure recommends that most new deployments use the Resource Manager model.


If you running SQL Server with on-premises machines, there are [steps you can follow to access Azure Key Vault from your on-premises SQL Server  machine](https://msdn.microsoft.com/library/dn198405.aspx)  machine](https://msdn.microsoft.com/zh-cn/library/dn198405.aspx) . But for SQL Server in Azure VMs, you can save time by using the *Azure Key Vault Integration* feature. With a few Azure PowerShell cmdlets to enable this feature, you can automate the configuration necessary for a SQL VM to access your key vault.

When this feature is enabled, it automatically installs the SQL Server Connector, configures the EKM provider to access Azure Key Vault, and creates the credential to allow you to access your vault. If you looked at the steps in the previously mentioned on-premises documentation, you can see that this feature automates steps 2 and 3. The only thing you would still need to do manually is to create the key vault and keys. From there, the entire setup of your SQL VM is automated. Once this feature has completed this setup, you can execute T-SQL statements to begin encrypting your databases or backups as you normally would.

[AZURE.INCLUDE [AKV Integration Prepare](../includes/virtual-machines-sql-server-akv-prepare.md)]

## Configure AKV Integration
Use PowerShell to configure Azure Key Vault Integration. The following sections provide an overview of the required parameters and then a sample PowerShell script.

### Input parameters
The following table lists the parameters required to run the PowerShell script in the next section.

|Parameter|Description|Example|
|---|---|---|

|**$akvURL**|**The key vault URL**|"https://contosokeyvault.vault.azure.net/"|


|**$akvURL**|**The key vault URL**|"https://contosokeyvault.vault.chinacloudapi.cn/"|

|**$spName**|**Service Principal name**|"fde2b411-33d5-4e11-af04eb07b669ccf2"|
|**$spSecret**|**Service Principal secret**|"9VTJSQwzlFepD8XODnzy8n2V01Jd8dAjwm/azF1XDKM="|
|**$credName**|**Credential name**: AKV Integration creates a credential within SQL Server, allowing the VM to have access to the key vault. Choose a name for this credential.|"mycred1"|
|**$vmName**|**Virtual machine name**: The name of a previously created SQL VM.|"myvmname"|
|**$serviceName**|**Service name**: The Cloud Service name that is associated with the SQL VM.|"mycloudservicename"|

### Enable AKV Integration with PowerShell
The **New-AzureVMSqlServerKeyVaultCredentialConfig** cmdlet creates a configuration object for the Azure Key Vault Integration feature. The **Set-AzureVMSqlServerExtension** configures this integration with the **KeyVaultCredentialSettings** parameter. The following steps show how to use these commands.

1. In Azure PowerShell, first configure the input parameters with your specific values as described in the previous sections of this topic. The following script is an example.


		$akvURL = "https://contosokeyvault.vault.azure.net/"


		$akvURL = "https://contosokeyvault.vault.chinacloudapi.cn/"

		$spName = "fde2b411-33d5-4e11-af04eb07b669ccf2"
		$spSecret = "9VTJSQwzlFepD8XODnzy8n2V01Jd8dAjwm/azF1XDKM="
		$credName = "mycred1"
		$vmName = "myvmname"
		$serviceName = "mycloudservicename"
2.	Then use the following script to configure and enable AKV Integration.

		$secureakv =  $spSecret | ConvertTo-SecureString -AsPlainText -Force
		$akvs = New-AzureVMSqlServerKeyVaultCredentialConfig -Enable -CredentialName $credname -AzureKeyVaultUrl $akvURL -ServicePrincipalName $spName -ServicePrincipalSecret $secureakv
		Get-AzureVM -ServiceName $serviceName -Name $vmName | Set-AzureVMSqlServerExtension -KeyVaultCredentialSettings $akvs | Update-AzureVM

The SQL IaaS Agent Extension will update the SQL VM with this new configuration.

[AZURE.INCLUDE [AKV Integration Next Steps](../includes/virtual-machines-sql-server-akv-next-steps.md)]
