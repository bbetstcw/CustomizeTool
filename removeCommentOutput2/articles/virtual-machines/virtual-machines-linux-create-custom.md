<properties
	pageTitle="Create a Linux VM | Windows Azure"
	description="Learn how to create a custom virtual machine with the classic deployment model running the Linux operating system."
	services="virtual-machines"
	documentationCenter=""
	authors="dsk-2015"
	manager="timlt"
	editor="tysonn"
	tags="azure-service-management"/>

<tags
	ms.service="virtual-machines"
	ms.date="10/14/2015"
	wacn.date=""/>

# How to Create a Custom Virtual Machine Running Linux in Azure

This topic describes how to create a *custom* virtual machine with the Azure CLI using the classic deployment model. We will use a Linux image from the available **IMAGES** on Azure. The Azure CLI commands give following configuration choices among others:

- Connecting the VM to a virtual network
- Adding the VM to an existing cloud service
- Adding the VM to an existing storage account
- Adding the VM to an availability set or location

> [AZURE.IMPORTANT] If you want your virtual machine to use a virtual network so you can connect to it directly by hostname or set up cross-premises connections, make sure you specify the virtual network when you create the virtual machine. A virtual machine can be configured to join a virtual network only when you create the virtual machine. For details on virtual networks, see [Azure Virtual Network Overview](http://msdn.microsoft.com/zh-CN/library/azure/jj156007.aspx).

<p/>
[AZURE.INCLUDE [service-management-pointer-to-resource-manager](../includes/service-management-pointer-to-resource-manager.md)]

- [Create a Virtual Machine Running Linux](/documentation/articles/virtual-machines-linux-tutorial)


## How to create a Linux virtual machine using the classic deployment model

[AZURE.INCLUDE [virtual-machines-create-LinuxVM](../includes/virtual-machines-create-linuxvm.md)]