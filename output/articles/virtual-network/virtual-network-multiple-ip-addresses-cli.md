<properties
    pageTitle="Multiple IP addresses for virtual machines - Azure CLI | Azure"
    description="Learn how to assign multiple IP addresses to a virtual machine using Azure CLI | Resource Manager."
    services="virtual-network"
    documentationcenter="na"
    author="anavinahar"
    manager="narayan"
    editor=""
    tags="azure-resource-manager" />
<tags
    ms.assetid="ms.service: virtual-network"
    ms.devlang="na"
    ms.topic="article"
    ms.tgt_pltfrm="na"
    ms.workload="infrastructure-services"
    ms.date="11/17/2016"
    wacn.date=""
    ms.author="annahar" />

# Assign multiple IP addresses to virtual machines using Azure CLI

> [!div class="op_single_selector"]
> * [Azure portal](/documentation/articles/virtual-network-multiple-ip-addresses-portal/)
> * [PowerShell](/documentation/articles/virtual-network-multiple-ip-addresses-powershell/)
> * [CLI](/documentation/articles/virtual-network-multiple-ip-addresses-cli/)

An Azure Virtual Machine (VM) has one or more network interfaces (NIC) attached to it. Any NIC can have one or more static or dynamic public and private IP addresses assigned to it. Assigning multiple IP addresses to a VM enables the following capabilities:

* Hosting multiple websites or services with different IP addresses and SSL certificates on a single server.
* Serve as a network virtual appliance, such as a firewall or load balancer.
* The ability to add any of the private IP addresses for any of the NICs to an Azure Load Balancer back-end pool. In the past, only the primary IP address for the primary NIC could be added to a back-end pool. To learn more about how to load balance multiple IP configurations, read the [Load balancing multiple IP configurations](/documentation/articles/load-balancer-multiple-ip/) article.

Every NIC attached to a VM has one or more IP configurations associated to it. Each configuration is assigned one static or dynamic private IP address. Each configuration may also have one public IP address resource associated to it. A public IP address resource has either a dynamic or static IP address assigned to it. If you're not familiar with IP addresses in Azure, read the [IP addresses in Azure](/documentation/articles/virtual-network-ip-addresses-overview-arm/) article to learn more about them.

This article explains how to use PowerShell to assign multiple IP addresses to a VM created through the Azure Resource Manager deployment model. Multiple IP addresses cannot be assigned to resources created through the classic deployment model. To learn more about Azure deployment models, read the [Understand deployment models](/documentation/articles/resource-manager-deployment-model/) article.

[AZURE.INCLUDE [virtual-network-preview](../../includes/virtual-network-preview.md)]

## Scenario
A VM with a single NIC is created and connected to a virtual network. The VM requires three different *private* IP addresses and two *public* IP addresses. The IP addresses are assigned to the following IP configurations:

* **IPConfig-1:** Assigns a *dynamic* private IP address (default) and a *static* public IP address.
* **IPConfig-2:** Assigns a *static* private IP address and a *static* public IP address.
* **IPConfig-3:** Assigns a *dynamic* private IP address and no public IP address.
  
	![Multiple IP addresses](./media/virtual-network-multiple-ip-addresses-powershell/OneNIC-3IP.png)

The IP configurations are associated to the NIC when the NIC is created and the NIC is attached to the VM when the VM is created. The types of IP addresses used for the scenario are for illustration. You can assign whatever IP address and assignment types you require.

## <a name = "create"></a>Create a VM with multiple IP addresses

The steps that follow explain how to create an example VM with multiple IP addresses, as described in the scenario. Change variable names and IP address types as required for your implementation.

1. Install and configure the Azure CLI by following the steps in the [Install and Configure the Azure CLI](/documentation/articles/xplat-cli-install/) article and log into your Azure account.

2. Register for the preview by sending an email to [Multiple IPs](mailto:MultipleIPsPreview@microsoft.com?subject=Request%20to%20enable%20subscription%20%3csubscription%20id%3e) with your subscription ID and intended use. Do not attempt to complete the remaining steps:
	- Until you receive an e-mail notifying you that you've been accepted into the preview
	- Without following the instructions in the email you receive
3. [Create a resource group](/documentation/articles/virtual-machines-linux-create-cli-complete/#create-resource-groups-and-choose-deployment-locations) followed by a [virtual network and subnet](/documentation/articles/virtual-machines-linux-create-cli-complete/#create-a-virtual-network-and-subnet). Change the ``` --address-prefixes ``` and ```--address-prefix``` fields to the following to follow the exact sceanrio outlined in this article:

	```azurecli
	--address-prefixes 10.0.0.0/16
	--address-prefix 10.0.0.0/24
	```
	>[AZURE.NOTE] 
	>The referenced article above uses West Europe as the location to create resources, but this article uses West China North. Make location changes appropriately.

4. [Create  a storage account](/documentation/articles/virtual-machines-linux-create-cli-complete/#create-a-storage-account) for your VM.


5. Create the NIC and the IP configurations you want to assign to the NIC. You can add, remove, or change the configurations as necessary. The following configurations are described in the scenario:

	**IPConfig-1**

	Enter the commands that follow to create:

	- A public IP address resource with a static public IP address
	- An IP configuration with the public IP address resource and a dynamic private IP address

	```azurecli
	azure network public-ip create --resource-group myResourceGroup --location westchinaeast --name myPublicIP --domain-name-label mypublicdns --allocation-method Static
	```
	> [AZURE.NOTE]
	> Public IP addresses have a nominal fee. To learn more about IP address pricing, read the [IP address pricing](/pricing/details/reserved-ip-addresses/) page. There is a limit to the number of public IP addresses that can be used in a subscription. To learn more about the limits, read the [Azure limits](/documentation/articles/azure-subscription-service-limits/#networking-limits) article.

	```azurecli
	azure network nic create --resource-group myResourceGroup --location westchinaeast --subnet-vnet-name myVnet --subnet-name mySubnet --name myNic1 --public-ip-name myPublicIP
	```

	**IPConfig-2**

	 Enter the following commands to create a new public IP address resource and a new IP configuration with a static public IP address and a static private IP address:
	
	```azurecli
	azure network public-ip create --resource-group myResourceGroup --location westchinaeast --name myPublicIP2 --domain-name-label mypublicdns2 --allocation-method Static

	azure network nic ip-config create --resource-group myResourceGroup --nic-name myNic1 --name IPConfig-2 --private-ip-address 10.0.0.5 --public-ip-name myPublicIP2
	```

	**IPConfig-3**

	Enter the following commands to create an IP configuration with a dynamic private IP address and no public IP address:

	```azurecli
	azure network nic ip-config create --resource-group myResourceGroup --nic-name myNic1 --name IPConfig-3
	```

	>[AZURE.NOTE] 
	>Though this article assigns all IP configurations to a single NIC, you can also assign multiple IP configurations to any NIC in a VM. To learn how to create a VM with multiple NICs, read the Create a VM with multiple NICs article.

6. [Create a Linux VM](/documentation/articles/virtual-machines-linux-create-cli-complete/#create-the-linux-vms) article. Be sure to remove the ```  --availset-name myAvailabilitySet \ ``` property as it is not required for this scenario. Use the appropriate location based on your scenario. 

	>[AZURE.WARNING] 
	> Step 6 in the Create a VM article fails if the VM size is not supported in the location you selected. Run the following command to get a full list of VMs in US West Central, for example. This location name can be changed based on your scenario.
	> 
	> 		azure vm sizes --location westchinaeast

	To change the VM size to Standard DS2 v2, for example, simply add the following property ```  --vm-size Standard_DS3_v2``` to the ``` azure vm create ``` command in step 6.

7. Enter the following command to view the NIC and the associated IP configurations:

	```azurecli
	azure network nic show --resource-group myResourceGroup --name myNic1
	```

## <a name="OsConfig"></a>Add IP addresses to a VM operating system

Connect and login to a VM you created with multiple private IP addresses. You must manually add all the private IP addresses (including the primary) that you added to the VM. Complete the following steps for your VM operating system:

### Windows

1. From a command prompt, type *ipconfig /all*.  You only see the *Primary* private IP address (through DHCP).
2. Type *ncpa.cpl* in the command prompt to open the **Network connections** window.
3. Open the properties for **Local Area Connection**.
4. Double-click Internet Protocol version 4 (IPv4).
5. Select **Use the following IP address** and enter the following values:

	* **IP address**: Enter the *Primary* private IP address
	* **Subnet mask**: Set based on your subnet. For example, if the subnet is a /24 subnet then the subnet mask is 255.255.255.0.
	* **Default gateway**: The first IP address in the subnet. If your subnet is 10.0.0.0/24, then the gateway IP address is 10.0.0.1.
	* Click **Use the following DNS server addresses** and enter the following values:
		* **Preferred DNS server**: If you are not using your own DNS server, enter 168.63.129.16.  If you are using your own DNS server, enter the IP address for your server.
	* Click the **Advanced** button and add additional IP addresses. Add each of the secondary private IP addresses listed in step 8 to the NIC with the same subnet specified for the primary IP address.
	* Click **OK** to close out the TCP/IP settings and then **OK** again to close the adapter settings. Your RDP connection is re-established.
6. From a command prompt, type *ipconfig /all*. All IP addresses you added are shown and DHCP is turned off.
	
### Linux (Ubuntu)

1. Open a terminal window.
2. Make sure you are the root user. If you are not, enter the following command:

	```bash
	sudo -i
	```

3. Update the configuration file of the network interface (assuming 'eth0').

	* Keep the existing line item for dhcp. The primary IP address remains configured as it was previously.
	* Add a configuration for an additional static IP address with the following commands:

		```bash
		cd /etc/network/interfaces.d/
		ls
		```

	You should see a .cfg file.
4. Open the file: vi *filename*.

	You should see the following lines at the end of the file:

	```bash
	auto eth0
	iface eth0 inet dhcp
	```

5. Add the following lines after the lines that exist in this file:

	```bash
	iface eth0 inet static
	address <your private IP address here>
	```

6. Save the file by using the following command:

	```bash
	:wq
	```

7. Reset the network interface with the following command:

	```bash
	sudo ifdown eth0 && sudo ifup eth0
	```

	> [AZURE.IMPORTANT]
	> Run both ifdown and ifup in the same line if using a remote connection.
	>

8. Verify the IP address is added to the network interface with the following command:

	```bash
	Ip addr list eth0
	```

	You should see the IP address you added as part of the list.
	
### Linux (Redhat, CentOS, and others)

1. Open a terminal window.
2. Make sure you are the root user. If you are not, enter the following command:

	```bash
	sudo -i
	```

3. Enter your password and follow instructions as prompted. Once you are the root user, navigate to the network scripts folder with the following command:

	```bash
	cd /etc/sysconfig/network-scripts
	```

4. List the related ifcfg files using the following command:

	```bash
	ls ifcfg-*
	```

	You should see *ifcfg-eth0* as one of the files.

5. Copy the *ifcfg-eth0* file and name it *ifcfg-eth0:0* with the following command:

	```bash
	cp ifcfg-eth0 ifcfg-eth0:0
	```

6. Edit the *ifcfg-eth0:0* file with the following command:

	```bash
	vi ifcfg-eth1
	```

7. Change the device to the appropriate name in the file; *eth0:0* in this case, with the following command:

	```bash
	DEVICE=eth0:0
	```

8. Change the *IPADDR = YourPrivateIPAddress* line to reflect the IP address.
9. Save the file with the following command:

	```bash
	:wq
	```

10. Restart the network services and make sure the changes are successful by running the following commands:

	```bash
	/etc/init.d/network restart
	Ipconfig
	```

	You should see the IP address you added, *eth0:0*, in the list returned.

## <a name="add"></a>Add IP addresses to a VM

You can add additional private and public IP addresses to an existing NIC by completing the steps that follow. The examples build upon the [scenario](#Scenario) described in this article.

1. Open Azure CLI and complete the remaining steps in this section within a single CLI session. If you don't already have Azure CLI installed and configured, complete the steps in the [Install and Configure the Azure CLI](/documentation/articles/xplat-cli-install/) article and log into your Azure account.

2. Register for the preview by sending an email to [Multiple IPs](mailto:MultipleIPsPreview@microsoft.com?subject=Request%20to%20enable%20subscription%20%3csubscription%20id%3e) with your subscription ID and intended use. Do not attempt to complete the remaining steps:
	- Until you receive an e-mail notifying you that you've been accepted into the preview
	- Without following the instructions in the email you receive


3. Complete the steps in one of the following sections, based on your requirements:

	**Add a private IP address**
	
	To add a private IP address to a NIC, you must create an IP configuration using the command below.  If you want to add a dynamic private IP address, remove ```-PrivateIpAddress 10.0.0.7``` before entering the command. When specifying a static IP address, it must be an unused address for the subnet.

	```azurecli
	azure network nic ip-config create --resource-group myResourceGroup --nic-name myNic1 --private-ip-address 10.0.0.7 --name IPConfig-4
	```
	Create as many configurations as you require, using unique configuration names and private IP addresses (for configurations with static IP addresses).

	**Add a public IP address**
	
	A public IP address is added by associating it to either a new IP configuration or an existing IP configuration. Complete the steps in one of the sections that follow, as you require.

	> [AZURE.NOTE]
	> Public IP addresses have a nominal fee. To learn more about IP address pricing, read the [IP address pricing](/pricing/details/reserved-ip-addresses/) page. There is a limit to the number of public IP addresses that can be used in a subscription. To learn more about the limits, read the [Azure limits](/documentation/articles/azure-subscription-service-limits/#networking-limits) article.
	>

	**Associate the resource to a new IP configuration**
	
	Whenever you add a public IP address in a new IP configuration, you must also add a private IP address, because all IP configurations must have a private IP address. You can either add an existing public IP address resource, or create a new one. To create a new one, enter the following command:
	
	```azurecli
  	azure network public-ip create --resource-group myResourceGroup --location westchinaeast --name myPublicIP3 --domain-name-label mypublicdns3
	```

 	To create a new IP configuration with a dynamic private IP address and the associated *myPublicIP3* public IP address resource, enter the following command:

	```azurecli
	azure network nic ip-config create --resource-group myResourceGroup --nic-name myNic --name IPConfig-4 --public-ip-name myPublicIP3
	```

	**Associate the resource to an existing IP configuration**
	A public IP address resource can only be associated to an IP configuration that doesn't already have one associated. You can determine whether an IP configuration has an associated public IP address by entering the following command:

	```azurecli
	azure network nic ip-config list --resource-group myResourceGroup --nic-name myNic1
	```

	Look for a line similar to the one that follows in the returned output:
	
		Name               Provisioning state  Primary  Private IP allocation  Private IP version  Private IP address  Subnet    Public IP
		-----------------  ------------------  -------  ---------------------  ------------------  ------------------  --------  -----------
		default-ip-config  Succeeded           true     Dynamic                IPv4                10.0.0.4            mySubnet  myPublicIP
		IPConfig-2         Succeeded           false    Static                 IPv4                10.0.0.5            mySubnet  myPublicIP2
		IPConfig-3         Succeeded           false    Dynamic                IPv4                10.0.0.6            mySubnet
	 
	Since the **Public IP** column for *IpConfig-3* is blank, no public IP address resource is currently associated to it. You can add an existing public IP address resource to IpConfig-3, or enter the following command to create one:

	```azurecli
	azure network public-ip create --resource-group  myResourceGroup --location westchinaeast --name myPublicIP3 --domain-name-label mypublicdns3 --allocation-method Static
	```
	
	Enter the following command to associate the public IP address resource to the existing IP configuration named *IPConfig-3*:
	
	```azurecli
	azure network nic ip-config set --resource-group myResourceGroup --nic-name myNic1 --name IPConfig-3 --public-ip-name myPublicIP3
	```


7. View the private IP addresses and the public IP address resources assigned to the NIC by entering the following command:

	```azurecli
	azure network nic ip-config list --resource-group myResourceGroup --nic-name myNic1
	```
	You should see output similar to the following: 
	
		Name               Provisioning state  Primary  Private IP allocation  Private IP version  Private IP address  Subnet    Public IP
		-----------------  ------------------  -------  ---------------------  ------------------  ------------------  --------  -----------
		default-ip-config  Succeeded           true     Dynamic                IPv4                10.0.0.4            mySubnet  myPublicIP
		IPConfig-2         Succeeded           false    Static                 IPv4                10.0.0.5            mySubnet  myPublicIP2
		IPConfig-3         Succeeded           false    Dynamic                IPv4                10.0.0.6            mySubnet  myPublicIP3
	 
9. Add the IP addresses you added to the NIC to the VM operating system by following the instructions in the [Add IP addresses to a VM operating system](#OsConfig) section of this article.