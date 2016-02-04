<properties
   pageTitle="NAT requirements for ExpressRoute circuits | Windows Azure"
   description="This page provides detailed requirements for configuring and managing NAT for ExpressRoute circuits."
   documentationCenter="na"
   services="expressroute"
   authors="cherylmc"
   manager="carolz"
   editor=""/>
<tags
   ms.service="expressroute"
   ms.date="10/06/2015"
   wacn.date=""/>

# ExpressRoute NAT requirements

To connect to Microsoft cloud services using ExpressRoute, youâll need to set up and manage NATs. Some connectivity providers offer setting up and managing NAT as a managed service. Check with your connectivity provider to see if they offer such a service. If not, you must adhere to the requirements described below. 

Review the [ExpressRoute circuits and routing domains](/documentation/articles/expressroute-circuit-peerings) page to get an overview of the various routing domains. To meet the public IP address requirements for Azure public and Microsoft peering, we recommend that you set up NAT between your network and Microsoft. This section provides a detailed description of the NAT infrastructure you need to set up.

## NAT requirements for Azure public peering

The Azure public peering path enables you to connect to all services hosted in Azure over their public IP addresses. These include services all services listed in the [ExpessRoute FAQ](/documentation/articles/expressroute-faqs) and any services hosted by ISVs on Windows Azure. Connectivity to Windows Azure services on public peering is always initiated from your network into the Microsoft network. Traffic destined to Windows Azure on public peering must be SNATed to valid public IPv4 addresses before they enter the Microsoft network. The figure below provides a high-level picture of how the NAT could be set up to meet the above requirement.

![](./media/expressroute-nat/expressroute-nat-azure-public.png) 

### NAT IP pool and route advertisements

You must ensure that traffic is entering the Azure public peering path with valid public IPv4 address. Microsoft must be able to validate the ownership of the IPv4 NAT address pool against a regional routing Internet registry (RIR) or an Internet routing registry (IRR). A check will be performed based on the AS number being peered with and the IP addresses used for the NAT. Refer to the [ExpressRoute routing requirements](/documentation/articles/expressroute-routing) page for information on routing registries.
 
There are no restrictions on the length of the NAT IP prefix advertised through this peering. You must monitor the NAT pool and ensure that you are not starved of NAT sessions.

>[AZURE.IMPORTANT] The NAT IP pool advertised to Microsoft must not be advertised to the Internet. This will break connectivity to other Microsoft services.

## NAT requirements for Microsoft peering

The Microsoft peering path lets you connect to Microsoft cloud services that are not supported through the Azure public peering path. The list of services includes Office 365 services, such as Exchange Online, SharePoint Online, Skype for Business, and CRM Online. Microsoft expects to support bi-directional connectivity on the Microsoft peering. Traffic destined to Microsoft cloud services must be SNATed to valid public IPv4 addresses before they enter the Microsoft network. Traffic destined to the your network from Microsoft cloud services must be SNATed before they enter your network. The figure below provides a high-level picture of how the NAT should be setup for Microsoft peering.
 
![](./media/expressroute-nat/expressroute-nat-microsoft.png) 


#### Traffic originating from your network destined to Microsoft

- You must ensure that traffic is entering the Microsoft peering path with a valid public IPv4 address. Microsoft must be able to validate the owner of the IPv4 NAT address pool against the regional routing internet registry (RIR) or an internet routing registry (IRR). A check will be performed based on the AS number being peered with and the IP addresses used for the NAT. Refer to the [ExpressRoute routing requirements](/documentation/articles/expressroute-routing) page for information on routing registries.

- IP addresses used for the Azure public peering setup and other ExpressRoute circuits must not be advertised to Microsoft through the BGP session. There is no restriction on the length of the NAT IP prefix advertised through this peering.

	>[AZURE.IMPORTANT] The NAT IP pool advertised to Microsoft must not be advertised to the Internet. This will break connectivity to other Microsoft services.

#### Traffic originating from Microsoft destined to your network

- Certain scenarios require Microsoft to initiate connectivity to service endpoints hosted within your network. A typical example of the scenario would be connectivity to ADFS servers hosted in your network from Office 365. In such cases, you must leak appropriate prefixes from your network into the Microsoft peering. 

- You must SNAT traffic destined to IP addresses within your network from Microsoft. 

## Next steps

- Refer to the requirements for [Routing](/documentation/articles/expressroute-routing) and [QoS](/documentation/articles/expressroute-qos).
- For workflow information, see [ExpressRoute circuit provisioning workflows and circuit states](/documentation/articles/expressroute-workflows).
- Configure your ExpressRoute connection.

	- [Create an ExpressRoute circuit](/documentation/articles/expressroute-howto-circuit-classic)
	- [Configure routing](/documentation/articles/expressroute-howto-routing-classic)
	- [Link a VNet to an ExpressRoute circuit](/documentation/articles/expressroute-howto-linkvnet-classic)

