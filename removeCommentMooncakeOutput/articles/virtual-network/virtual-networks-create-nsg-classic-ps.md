<!-- ARM: tested -->

<properties
   pageTitle="How to create NSGs in classic mode using PowerShell| Azure"
   description="Learn how to create and deploy NSGs in classic mode using PowerShell"
   services="virtual-network"
   documentationCenter="na"
   authors="telmosampaio"
   manager="carmonm"
   editor="tysonn"
   tags="azure-service-management"
/>
<tags
	ms.service="virtual-network"
	ms.date="02/02/2016"
	wacn.date=""/>

# How to create NSGs (classic) in PowerShell

[AZURE.INCLUDE [virtual-networks-create-nsg-selectors-classic-include](../includes/virtual-networks-create-nsg-selectors-classic-include.md)]

[AZURE.INCLUDE [arm-api-version-powershell](../includes/arm-api-version-powershell.md)]

[AZURE.INCLUDE [virtual-networks-create-nsg-intro-include](../includes/virtual-networks-create-nsg-intro-include.md)]

>[AZURE.IMPORTANT]Before you work with Azure resources, it's important to understand that Azure currently has two deployment models: Resource Manager, and classic. Make sure you understand [deployment models and tools](/documentation/articles/azure-classic-rm/) before working with any Azure resource. You can view the documentation for different tools by clicking the tabs at the top of this article. This article covers the classic deployment model. You can also [create NSGs in the Resource Manager deployment model](/documentation/articles/virtual-networks-create-nsg-arm-ps/).

[AZURE.INCLUDE [virtual-networks-create-nsg-scenario-include](../includes/virtual-networks-create-nsg-scenario-include.md)]

The sample PowerShell commands below expect a simple environment already created based on the scenario above. If you want to run the commands as they are displayed in this document, first build the test environment by [creating a VNet](/documentation/articles/virtual-networks-create-vnet-classic-netcfg-ps/).

## How to create the NSG for the front end subnet
To create an NSG named named **NSG-FrontEnd** based on the scenario above, follow the steps below:

1. If you have never used Azure PowerShell, see [How to Install and Configure Azure PowerShell](/documentation/articles/powershell-install-configure/) and follow the instructions all the way to the end to sign into Azure and select your subscription.

3. Create a network security group named **NSG-FrontEnd**.

		New-AzureNetworkSecurityGroup -Name "NSG-FrontEnd" -Location chinanorth `
		    -Label "Front end subnet NSG"

	Expected output:

		Name         Location   Label               
		----         --------   -----               
		NSG-FrontEnd China North 	Front end subnet NSG


4. Create a security rule allowing access from the Internet to port 3389.

		Get-AzureNetworkSecurityGroup -Name "NSG-FrontEnd" `
		| Set-AzureNetworkSecurityRule -Name rdp-rule `
		    -Action Allow -Protocol TCP -Type Inbound -Priority 100 `
		    -SourceAddressPrefix Internet  -SourcePortRange '*' `
		    -DestinationAddressPrefix '*' -DestinationPortRange '3389'

	Expected output:

		Name     : NSG-FrontEnd
		Location : China North
		Label    : Front end subnet NSG
		Rules    :

		              Type: Inbound

		           Name                 Priority  Action   Source Address  Source Port   Destination      Destination    Protocol
		                                                   Prefix          Range         Address Prefix   Port Range             
		           ----                 --------  ------   --------------- ------------- ---------------- -------------- --------
		           rdp-rule             100       Allow    INTERNET        *             *                3389           TCP     
		           ALLOW VNET INBOUND   65000     Allow    VIRTUAL_NETWORK *             VIRTUAL_NETWORK  *              *       
		           ALLOW AZURE LOAD     65001     Allow    AZURE_LOADBALAN *             *                *              *       
		           BALANCER INBOUND                        CER                                                                   
		           DENY ALL INBOUND     65500     Deny     *               *             *                *              *       


		              Type: Outbound

		           Name                 Priority  Action   Source Address  Source Port   Destination      Destination    Protocol
		                                                   Prefix          Range         Address Prefix   Port Range             
		           ----                 --------  ------   --------------- ------------- ---------------- -------------- --------
		           ALLOW VNET OUTBOUND  65000     Allow    VIRTUAL_NETWORK *             VIRTUAL_NETWORK  *              *       
		           ALLOW INTERNET       65001     Allow    *               *             INTERNET         *              *       
		           OUTBOUND                                                                                                      
		           DENY ALL OUTBOUND    65500     Deny     *               *             *                *              *

4. Create a security rule allowing access from the Internet to port 80.

		Get-AzureNetworkSecurityGroup -Name "NSG-FrontEnd" `
		| Set-AzureNetworkSecurityRule -Name web-rule `
		    -Action Allow -Protocol TCP -Type Inbound -Priority 200 `
		    -SourceAddressPrefix Internet  -SourcePortRange '*' `
		    -DestinationAddressPrefix '*' -DestinationPortRange '80'

	Expected output:


		Name     : NSG-FrontEnd
		Location : China North
		Label    : Front end subnet NSG
		Rules    :

		              Type: Inbound

		           Name                 Priority  Action   Source Address  Source Port   Destination      Destination    Protocol
		                                                   Prefix          Range         Address Prefix   Port Range             
		           ----                 --------  ------   --------------- ------------- ---------------- -------------- --------
		           rdp-rule             100       Allow    INTERNET        *             *                3389           TCP     
		           web-rule             200       Allow    INTERNET        *             *                80             TCP     
		           ALLOW VNET INBOUND   65000     Allow    VIRTUAL_NETWORK *             VIRTUAL_NETWORK  *              *       
		           ALLOW AZURE LOAD     65001     Allow    AZURE_LOADBALAN *             *                *              *       
		           BALANCER INBOUND                        CER                                                                   
		           DENY ALL INBOUND     65500     Deny     *               *             *                *              *       


		              Type: Outbound

		           Name                 Priority  Action   Source Address  Source Port   Destination      Destination    Protocol
		                                                   Prefix          Range         Address Prefix   Port Range             
		           ----                 --------  ------   --------------- ------------- ---------------- -------------- --------
		           ALLOW VNET OUTBOUND  65000     Allow    VIRTUAL_NETWORK *             VIRTUAL_NETWORK  *              *       
		           ALLOW INTERNET       65001     Allow    *               *             INTERNET         *              *       
		           OUTBOUND                                                                                                      
		           DENY ALL OUTBOUND    65500     Deny     *               *             *                *              *   

## How to create the NSG for the back end subnet
3. Create a network security group named **NSG-BackEnd**.

		New-AzureNetworkSecurityGroup -Name "NSG-BackEnd" -Location chinanorth `
		    -Label "Back end subnet NSG"

	Expected output:

		Name        Location   Label              
		----        --------   -----              
		NSG-BackEnd China North    Back end subnet NSG


4. Create a security rule allowing access from the front end subnet to port 1433 (default port used by SQL Server).

		Get-AzureNetworkSecurityGroup -Name "NSG-FrontEnd" `
		| Set-AzureNetworkSecurityRule -Name rdp-rule `
		    -Action Allow -Protocol TCP -Type Inbound -Priority 100 `
		    -SourceAddressPrefix 192.168.1.0/24  -SourcePortRange '*' `
		    -DestinationAddressPrefix '*' -DestinationPortRange '1433'

	Expected output:

		Name     : NSG-BackEnd
		Location : China North
		Label    : Back end subnet NSG
		Rules    :

		              Type: Inbound

		           Name                 Priority  Action   Source Address  Source Port   Destination      Destination    Protocol
		                                                   Prefix          Range         Address Prefix   Port Range             
		           ----                 --------  ------   --------------- ------------- ---------------- -------------- --------
		           fe-rule              100       Allow    192.168.1.0/24  *             *                1433           TCP     
		           ALLOW VNET INBOUND   65000     Allow    VIRTUAL_NETWORK *             VIRTUAL_NETWORK  *              *       
		           ALLOW AZURE LOAD     65001     Allow    AZURE_LOADBALAN *             *                *              *       
		           BALANCER INBOUND                        CER                                                                   
		           DENY ALL INBOUND     65500     Deny     *               *             *                *              *       


		              Type: Outbound

		           Name                 Priority  Action   Source Address  Source Port   Destination      Destination    Protocol
		                                                   Prefix          Range         Address Prefix   Port Range             
		           ----                 --------  ------   --------------- ------------- ---------------- -------------- --------
		           ALLOW VNET OUTBOUND  65000     Allow    VIRTUAL_NETWORK *             VIRTUAL_NETWORK  *              *       
		           ALLOW INTERNET       65001     Allow    *               *             INTERNET         *              *       
		           OUTBOUND                                                                                                      
		           DENY ALL OUTBOUND    65500     Deny     *               *             *                *              *      

4. Create a security rule blocking access from the subnet to the Internet.

		Get-AzureNetworkSecurityGroup -Name "NSG-BackEnd" `
		| Set-AzureNetworkSecurityRule -Name block-internet `
		    -Action Deny -Protocol '*' -Type Outbound -Priority 200 `
		    -SourceAddressPrefix '*'  -SourcePortRange '*' `
		    -DestinationAddressPrefix Internet -DestinationPortRange '*'

	Expected output:

		Name     : NSG-BackEnd
		Location : China North
		Label    : Back end subnet NSG
		Rules    :

		              Type: Inbound

		           Name                 Priority  Action   Source Address  Source Port   Destination      Destination    Protocol
		                                                   Prefix          Range         Address Prefix   Port Range             
		           ----                 --------  ------   --------------- ------------- ---------------- -------------- --------
		           fe-rule              100       Allow    192.168.1.0/24  *             *                1433           TCP     
		           ALLOW VNET INBOUND   65000     Allow    VIRTUAL_NETWORK *             VIRTUAL_NETWORK  *              *       
		           ALLOW AZURE LOAD     65001     Allow    AZURE_LOADBALAN *             *                *              *       
		           BALANCER INBOUND                        CER                                                                   
		           DENY ALL INBOUND     65500     Deny     *               *             *                *              *       


		              Type: Outbound

		           Name                 Priority  Action   Source Address  Source Port   Destination      Destination    Protocol
		                                                   Prefix          Range         Address Prefix   Port Range             
		           ----                 --------  ------   --------------- ------------- ---------------- -------------- --------
		           block-internet       200       Deny     *               *             INTERNET         *              *       
		           ALLOW VNET OUTBOUND  65000     Allow    VIRTUAL_NETWORK *             VIRTUAL_NETWORK  *              *       
		           ALLOW INTERNET       65001     Allow    *               *             INTERNET         *              *       
		           OUTBOUND                                                                                                      
		           DENY ALL OUTBOUND    65500     Deny     *               *             *                *              *   
