<properties 
	pageTitle="Compute Hosting Options Provided by Azure" 
	description="Learn about Azure compute hosting options and how they work: Virtual Machines, Websites, Cloud Services, and others." 
	headerExpose="" 
	footerExpose="" 
	services="cloud-services,virtual-machines"
	authors="Thraka" 
	documentationCenter=""
	manager="timlt"/>

<tags 
	ms.service="multiple" 
	ms.date="09/08/2015" 
	wacn.date=""/>




# Compute Hosting Options Provided by Azure

Azure provides different hosting models for running applications. Each one provides a different set of services, and so which one you choose depends on exactly what you're trying to do. This article looks at the options, describing each technology and giving examples of when you'd use it.

| Compute Options    | Audience   |
| ------------------ | --------   |
| [Azure Websites]      | Scalable Web Sites, Mobile Apps, API Apps, and Logic Apps for any device |
| [Cloud Services]   | Highly available, scalable n-tier cloud apps with more control of the OS |
| [Virtual Machines] | Customized Windows and Linux VMs with complete control of the OS |

[AZURE.INCLUDE [content](../includes/app-service-choose-me-content.md)]

[AZURE.INCLUDE [content](../includes/cloud-services-choose-me-content.md)]

[AZURE.INCLUDE [content](../includes/virtual-machines-choose-me-content.md)]

## Other Options

Azure also offers other compute hosting models for more specialized purposes, such as the following:

* [Mobile Services](/home/features/mobile-services/)  
  Optimized to provide a cloud back-end for apps that run on mobile devices.
* [Batch](/home/features/batch/)  
  Optimized for processing large volumes of similar tasks, ideally workloads which lend themselves to running as parallel tasks on multiple computers.
* [HDInsight (Hadoop)](/home/features/hdinsight/)  
  Optimized for running [MapReduce](http://www.asp.net/aspnet/overview/developing-apps-with-windows-azure/building-real-world-cloud-apps-with-windows-azure/data-storage-options/#hadoop) jobs on Hadoop clusters. 

## What should I use? Making a choice

All three of the general purpose Azure compute hosting models let you build scalable, reliable applications in the cloud. Given this essential similarity, which one should you use?

Azure Websites is the best choice for most web sites. Deployment and management are integrated into the platform, sites can scale quickly to handle high traffic loads, and the built-in load balancing and traffic manager provide high availability. You can move existing sites to Azure Websites easily with an [online migration tool](https://www.migratetochinacloudapi.cn/), use an open-source app from the Web Site Gallery, or create a new site using the framework and tools of your choice. The [WebJobs](/documentation/articles/websites-webjobs-resources) feature makes it easy to add background job processing to your app, or even run a compute workload that isn't a web site at all. 

If you need more control over the web server environment, such as the ability to remote into your server or configure server startup tasks, Azure Cloud Services is typically the best option.

If you have an existing application that would require substantial modifications to run in Azure Websites or Azure Cloud Services, you could choose Azure Virtual Machines in order to simplify migrating to the cloud. However, correctly configuring, securing, and maintaining VMs requires much more time and IT expertise compared to Azure Websites and Cloud Services. If you are considering Azure Virtual Machines, make sure you take into account the ongoing maintenance effort required to patch, update, and manage your VM environment.

Sometimes, no single option is right. In situations like this, it's perfectly legal to combine options. For example, suppose you're building an application where you'd like the management benefits of Cloud Services web roles, but you also need to use standard SQL Server hosted in a Virtual Machine for compatibility or performance reasons. 

<!-- In this case, the best option is to combine compute hosting options, as the figure below shows.--

<a name="fig4"></a>
![07_CombineTechnologies][07_CombineTechnologies] 
 
**Figure: A single application can use multiple hosting options.**

As the figure illustrates, the Cloud Services VMs run in a separate cloud service from the Virtual Machines VMs. Still, the two can communicate quite efficiently, so building an app this way is sometimes the best choice.
[07_CombineTechnologies]: ./media/fundamentals-application-models/ExecModels_07_CombineTechnologies.png
!-->

[Azure Websites]: #tellmeas
[Virtual Machines]: #tellmevm
[Cloud Services]: #tellmecs

## Next steps

* [Compare](/documentation/articles/choose-web-site-cloud-service-vm) Azure Websites, Cloud Services, and Virtual Machines
* Learn more about [Azure Websites](/home/features/web-site/)
* Learn more about [Cloud Service](/home/features/cloud-services/)
* Learn more about [Virtual Machines](https://msdn.microsoft.com/zh-cn/library/azure/jj156143.aspx) 