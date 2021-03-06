<properties
   pageTitle="Understand differences between Resource Manager and classic deployment models"
   description="Describes the differences between the Resource Manager deployment model and the classic (or Service Management) deployment model."
   services="azure-resource-manager"
   documentationCenter="na"
   authors="tfitzmac"
   manager="wpickett"
   editor=""/>

<tags
	ms.service="azure-resource-manager"
	ms.date="12/02/2015"
	wacn.date=""/>

# Understanding Resource Manager deployment and classic deployment

The Resource Manager deployment model provides a new way to deploy and manage the services that make up your application. This new model contains important 
differences from the classic deployment model, and the two models are not completely compatible with each other. To simplify the deployment 
and management of resources, Microsoft recommends that you use Resource Manager for new resources, and, if possible, re-deploy existing resources through Resource Manager.

You may also know the classic deployment model as the Service Management model.

This topic describes the differences between the two models, and some of the issues you may encounter when transitioning from the classic model to Resource Manager. 
It provides an overview of the models but does not cover in detail the differences in individual services. For more details about transitioning Compute, Storage, and 
Networking resources, see [Azure Compute, Network & Storage Providers under the Azure Resource Manager](/documentation/articles/virtual-machines-azurerm-versus-azuresm).

Many resources operate without issue in both the classic model and Resource Manager. These resources fully support Resource Manager even if created in 
the classic model. You can transition to Resource Manager without any concerns or extra effort.

However, a few resource providers offer two versions of the resource (one for classic, and one for Resource Manager) because of the architectural differences between the models. 
The resource providers that differentiate between the two models are:

- Compute
- Storage
- Network

For these resource types, you must be aware of which version you are using because the supported operations will differ. 

To understand the architectural differences between the two models, see [Azure Resource Manager Architecture](/documentation/articles/virtual-machines-azure-resource-manager-architecture).

## Resource Manager characteristics

Resources created through Resource Manager share the following characteristics:

- Created through one of the following methods:

  - The [Azure Management Portal](https://manage.windowsazure.cn/).

        ![Azure Management Portal](./media/resource-manager-deployment-model/preview-portal.png)

        For Compute, Storage, and Networking resources, you have the option of using either Resource Manager or Classic deployment. Select **Resource Manager**.

        ![Resource Manager deployment](./media/resource-manager-deployment-model/select-resource-manager.png)

  - For Azure PowerShell versions earlier than 1.0, commands run in the **AzureResourceManager** mode.

            PS C:\> Switch-AzureMode -Name AzureResourceManager

  - For Azure PowerShell 1.0, use the Resource Manager version of commands. These commands have the format *verb-AzureRm*, as shown below.

            PS C:\> Get-AzureRmResourceGroupDeployment

  - [Azure Resource Manager REST API](https://msdn.microsoft.com/zh-cn/library/azure/dn790568.aspx) for REST operations.
  - Azure CLI commands run in the **arm** mode.

            azure config mode arm

- The resource type does not include **(classic)** in the name. The image below shows the type as **Storage account**.

    ![web site](./media/resource-manager-deployment-model/resource-manager-type.png)

## Classic deployment characteristics

Resources created in the classic deployment model share the following characteristics:

- Created through one of the following methods:

  - [Management Portal](https://manage.windowsazure.cn)

        ![Management Portal](./media/resource-manager-deployment-model/azure-portal.png)

        Or, the portal and you specify **Classic** deployment (for Compute, Storage, and Networking).

        ![Classic deployment](./media/resource-manager-deployment-model/select-classic.png)

  - For versions of Azure PowerShell earlier than 1.0, commands run in the **AzureServiceManagement** mode (which is the default mode, so if do not you specifically switch to AzureResourceManager, you are running in AzureServiceManagement mode).

            PS C:\> Switch-AzureMode -Name AzureServiceManagement

  - For Azure PowerShell 1.0, use the Service Management version of commands. These command names **do not** have the format *verb-AzureRm*, as shown below.

            PS C:\> Get-AzureDeployment

  - [Service Management REST API](https://msdn.microsoft.com/zh-cn/library/azure/ee460799.aspx) for REST operations.
  - Azure CLI commands run in **asm** or default mode.
- The resource type includes **(classic)** in the name. The image below shows the type as **Storage account (classic)**.

    ![classic type](./media/resource-manager-deployment-model/classic-type.png)

You can still use the portal to manage resources that were created through classic deployment.

## Benefits of using Resource Manager and resource groups

Resource Manager added the concept of the resource group. Every resource you create through Resource Manager exists within a resource group. The Resource Manager deployment model provide several benefits:

- You can deploy, manage, and monitor all of the services for your solution as a group, rather than handling these services individually.
- You can repeatedly deploy your application throughout the app lifecycle and have confidence your resources are deployed in a consistent state.
- You can use declarative templates to define your deployment. 
- You can define the dependencies between resources so they are deployed in the correct order.
- You can apply access control to all services in your resource group because Role-Based Access Control (RBAC) is natively integrated into the management platform.
- You can apply tags to resources to logically organize all of the resources in your subscription.


Prior to Resource Manager, every resource you created through classic deployment did not exist within a resource group. When Resource Manager was added, all resources were retroactively added to default resource groups. If you create a resource through classic deployment now, the resource is 
automatically created within a default resource group for that service, even though you did not specify that resource group at deployment. However, just existing within a resource group does not mean that the resource has been converted to the Resource Manager model. For Virtual Machines, Storage, and Virtual Networks, if the resource was created through classic deployment, you must continue to operate on it through classic operations. 

You can move resources to a different resource group, and add new resources to an existing resource group. So, your resource group can contain a 
mix of resources created through Resource Manager and classic deployment. This combination of resources can create unexpected results because the resources 
do not support the same operations.

By using declarative templates, you might be able to simplify your scripts for deployment. Instead of attempting to convert existing scripts from Service Management to Resource Manager, consider re-working your deployment strategy to 
take advantage of defining your infrastructure and configuration in the template.

## Using tags

Tags enable you to logically organize your resources. Only resources created through Resource Manager support tags. You cannot apply tags to classic resources.

For more information about using tags in Resource Manager, see [Using tags to organize your Azure resources](/documentation/articles/resource-group-using-tags).

## Supported operations for the deployment models

The resources you created in the classic deployment model do not support Resource Manager operations. In some cases, a Resource Manager command can retrieve 
information about a resource created through classic deployment, or can perform an administrative tasks such as moving a classic resource to another resource group, but 
these cases should not give the impression that the type supports Resource Manager operations. For example, suppose you have a resource group 
that contains Virtual Machines that were created with Resource Manager and classic. If you run the following PowerShell command, you will see all of the Virtual Machines:

    PS C:\> Get-AzureRmResourceGroup -Name ExampleGroup
    ...
    Resources :
     Name                 Type                                          Location
     ================     ============================================  ========
     ExampleClassicVM     Microsoft.ClassicCompute/domainNames          chinaeast
     ExampleClassicVM     Microsoft.ClassicCompute/virtualMachines      chinaeast
     ExampleResourceVM    Microsoft.Compute/virtualMachines             chinaeast
    ...

However, if you run the Get-AzureVM command, you will only get Virtual Machines that were created with Resource Manager.

    PS C:\> Get-AzureVM -ResourceGroupName ExampleGroup
    ...
    Id       : /subscriptions/xxxx/resourceGroups/ExampleGroup/providers/Microsoft.Compute/virtualMachines/ExampleResourceVM
    Name     : ExampleResourceVM
    ...

In general, you should not expect resources created through classic deployment to work with Resource Manager commands.

When working with resources created through Resource Manager, you should use Resource Manager operations and not switch back to Service Management operations.

## Considerations for Virtual Machines

There are some important considerations when working Virtual Machines.

- Virtual machines deployed with the classic deployment model cannot be included in a virtual network deployed with Resource Manager.
- Virtual machines deployed with the Resource Manager deployment model must be included in a virtual network.
- Virtual machines deployed with the classic deployment model don't have to be included in a virtual network.

If you can afford downtime for your Virtual Machines, you can transition them from classic deployment to Resource Manager with the [ASM2ARM PowerShell scripts](https://github.com/fullscale180/asm2arm). 

For a list of equivalent Azure CLI commands when transitioning from classic deployment to Resource Manager, see [Equivalent Resource Manager and Service Management commands for VM operations](/documentation/articles/xplat-cli-azure-manage-vm-asm-arm).

For more details about transitioning Compute, Storage, and 
Networking resources, see [Azure Compute, Network & Storage Providers under the Azure Resource Manager](/documentation/articles/virtual-machines-azurerm-versus-azuresm).

To learn about connecting virtual networks from different deployment models, see [Connecting classic VNets to new VNets](/documentation/articles/virtual-networks-arm-asm-s2s).

## Next steps

- To learn about creating declarative deployment templates, see [Authoring Azure Resource Manager templates](/documentation/articles/resource-group-authoring-templates).
- To see the commands for deploying a template, see [Deploy an application with Azure Resource Manager template](/documentation/articles/resource-group-template-deploy).

