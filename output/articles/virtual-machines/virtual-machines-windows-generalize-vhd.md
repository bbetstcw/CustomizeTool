<properties
	pageTitle="Generalize a Windows VHD | Azure"
	description="Learn to use Sysprep to generalize a Windows VM to use with the Resource Manager deployment model."
	services="virtual-machines-windows"
	documentationCenter=""
	authors="cynthn"
	manager="timlt"
	editor="tysonn"
	tags="azure-resource-manager"/>

<tags
	ms.service="virtual-machines-windows"
	ms.workload="infrastructure-services"
	ms.tgt_pltfrm="vm-windows"
	ms.devlang="na"
	ms.topic="article"
	ms.date="10/11/2016"
	wacn.date=""
	ms.author="cynthn"/>
	
	
	
	
# Generalize a Windows virtual machine using Sysprep

This section shows you how to generalize your Windows virtual machine for use as an image. Sysprep removes all your personal account information, among other things, and prepares the machine to be used as an image. For details about Sysprep, see [How to Use Sysprep: An Introduction](http://technet.microsoft.com/zh-cn/library/bb457073.aspx).

>[AZURE.IMPORTANT] If you are running Sysprep before uploading your VHD to Azure for the first time, make sure you have [prepared your VM](/documentation/articles/virtual-machines-windows-prepare-for-upload-vhd-image/) before running Sysprep. 

1. Sign in to the Windows virtual machine.

2. Open the Command Prompt window as an administrator. Change the directory to **%windir%\system32\sysprep**, and then run `sysprep.exe`.

3. In the **System Preparation Tool** dialog box, select **Enter System Out-of-Box Experience (OOBE)**, and make sure that the **Generalize** check box is selected.

4. In **Shutdown Options**, select **Shutdown**.

5. Click **OK**.

	![Start Sysprep](./media/virtual-machines-windows-upload-image/sysprepgeneral.png)

6. When Sysprep completes, it will shutdown the virtual machine. 

## Next Steps

- If the VM is on-premises, you can now [upload the VHD to Azure](/documentation/articles/virtual-machines-windows-upload-image/).
- If the VM is already in Azure, you can now [create a VM from the generalized VHD](/documentation/articles/virtual-machines-windows-create-vm-generalized/).