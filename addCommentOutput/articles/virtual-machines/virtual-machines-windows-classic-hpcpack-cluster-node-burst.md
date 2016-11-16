<properties
 pageTitle="Add burst nodes to an HPC Pack cluster | Azure"
 description="Learn how to expand the HPC Pack cluster capacity on-demand by adding worker role instances running in a cloud service"
 services="virtual-machines-windows"
 documentationCenter=""
 authors="dlepow"
 manager="timlt"
 editor=""
 tags="azure-service-management,hpc-pack"/>
<tags
ms.service="virtual-machines-windows"
 ms.devlang="na"
 ms.topic="article"
 ms.tgt_pltfrm="vm-multiple"
 ms.workload="big-compute"
 ms.date="07/15/2016"
 wacn.date=""
 ms.author="danlep"/>

# Add on-demand "burst" nodes to an HPC Pack cluster in Azure



This article shows you how to add Azure "burst" nodes (worker role instances
running in a cloud service) on-demand as compute resources to an
existing HPC Pack head node in Azure. This lets you scale up the compute capacity of the HPC cluster in Azure on-demand, without maintaining a set of preconfigured compute node VMs.

[AZURE.INCLUDE [learn-about-deployment-models](../../includes/learn-about-deployment-models-classic-include.md)]

![Burst nodes][burst]

The steps in this article will help you add Azure nodes quickly to a
cloud-based HPC Pack head node VM for a test or proof of concept deployment. The procedure is essentially the
same as the one to "burst to Azure" to add cloud compute capacity to an
on-premises HPC Pack cluster. For a tutorial, see [Set up a hybrid compute cluster with Microsoft HPC Pack](/documentation/articles/cloud-services-setup-hybrid-hpcpack-cluster/). For
detailed guidance and considerations for production deployments, see
[Burst to Azure with Microsoft HPC
Pack](https://technet.microsoft.com/zh-cn/library/gg481749.aspx).


For considerations to use the A8 or A9 compute intensive instance size for the burst nodes, see
[About the A8, A9, A10, and A11 compute-intensive instances](/documentation/articles/virtual-machines-windows-a8-a9-a10-a11-specs/).


## Prerequisites

* **HPC Pack head node deployed in an Azure VM** - You can use a stand-alone head node VM or one that is part of a larger cluster. To create a stand-alone head node, see [Deploy an HPC
Pack Head Node in an Azure VM](/documentation/articles/virtual-machines-windows-hpcpack-cluster-headnode/). For automated HPC Pack cluster deployment options, see [Options to create and manage a Windows HPC cluster in Azure with Microsoft HPC Pack](/documentation/articles/virtual-machines-windows-hpcpack-cluster-options/).

    >[AZURE.TIP] If you use the [HPC Pack IaaS deployment script](/documentation/articles/virtual-machines-windows-classic-hpcpack-cluster-powershell-script/) to create the cluster in Azure,
you can include Azure burst nodes in your automated
deployment. See the examples in that article.

* **Azure subscription** - To add Azure nodes, you can choose the same
subscription used to deploy the head node VM, or a different
subscription (or subscriptions).

* **Cores quota** - You might need to increase the quota of cores, especially if you choose to deploy several Azure nodes with multicore sizes. To increase a quota, [open an online customer support request](https://azure.microsoft.com/blog/2014/06/04/azure-limits-quotas-increase-requests/) at no charge.

## Step 1: Create a cloud service and a storage account to add Azure nodes

Use the Azure Classic Management Portal or equivalent tools to configure the following, which are needed to deploy
your Azure nodes:

* A new Azure cloud service
* A new Azure storage account

>[AZURE.NOTE] Don't reuse an existing cloud service in your subscription. 

**Considerations**

* Configure a separate cloud service for each Azure node template that you plan to create. However, you can use the same storage account for multiple node templates.

* You should generally locate the cloud service and the storage account for the deployment in the same Azure region.




## Step 2: Configure an Azure management certificate

To add Azure nodes as compute resources, you'll need to have a management
certificate on the head node and upload a corresponding certificate
 to the Azure subscription used for the deployment.

For this scenario, you can choose the **Default HPC Azure Management
Certificate** that HPC Pack installs and configures automatically on the
head node. This certificate is useful for testing purposes and
proof-of-concept deployments. To use this certificate, simply upload the
file C:\Program Files\Microsoft HPC Pack 2012\Bin\hpccert.cer from the head node VM to the
subscription. You can do this in the [Azure Classic Management Portal](https://manage.windowsazure.cn). Click **Settings** > **Management Certificates**.

For additional options to configure the management certificate, see
[Scenarios to Configure the Azure Management Certificate for Azure Burst
Deployments](http://technet.microsoft.com/zh-cn/library/gg481759.aspx).

## Step 3: Deploy Azure nodes to the cluster



The steps to add and start
Azure nodes in this scenario are generally the same as those used with
an on-premises head node. For more information, see the following
sections in [Steps to Deploy Azure Nodes with Microsoft HPC Pack](https://technet.microsoft.com/zh-cn/library/gg481758.aspx):

* Create an Azure node template

* Add Azure nodes to the Windows HPC cluster

* Start (provision) the Azure nodes

After you add and start the nodes, they are ready for you to use to run cluster jobs.

If you encounter problems when deploying Azure nodes, see [Troubleshoot
Deployments of Azure Nodes with Microsoft HPC
Pack](http://technet.microsoft.com/zh-cn/library/jj159097.aspx).

## Next steps

* If you want to
automatically grow or shrink the Azure computing resources according to
the current workload of jobs and tasks on the cluster, see [Automatically grow and shrink Azure compute resources in an HPC Pack cluster](/documentation/articles/virtual-machines-windows-classic-hpcpack-cluster-node-autogrowshrink/).

<!--Image references-->
[burst]: ./media/virtual-machines-windows-classic-hpcpack-cluster-node-burst/burst.png