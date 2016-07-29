<properties
	pageTitle="Create a Secure Linux VM using a Azure template | Azure"
	description="Create a Secure Linux VM on Azure using an Azure Resource Manager template."
	services="virtual-machines-linux"
	documentationCenter=""
	authors="vlivech"
	manager="timlt"
	editor=""
	tags="azure-service-management,azure-resource-manager" />

<tags
	ms.service="virtual-machines-linux"
	ms.date="04/27/2016"
	wacn.date=""/>

# Create a secured Linux VM using an Azure template


[AZURE.INCLUDE [arm-api-version-cli](../includes/arm-api-version-cli.md)]


To create a Linux VM from a template you will need [the Azure CLI](/documentation/articles/xplat-cli-install/) in resource manager mode (`azure config mode arm`).

## Quick Command Summary

```bash
chrisl@fedora$ azure group create -n <exampleRGname> -l <exampleAzureRegion> [--template-uri <URL> | --template-file <path> | <template-file> -e <parameters.json file>]
```

## Detailed Walkthrough

Templates allow you to create VMs on Azure with settings that you want to customize during the launch, things like usernames and hostnames. For this article we will focus on launching an Ubuntu VM using an Azure template that creates a network security group (NSG) with only one port open (22 for SSH) and which requires SSH keys for login.

Azure Resource Manager templates are JSON files that can be used for simple one-off tasks -- like launching an Ubuntu VM as done in this article -- or to construct complex Azure configurations of entire environments like a testing, dev or production deployment from the networking to the OS to application stack deployment.

## Create the Linux VM

The following code example shows how to call `azure group create` to create a resource group and deploy an SSH-secured Linux VM at the same time using [this Azure Resource Manager template](https://raw.githubusercontent.com/Azure/azure-quickstart-templates/master/101-vm-sshkey/azuredeploy.json). Remember that in your example you need to use names that are unique to your environment. This example uses `quicksecuretemplate` as the resource group name, `securelinux` as the VM name, and `quicksecurelinux` as a subdomain name.


>[AZURE.NOTE] Templates you downloaded from the GitHub Repo "azure-quickstart-templates" must be modified in order to fit in the Azure China Cloud Environment. For example, replace some endpoints -- "blob.core.chinacloudapi.cn" by "blob.core.chinacloudapi.cn", "chinacloudapp.cn" by "chinacloudapp.cn"; change some unsupported VM images; and, changes some unsupported VM sizes.


```bash

chrisl@fedora$ azure group create -n quicksecuretemplate -l chinaeast --template-uri https://raw.githubusercontent.com/Azure/azure-quickstart-templates/master/101-vm-sshkey/azuredeploy.json


chrisl@fedora$ azure group create -n quicksecuretemplate -l chinaeast --template-file /path/to/azuredeploy.json

info:    Executing command group create
+ Getting resource group quicksecuretemplate
+ Creating resource group quicksecuretemplate
info:    Created resource group quicksecuretemplate
info:    Supply values for the following parameters
adminUserName: ops
sshKeyData: ssh-rsa AAAAB3NzaC1yc2EAAAADAQABAAABAQDDRZ/XB8p8uXMqgI8EoN3dWQw... user@contoso.com
dnsLabelPrefix: quicksecurelinux
vmName: securelinux
+ Initializing template configurations and parameters
+ Creating a deployment
info:    Created template deployment "azuredeploy"
data:    Id:                  /subscriptions/<guid>/resourceGroups/quicksecuretemplate
data:    Name:                quicksecuretemplate
data:    Location:            chinaeast
data:    Provisioning State:  Succeeded
data:    Tags: null
data:
info:    group create command OK
```

You can create a new resource group and deploy a VM using the `--template-uri` parameter, or you can download or create a template locally and pass the template using the `--template-file` parameter with a path to the template file as an argument. The Azure CLI prompts you for the parameters required by the template.

## Next steps

Once you create Linux VMs with templates, you'll want to see what other app frameworks are available to use with templates. Search the [templates gallery](https://azure.microsoft.com/documentation/templates/) to discover what app frameworks to deploy next.
