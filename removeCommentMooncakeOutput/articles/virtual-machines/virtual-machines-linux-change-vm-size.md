<!-- ARM: tested -->

<properties
   pageTitle="How to resize a Linux VM | Azure"
   description="How to scale up or scale down a Linux virtual machine, by changing the VM size."
   services="virtual-machines-linux"
   documentationCenter="na"
   authors="mikewasson"
   manager="roshar"
   editor=""
   tags=""/>

<tags
	ms.service="virtual-machines-linux"
	ms.date="05/16/2016"
	wacn.date=""/>


# How to resize a Linux VM

[AZURE.INCLUDE [arm-api-version-cli](../includes/arm-api-version-cli.md)]

## Overview 

After you provision a virtual machine (VM), you can scale the VM up or down by changing the [VM size][vm-sizes]. In some cases, you must deallocate the VM first. This can happen if the new size is not available on the hardware cluster that is hosting the VM.

This article shows how to resize a Linux VM using the [Azure CLI][azure-cli].

> [AZURE.NOTE] Azure has two different deployment models for creating and working with resources:  [Resource Manager and classic](/documentation/articles/resource-manager-deployment-model/).  This article covers using the Resource Manager deployment model, which Azure recommends for most new deployments instead of the classic deployment model.


## Resize a Linux VM 

To resize a VM, perform the following steps.

1. Run the following CLI command. This command lists the VM sizes that are available on the hardware cluster where the VM is hosted.

    	azure vm sizes -g <resource-group> --vm-name <vm-name>

2. If the desired size is listed, run the following command to resize the VM.

	    azure vm set -g <resource-group> --vm-size <new-vm-size> -n <vm-name>  
	        --enable-boot-diagnostics --boot-diagnostics-storage-uri
	        https://<storage-account-name>.blob.core.chinacloudapi.cn/ 

    The VM will restart during this process. After the restart, your existing OS and data disks will be remapped. Anything on the temporary disk will be lost.

    Use the `--enable-boot-diagnostics` option enables [boot diagnostics][boot-diagnostics], to log any errors related to startup.

3. Otherwise, if the desired size is not listed, run the following commands to deallocate the VM, resize it, and then restart the VM.

	    azure vm deallocate -g <resource-group> <vm-name>
	    azure vm set -g <resource-group> --vm-size <new-vm-size> -n <vm-name>  
	        --enable-boot-diagnostics --boot-diagnostics-storage-uri
	        https://<storage-account-name>.blob.core.chinacloudapi.cn/ 
	    azure vm start -g <resource-group> <vm-name>

   > [AZURE.WARNING] Deallocating the VM also releases any dynamic IP addresses assigned to the VM. The OS and data disks are not affected.

<!-- links -->
   
[azure-cli]: /documentation/articles/xplat-cli-install/
[boot-diagnostics]: https://azure.microsoft.com/blog/boot-diagnostics-for-virtual-machines-v2/
[vm-sizes]: /documentation/articles/virtual-machines-linux-sizes