<properties
	pageTitle="Create and upload a Linux VHD | Windows Azure"
	description="Create and upload an Azure virtual hard disk (VHD) with the classic deployment model that contains the Linux operating system."
	services="virtual-machines"
	documentationCenter=""
	authors="dsk-2015"
	manager="timlt"
	editor="tysonn"
	tags="azure-service-management"/>

<tags
	ms.service="virtual-machines"
	ms.date="07/29/2015"
	wacn.date=""/>

# Creating and Uploading a Virtual Hard Disk that Contains the Linux Operating System

This article shows you how to create and upload a virtual hard disk (VHD) so you can use it as your own image to create virtual machines in Azure. You'll learn how to prepare the operating system so you can use it to create multiple virtual machines based on that image. Note that this article refers to virtual machines created using the classic deployment model.

[AZURE.INCLUDE [free-trial-note](../includes/free-trial-note.md)]

[AZURE.INCLUDE [learn-about-deployment-models](../includes/learn-about-deployment-models-include.md)] This article covers uploading a VHD with the Service Manager.

A virtual machine in Azure runs the operating system that's based on the image you choose when you create the virtual machine. Your images are stored in VHD format, in .vhd files in a storage account. For details, see [About Disks and Images in Azure](https://msdn.microsoft.com/zh-cn/library/azure/jj672979.aspx).

When you create the virtual machine, you can customize some of the operating system settings so they're appropriate for the application you want to run. For instructions, see [How to Create a Custom Virtual Machine](/documentation/articles/virtual-machines-create-custom).

**Important**: The Azure platform SLA applies to virtual machines running the Linux OS only when one of the endorsed distributions is used with the configuration details as specified under 'Supported Versions' in [Linux on Azure-Endorsed Distributions](/documentation/articles/virtual-machines-linux-endorsed-distributions). All Linux distributions in the Azure image gallery are endorsed distributions with the required configuration.


##Prerequisites##
This article assumes that you have the following items:

- **A management certificate** - You have created a management certificate for the subscription for which you want to upload a VHD, and exported the certificate to a .cer file. For more information about creating certificates, see [Create and Upload a Management Certificate for Azure](https://msdn.microsoft.com/zh-cn/library/azure/gg551722.aspx).

- **Linux operating system installed in a .vhd file**  - You have installed a supported Linux operating system to a virtual hard disk. Multiple tools exist to create .vhd files, for example you can use a virtualization solution such as Hyper-V to create the .vhd file and install the operating system. For instructions, see [Install the Hyper-V Role and Configure a Virtual Machine](http://technet.microsoft.com/zh-cn/library/hh846766.aspx).

	**Important**: The newer VHDX format is not supported in Azure. You can convert the disk to VHD format using Hyper-V Manager or the convert-vhd cmdlet.

	For a list of endorsed distributions, see [Linux on Azure-Endorsed Distributions](/documentation/articles/linux-endorsed-distributions). Alternatively, see the section at the end of this article for [Information for Non-Endorsed Distributions](/documentation/articles/virtual-machines-linux-create-upload-vhd-generic).

- **Azure Command-line Interface** - if you are using a Linux operating system to create your image, you use the [Azure Command-Line Interface](/documentation/articles/virtual-machines-command-line-tools) to upload the VHD.

- **Azure Powershell tools** - the `Add-AzureVhd` cmdlet can also be used to upload the VHD. See [Azure Downloads](/downloads/) to download the Azure Powershell cmdlets. For reference information, see [Add-AzureVhd](https://msdn.microsoft.com/zh-cn/library/azure/dn495173.aspx).

## <a id="prepimage"> </a>Step 1: Prepare the image to be uploaded ##

Azure supports a variety of Linux distributions (see [Endorsed Distributions](/documentation/articles/linux-endorsed-distributions)). The following articles will guide you through how to prepare the various Linux distributions that are supported on Azure:

- **[CentOS-based Distributions](/documentation/articles/virtual-machines-linux-create-upload-vhd-centos)**
- **[Oracle Linux](/documentation/articles/virtual-machines-linux-create-upload-vhd-oracle)**
- **[SLES & openSUSE](/documentation/articles/virtual-machines-linux-create-upload-vhd-suse)**
- **[Ubuntu](/documentation/articles/virtual-machines-linux-create-upload-vhd-ubuntu)**
- **[Other - Non-Endorsed Distributions](/documentation/articles/virtual-machines-linux-create-upload-vhd-generic)**

Also see the **[Linux Installation Notes](/documentation/articles/virtual-machines-linux-create-upload-vhd-generic#linuxinstall)** for more tips on preparing Linux images for Azure.

After following the steps in the guides above you should have a VHD file that is ready to upload into Azure.


## <a id="connect"> </a>Step 2: Prepare the connection to Azure ##

Before you can upload a .vhd file, you need to establish a secure connection between your computer and your subscription in Azure.


### If using Azure CLI

Use Azure AD method to login:

1. Open an Azure CLI window

2. Type:

	`azure login`

	When prompted, type your username and password.

**OR**, use a PublishSettings file instead:

1. Open an Azure CLI window

2. Type:

	`azure account download`

	This command opens a browser window and automatically downloads a .publishsettings file that contains information and a certificate for your Azure subscription.

3. Save the .publishsettings file

4. Type:

	`azure account import <PathToFile>`

	Where `<PathToFile>` is the full path to the .publishsettings file.

	For more information, read [Connect to Azure from Azure CLI](/documentation/articles/xplat-cli-connect).


### If using Azure PowerShell

Use Azure AD method to login:

1. Open an Azure PowerShell window.

2. Type:

	`Add-AzureAccount`

	When prompted, enter your organizational user id and password.

**OR**, use the PublishSettings files instead:

1. Open an Azure PowerShell window.

2. Type:

	`Get-AzurePublishSettingsFile`

	This command opens a browser window and automatically downloads a .publishsettings file that contains information and a certificate for your Azure subscription.

3. Save the .publishsettings file.

4. Type:

	`Import-AzurePublishSettingsFile <PathToFile>`

	Where `<PathToFile>` is the full path to the .publishsettings file.

	For more information, see [How to install and configure Azure PowerShell](/documentation/articles/powershell-install-configure)

> [AZURE.NOTE] We recommend you use the newer Azure Active Directory method to login to your Azure subscription, either from the Azure CLI or the Azure PowerShell.

## <a id="upload"> </a>Step 3: Upload the image to Azure ##

### If using Azure CLI

Use the Azure CLI to upload the image. You can upload an image by using the following command:

		azure vm image create <image-name> --location <location-of-the-data-center> --os Linux <source-path-to the vhd>

### If using PowerShell

You will need a storage account to upload your VHD file to. You can either pick an existing one or create a new one. To create a storage account please refer to [Create a Storage Account](/documentation/articles/storage-create-storage-account)

When you upload the .vhd file, you can place the .vhd file anywhere within your blob storage. In the following command examples, **BlobStorageURL** is the URL for the storage account you plan to use, **YourImagesFolder** is the container within blob storage where you want to store your images. **VHDName** is the label that appears in the [Management Portal](http://manage.windowsazure.cn) to identify the virtual hard disk. **PathToVHDFile** is the full path and name of the .vhd file.

From the Azure PowerShell window you used in the previous step, type:

		Add-AzureVhd -Destination <BlobStorageURL>/<YourImagesFolder>/<VHDName> -LocalFilePath <PathToVHDFile>

For more information, see [Add-AzureVhd]((https://msdn.microsoft.com/zh-cn/library/azure/dn495173.aspx).




[Step 1: Prepare the image to be uploaded]: #prepimage
[Step 2: Prepare the connection to Azure]: #connect
[Step 3: Upload the image to Azure]: #upload
