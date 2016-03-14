<properties
	pageTitle="Different ways to create a Linux VM | Windows Azure"
	description="Lists the different ways to create a Linux virtual machine on Azure and gives links to further instructions."
	services="virtual-machines"
	documentationCenter=""
	authors="dsk-2015"
	manager="timlt"
	editor=""
	tags="azure-service-management,azure-resource-manager"/>

<tags
	ms.service="virtual-machines"
	ms.date="01/20/2016"
	wacn.date=""/>

# Different Ways to Create a Linux Virtual Machine

[AZURE.INCLUDE [learn-about-deployment-models](../includes/learn-about-deployment-models-both-include.md)]

Azure offers different ways to create a VM, to suit different users and purposes. This article summarizes these differences and the choices you can make for creating your Linux virtual machines.

Azure Resource Manager templates can also be used to create and manage a virtual machine and its different resources as one logical deployment unit. Instructions for this approach are included where applicable. To learn more about Azure Resource Manager and how to manage resources as one unit, see the [overview][].

## Tool choices

### GUI: The Azure Management Portal or Azure Management Portal

The graphical user interface of the portal is an easy way to try out a virtual machine, especially if you're just starting out with Azure. Use either the [Azure Management Portal](https://manage.windowsazure.cn) or the [Azure Management Portal](https://manage.windowsazure.cn) to create the VM. For general instructions, see [Create a Custom Virtual Machine][] and select any Linux image from the **Gallery**. Note that the [Azure Management Portal](https://manage.windowsazure.cn) creates virtual machines using only the classic deployment model.

### Command Shell: Azure CLI or Azure PowerShell

If you prefer working in a command shell, choose between the Azure command-line interface (CLI) for Mac, Linux and Windows users, or the Azure PowerShell, which has Windows PowerShell cmdlets for Azure and a custom console.

To create virtual machines in Resource Manager deployment model using the Azure CLI, see [Create a Virtual Machine Running Linux][]. Follow the tabs or article selectors on this article to read instructions using Azure PowerShell and Templates.  

For classic deployment model, see [Create a custom Linux virtual machine using Azure CLI](/documentation/articles/virtual-machines-linux-create-custom) and
[Use Azure PowerShell to create and preconfigure Linux-based Virtual Machines][].


### Development Environment: Visual Studio

Visual Studio also supports creating Azure virtual machines. For classic deployment model, read [Creating a virtual machine for a website with Visual Studio][]. For Resource Manager deployment model, read [Deploy Azure Resources Using the Compute, Network, and Storage .NET Libraries][].


## Operating System and Image Choices

Choose an image based on the operating system you want to run. Azure and its partners offer many images, some of which include applications and tools. Or, use one of your own images.


### Azure Images

In all of the above articles, you can easily use an existing Azure image to create a virtual machine and customize it for networking, load balancing, and more. The portals provide a gallery or image list for Azure supplied images. You can get similar lists using the command line. For example, in Azure CLI, run `azure vm image list` to get a list of all available images, by location and publisher.


### Use Your Own Image

Use an image based on an existing Azure virtual machine by *capturing* that VM, or upload an image of your own, stored in a virtual hard disk (VHD). For classic deployment model, see [How to Capture a Linux Virtual Machine to Use as a Template with the CLI][] and [Creating and Uploading a Virtual Hard Disk that Contains the Linux Operating System][]. For Resource Manager deployment model, see [How to capture a Linux virtual machine as a Resource Manager template](/documentation/articles/virtual-machines-linux-capture-image-resource-manager).

## Next Steps

[Log On to the Virtual Machine][]

[Attach a Data Disk][]

## Additional resources

[Base Configuration Test Environment][]

[Azure hybrid cloud test environments][]

[Equivalent Resource Manager and Service Management Commands for VM Operations with the Azure CLI for Mac, Linux, and Windows][]

<!-- LINKS -->
[overview]: ../documentation/articles/resource-group-overview

[Create a Virtual Machine Running Windows]: virtual-machines-windows-tutorial.md
[Create a Virtual Machine Running Linux]: virtual-machines-linux-tutorial.md

[Equivalent Resource Manager and Service Management Commands for VM Operations with the Azure CLI for Mac, Linux, and Windows]:xplat-cli-azure-manage-vm-asm-arm.md
[Deploy and Manage Virtual Machines using Azure Resource Manager Templates and the Azure CLI]: virtual-machines-deploy-rmtemplates-azure-cli.md
[Deploy and Manage Virtual Machines using Azure Resource Manager Templates and PowerShell]:  virtual-machines-deploy-rmtemplates-powershell.md
[Use Azure PowerShell to create and preconfigure Linux-based Virtual Machines]: virtual-machines-ps-create-preconfigure-linux-vms.md

[How to Create a Custom Virtual Machine Running Linux in Azure]: virtual-machines-linux-create-custom.md
[How to Capture a Linux Virtual Machine to Use as a Template with the CLI]: virtual-machines-linux-capture-image.md

[Creating and Uploading a Virtual Hard Disk that Contains the Linux Operating System]: virtual-machines-linux-create-upload-vhd.md

[Creating a virtual machine for a website with Visual Studio]: virtual-machines-dotnet-create-visual-studio-powershell.md
[Deploy Azure Resources Using the Compute, Network, and Storage .NET Libraries]: virtual-machines-arm-deployment.md

[Log On to the Virtual Machine]: virtual-machines-linux-how-to-log-on.md

[Attach a Data Disk]: virtual-machines-linux-how-to-attach-disk.md

[Base Configuration Test Environment]: virtual-machines-base-configuration-test-environment.md
[Azure hybrid cloud test environments]: virtual-machines-hybrid-cloud-test-environments.md

[Create a Virtual Machine Running Linux]: virtual-machines-linux-tutorial.md
[Create a Custom Virtual Machine]: virtual-machines-create-custom.md
