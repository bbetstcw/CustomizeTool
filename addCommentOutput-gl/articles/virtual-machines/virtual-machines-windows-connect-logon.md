<properties
	pageTitle="Connect to a Windows Server VM | Microsoft Azure"
	description="Learn how to connect and log on to a Windows VM using the Azure portal and the Resource Manager deployment model."
	services="virtual-machines-windows"
	documentationCenter=""
	authors="cynthn"
	manager="timlt"
	editor="tysonn"
	tags="azure-resource-manager"/>

<tags
	ms.service="virtual-machines-windows"
	ms.date="05/05/2016"
	wacn.date=""/>

# How to connect and log on to an Azure virtual machine running Windows 


You'll use the **Connect** button in the Azure portal  Preview  to start a Remote Desktop (RDP) session. First you connect to the virtual machine, then you log on.

## Connect to the virtual machine

1. If you haven't already done so, sign in to the [Azure  portal](https://portal.azure.com/)  portal Preview](https://portal.azure.cn/) .

2.	On the Hub menu, click **Virtual Machines**.

3.	Select the virtual machine from the list.

4. On the blade for the virtual machine, click **Connect**.

	![Screenshot of the Azure portal  Preview  showing how to connect to your VM.](./media/virtual-machines-windows-connect-logon/connect.png)
	
 > [AZURE.TIP] If the 'Connect' button in the portal is greyed out and you are not connected to Azure via an [Express Route](/documentation/articles/expressroute-introduction/) or [Site-to-Site VPN](/documentation/articles/vpn-gateway-howto-site-to-site-resource-manager-portal/) connection, you need to create and assign your VM a public IP address before you can use RDP. You can read more about [public IP addresses in Azure](/documentation/articles/virtual-network-ip-addresses-overview-arm/).

## Log on to the virtual machine

[AZURE.INCLUDE [virtual-machines-log-on-win-server](../includes/virtual-machines-log-on-win-server.md)]


## Next steps

If you run into trouble when you try to connect, see [Troubleshoot Remote Desktop connections to a Windows-based Azure Virtual Machine](/documentation/articles/virtual-machines-windows-troubleshoot-rdp-connection/). This article walks you through diagnosing and resolving common problems.
