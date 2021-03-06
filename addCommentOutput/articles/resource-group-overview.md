<properties
   pageTitle="Azure Resource Manager Overview | Windows Azure"
   description="Describes how to use Azure Resource Manager for deployment, management, and access control of resources on Azure."
   services="azure-resource-manager"
   documentationCenter="na"
   authors="tfitzmac"
   manager="wpickett"
   editor=""/>

<tags
	ms.service="azure-resource-manager"
	ms.date="12/08/2015"
	wacn.date=""/>

# Azure Resource Manager overview

The infrastructure for your application is typically made up of many components â maybe a virtual machine, storage account, and virtual network, or a web site, database, database server, and 3rd party services. You do not see these components as separate entities, instead you see them as related and interdependent parts of a single entity. You want to deploy, manage, and monitor them as a group. Azure Resource Manager enables you to work with the resources in your solution as a group. You can deploy, update or delete all of the resources for your solution in a single, coordinated operation. You use a template for deployment and that template can work for different environments such as testing, staging and production. Resource Manager provides security, auditing, and tagging features to help you manage your resources after deployment. 

## The benefits of using Resource Manager

Resource Manager provides several benefits:

- You can deploy, manage, and monitor all of the resources for your solution as a group, rather than handling these resources individually.
- You can repeatedly deploy your solution throughout the development lifecycle and have confidence your resources are deployed in a consistent state.
- You can use declarative templates to define your deployment.
- You can define the dependencies between resources so they are deployed in the correct order.
- You can apply access control to all services in your resource group because Role-Based Access Control (RBAC) is natively integrated into the management platform.
- You can apply tags to resources to logically organize all of the resources in your subscription.
- You can clarify billing for your organization by viewing the rolled-up costs for the entire group or for a group of resources sharing the same tag.  

Resource Manager provides a new way to deploy and manage your solutions. If you used the earlier deployment model and want to learn about the changes, see [Understanding Resource Manager deployment and classic deployment](/documentation/articles/resource-manager-deployment-model).

## Guidance

The following suggestions will help you take full advantage of Resource Manager when working with your solutions.

1. Define and deploy your infrastructure through the declarative syntax in Resource Manager templates, rather than through imperative commands.
2. Define all deployment and configuration steps in the template. You should have no manual steps for setting up your solution.
3. Run imperative commands to manage your resources, such as to start or stop an app or machine.
4. Arrange resources with the same lifecycle in a resource group. Use tags for all other organizing of resources.

## Resource groups

A resource group is a container that holds related resources for an application. The resource group could include all of the resources for an application, or only those resources that are logically grouped together. You can decide how you want to allocate resources to resource groups based on what makes the most sense for your organization.

There are some important factors to consider when defining your resource group:

1. All of the resources in your group must share the same lifecycle. You will deploy, update and delete them together. If one resource, such as a database server, needs to exist on a different deployment cycle it should be in another resource group.
2. Each resource can only exist in one resource group.
3. You can add or remove a resource to a resource group at any time.
4. You can move a resource from one resource group to another group. For more information, see [Move resources to new resource group or subscription](/documentation/articles/resource-group-move-resources).
4. A resource group can contain resources that reside in different regions.
5. A resource group can be used to scope access control for administrative actions.
6. A resource can be linked to a resource in another resource group when the two resources must interact with each other but they do not share the same lifecycle (for example, multiple apps connecting to a database). For more information, see [Linking resources in Azure Resource Manager](/documentation/articles/resource-group-link-resources).

## Template deployment

With Resource Manager, you can create a simple template (in JSON format) that defines deployment and configuration of your application. This template is known as a Resource Manager template and provides a declarative way to define deployment. By using a template, you can repeatedly deploy your application throughout the app lifecycle and have confidence your resources are deployed in a consistent state.

Within the template, you define the infrastructure for your app, how to configure that infrastructure, and how to publish your app code to that infrastructure. You do not need to worry about the order for deployment because Azure Resource Manager analyzes dependencies to ensure resources are created in the correct order. For more information, see [Defining dependencies in Azure Resource Manager templates](/documentation/articles/resource-group-define-dependencies).

You do not have to define your entire infrastructure in a single template. Often, it makes sense to divide your deployment requirements into a set of targeted, purpose-specific templates. You can easily re-use these templates for different solutions. To deploy a particular solution, you create a master template that links all of the required templates. For more information, see [Using linked templates with Azure Resource Manager](/documentation/articles/resource-group-linked-templates).

You can also use the template for updates to the infrastructure. For example, you can add a new resource to your app and add configuration rules for the resources that are already deployed. If the template specifies creating a new resource but that resource already exists, Azure Resource Manager performs an update instead of creating a new asset. Azure Resource Manager updates the existing asset to the same state as it would be as new. Or, you can specify that Resource Manager delete any resources that are not specified in the template. To understand the differences options when deploying, see [Deploy an application with Azure Resource Manager template](/documentation/articles/resource-group-template-deploy). 

You can specify parameters in your template to allow for customization and flexibility in deployment. For example, you can pass parameter values that tailor deployment for your test environment. By specifying the parameters, you can use the same template for deployment to all of your appâs environments.

Resource Manager provides extensions for scenarios when you need additional operations such as installing particular software that is not included in the setup. If you are already using a configuration management service, like DSC, Chef or Puppet, you can continue working with that service by using extensions.

When you create a solution from the Marketplace, the solution automatically includes a deployment template. You do not have to create your template from scratch because you can start with the template for your solution and customize it to meet your specific needs.

Finally, the template becomes part of the source code for your app. You can check it in to your source code repository and update it as your app evolves. You can edit the template through Visual Studio.

For more information about defining the template, see [Authoring Azure Resource Manager Templates](/documentation/articles/resource-group-authoring-templates).

For template schemas, see [Azure Resource Manager Schemas](https://github.com/Azure/azure-resource-manager-schemas).

For information about using a template for deployment, see [Deploy an application with Azure Resource Manager template](/documentation/articles/resource-group-template-deploy).

For guidance about how to structure your templates, see [Best practices for designing Azure Resource Manager templates](/documentation/articles/best-practices-resource-manager-design-templates).

For guidance on deploying your solution to different environments, see [Development and test environments in Windows Azure](/documentation/articles/solution-dev-test-environments-preview-portal). 

## Tags

Resource Manager provides a tagging feature that enables you to categorize resources according to your requirements for managing or billing. You might want to use tags when you have a complex collection of resource groups and resources, and need to visualize those assets in the way that makes the most sense to you. For example, you could tag resources that serve a similar role in your organization or belong to the same department.

Resources do not need to reside in the same resource group to share a tag. You can create your own tag taxonomy to ensure that all users in your organization use common tags rather than users inadvertently applying slightly different tags (such as "dept" instead of "department").

For more information about tags, see [Using tags to organize your Azure resources](/documentation/articles/resource-group-using-tags).

## Access control

Resource Manager enables you to control who has access to specific actions for your organization. It natively integrates OAuth and Role-Based Access Control (RBAC) into the management platform and applies that access control to all services in your resource group. You can add users to pre-defined platform and resource-specific roles and apply those roles to a subscription, resource group or resource to limit access. For example, you can take advantage of the pre-defined role called SQL DB Contributor that permits users to manage databases, but not database servers or security policies. You add users in your organization that need this type of access to the SQL DB Contributor role and apply the role to the subscription, resource group or resource.

Resource Manager automatically logs user actions for auditing. For informatin about working with the audit logs, see [Audit operations with Resource Manager](/documentation/articles/resource-group-audit).

For more information about role-based access control, see <!-- deleted by customization [Azure Role-based Access --><!-- keep by customization: begin --> [Role-based access <!-- keep by customization: end --><!-- deleted by customization Control](/documentation/articles/role-based-access-control-configure) --><!-- keep by customization: begin --> control in the Windows Azure Management Portal](/documentation/articles/role-based-access-control-configure) <!-- keep by customization: end -->. <!-- deleted by customization The [RBAC: Built in Roles](/documentation/articles/role-based-access-built-in-roles) --><!-- keep by customization: begin --> This <!-- keep by customization: end --> topic contains a list of the built-in roles and the permitted actions. The built-in roles include general roles such as Owner, Reader, and Contributor; as well as, service-specific roles such as Virtual Machine Contributor, Virtual Network Contributor, and SQL Security Manager (to name just a few of the available roles).

You can also explicitly lock critical resources to prevent users from deleting or modifying them. For more information, see [Lock resources with Azure Resource Manager](/documentation/articles/resource-group-lock-resources).

For best practices, see [Security considerations for Azure Resource Manager](/documentation/articles/best-practices-resource-manager-security)

## Manage resources with customized policies

Resource Manager enables you to create customized policies for managing your resources. The types of policies you create can include scenarios as diverse as enforcing a naming convention on resources, limiting which regions can host a type of resource, or requiring a tag value on resources to organize billing by departments. For more information, see [Use Policy to manage resources and control access](/documentation/articles/resource-manager-policy).

## Consistent management layer

Resource Manager provides completely compatible operations through Azure PowerShell, Azure CLI for Mac, Linux, and Windows, the Azure <!-- deleted by customization preview portal --><!-- keep by customization: begin --> Management Portal <!-- keep by customization: end -->, or REST API. You can use the interface that works best for you, and move quickly between the interfaces without confusion. The portal even displays notification for actions taken outside of the portal.

For information about PowerShell, see [Using Azure PowerShell with Resource Manager](/documentation/articles/powershell-azure-resource-manager) and [Azure Resource Manager Cmdlets](https://msdn.microsoft.com/zh-cn/library/azure/dn757692.aspx).

For information about Azure CLI, see [Using the Azure CLI for Mac, Linux, and Windows with Azure Resource Management](/documentation/articles/xplat-cli-azure-resource-manager).

For information about the REST API, see [Azure Resource Manager REST API Reference](https://msdn.microsoft.com/zh-cn/library/azure/dn790568.aspx).

For information about using the preview portal, see [Using the Azure Preview Portal to manage your Azure resources](/documentation/articles/resource-group-portal).

Azure Resource Manager supports cross-origin resource sharing (CORS). With CORS, you can call the Resource Manager REST API or an Azure service REST API from a web site that resides in a different domain. Without CORS support, the web browser would prevent an app in one domain from accessing resources in another domain. Resource Manager enables CORS for all requests with valid authentication credentials.

## Next steps

- To learn about creating templates, see [Authoring templates](/documentation/articles/resource-group-authoring-templates)
- To deploy the template you created, see [Deploying templates](/documentation/articles/resource-group-template-deploy)
- To understand the functions you can use in a template, see [Template functions](/documentation/articles/resource-group-template-functions)
- For guidance on designing your templates, see [Best practices for designing Azure Resource Manager templates](/documentation/articles/best-practices-resource-manager-design-templates)
<!-- deleted by customization

Here's a video demonstration of this overview:

[AZURE.VIDEO azure-resource-manager-overview]

-->