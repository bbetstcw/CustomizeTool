<properties 
   pageTitle="Get started creating Internet facing load balancer in classic mode using PowerShell | Windows Azure"
   description="Learn how to create an Internet facing load balancer in classic mode using PowerShell"
   services="load-balancer"
   documentationCenter="na"
   authors="joaoma"
   manager="carolz"
   editor=""
   tags="azure-service-management"
/>
<tags
	ms.service="load-balancer"
	ms.date="09/23/2015"
	wacn.date=""/>

# Get started creating Internet facing load balancer (classic) in PowerShell

[AZURE.INCLUDE [load-balancer-get-started-internet-classic-selectors-include.md](../includes/load-balancer-get-started-internet-classic-selectors-include.md)]

[AZURE.INCLUDE [load-balancer-get-started-internet-intro-include.md](../includes/load-balancer-get-started-internet-intro-include.md)]

[AZURE.INCLUDE [azure-arm-classic-important-include](../includes/azure-arm-classic-important-include.md)] This article covers the classic deployment model. If you are looking for Azure Resource Manager deployment model, go to [Get started creating Internet facing load balancer using resource manager](/documentation/articles/load-balancer-get-started-internet-arm-ps).

[AZURE.INCLUDE [load-balancer-get-started-internet-scenario-include.md](../includes/load-balancer-get-started-internet-scenario-include.md)]



## Set up load balancer using PowerShell

To set up a load balancer using powershell, follow the steps below:

1. If you have never used Azure PowerShell, see [How to Install and Configure Azure PowerShell](/documentation/articles/powershell-install-configure) and follow the instructions all the way to the end to sign into Azure and select your subscription.


2. After creating a virtual machine, you can use PowerShell cmdlets to add a load balancer to a virtual machine within the same cloud service.

In the following example you will add a load balancer set called "webfarm" to cloud service "mytestcloud" (or myctestcloud.chinacloudapp.cn) , adding the endpoints for the load balancer to virtual machines named "web1" and "web2". The load balancer receives network traffic on port 80 and load balances between the virtual machines defined by the local endpoint (in this case port 80) using TCP.


### Step 1
Create a load balanced endpoint for the first VM "web1"

	Get-AzureVM -ServiceName "mytestcloud" -Name "web1" | Add-AzureEndpoint -Name "HttpIn" -Protocol "tcp" -PublicPort 80 -LocalPort 80 -LBSetName "WebFarm" -ProbePort 80 -ProbeProtocol "http" -ProbePath '/' | Update-AzureVM

### Step 2 

Create another endpoint for the second VM  "web2" using the same load balancer set name

	Get-AzureVM -ServiceName "mytestcloud" -Name "web2" | Add-AzureEndpoint -Name "HttpIn" -Protocol "tcp" -PublicPort 80 -LocalPort 80 -LBSetName "WebFarm" -ProbePort 80 -ProbeProtocol "http" -ProbePath '/' | Update-AzureVM

## Remove a virtual machine from a load balancer

You can use Remove-AzureEndpoint to remove a virtual machine endpoint from the load balancer 

	Get-azureVM -ServiceName mytestcloud  -Name web1 |Remove-AzureEndpoint -Name httpin| Update-AzureVM

## Next steps

[Get started configuring an internal load balancer](/documentation/articles/load-balancer-internal-getstarted)

[Configure a load balancer distribution mode](/documentation/articles/load-balancer-distribution-mode)

[Configure idle TCP timeout settings for your load balancer](/documentation/articles/load-balancer-tcp-idle-timeout)

