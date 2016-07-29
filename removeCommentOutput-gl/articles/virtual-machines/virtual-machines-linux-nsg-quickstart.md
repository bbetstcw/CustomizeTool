<properties
   pageTitle="Allow external access to a Linux VM | Microsoft Azure"
   description="Learn how to open a port / create an endpoint that allows external access to your Linux VM using the resource manager deployment model and the Azure CLI"
   services="virtual-machines-linux"
   documentationCenter=""
   authors="iainfoulds"
   manager="timlt"
   editor=""/>

<tags
	ms.service="virtual-machines-linux"
	ms.date="05/24/2016"
	wacn.date=""/>

# Allow external access to your VM
[AZURE.INCLUDE [virtual-machines-common-nsg-quickstart](../includes/virtual-machines-common-nsg-quickstart.md)]

## Quick commands
To create a Network Security Group and rules you will need [the Azure CLI](/documentation/articles/xplat-cli-install/) in resource manager mode (`azure config mode arm`).

Create your Network Security Group, entering your own names and location appropriately:

```
azure network nsg create --resource-group TestRG --name TestNSG --location westus
```

Add a rule to allow HTTP traffic to your webserver (this can be adjusted for your own scenario, such as SSH access or database connectivity):

```
azure network nsg rule create --protocol tcp --direction inbound --priority 1000 \
    --destination-port-range 80 --access allow --resource-group TestRG --nsg-name TestNSG --name AllowHTTP
```

Associate the Network Security Group with your VM's network interface:

```
azure network nic set --resource-group TestRG --name TestNIC --network-security-group-name TestNSG
```

Alternatively, you can associate your Network Security Group with a virtual network subnet rather than just to the network interface on a single VM:

```
azure network vnet subnet set --resource-group TestRG --name TestSubnet --network-security-group-name TestNSG
```

## More information on Network Security Groups
The quick commands here allow you to get up and running with traffic flowing to your VM. Network Security Groups provide a lot of great features and granularity for controlling access to the your resources. You can read more about [creating a Network Security Group and ACL rules here](/documentation/articles/virtual-networks-create-nsg-arm-cli/).

Network Security Groups and ACL rules can also be defined as part of Azure Resource Manager templates. Read more about [creating Network Security Groups with templates](/documentation/articles/virtual-networks-create-nsg-arm-template/).

If you need to use port-forwarding to map a unique external port to an internal port on your VM, you need to use a load balancer and Network Address Translation (NAT) rules. For example, you may want to expose TCP port 8080 externally and have traffic directed to TCP port 80 on a VM. You can learn about [creating an Internet-facing load balancer](/documentation/articles/load-balancer-get-started-internet-arm-cli/).

## Next steps
In this example, you created a simple rule to allow HTTP traffic. You can find information on creating more detailed environments in the following articles:

- [Azure Resource Manager overview](/documentation/articles/resource-group-overview/)
- [What is a Network Security Group (NSG)?](/documentation/articles/virtual-networks-nsg/)
- [Azure Resource Manager Overview for Load Balancers](../load-balancer2    /load-balancer-arm.md)