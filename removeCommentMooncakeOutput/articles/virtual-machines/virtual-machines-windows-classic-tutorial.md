<properties
	pageTitle="Create a VM running Windows in the Classic Management Portal | Azure"
	description="Create a Windows virtual machine in the Azure Classic Management Portal."
	services="virtual-machines-windows"
	documentationCenter=""
	authors="cynthn"
	manager="timlt"
	editor=""
	tags="azure-service-management"/>

<tags
	ms.service="virtual-machines-windows"
	ms.date="04/19/2016"
	wacn.date=""/>

# Create a virtual machine running Windows in the Azure Classic Management Portal

> [AZURE.SELECTOR]
- [Azure portal Preview](/documentation/articles/virtual-machines-windows-hero-tutorial/)
- [Azure Classic Management Portal](/documentation/articles/virtual-machines-windows-classic-tutorial/)
- [PowerShell: Resource Manager deployment](/documentation/articles/virtual-machines-windows-ps-manage/)
- [PowerShell: Classic deployment](/documentation/articles/virtual-machines-windows-classic-create-powershell/)

<!-- HHTML comment in to break between the selector and the note in the include below-->

[AZURE.INCLUDE [learn-about-deployment-models](../includes/learn-about-deployment-models-classic-include.md)] [Resource Manager deployment model](/documentation/articles/virtual-machines-windows-hero-tutorial/).

This tutorial shows you how easy it is to create an Azure virtual machine (VM) running Windows in the Azure Classic Management Portal. We'll use a Windows Server image as an example, but that's just one of the many images Azure offers.

You can also create VMs using [your own images](/documentation/articles/virtual-machines-windows-classic-createupload-vhd/). To learn about this and other methods, see [Different ways to create a Windows virtual machine](/documentation/articles/virtual-machines-windows-creation-choices/).

[AZURE.INCLUDE [free-trial-note](../includes/free-trial-note.md)]

## <a id="createvirtualmachine"> </a>How to create the virtual machine

This section shows you how to use the **From Gallery** option in the Azure Classic Management Portal to create the virtual machine. This option provides more configuration choices than the **Quick Create** option. For example, if you want to join a virtual machine to a virtual network, you'll need to use the **From Gallery** option.

> [AZURE.NOTE] You can also try the richer, customizable Azure portal Preview to create a virtual machine, use enhanced monitoring and diagnostics, use Premium storage, and more. The available options for configuring a virtual machine in the two portals overlap substantially but aren't identical. For example, use the Azure portal Preview to configure a virtual machine with Premium storage.

[AZURE.INCLUDE [virtual-machines-create-WindowsVM](../includes/virtual-machines-create-windowsvm.md)]

## Next steps

- Log on to the virtual machine. For instructions, see [Log on to a virtual machine running Windows Server](/documentation/articles/virtual-machines-windows-classic-connect-logon/).

- Attach a disk to store data. You can attach both empty disks and disks that contain data. For instructions, see the [Attach a data disk to a Windows virtual machine created with the classic deployment model](/documentation/articles/virtual-machines-windows-classic-attach-disk/).
