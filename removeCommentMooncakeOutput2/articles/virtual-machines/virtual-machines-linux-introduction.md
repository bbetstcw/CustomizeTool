<properties
	pageTitle="Introduction to Linux in Azure | Windows Azure"
	description="Learn about using Linux virtual machines on Azure."
	services="virtual-machines"
	documentationCenter="python"
	authors="szarkos"
	manager="timlt"
	editor=""
	tags="azure-resource-manager,azure-service-management"/>

<tags
	ms.service="virtual-machines"
	ms.date="06/11/2015"
	wacn.date=""/>

#Introduction to Linux on Azure

[AZURE.INCLUDE [learn-about-deployment-models](../includes/learn-about-deployment-models-include.md)]

This topic provides an overview of some aspects of using Linux virtual machines in the Azure cloud. Deploying a Linux virtual machine is a straightforward process using an image from the gallery.

## Authentication: Usernames, Passwords and SSH Keys

When creating a Linux virtual machine using the Azure Management Portal, you are asked to provide a username, password or an SSH public key. The choice of a username for deploying a Linux virtual machine on Azure is subject to the following constraint: names of system accounts (UID <100) already present in the virtual machine are not allowed, 'root' for example.


 - See [Create a Virtual Machine Running Linux](/documentation/articles/virtual-machines-linux-tutorial)
 - See [How to Use SSH with Linux on Azure](/documentation/articles/linux-use-ssh-key)


## Obtaining Superuser Privileges Using `sudo`

The user account that is specified during virtual machine instance deployment on Azure is a privileged account. This account is configured by the Azure Linux Agent to be able to elevate privileges to root (superuser account) using the `sudo` utility. Once logged in using this user account, you will be able to run commands as root using the command syntax

	# sudo <COMMAND>

You can optionally obtain a root shell using **sudo -s**.

- See [Using root privileges on Linux virtual machines in Azure](/documentation/articles/virtual-machines-linux-use-root-privileges)


## Firewall Configuration

Azure provides an inbound packet filter that restricts connectivity to ports specified in the Management Portal. By default, the only allowed port is SSH. You may open up access to additional ports on your Linux virtual machine by configuring endpoints in the Management Portal:

 - See: [How to Set Up Endpoints to a Virtual Machine](/documentation/articles/virtual-machines-set-up-endpoints)

The Linux images in the Azure Gallery do not enable the *iptables* firewall by default. If desired, the firewall may be configured to provide additional filtering.


## Hostname Changes

When you initially deploy an instance of a Linux image, you are required to provide a host name for the virtual machine. Once the virtual machine is running, this hostname is published to the platform DNS servers so that multiple virtual machines connected to each other can perform IP address lookups using hostnames.

If hostname changes are desired after a virtual machine has been deployed, please use the command

	# sudo hostname <newname>

The Azure Linux Agent includes functionality to automatically detect this name change and appropriately configure the virtual machine to persist this change and publish this change to the platform DNS servers.

 - [Azure Linux Agent User Guide](/documentation/articles/virtual-machines-linux-agent-user-guide)

### Cloud-Init
**Ubuntu** and **CoreOS** images utilize cloud-init pn Azure, which provides additional capabilities for bootstrapping a virtual machine.

 - [How to Inject Custom Data](/documentation/articles/virtual-machines-how-to-inject-custom-data)
 - [Custom Data and Cloud-Init on Windows Azure](http://azure.microsoft.com/blog/2014/04/21/custom-data-and-cloud-init-on-windows-azure/)
 - [Create Azure Swap Partitions Using Cloud-Init](https://wiki.ubuntu.com/AzureSwapPartitions)
 - [How to Use CoreOS on Azure](/documentation/articles/virtual-machines-linux-coreos-how-to)


## Virtual Machine Image Capture

Azure provides the ability to capture the state of an existing virtual machine into an image that can subsequently be used to deploy additional virtual machine instances. The Azure Linux Agent may be used to rollback some of the customization that was performed during the provisioning process. You may follow the steps below to capture a virtual machine as an image:

1. Run **waagent -deprovision** to undo provisioning customization. Or **waagent -deprovision+user** to optionally, delete the user account specified during provisioning and all associated data.

2. Shut down/power off the virtual machine.

3. Click *Capture* in the Management Portal or use the Powershell or CLI tools to capture the virtual machine as an image.

 - See: [How to Capture a Linux Virtual Machine to Use as a Template](/documentation/articles/virtual-machines-linux-capture-image)


## Attaching Disks

Each virtual machine has a temporary, local *resource disk* attached. Because data on a resource disk may not be durable across reboots, it is often used by applications and processes running in the virtual machine for transient and **temporary** storage of data. It is also used to store the page or swap files for the operating system.

On Linux, the resource disk is typically managed by the Azure Linux Agent and automatically mounted to **/mnt/resource** (or **/mnt** on Ubuntu images).


	>[AZURE.NOTE] Note that the resource disk is a **temporary** disk, and might be deleted and reformatted when the VM is rebooted.

On Linux the data disk might be named by the kernel as `/dev/sdc`, and users will need to partition, format and mount that resource. This is covered step-by-step in the tutorial: [How to Attach a Data Disk to a Virtual Machine](/documentation/articles/virtual-machines-linux-how-to-attach-disk).

 - **See also:** [Configure Software RAID on Linux](/documentation/articles/virtual-machines-linux-configure-raid)
