<properties
    pageTitle="Technical deep dive on platform-supported migration from classic to Azure Resource Manager | Azure"
    description="This article does a technical deep dive on platform-supported migration of resources from classic to Azure Resource Manager"
    services="virtual-machines-windows"
    documentationcenter=""
    author="singhkays"
    manager="timlt"
    editor=""
    tags="azure-resource-manager" />
<tags
    ms.assetid="1ee40d32-a5e8-42a2-97d0-3232fd3cbb98"
    ms.service="virtual-machines-windows"
    ms.workload="infrastructure-services"
    ms.tgt_pltfrm="vm-windows"
    ms.devlang="na"
    ms.topic="article"
    ms.date="08/22/2016"
    wacn.date=""
    ms.author="kasing" />

# Technical deep dive on platform-supported migration from classic to Azure Resource Manager
Let's take a deep-dive on migrating from the Azure classic deployment model to the Azure Resource Manager deployment model. We look at resources at a resource and feature level to help you understand how the Azure platform migrates resources between the two deployment models. For more information, please read the service announcement article: [Platform-supported migration of IaaS resources from classic to Azure Resource Manager](/documentation/articles/virtual-machines-windows-migration-classic-resource-manager/).

## Detailed guidance on migration
You can find the classic and Resource Manager representations of the resources in the following table. Other features and resources are not currently supported.

| Classic representation | Resource Manager representation | Detailed notes |
| --- | --- | --- |
| Cloud service name |DNS name |During migration, a new resource group is created for every cloud service with the naming pattern `<cloudservicename>-migrated`. This resource group contains all your resources. The cloud service name becomes a DNS name that is associated with the public IP address. |
| Virtual machine |Virtual machine |VM-specific properties are migrated unchanged. Certain osProfile information, like computer name, is not stored in the classic deployment model and remains empty after migration. |
| Disk resources attached to VM |Implicit disks attached to VM |Disks are not modeled as top-level resources in the Resource Manager deployment model. They are migrated as implicit disks under the VM. Only disks that are attached to a VM are currently supported. Resource Manager VMs can now use classic storage accounts, which allows the disks to be easily migrated without any updates. |
| VM extensions |VM extensions |All the resource extensions, except XML extensions, are migrated from the classic deployment model. |
| Virtual machine certificates |Certificates in Azure Key Vault |If a cloud service contains service certificates, a new Azure key vault per cloud service and moves the certificates into the key vault. The VMs are updated to reference the certificates from the key vault. <br><br> **NOTE:** Please do not delete the keyvault as it can cause the VM to go into a failed state. We're working on improving things in the backend so that Key Vaults can be deleted safely or moved along with the VM to a new subscription. |
| WinRM configuration |WinRM configuration under osProfile |Windows Remote Management configuration is moved unchanged, as part of the migration. |
| Availability-set property |Availability-set resource |Availability-set specification was a property on the VM in the classic deployment model. Availability sets become a top-level resource as part of the migration. The following configurations are not supported: multiple availability sets per cloud service, or one or more availability sets along with VMs that are not in any availability set in a cloud service. |
| Network configuration on a VM |Primary network interface |Network configuration on a VM is represented as the primary network interface resource after migration. For VMs that are not in a virtual network, the internal IP address changes during migration. |
| Multiple network interfaces on a VM |Network interfaces |If a VM has multiple network interfaces associated with it, each network interface becomes a top-level resource as part of the migration in the Resource Manager deployment model, along with all the properties. |
| Load-balanced endpoint set |Load balancer |In the classic deployment model, the platform assigned an implicit load balancer for every cloud service. During migration, a new load-balancer resource is created, and the load-balancing endpoint set becomes load-balancer rules. |
| Inbound NAT rules |Inbound NAT rules |Input endpoints defined on the VM are converted to inbound network address translation rules under the load balancer during the migration. |
| VIP address |Public IP address with DNS name |The virtual IP address becomes a public IP address and is associated with the load balancer. |
| Virtual network |Virtual network |The virtual network is migrated, with all its properties, to the Resource Manager deployment model. A new resource group is created with the name `-migrated`. There are [unsupported configurations](/documentation/articles/virtual-machines-windows-migration-classic-resource-manager/). |
| Reserved IPs |Public IP address with static allocation method |Reserved IPs associated with the load balancer are migrated, along with the migration of the cloud service or the virtual machine. Unassociated reserved IP migration is not currently supported. |
| Public IP address per VM |Public IP address with dynamic allocation method |The public IP address associated with the VM is converted as a public IP address resource, with the allocation method set to static. |
| NSGs |NSGs |Network security groups associated with a subnet are cloned as part of the migration to the Resource Manager deployment model. The NSG in the classic deployment model is not removed during the migration. However, the management-plane operations for the NSG are blocked when the migration is in progress. |
| DNS servers |DNS servers |DNS servers associated with a virtual network or the VM are migrated as part of the corresponding resource migration, along with all the properties. |
| UDRs |UDRs |User-defined routes associated with a subnet are cloned as part of the migration to the Resource Manager deployment model. The UDR in the classic deployment model is not removed during the migration. The management-plane operations for the UDR are blocked when the migration is in progress. |
| IP forwarding property on a VM's network configuration |IP forwarding property on the NIC |The IP forwarding property on a VM is converted to a property on the network interface during the migration. |
| Load balancer with multiple IPs |Load balancer with multiple public IP resources |Every public IP associated with the load balancer is converted to a public IP resource and associated with the load balancer after migration. |
| Internal DNS names on the VM |Internal DNS names on the NIC |During migration, the internal DNS suffixes for the VMs are migrated to a read-only property named "InternalDomainNameSuffix" on the NIC. The suffix remains unchanged after migration and VM resolution should continue to work as previously. |
| Virtual Network Gateway |Virtual Network Gateway |Virtual Network Gateway properties are migrated unchanged. The VIP associated with the gateway does not change either. |
| Local network site |Local Network Gateway |Local network site properties are migrated unchanged to a new resource called Local Network Gateway. This represent on premises address prefixes and remote gateway IP. |
| Connections references |Connection |Connectivity references between gateway and local network site in network configuration is represented by a newly created resource called Connection in resource manager after migration. All properties of connectivity reference in network configuration files are copied unchanged to the newly created Connection resource. VNet to VNet connectivity in classic is achieved by creating two IPsec tunnels to local network sites representing the VNets. This is transformed to Vnet2Vnet connection type in resource manager model without requiring local network gateways. |

## Illustration of a simple migration walkthrough
The following screenshot shows an existing cloud service with a VM (not in a virtual network) after the preparation phase:

![Classic representation after prepare](./media/virtual-machines-windows-migration-classic-resource-manager/classic-migration-prepare-portal.png)

After the migration process has completed, the following screenshots show that the new resources have been created in a new resource group:
![Resource Manager representation after prepare](./media/virtual-machines-windows-migration-classic-resource-manager/resourcemanager-migration-prepare-portal.png)

## Next steps
Now that you understand the migration of classic IaaS resources to Resource Manager, you can start migrating resources.

* [Use PowerShell to migrate IaaS resources from classic to Azure Resource Manager](/documentation/articles/virtual-machines-windows-ps-migration-classic-resource-manager/)
* [Use CLI to migrate IaaS resources from classic to Azure Resource Manager](/documentation/articles/virtual-machines-linux-cli-migration-classic-resource-manager/)
* [Platform-supported migration of IaaS resources from classic to Azure Resource Manager](/documentation/articles/virtual-machines-windows-migration-classic-resource-manager/)
* [Clone a classic virtual machine to Azure Resource Manager by using community PowerShell scripts](/documentation/articles/virtual-machines-windows-migration-scripts/)
* [Review most common migration errors](/documentation/articles/virtual-machines-migration-errors/)

