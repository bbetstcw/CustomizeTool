<properties
    pageTitle="Troubleshoot routes - PowerShell | Azure"
    description="Learn how to troubleshoot routes in the Azure Resource Manager deployment model using Azure PowerShell."
    services="virtual-network"
    documentationcenter="na"
    author="AnithaAdusumilli"
    manager="narayan"
    editor=""
    tags="azure-resource-manager" />
<tags
    ms.assetid="bf7dc5e7-9399-460e-8e0d-8992dbed98a6"
    ms.service="virtual-network"
    ms.devlang="na"
    ms.topic="article"
    ms.tgt_pltfrm="na"
    ms.workload="infrastructure-services"
    ms.date="09/23/2016"
    wacn.date=""
    ms.author="anithaa" />

# Troubleshoot routes using Azure PowerShell
> [AZURE.SELECTOR]
- [Azure Portal Preview](/documentation/articles/virtual-network-routes-troubleshoot-portal/)
- [PowerShell](/documentation/articles/virtual-network-routes-troubleshoot-powershell/)

If you are experiencing network connectivity issues to or from your Azure Virtual Machine (VM), routes may be impacting your VM traffic flows. This article provides an overview of diagnostics capabilities for routes to help troubleshoot further.

Route tables are associated with subnets and are effective on all network interfaces (NIC) in that subnet. The following types of routes can be applied to each network interface:

* **System routes:** By default, every subnet created in an Azure Virtual Network (VNet) has system route tables that allow local VNet traffic, on-premises traffic via VPN gateways, and Internet traffic. System routes also exist for peered VNets.
* **BGP routes:** Propagated to network interfaces through ExpressRoute or site-to-site VPN connections. Learn more about BGP routing by reading the [BGP with VPN gateways](/documentation/articles/vpn-gateway-bgp-overview/) and [ExpressRoute overview](/documentation/articles/expressroute-introduction/) articles.
* **User-defined routes (UDR):** If you are using network virtual appliances or are forced-tunneling traffic to an on-premises network via a site-to-site VPN, you may have user-defined routes (UDRs) associated with your subnet route table. If you're not familiar with UDRs, read the [user-defined routes](/documentation/articles/virtual-networks-udr-overview/#user-defined-routes) article.

With the various routes that can be applied to a network interface, it can be difficult to determine which aggregate routes are effective. To help troubleshoot VM network connectivity, you can view all the effective routes for a network interface in the Azure Resource Manager deployment model.

## Using Effective Routes to troubleshoot VM traffic flow
This article uses the following scenario as an example to illustrate how to troubleshoot the effective routes for a network interface:

A VM (*VM1*) connected to the VNet (*VNet1*, prefix: 10.9.0.0/16) fails to connect to a VM(VM3) in a newly peered VNet (*VNet3*, prefix 10.10.0.0/16). There are no UDRs or BGP routes applied to VM1-NIC1 network interface connected to the VM, only system routes are applied.

This article explains how to determine the cause of the connection failure, using effective routes capability in Azure Resource Management deployment model.
While the example uses only system routes, the same steps can be used to determine inbound and outbound connection failures over any route type.

> [AZURE.NOTE]
> If your VM has more than one NIC attached, check effective routes for each of the NICs to diagnose network connectivity issues to and from a VM.
> 
> 

### View effective routes for a virtual machine
To see the aggregate routes that are applied to a VM, complete the following steps:

### View effective routes for a network interface
To see the aggregate routes that are applied to a network interface, complete the following steps:

1. Start an Azure PowerShell session and login to Azure. If you're not familiar with Azure PowerShell, read the [How to install and configure Azure PowerShell](/documentation/articles/powershell-install-configure/) article.
2. The following command returns all routes applied to a network interface named *VM1-NIC1* in the resource group *RG1*.
   
       Get-AzureRmEffectiveRouteTable -NetworkInterfaceName VM1-NIC1 -ResourceGroupName RG1
   
   > [AZURE.TIP]
   > If you don't know the name of a network interface, type the following command to retrieve the names of all network interfaces in a resource group.*
   > 
   > 
   
       Get-AzureRmNetworkInterface -ResourceGroupName RG1 | Format-Table Name
   
   The following output looks similar to the output for each route applied to the subnet the NIC is connected to:
   
       Name :
       State : Active
       AddressPrefix : {10.9.0.0/16}
       NextHopType : VNetLocal
       NextHopIpAddress : {}
   
       Name :
       State : Active
       AddressPrefix : {0.0.0.0/16}
       NextHopType : Internet
       NextHopIpAddress : {}
   
   Notice the following in the output:
   
   * **Name**: Name of the effective route may be empty, unless explicitly specified, for user-defined routes. 
   * **State**: Indicates state of the effective route. Possible values are "Active" or "Invalid"
   * **AddressPrefixes**: Specifies the address prefix of the effective route in CIDR notation. 
   * **nextHopType**: Indicates the next hop for the given route. Possible values are *VirtualAppliance*, *Internet*, *VNetLocal*, *VNetPeering*, or *Null*. A value of *Null* for **nextHopType** in a UDR may indicate an invalid route. For example, if **nextHopType** is *VirtualAppliance* and the network virtual appliance VM is not in a provisioned/running state. If **nextHopType** is *VPNGateway* and there is no gateway provisioned/running in the given VNet, the route may become invalid.
   * **NextHopIpAddress**: Specifies the IP address of the next hop of the effective route.
   
   The following command returns the routes in an easier to view table:
   
       Get-AzureRmEffectiveRouteTable -NetworkInterfaceName VM1-NIC1 -ResourceGroupName RG1 | Format-Table
   
   The following output is some of the output received for the scenario described previously:
   
       Name State AddressPrefix NextHopType NextHopIpAddress
       ---- ----- ------------- ----------- ----------------
       Active {10.9.0.0/16} VnetLocal {}
       Active {0.0.0.0/0} Internet {}
3. There is no route listed to the *ChinaNorth-VNet3* VNet (Prefix 10.10.0.0/16)** from *ChinaNorth-VNet1* (Prefix 10.9.0.0/16) in the output from the previous step. As shown in the following picture, the VNet peering link with the *ChinaNorth-VNet3* VNet is in the *Disconnected* state.
   
    ![](./media/virtual-network-routes-troubleshoot-portal/image4.png)
   
    The bi-directional link for the peering is broken, which explains why VM1 could not connect to VM3 in the *ChinaNorth-VNet3* VNet. Setup a bi-directional VNet peering link again for *ChinaNorth-VNet1* and *ChinaNorth-VNet3* VNets. The output returned after the VNet peering link is correctly established follows:
   
        Name State AddressPrefix NextHopType NextHopIpAddress
        ---- ----- ------------- ----------- ----------------
        Active {10.9.0.0/16} VnetLocal {}
        Active {10.10.0.0/16} VNetPeering {}
        Active {0.0.0.0/0} Internet {}
   
    Once you determine the issue, you can add, remove, or change routes and route tables. Type the following command to see a list of the commands used to do so:
   
        Get-Help *-AzureRmRouteConfig

## Considerations
A few things to keep in mind when reviewing the list of routes returned:

* Routing is based on Longest Prefix Match (LPM) among UDRs, BGP and system routes. If there is more than one route with the same LPM match, then a route is selected based on its origin in the following order:
  
  * User-defined route
  * BGP route
  * System (Default) route
    
    With effective routes, you can only see effective routes that are LPM match based on all the availble routes. By showing how the routes are actually evaluated for a given NIC, this makes it a lot easier to troubleshoot specific routes that may be impacting connectivity to/from your VM.
* If you have UDRs and are sending traffic to a network virtual appliance (NVA), with *VirtualAppliance* as **nextHopType**, ensure that IP forwarding is enabled on the NVA receiving the traffic or packets are dropped. 
* If Forced tunneling is enabled, all outbound Internet traffic will be routed to on-premises. RDP/SSH from Internet to your VM may not work with this setting, depending on how the on-premises handles this traffic. 
  Forced-tunneling can be enabled:
  * If using site-to-site VPN, by setting a user-defined route (UDR) with nextHopType as VPN Gateway
  * If a default route is advertised over BGP
* For VNet peering traffic to work correctly, a system route with **nextHopType** *VNetPeering* must exist for the peered VNet's prefix range. If such a route doesn't exist and the VNet peering link looks OK:
  * Wait a few seconds and retry if it's a newly established peering link. It occasionally takes longer to propagate routes to all the network interfaces in a subnet.
  * Network Security Group (NSG) rules may be impacting the traffic flows. For more information, see the [Troubleshoot Network Security Groups](/documentation/articles/virtual-network-nsg-troubleshoot-powershell/) article.

