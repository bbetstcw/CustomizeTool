<properties
	pageTitle="Hybrid cloud test environments in Azure | Microsoft Azure"
	description="Find the articles that describe how to build dev/test or proof-of-concept IT pro environments for your Azure-based hybrid cloud."
	documentationCenter=""
	services="virtual-machines-windows"
	authors="JoeDavies-MSFT"
	manager="timlt"
	editor=""
	tags="azure-resource-manager"/>

<tags
	ms.service="virtual-machines-windows"
	ms.date="04/01/2016"
	wacn.date=""/>

# Azure hybrid cloud test environments

For dev/test or a proof-of-concept, hybrid cloud test environments use your local Internet connection and one of your public IP addresses and step you through setting up a functioning, cross-premises Azure Virtual Network (VNet). When you are finished, you can develop and test applications, experiment with simplified IT workloads, and gauge the performance of a site-to-site virtual private network (VPN) connection relative to your location on the Internet.

## Hybrid cloud base configuration

The [hybrid cloud base configuration](/documentation/articles/virtual-machines-windows-ps-hybrid-cloud-test-env-base/) consists of:

- A simplified on-premises network with four virtual machines (a domain controller, an application server, a client computer, and a VPN device running Windows Server and Routing and Remote Access)
- An Azure virtual network with a replica domain controller
- A site-to-site VPN connection

## SharePoint intranet farm in a hybrid cloud

The [SharePoint intranet farm in a hybrid cloud test environment](/documentation/articles/virtual-machines-windows-ps-hybrid-cloud-test-env-sp/) adds a SQL Server 2014 server and a SharePoint Server 2013 server to the hybrid cloud base configuration. This creates a two-tier SharePoint farm that you can access from the client computer on the simplified on-premises network.

## Web-based line-of-business (LOB) application in a hybrid cloud

The [web-based LOB application in a hybrid cloud test environment](/documentation/articles/virtual-machines-windows-ps-hybrid-cloud-test-env-lob/) adds a SQL Server 2014 server and an Internet Information Services (IIS) server to the hybrid cloud base configuration. This creates the infrastructure in which you can deploy and test a tiered, web-based LOB application.

## Office 365 Directory Synchronization (DirSync) server in a hybrid cloud

The [Office 365 DirSync server in a hybrid cloud test environment](/documentation/articles/virtual-machines-windows-ps-hybrid-cloud-test-env-dirsync/) adds a DirSync server to the hybrid cloud base configuration and demonstrates Office 365 DirSync with password synchronization to a trial Office 365 subscription.

## Simulated hybrid cloud test environment

For organizations and individuals for which a direct Internet connection and public IP address are not readily available, the [simulated hybrid cloud test environment](/documentation/articles/virtual-machines-windows-ps-hybrid-cloud-test-env-sim/) builds out the simplified on-premises network in a separate Azure Virtual Network and then connects the two virtual networks with a VNet-to-VNet VPN connection.


## Next step

- Learn about [implementation guidelines](/documentation/articles/virtual-machines-linux-infrastructure-service-guidelines/) to design a custom dev/test or production deployment in Azure infrastructure services.
