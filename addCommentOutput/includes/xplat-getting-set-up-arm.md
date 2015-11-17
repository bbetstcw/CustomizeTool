<properties services="virtual-machines" title="Using Azure CLI with Azure Resource Manager" authors="squillace" solutions="" manager="timlt" editor="tysonn" />

<tags
	ms.service="virtual-machine"
	ms.date="04/13/2015"
	wacn.date=""/>

<!-- deleted by customization
## Using Azure CLI with Azure Resource Manager (ARM)

Before you can use the Azure CLI with Resource Manager commands and templates to deploy Azure resources and workloads using resource groups, you will need an account with Azure (of course). If you do not have an account, you can get a [free Azure trial here](/pricing/1rmb-trial/).
-->
<!-- keep by customization: begin -->
## Using the xplat-cli with Azure Resource Manager (ARM)

Before you can use the xplat-cli with Resource Manager commands and templates to deploy Azure resources and workloads using resource groups, you will need an account with Azure (of course). If you do not have an account, you can get a [free Azure trial here](/pricing/1rmb-trial/).
<!-- keep by customization: end -->

> [AZURE.NOTE] If you don't already have an Azure account but you do have a subscription to MSDN subscription, you can get free Azure credits by activating your [MSDN subscriber benefits here](/pricing/member-offers/msdn-benefits-details/) -- or you can use the free account. Either will work for Azure access.

<!-- deleted by customization
### Step 1: Verify the Azure CLI version

To use Azure CLI for imperative commands and ARM templates, you need to have at least version 0.8.17. To verify your version, type `azure --version`. You should see something like:
-->
<!-- keep by customization: begin -->
## Step 1: Verify the xplat-cli version

To use the xplat-cli for imperative commands and ARM templates, you need to have at least version 0.8.17. To verify your version, type `azure --version`. You should see something like:
<!-- keep by customization: end -->

    $ azure --version
    0.8.17 (node: 0.10.25)
<!-- deleted by customization

If you need to update your version of Azure CLI, see [Azure CLI](https://github.com/Azure/azure-xplat-cli).

### Step 2: Verify you are using a work or school identity with Azure
-->
<!-- keep by customization: begin -->
    
If you need to update your version of the xplat-cli, see [xplat-cli](https://github.com/Azure/azure-xplat-cli).

## Step 2: Verify you are using a work or school identity with Azure
<!-- keep by customization: end -->

You can only use the ARM command mode if you are using an [Azure Active Directory tenant](https://msdn.microsoft.com/zh-cn/library/azure/jj573650.aspx#BKMK_WhatIsAnAzureADTenant) or a [Service Principal Name](https://msdn.microsoft.com/zh-cn/library/azure/dn132633.aspx). (These are also called *organizational ids*.)

To see if you have one, log in by typing `azure login` and using your work or school username and password when prompted. If you do have one, you should see something like the following:

    $ azure login
    info:    Executing command login
    warn:    Please note that currently you can login only via Microsoft organizational account or service principal. For instructions on how to set them up, please read http://aka.ms/Dhf67j.
    Username: ahmet@contoso.partner.onmschina.cn
    Password: *********
    |info:    Added subscription Visual Studio Ultimate with MSDN
    info:    Setting subscription Visual Studio Ultimate with MSDN as default
    info:    Added subscription Azure Trial
    +
    info:    login command OK
If you do not see this, you must create a new tenant (or service principal) with your Microsoft account identity. (This is often the case with personal MSDN subscriptions or trial subscriptions.) To create a work or school id from your Azure account created with a Microsoft id, see [Associate an Azure AD Directory with a new Azure Subscription](https://msdn.microsoft.com/zh-cn/library/azure/jj573650.aspx#BKMK_WhatIsAnAzureADTenant). If you think you should have an organizational id already, you may need to talk with the person who created the account for you.

<!-- deleted by customization ### --><!-- keep by customization: begin --> ## <!-- keep by customization: end --> Step 3: Choose your Azure subscription

If you have only one subscription in your Azure account, <!-- deleted by customization Azure CLI --><!-- keep by customization: begin --> the xplat-cli <!-- keep by customization: end --> associates itself with that subscription by default. If you have more than one subscription, you need to select the subscription you want to use by typing `azure account set <subscription id or name> true` where _subscription id or name_ is either the subscription id or the subscription name that you would like to work with in the current session.

You should see something like the following:

    $ azure account set "Azure Trial" true
    info:    Executing command account set
    info:    Setting subscription to "Azure Trial" with id "2lskd82-434-4730-9df9-akd83lsk92sa".
    info:    Changes saved
    info:    account set command OK
<!-- deleted by customization

### Step 4: Place your Azure CLI in the ARM mode

To use the Azure Resource Management (ARM) mode with the Azure CLI, type `azure config mode arm`. You should see something like the following:
-->
<!-- keep by customization: begin -->
    
## Step 4: Place your xplat-cli in the ARM mode

To use the Azure Resource Management (ARM) mode with the xplat-cli, type `azure config mode arm`. You should see something like the following:
<!-- keep by customization: end -->

    $ azure config mode arm
    info:    New mode is arm

> [AZURE.NOTE] You can switch back to use Azure service management commands by typing `azure config mode asm`.