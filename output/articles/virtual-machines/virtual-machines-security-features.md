<!-- deleted in Global -->

<properties title="Security offerings for Azure Virtual Machines" pageTitle="Security offerings for Azure Virtual Machines" description="Quick overview of key security features for Azure VMs and links to details" metaKeywords="" services="virtual machines" solutions="" documentationCenter="" authors="kathydav" manager="timlt" videoId="" scriptId="" />
<tags 
	ms.service="virtual-machines" 

	ms.date="01/23/2015" 
	wacn.date=""/>

#Security offerings for Azure Virtual Machines

<p>The security tasks and configuration for Azure virtual machines are, for the most part, the same as for any other virtual machine or physical computer. These include such basics as using strong passwords, not using well-known user names, and establishing a policy and schedule for updating the operating system and applications. Additionally, we recommend the following practices and provide features that you can use to implement them: 

- Run anti-virus/anti-malware

- Use network access control lists (ACLs) on endpoints
 
##Run anti-virus/antimalware

Azure offers several options for anti-virus/antimalware solutions, but it's up to you to manage it. For example, you'll need to decide when to run scans and install updates. You can add anti-virus support when you create the virtual machine, or at a later point. Currently three solutions are offered as security extensions, which can be installed on both new and existing virtual machines:

- [How to install and configure Symantec Endpoint Protection on an Azure VM](/documentation/articles/virtual-machines-install-symantec/)
- [How to install and configure Trend Micro Deep Security as a Service on an Azure VM](/documentation/articles/virtual-machines-windows-classic-install-trend/)
- [Deploying Antimalware Solutions on Azure Virtual Machines](http://azure.microsoft.com/blog/2014/05/13/deploying-antimalware-solutions-on-azure-virtual-machines/)
 

##Use network access control lists (ACLs) on virtual machine endpoints

Network access control lists (ACLs) let you selectively permit or deny inbound traffic to a virtual machine endpoint. This packet filtering capability provides an additional layer of security. For details on how this works and links to instructions, see [About Network Access Control Lists (ACLs)](http://msdn.microsoft.com/zh-cn/library/azure/dn376541.aspx).

##Additional resources
[Resources](/support/trust-center/resources) on the Azure Trust Center


