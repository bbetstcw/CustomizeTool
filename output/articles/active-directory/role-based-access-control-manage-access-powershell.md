<properties
	pageTitle="Manage Role Based Access Control (RBAC) with Azure PowerShell | Windows Azure"
	description="How to manage RBAC with Azure PowerShell including listing roles, assigning roles and deleting role assignments."
	services="active-directory"
	documentationCenter=""
	authors="IHenkel"
	manager="stevenpo"
	editor=""/>

<tags
	ms.service="active-directory"
	ms.date="01/04/2016"
	wacn.date=""/>

# Manage Role Based Access Control (RBAC) with Azure PowerShell
> [AZURE.SELECTOR]
- [PowerShell](/documentation/articles/role-based-access-control-manage-access-powershell)
- [Azure CLI](/documentation/articles/role-based-access-control-manage-access-azure-cli)
- [REST API](/documentation/articles/role-based-access-control-manage-access-rest)

## List RBAC roles
### List all available roles
To list RBAC roles available for assignment and to inspect the operations to which they grant access use:

		Get-AzureRmRoleDefinition

![](./media/role-based-access-control-manage-access-powershell/1-get-azure-rm-role-definition1.png)

### List actions of a role
To list the actions for a specific role use:

    Get-AzureRmRoleDefinition <role name>

![](./media/role-based-access-control-manage-access-powershell/1-get-azure-rm-role-definition2.png)

## List Access
### List all role assignments in the selected subscription
To list RBAC access assignments effective at the specified subscription, resource, or resource group use:

    Get-AzureRmRoleAssignment

###	List role assignments effective on a resource group
To list the access assignments for a resource group use:

    Get-AzureRmRoleAssignment -ResourceGroupName <resource group name>

![](./media/role-based-access-control-manage-access-powershell/4-get-azure-rm-role-assignment1.png)

### List role assignments to a user, including ones assigned to users groups
To list access assignments to the specified user as well as to the groups of which the user is member use:

    Get-AzureRmRoleAssignment -ExpandPrincipalGroups

![](./media/role-based-access-control-manage-access-powershell/4-get-azure-rm-role-assignment2.png)

### List classic service administrator and co-admin role assignments
To list access assignments for the classic subscription administrator and co-administrators use:

    Get-AzureRmRoleAssignment -IncludeClassicAdministrators

## Grant Access
### Search for object ids
In order to use the following command sequences, you must find the object ids first.  It is assumed that you already know the subscription id that you are working with, otherwise see [Get-AzureSubscription](https://msdn.microsoft.com/zh-cn/library/dn495302.aspx) on MSDN.

#### Find the object id for an Azure AD Group
To get the object id for an Azure AD Group use:

    Get-AzureRmADGroup -SearchString <group name in quotes>

#### Find the object id for an Azure AD Service Principal
To get the object id for an Azure AD Service Principal use:
    Get-AzureRmADServicePrincipal -SearchString <service name in quotes>

### Assign role to group at subscription scope
To grant access to a group at subscription scope use:

    New-AzureRmRoleAssignment -ObjId <object id> -RoleDefinitionName <role name in quotes> -Scope <scope such as subscription/subscription id>

![](./media/role-based-access-control-manage-access-powershell/2-new-azure-rm-role-assignment1.png)

### Assign role to application at subscription scope
To grant access to an application at subscription scope use:

    New-AzureRmRoleAssignment -ObjId <object id> -RoleDefinitionName <role name in quotes> -Scope <scope such as subscription/subscription id>

![](./media/role-based-access-control-manage-access-powershell/2-new-azure-rm-role-assignment2.png)

### Assign role to user at resource group scope
To grant access to a user at resource group scope:

    New-AzureRmRoleAssignment -SignInName <email of user> -RoleDefinitionName <role name in quotes> -ResourceGroupName <resource group name>

![](./media/role-based-access-control-manage-access-powershell/2-new-azure-rm-role-assignment3.png)

### Assign role to group at resource scope
To grant access to a group at the resource scope use:

    New-AzureRmRoleAssignment -ObjId <object id> -RoleDefinitionName <role name in quotes> -ResourceName <resource name> -ResourceType <resource type> -ParentResource <parent resource> -ResourceGroupName <resource group name>

![](./media/role-based-access-control-manage-access-powershell/2-new-azure-rm-role-assignment4.png)

## Remove access
To remove access for users, groups and applications use:

    Remove-AzureRmRoleAssignment -ObjId <object id> -RoleDefinitionName <role name> -Scope <scope such as subscription/subscription id>

![](./media/role-based-access-control-manage-access-powershell/3-remove-azure-rm-role-assignment.png)

## Create custom role
To create a custom role, use the `New-AzureRmRoleDefinition` command.

The following example creates a custom role called *Virtual Machine Operator* that grants access to all read operations of *Microsoft.Compute*, *Microsoft.Storage*, and *Microsoft.Network* resource providers, and grants access to start, restart, and monitor virtual machines. The custom role can be used in two subscriptions.

![](./media/role-based-access-control-manage-access-powershell/2-new-azurermroledefinition.png)

## Modify a custom role
To modify a custom role first, use the `Get-AzureRmRoleDefinition` command to retrieve role definition. Then, make desired changes to the role definition. Finally, use `Set-AzureRmRoleDefinition` command to save the modified role definition.

The following example adds the `Microsoft.Insights/diagnosticSettings/*` operation to the *Virtual Machine Operator* custom role.

![](./media/role-based-access-control-manage-access-powershell/3-set-azurermroledefinition-1.png)

The following example adds an Azure subscription to the assignable scopes of the Virtual Machine Operator custom role.

![](./media/role-based-access-control-manage-access-powershell/3-set-azurermroledefinition-2.png)

## Delete a custom role

To delete a custom role, use the `Remove-AzureRmRoleDefinition` command.

The following example removes the *Virtual Machine Operator* custom role.

![](./media/role-based-access-control-manage-access-powershell/4-remove-azurermroledefinition.png)

## List custom roles
To list the roles that are available for assignment at a scope, use the `Get-AzureRmRoleDefinition` command.

The following example lists all role available for assignment in the selected subscription.

![](./media/role-based-access-control-manage-access-powershell/5-get-azurermroledefinition-1.png)

In the following example, the *Virtual Machine Operator* custom role isnât available in the *Production4* subscription because that subscription isnât in the **AssignableScopes** of the role.

![](./media/role-based-access-control-manage-access-powershell/5-get-azurermroledefinition2.png)

## RBAC topics
[AZURE.INCLUDE [role-based-access-control-toc.md](../includes/role-based-access-control-toc.md)]
