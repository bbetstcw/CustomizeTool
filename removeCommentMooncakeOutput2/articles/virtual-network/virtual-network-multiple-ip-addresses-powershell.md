<properties
    pageTitle="Multiple IP addresses for virtual machines - PowerShell | Azure"
    description="Learn how to assign multiple IP addresses to a virtual machine using PowerShell | Resource Manager."
    services="virtual-network"
    documentationcenter="na"
    author="jimdial"
    manager="carmonm"
    editor=""
    tags="azure-resource-manager" />
<tags
    ms.assetid="c44ea62f-7e54-4e3b-81ef-0b132111f1f8"
    ms.service="virtual-network"
    ms.devlang="na"
    ms.topic="article"
    ms.tgt_pltfrm="na"
    ms.workload="infrastructure-services"
    ms.date="11/16/2016"
    wacn.date=""
    ms.author="jdial;annahar" />

# Assign multiple IP addresses to virtual machines using PowerShell
> [AZURE.SELECTOR]
- [Azure portal preview](/documentation/articles/virtual-network-multiple-ip-addresses-portal/)
- [PowerShell](/documentation/articles/virtual-network-multiple-ip-addresses-powershell/)
- [CLI](/documentation/articles/virtual-network-multiple-ip-addresses-cli/)

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

1. Open a PowerShell command prompt and complete the remaining steps in this section within a single PowerShell session. If you don't already have PowerShell installed and configured, complete the steps in the [How to install and configure Azure PowerShell](/documentation/articles/powershell-install-configure/) article.
2. Register for the preview by sending an email to [Multiple IPs](mailto:MultipleIPsPreview@microsoft.com?subject=Request%20to%20enable%20subscription%20%3csubscription%20id%3e) with your subscription ID and intended use. Do not attempt to complete the remaining steps:
	- Until you receive an e-mail notifying you that you've been accepted into the preview
	- Without following the instructions in the email you receive
3. Complete steps 1-4 of the [Create a Windows VM](/documentation/articles/virtual-machines-windows-ps-create/) article. Do not complete step 5 (creation of public IP resource and network interface). If you change the names of any variables used in that article, change the names of the variables in the remaining steps too. To create a Linux VM, select a Linux operating system instead of Windows.
4. Create a variable to store the subnet object created in Step 4 (Create a VNet) of the Create a Windows VM article by typing the following command:

        $SubnetName = $mySubnet.Name
        $Subnet = $myVnet.Subnets | Where-Object { $_.Name -eq $SubnetName }

5. Define the IP configurations you want to assign to the NIC. You can add, remove, or change the configurations as necessary. The following configurations are described in the scenario:

	**IPConfig-1**

	Enter the commands that follow to create:
	- A public IP address resource with a static public IP address
	- An IP configuration with the public IP address resource and a dynamic private IP address

        $myPublicIp1     = New-AzureRmPublicIpAddress -Name "myPublicIp1" -ResourceGroupName $myResourceGroup -Location $location -AllocationMethod Static
        $IpConfigName1  = "IPConfig-1"
        $IpConfig1      = New-AzureRmNetworkInterfaceIpConfig -Name $IpConfigName1 -Subnet $Subnet -PublicIpAddress $myPublicIp1 -Primary

	Note the `-Primary` switch in the previous command. When you assign multiple IP configurations to a NIC, one configuration must be assigned as the *Primary*.

	> [AZURE.NOTE]
	> Public IP addresses have a nominal fee. To learn more about IP address pricing, read the [IP address pricing](/pricing/details/reserved-ip-addresses/) page. There is a limit to the number of public IP addresses that can be used in a subscription. To learn more about the limits, read the [Azure limits](/documentation/articles/azure-subscription-service-limits/#networking-limits) article.
	>

	**IPConfig-2**

	Change the value of the **$IPAddress** variable that follows to an available, valid address on the subnet you created. To check whether the address 10.0.0.5 is available on the subnet, enter the command `Test-AzureRmPrivateIPAddressAvailability -IPAddress 10.0.0.5 -VirtualNetwork $myVnet`. If the address is available, the output returns *True*. If it's not available, the output returns *False* and a list of addresses that are available. Enter the following commands to create a new public IP address resource and a new IP configuration with a static public IP address and a static private IP address:

        $IpConfigName2 = "IPConfig-2"
        $IPAddress     = 10.0.0.5
        $myPublicIp2   = New-AzureRmPublicIpAddress -Name "myPublicIp2" -ResourceGroupName $myResourceGroup `
        -Location $location -AllocationMethod Static
        $IpConfig2     = New-AzureRmNetworkInterfaceIpConfig -Name $IpConfigName2 `
        -Subnet $Subnet -PrivateIpAddress $IPAddress -PublicIpAddress $myPublicIp2

	**IPConfig-3**

	Enter the following commands to create an IP configuration with a dynamic private IP address and no public IP address:

        $IpConfigName3 = "IpConfig-3"
        $IpConfig3 = New-AzureRmNetworkInterfaceIpConfig -Name $IPConfigName3 -Subnet $Subnet

6. Create the NIC using the IP configurations defined in the previous step by entering the following command:

        $myNIC = New-AzureRmNetworkInterface -Name myNIC -ResourceGroupName $myResourceGroup `
        -Location $location -IpConfiguration $IpConfig1,$IpConfig2,$IpConfig3

	> [AZURE.NOTE]
	> Though this article assigns all IP configurations to a single NIC, you can also assign multiple IP configurations to any NIC in a VM. To learn how to create a VM with multiple NICs, read the [Create a VM with multiple NICs](/documentation/articles/virtual-network-deploy-multinic-arm-ps/) article.

7. Complete step 6 of the [Create a VM](/documentation/articles/virtual-machines-windows-ps-create/) article. 

	> [AZURE.WARNING]
	> Step 6 in the Create a VM article fails if:
	> - You changed the variable named $myNIC to something else in step 6 of this article.
	> - You haven't completed the previous steps of this article and the Create a VM article.
	>
8. Enter the following command to view the private IP addresses and public IP address resources assigned to the NIC:

        $myNIC.IpConfigurations | Format-Table Name, PrivateIPAddress, PublicIPAddress, Primary

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

        sudo -i

3. Update the configuration file of the network interface (assuming 'eth0').

	* Keep the existing line item for dhcp. The primary IP address remains configured as it was previously.
	* Add a configuration for an additional static IP address with the following commands:

            cd /etc/network/interfaces.d/
            ls

	You should see a .cfg file.
4. Open the file: vi *filename*.

	You should see the following lines at the end of the file:

        auto eth0
        iface eth0 inet dhcp

5. Add the following lines after the lines that exist in this file:

        iface eth0 inet static
        address <your private IP address here>

6. Save the file by using the following command:

        :wq

7. Reset the network interface with the following command:

        sudo ifdown eth0 && sudo ifup eth0

	> [AZURE.IMPORTANT]
	> Run both ifdown and ifup in the same line if using a remote connection.
	>

8. Verify the IP address is added to the network interface with the following command:

        Ip addr list eth0

	You should see the IP address you added as part of the list.
	
### Linux (Redhat, CentOS, and others)

1. Open a terminal window.
2. Make sure you are the root user. If you are not, enter the following command:

        sudo -i

3. Enter your password and follow instructions as prompted. Once you are the root user, navigate to the network scripts folder with the following command:

        cd /etc/sysconfig/network-scripts

4. List the related ifcfg files using the following command:

        ls ifcfg-*

	You should see *ifcfg-eth0* as one of the files.

5. Copy the *ifcfg-eth0* file and name it *ifcfg-eth0:0* with the following command:

        cp ifcfg-eth0 ifcfg-eth0:0

6. Edit the *ifcfg-eth0:0* file with the following command:

        vi ifcfg-eth1

7. Change the device to the appropriate name in the file; *eth0:0* in this case, with the following command:

        DEVICE=eth0:0

8. Change the *IPADDR = YourPrivateIPAddress* line to reflect the IP address.
9. Save the file with the following command:

        :wq

10. Restart the network services and make sure the changes are successful by running the following commands:

        /etc/init.d/network restart
        Ipconfig

You should see the IP address you added, *eth0:0*, in the list returned.

## <a name="add"></a>Add IP addresses to a VM

You can add additional private and public IP addresses to an existing NIC by completing the steps that follow. The examples build upon the [scenario](#Scenario) described in this article.

1. Open a PowerShell command prompt and complete the remaining steps in this section within a single PowerShell session. If you don't already have PowerShell installed and configured, complete the steps in the [How to install and configure Azure PowerShell](/documentation/articles/powershell-install-configure/) article.
2. Register for the preview by sending an email to [Multiple IPs](mailto:MultipleIPsPreview@microsoft.com?subject=Request%20to%20enable%20subscription%20%3csubscription%20id%3e) with your subscription ID and intended use. Do not attempt to complete the remaining steps:
	- Until you receive an e-mail notifying you that you've been accepted into the preview
	- Without following the instructions in the email you receive
3. Change the "values" of the following $Variables to the name of the NIC you want to add IP address to and the resource group and location the NIC exists in:

        $NICname         = "myNIC"
        $myResourceGroup = "myResourceGroup"
        $location        = "westchinaeast"

	If you don't know the name of the NIC you want to change, enter the following commands, then change the values of the previous variables:

        Get-AzureRmNetworkInterface | Format-Table Name, ResourceGroupName, Location

4. Create a variable and set it to the existing NIC by typing the following command:

        $myNIC = Get-AzureRmNetworkInterface -Name $NICname -ResourceGroupName $myResourceGroup

5. In the following commands, change *myVNet* and *mySubnet* to the names of the VNet and subnet the NIC is connected to. Enter the commands to retrieve the VNet and subnet objects the NIC is connected to:

        $myVnet = Get-AzureRMVirtualnetwork -Name myVNet -ResourceGroupName $myResourceGroup
        $Subnet = $myVnet.Subnets | Where-Object { $_.Name -eq "mySubnet" }

	If you don't know the VNet or subnet name the NIC is connected to, enter the following command:

        $mynic.IpConfigurations

	Look for text similar to the following text in the returned output:

		Subnet   : {
					 "Id": "/subscriptions/[Id]/resourceGroups/myResourceGroup/providers/Microsoft.Network/virtualNetworks/myVnet/subnets/mySubnet"

	In this output, *myVnet* is the VNet and *mySubnet* is the subnet the NIC is connected to.

6. Complete the steps in one of the following sections, based on your requirements:

	**Add a private IP address**
	
	To add a private IP address to a NIC, you must create an IP configuration. The following command creates a configuration with a static IP address of 10.0.0.7. If you want to add a dynamic private IP address, remove `-PrivateIpAddress 10.0.0.7` before entering the command. When specifying a static IP address, it must be an unused address for the subnet. It's recommended that you first test the address to ensure it's available by entering the `Test-AzureRmPrivateIPAddressAvailability -IPAddress 10.0.0.7 -VirtualNetwork $myVnet` command. If the IP address is available, the output returns *True*. If it's not available, the output returns *False*, and a list of addresses that are available.

        Add-AzureRmNetworkInterfaceIpConfig -Name IPConfig-4 -NetworkInterface `
         $myNIC -Subnet $Subnet -PrivateIpAddress 10.0.0.7

	Create as many configurations as you require, using unique configuration names and private IP addresses (for configurations with static IP addresses).

	**Add a public IP address**
	
	A public IP address is added by associating it to either a new IP configuration or an existing IP configuration. Complete the steps in one of the sections that follow, as you require.

	> [AZURE.NOTE]
	> Public IP addresses have a nominal fee. To learn more about IP address pricing, read the [IP address pricing](/pricing/details/reserved-ip-addresses/) page. There is a limit to the number of public IP addresses that can be used in a subscription. To learn more about the limits, read the [Azure limits](/documentation/articles/azure-subscription-service-limits/#networking-limits) article.
	>

	**Associate the resource to a new IP configuration**
	
	Whenever you add a public IP address in a new IP configuration, you must also add a private IP address, because all IP configurations must have a private IP address. You can either add an existing public IP address resource, or create a new one. To create a new one, enter the following command:

        $myPublicIp3   = New-AzureRmPublicIpAddress -Name "myPublicIp3" -ResourceGroupName $myResourceGroup `
        -Location $location -AllocationMethod Static

 	To create a new IP configuration with a dynamic private IP address and the associated *myPublicIp3* public IP address resource, enter the following command:

        Add-AzureRmNetworkInterfaceIpConfig -Name IPConfig-4 -NetworkInterface `
         $myNIC -Subnet $Subnet -PublicIpAddress $myPublicIp3

	**Associate the resource to an existing IP configuration**
	A public IP address resource can only be associated to an IP configuration that doesn't already have one associated. You can determine whether an IP configuration has an associated public IP address by entering the following command:

        $myNIC.IpConfigurations | Format-Table Name, PrivateIPAddress, PublicIPAddress, Primary

	Look for a line similar to the one that follows in the returned output:

		Name       PrivateIpAddress PublicIpAddress                                           Primary
		----       ---------------- ---------------                                           -------
		IPConfig-1 10.0.0.4         Microsoft.Azure.Commands.Network.Models.PSPublicIpAddress    True
		IPConfig-2 10.0.0.5         Microsoft.Azure.Commands.Network.Models.PSPublicIpAddress   False
		IpConfig-3 10.0.0.6                                                                     False

	Since the **PublicIpAddress** column for *IpConfig-3* is blank, no public IP address resource is currently associated to it. You can add an existing public IP address resource to IpConfig-3, or enter the following command to create one:

        $myPublicIp3   = New-AzureRmPublicIpAddress -Name "myPublicIp3" -ResourceGroupName $myResourceGroup `
        -Location $location -AllocationMethod Static

	Enter the following command to associate the public IP address resource to the existing IP configuration named *IpConfig-3*:

        Set-AzureRmNetworkInterfaceIpConfig -Name IpConfig-3 -NetworkInterface $mynic -Subnet $Subnet -PublicIpAddress $myPublicIp3

7. Set the NIC with the new IP configuration by entering the following command:

        Set-AzureRmNetworkInterface -NetworkInterface $myNIC

8. View the private IP addresses and the public IP address resources assigned to the NIC by entering the following command:

        $myNIC.IpConfigurations | Format-Table Name, PrivateIPAddress, PublicIPAddress, Primary

9. Add the IP addresses you added to the NIC to the VM operating system by following the instructions in the [Add IP addresses to a VM operating system](#OsConfig) section of this article.
