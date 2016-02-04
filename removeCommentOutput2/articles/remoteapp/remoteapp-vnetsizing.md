
<properties
    pageTitle="Sizing information for a VNET in Azure RemoteApp | Windows Azure"
    description="Learn about the IP address requirements for Azure RemoteApp running with a VNET"
    services="remoteapp"
    documentationCenter=""
    authors="lizap"
    manager="mbaldwin" />

<tags
	ms.service="remoteapp"
	ms.date="12/05/2015"
	wacn.date=""/>



# Sizing information for a VNET in Azure RemoteApp

When you use Azure RemoteApp with a virtual network (VNET), RemoteApp uses IP addresses within the subnet. Based on the scale of your RemoteApp service, you need to ensure that your subnet has enough IP addresses available for RemoteApp virtual machines. While this sizing guidance isnât perfect given how RemoteApp dynamically spins up and spins down virtual machines within a collection, it will help you estimate your subnet range. This is especially important as, once a RemoteApp service is placed in a VNET, you cannot increase the subnet size without removing RemoteApp.

For each RemoteApp collection that you want to run at maximum capacity, you should have 100 IP addresses available. For example, if you have one RemoteApp collection in the Standard plan and you want to have the maximum 500 users, you should have 100 IP addresses for that collection. Similarly, you need 100 IP addresses for a RemoteApp collection in the Basic plan that has 800 users. If you plan to have fewer users (less than the maximum), you can reduce the IP addresses needed per collection. The minimum subnet size requirement is 30 IP addresses (/27).

Check out the following information to make sure your VNET is configured and working propertly:

- [Migrate from a personal VNET to an Azure VNET](/documentation/articles/remoteapp-migratevnet)
- [Validate the Azure VNET to use with Azure RemoteApp](/documentation/articles/remoteapp-vnet)

