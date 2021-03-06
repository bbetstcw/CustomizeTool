<properties
	pageTitle="Role based access control troubleshooting"
	description="Working with different resource types for role based access control."
	services="azure-portal"
	documentationCenter="na"
	authors="IHenkel"
	manager="stevenpo"
	editor=""/>

<tags
	ms.service="active-directory"
	ms.date="12/04/2015"
	wacn.date=""/>

# Role based access control troubleshooting

## Introduction

[Role based access control](/documentation/articles/role-based-access-control-configure) is a powerful feature that allows you to delegate fine-grained access to resources in Azure. This means you can feel confident granting a specific person the right to just exactly what they need. However, at times the resource model for Azure resources can be complicated and it can be difficult to understand exactly what you are granting permissions to.

This document will let you know what to expect when using some of the roles in the Azure Management Portal. There are three common roles that are included that cover all resource types:
* Owner
* Contributor
* Reader

Owners and contributors will have full access to the management experience, the difference being that a contributor can't give access to other users or groups. Things get a little more interesting with the reader role, so that's where we'll spend some time. [See this article](/documentation/articles/role-based-access-control-configure) for details on how exactly to grant access.

## Azure Websites workloads

### Having read-access only

If you grant a user, or yourself only have, read access to a single web site, then there may be some features that are disabled that you might not expect. The following management capabilities require **write** access to a web site (either Contributor or Owner), and won't be available in any read only scenario.

1. Commands (e.g. start, stop, etc.)
2. Changing settings like general configuration, scale settings, backup settings, and monitoring settings.
3. Accessing publishing credentials and other secrets like app settings and connection strings.
4. Streaming logs
5. Diagnostic logs configuration
6. Console (command prompt)
7. Active and recent deployments (for local git continuous deployment)
8. Estimated spend
9. Web tests
10. Virtual network (only visible to a reader if a virtual network has previously been configured by a user with write access).

If you can't access any of these tiles, you'll need to have Contributor access to the web site.

### Dealing with related resources

web sites are complicated by the presence of a few different resources that interplay. Here is a typical resource group with a couple websites:

![web site resource group](./media/role-based-access-control-troubleshooting/website-resource-model.png)

As a result, if you grant someone access to just the website, much functionality on the website blade will be completely disabled.

1. These items require access to the **App Service plan** that corresponds to your website:  
    * Viewing the web site's pricing tier (e.g. Free or Standard).
    * Scale configuration (i.e. # of instances, virtual machine size, autoscale settings).
    * Quotas (e.g. Storage, bandwidth, CPU).
2. These items require access to the whole **Resource group** that contains your website:  
    * SSL Certificates and bindings (This is because SSL certificates can be shared between sites in the same resource group and geo-location).
    * Alert rules
    * Autoscale settings
    * Application insights components
    * Web tests

## Virtual machine workloads

Much like with web sites, some features on the virtual machine blade require write access to the virtual machine, or to other resources in the resource group.

Virtual machines have these related resources:
* Domain names
* Virtual networks
* Storage accounts
* Alert rules

1. These items require **write** access to the Virtual machine:  
    * Endpoints
    * IP addresses
    * Disks
    * Extensions
2. These require write access to both the Virtual machine, and the **Resource group** (along with the Domain name) that it is in:  
    * Availability set
    * Load balanced set
    * Alert rules

If you can't access any of these tiles, you'll need to ask your administrator for Contributor access to the Resource group.
