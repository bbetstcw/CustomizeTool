<properties
	pageTitle="Overview of SQL Server on Azure Virtual Machines | Azure"
	description="Learn about how to run full SQL Server editions on Azure Virtual machines. Get direct links to all SQL Server VM images and related content."
	services="virtual-machines-windows"
	documentationCenter=""
	authors="rothja"
	manager="jhubbard"
	editor=""
	tags="azure-service-management"/>

<tags
	ms.service="virtual-machines-windows"
	ms.devlang="na"
	ms.topic="get-started-article"
	ms.tgt_pltfrm="vm-windows-sql-server"
	ms.workload="infrastructure-services"
	ms.date="08/29/2016"
	wacn.date=""
	ms.author="jroth"/>

# Overview of SQL Server on Azure Virtual Machines

This topic describes your options for running SQL Server on Azure virtual machines, along with [links to portal images](#option-1-deploy-a-sql-vm-per-minute-licensing) and an overview of [common tasks](#manage-your-sql-vm).

>[AZURE.NOTE] If you're already familiar with SQL Server and just want to see how to deploy a SQL Server VM, see [Provision a SQL Server virtual machine in the Azure Portal](/documentation/articles/virtual-machines-windows-portal-sql-server-provision/).

## Overview
You might be a database administrator looking to move your on-premises SQL Server workloads to the Cloud. Or you might be a developer considering the relational database capabilities of SQL Server for your Azure application. What is the advantage to running SQL Server workloads in Azure virtual machines? The following overview video discusses the benefits and provides a technical overview.

> [AZURE.VIDEO data-driven-sql-server-2016-azure-vm-is-the-best-platform-for-sql-server-2016]

## Evaluate the benefits

Before you begin, first evaluate what you gain by using SQL Server on Azure VMs.

If you're moving other workloads to Azure, such as an enterprise application, it makes sense to also move any dependent SQL Server databases to Azure for improved performance. But hosting SQL Server in Azure VMs provides other benefits. For example, you automatically have access to multiple data centers for a global presence and disaster recovery. For a complete list of scenarios and benefits, see the [SQL Server on Azure VMs product  page](/home/features/virtual-machines/sql-server/)  page](/home/features/virtual-machines/#home_vm_overview_info) .

> [AZURE.NOTE] When you're evaluating SQL Server on Azure VMs, also review the other storage and SQL options on Azure, such as [SQL Database](/documentation/articles/sql-database-technical-overview/), [SQL Data Warehouse](/documentation/articles/sql-data-warehouse-overview-what-is/), and [SQL Server Stretch  Database](../sql -server-stretch-database/sql-server-stretch-database-overview.md)  Database](/documentation/articles/sql-server-stretch-database-overview/) . For one detailed comparison, see [Choose a cloud SQL Server option: Azure SQL (PaaS) Database or SQL Server on Azure VMs (IaaS)](/documentation/articles/sql-database-paas-vs-sql-server-iaas/).

After you decide to run SQL Server on Azure VMs, one of your first decisions is whether to use a VM image that includes the SQL Server licensing costs. Your other option is to bring your own license (BYOL) and only pay for the VM itself. The next two sections describe these options.

## Option 1: Deploy a SQL VM (per-minute licensing)
The following table provides a matrix of available SQL Server images in the virtual machine gallery. Click on any link to begin creating a new SQL VM with your specified version, edition, and operating system. All images include [SQL Server licensing costs](/pricing/details/virtual-machines/).

Step-by-step guidance is available in the tutorial, [Provision a SQL Server virtual machine in the Azure  Portal](/documentation/articles/virtual-machines-windows-portal-sql-server-provision/)  PowerShell (Classic)](/documentation/articles/virtual-machines-windows-classic-ps-sql-create/) . Also, review the [Performance best practices for SQL Server VMs](/documentation/articles/virtual-machines-windows-sql-performance/), which explains how to select the appropriate machine size and other features available during provisioning.

|Version|Operating System|Edition|
|---|---|---|

|**SQL 2016**|Windows Server 2012 R2|[Enterprise](https://portal.azure.cn/#create/Microsoft.SQLServer2016RTMEnterpriseWindowsServer2012R2), [Standard](https://portal.azure.cn/#create/Microsoft.SQLServer2016RTMStandardWindowsServer2012R2), [Web](https://portal.azure.cn/#create/Microsoft.SQLServer2016RTMWebWindowsServer2012R2), [Dev](https://portal.azure.cn/#create/Microsoft.SQLServer2016RTMDeveloperWindowsServer2012R2), [Express](https://portal.azure.cn/#create/Microsoft.SQLServer2016RTMExpressWindowsServer2012R2)|
|**SQL 2014 SP1**|Windows Server 2012 R2|[Enterprise](https://portal.azure.cn/#create/Microsoft.SQLServer2014SP1EnterpriseWindowsServer2012R2), [Standard](https://portal.azure.cn/#create/Microsoft.SQLServer2014SP1StandardWindowsServer2012R2), [Web](https://portal.azure.cn/#create/Microsoft.SQLServer2014SP1WebWindowsServer2012R2), [Express](https://portal.azure.cn/#create/Microsoft.SQLServer2014SP1ExpressWindowsServer2012R2)|
|**SQL 2014**|Windows Server 2012 R2|[Enterprise](https://portal.azure.cn/#create/Microsoft.SQLServer2014EnterpriseWindowsServer2012R2), [Standard](https://portal.azure.cn/#create/Microsoft.SQLServer2014StandardWindowsServer2012R2), [Web](https://portal.azure.cn/#create/Microsoft.SQLServer2014WebWindowsServer2012R2)|
|**SQL 2012 SP3**|Windows Server 2012 R2|[Enterprise](https://portal.azure.cn/#create/Microsoft.SQLServer2012SP3EnterpriseWindowsServer2012R2), [Standard](https://portal.azure.cn/#create/Microsoft.SQLServer2012SP3StandardWindowsServer2012R2), [Web](https://portal.azure.cn/#create/Microsoft.SQLServer2012SP3WebWindowsServer2012R2), [Express](https://portal.azure.cn/#create/Microsoft.SQLServer2012SP3ExpressWindowsServer2012R2)|
|**SQL 2012 SP2**|Windows Server 2012 R2|[Enterprise](https://portal.azure.cn/#create/Microsoft.SQLServer2012SP2EnterpriseWindowsServer2012R2), [Standard](https://portal.azure.cn/#create/Microsoft.SQLServer2012SP2StandardWindowsServer2012R2), [Web](https://portal.azure.cn/#create/Microsoft.SQLServer2012SP2WebWindowsServer2012R2)|
|**SQL 2012 SP2**|Windows Server 2012|[Enterprise](https://portal.azure.cn/#create/Microsoft.SQLServer2012SP2EnterpriseWindowsServer2012), [Standard](https://portal.azure.cn/#create/Microsoft.SQLServer2012SP2StandardWindowsServer2012), [Web](https://portal.azure.cn/#create/Microsoft.SQLServer2012SP2WebWindowsServer2012), [Express](https://portal.azure.cn/#create/Microsoft.SQLServer2012SP2ExpressWindowsServer2012)|
|**SQL 2008 R2 SP3**|Windows Server 2008 R2|[Enterprise](https://portal.azure.cn/#create/Microsoft.SQLServer2008R2SP3EnterpriseWindowsServer2008R2), [Standard](https://portal.azure.cn/#create/Microsoft.SQLServer2008R2SP3StandardWindowsServer2008R2), [Web](https://portal.azure.cn/#create/Microsoft.SQLServer2008R2SP3WebWindowsServer2008R2)|
|**SQL 2008 R2 SP3**|Windows Server 2012|[Express](https://portal.azure.cn/#create/Microsoft.SQLServer2008R2SP3ExpressWindowsServer2012)|


|**SQL 2016**|Windows Server 2012 R2|Enterprise, Standard, Web, Dev, Express|
|**SQL 2014 SP1**|Windows Server 2012 R2|Enterprise, Standard, Web, Express|
|**SQL 2014**|Windows Server 2012 R2|Enterprise, Standard, Web|
|**SQL 2012 SP3**|Windows Server 2012 R2|Enterprise, Standard, Web, Express|
|**SQL 2012 SP2**|Windows Server 2012 R2|Enterprise, Standard, Web|
|**SQL 2012 SP2**|Windows Server 2012|Enterprise, Standard, Web, Express|
|**SQL 2008 R2 SP3**|Windows Server 2008 R2|Enterprise, Standard, Web|
|**SQL 2008 R2 SP3**|Windows Server 2012|Express|


## Option 2: Deploy a SQL VM (BYOL)
The other option is to bring your own license (BYOL). In this scenario, you only pay for the VM without any additional charges for SQL Server licensing. To use your own license, use the matrix of SQL Server versions, editions, and operating systems below. In the portal, the image names are prefixed with **{BYOL}** in the Portal.

> [AZURE.IMPORTANT] In order to use BYOL VM images, you must have and Enterprise Agreement with [License Mobility through Software Assurance on Azure](https://azure.microsoft.com/pricing/license-mobility/). You also need a valid license for the version/edition of SQL Server you want to use. You must [provide the necessary BYOL information to Microsoft](http://d36cz9buwru1tt.cloudfront.net/License_Mobility_Customer_Verification_Guide.pdf) within **10** days of provisioning your VM.

The guidance in the [provisioning  tutorial](/documentation/articles/virtual-machines-windows-portal-sql-server-provision/)  tutorial](/documentation/articles/virtual-machines-windows-classic-ps-sql-create/)  applies, but you must use one of the following **BYOL** image options. Also, review the [Performance best practices for SQL Server VMs](/documentation/articles/virtual-machines-windows-sql-performance/), which explains how to select the appropriate machine size and other features available during provisioning.

|Version|Operating system|Edition|
|---|---|---|

|**SQL Server 2016**|Windows Server 2012 R2|[Enterprise BYOL](https://portal.azure.cn/#create/Microsoft.BYOLSQLServer2016RTMStandardWindowsServer2012R2), [Standard BYOL](https://portal.azure.cn/#create/Microsoft.BYOLSQLServer2016RTMStandardWindowsServer2012R2)|
|**SQL Server 2014 SP1**|Windows Server 2012 R2|[Enterprise BYOL](https://portal.azure.cn/#create/Microsoft.BYOLSQLServer2014SP1EnterpriseWindowsServer2012R2), [Standard BYOL](https://portal.azure.cn/#create/Microsoft.BYOLSQLServer2014SP1StandardWindowsServer2012R2)|
|**SQL Server 2012 SP2**|Windows Server 2012 R2|[Enterprise BYOL](https://portal.azure.cn/#create/Microsoft.BYOLSQLServer2012SP3EnterpriseWindowsServer2012R2), [Standard  BYOL](https://portal.azure.cn/#create/Microsoft.BYOLSQLServer2012SP3StandardWindowsServer2012R2)|


|**SQL Server 2016**|Windows Server 2012 R2|Enterprise BYOL, Standard BYOL|
|**SQL Server 2014 SP1**|Windows Server 2012 R2|Enterprise BYOL, Standard BYOL|
|**SQL Server 2012 SP2**|Windows Server 2012 R2|Enterprise BYOL, Standard  BYOL|


## Manage your SQL VM
After provisioning your SQL Server VM, there are several optional management tasks. In some aspects, you configure and manage SQL Server exactly like you would on-premises. But some tasks are specific to Azure. The following sections highlight some of these areas with links to more information.

### Migrate your data

If you have an existing database, you'll want to move that to the newly provisioned SQL VM. For a list of migration options and guidance, see [Migrating a Database to SQL Server on an Azure VM](/documentation/articles/virtual-machines-windows-migrate-sql/).

### Configure high availability

If you require high availability, consider configuring SQL Server Availability Groups. This involves multiple Azure VMs in a virtual network. The Azure  portal  Portal   Preview  has a template that sets up this configuration for you. For more information, see [Configure an AlwaysOn availability group in  Azure Resource Manager  Classic  virtual  machines](/documentation/articles/virtual-machines-windows-portal-sql-alwayson-availability-groups/)  machines](/documentation/articles/virtual-machines-windows-classic-portal-sql-alwayson-availability-groups/) .  If you want to manually configure your Availability Group and associated listener, see [Configure AlwaysOn Availability Groups in Azure VM](/documentation/articles/virtual-machines-windows-portal-sql-alwayson-availability-groups-manual/). 

For other high availability considerations, see [High Availability and Disaster Recovery for SQL Server in Azure Virtual Machines](/documentation/articles/virtual-machines-windows-sql-high-availability-dr/).

### Back up your data
 Azure VMs  You  can  take advantage of [Automated Backup](/documentation/articles/virtual-machines-windows-sql-automated-backup/), which regularly  manually  creates backups of your database to blob storage. You can also manually use this technique. For more information, see [Use Azure Storage for SQL Server Backup and Restore](/documentation/articles/virtual-machines-windows-use-storage-sql-server-backup-restore/). For an overview of all backup and restore options, see [Backup and Restore for SQL Server in Azure Virtual Machines](/documentation/articles/virtual-machines-windows-sql-backup-recovery/).

### Automate updates
Azure VMs can use [Automated  Patching](/documentation/articles/virtual-machines-windows-sql-automated-patching/)  Patching](/documentation/articles/virtual-machines-windows-classic-sql-automated-patching/)  to schedule a maintenance window for installing important windows and SQL Server updates automatically.

### Customer experience improvement program (CEIP)
The Customer Experience Improvement Program (CEIP) is enabled by default. This is not a management task, unless you want to disable CEIP after provisioning. You can customize or disable the CEIP by connecting to the VM with remote desktop. Then run the **SQL Server Error and Usage Reporting** utility. Follow the instructions to disable reporting.

## Next steps

[Explore the Learning Path](https://azure.microsoft.com/documentation/learning-paths/sql-azure-vm/) for SQL Server on Azure virtual machines.


More question? First, see the [SQL Server on Azure Virtual Machines FAQ](/documentation/articles/virtual-machines-windows-sql-server-iaas-faq/). But also add your questions or comments to the bottom of any SQL VM topics to interact with  Microsoft  Azure.cn  and the community.
