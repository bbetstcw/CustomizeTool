<properties
    pageTitle="Verify a gateway connection | Azure"
    description="This article shows you how to verify a gateway connection in the Resource Manager deployment model"
    services="vpn-gateway"
    documentationcenter="na"
    author="cherylmc"
    manager="carmonm"
    editor=""
    tags="azure-resource-manager" />
<tags
    ms.assetid="7e3d1043-caa9-4472-96d3-832f4e2c91ee"
    ms.service="vpn-gateway"
    ms.devlang="na"
    ms.topic="article"
    ms.tgt_pltfrm="na"
    ms.workload="infrastructure-services"
    ms.date="10/14/2016"
    wacn.date=""
    ms.author="cherylmc" />

# Verify a gateway connection
You can verify your gateway connection in a few different ways. This article will show you how to verify the status of a Resource Manager gateway connection by using the Azure portal and by using PowerShell.

## Verify using PowerShell
You'll need to install the latest version of the Azure Resource Manager PowerShell cmdlets. For information on installing PowerShell cmdlets, see [How to install and configure Azure PowerShell](/documentation/articles/powershell-install-configure/). For more information about using Resource Manager cmdlets, see [Using Windows PowerShell with Resource Manager](/documentation/articles/powershell-azure-resource-manager/).

### Step 1: Log in to your Azure account
1. Open your PowerShell console with elevated privileges and connect to your account.
   
        Login-AzureRmAccount
2. Check the subscriptions for the account.
   
        Get-AzureRmSubscription 
3. Specify the subscription that you want to use.
   
        Select-AzureRmSubscription -SubscriptionName "Replace_with_your_subscription_name"

### Step 2: Verify your connection
[AZURE.INCLUDE [verify connection powershell](../../includes/vpn-gateway-verify-connection-ps-rm-include.md)]

## Verify using the Azure portal
[AZURE.INCLUDE [verify connection portal](../../includes/vpn-gateway-verify-connection-portal-rm-include.md)]

## Next steps
* You can add virtual machines to your virtual networks. See [Create a Virtual Machine](/documentation/articles/virtual-machines-windows-hero-tutorial/) for steps.

