<properties
   pageTitle="Create an application gateway using the Azure CLI in Resource Manager | Azure"
   description="Learn how to create an Application Gateway by using the Azure CLI in Resource Manager"
   services="application-gateway"
   documentationCenter="na"
   authors="georgewallace"
   manager="carmonm"
   editor=""
   tags="azure-resource-manager"
/>
<tags  
   ms.service="application-gateway"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="infrastructure-services"
   ms.date="09/09/2016"
   wacn.date=""
   ms.author="gwallace" />

# Create an application gateway by using the Azure CLI

Azure Application Gateway is a layer-7 load balancer. It provides failover, performance-routing HTTP requests between different servers, whether they are on the cloud or on-premises. Application gateway has the following application delivery features: HTTP load balancing, cookie-based session affinity, and Secure Sockets Layer (SSL) offload, custom health probes, and support for multi-site.

> [AZURE.SELECTOR]
- [Azure portal](/documentation/articles/application-gateway-create-gateway-portal/)
- [Azure Resource Manager PowerShell](/documentation/articles/application-gateway-create-gateway-arm/)
- [Azure Classic PowerShell](/documentation/articles/application-gateway-create-gateway/)
- [Azure Resource Manager template](/documentation/articles/application-gateway-create-gateway-arm-template/)
- [Azure CLI](/documentation/articles/application-gateway-create-gateway-cli/)

## Prerequisite: Install the Azure CLI

To perform the steps in this article, you need to [install the Azure Command-Line Interface for Mac, Linux, and Windows (Azure CLI)](/documentation/articles/xplat-cli-install/) and you need to [log on to Azure](/documentation/articles/xplat-cli-connect/). 

> [AZURE.NOTE] If you don't have an Azure account, you need one. Go sign up for a [trial here](/documentation/articles/sign-up-organization/).

## Scenario

In this scenario, you learn how to create an application gateway using the Azure portal.

This scenario will:

- Create a medium application gateway with two instances.
- Create a virtual network named AdatumAppGatewayVNET with a reserved CIDR block of 10.0.0.0/16.
- Create a subnet called Appgatewaysubnet that uses 10.0.0.0/28 as its CIDR block.
- Configure a certificate for SSL offload.

![Scenario example][scenario]

>[AZURE.NOTE] Additional configuration of the application gateway, including custom health probes, backend pool addresses, and additional rules are configured after the application gateway is configured and not during initial deployment.

## Before you begin

Azure Application Gateway requires its own subnet. When creating a virtual network, ensure that you leave enough address space to have multiple subnets. Once you deploy an application gateway to a subnet,
only additional application gateways are able to be added to the subnet.

## Log in to Azure

Open the **Azure Command Prompt**, and log in. 

    azure login

Once you type the preceding example, a code is provided. Navigate to https://aka.ms/devicelogin in a browser to continue the login process.

![cmd showing device login][1]

In the browser, enter the code you received. You are redirected to a sign-in page.

![browser to enter code][2]

Once the code has been entered you are signed in, close the browser to continue on with the scenario.

![successfully signed in][3]

## Switch to Resource Manager Mode

    azure config mode arm

## Create the resource group

Before creating the application gateway, a resource group is created to contain the application gateway. The following shows the command.

    azure group create -n AdatumAppGatewayRG -l chinaeast

## Create a virtual network

Once the resource group is created, a virtual network is created for the application gateway.  In the following example, the address space was as 10.0.0.0/16 as defined in the preceding scenario notes.

    azure network vnet create -n AdatumAppGatewayVNET -a 10.0.0.0/16 -g AdatumAppGatewayRG -l chinaeast

## Create a subnet

After the virtual network is created, a subnet is added for the application gateway.  If you plan to use application gateway with a web app hosted in the same virtual network as the application gateway, be sure to leave enough room for another subnet.

    azure network vnet subnet create -g AdatumAppGatewayRG -n Appgatewaysubnet -v AdatumAppGatewayVNET -a 10.0.0.0/28 

## Create the application gateway

Once the virtual network and subnet are created, the pre-requisites for the application gateway are complete. Additionally a previously exported .pfx certificate and the password for the certificate are required for the following step:
The IP addresses used for the backend are the IP addresses for your backend server. These values can be either private IPs in the virtual network, public ips, or fully qualified domain names for your backend servers.

    azure network application-gateway create -n AdatumAppGateway -l chinaeast -g AdatumAppGatewayRG -e AdatumAppGatewayVNET -m Appgatewaysubnet -r 134.170.185.46,134.170.188.221,134.170.185.50 -y c:\AdatumAppGateway\adatumcert.pfx -x P@ssw0rd -z 2 -a Standard_Medium -w Basic -j 443 -f Enabled -o 80 -i http -b https -u Standard



This example creates a basic application gateway with default settings for the listener, backend pool, backend http settings, and rules. It also configures SSL offload. You can modify these settings to suit your deployment once the provisioning is successful.
If you already have your web application defined with the the backend pool in the preceding steps, once created, load balancing begins.

## Next steps

Learn how to create custom health probes by visiting [Create a custom health probe](/documentation/articles/application-gateway-create-probe-portal/)

Learn how to configure SSL Offloading and take the costly SSL decryption off your web servers by visiting [Configure SSL Offload](/documentation/articles/application-gateway-ssl-arm/)

<!--Image references-->

[scenario]: ./media/application-gateway-create-gateway-cli/scenario.png
[1]: ./media/application-gateway-create-gateway-cli/figure1.png
[2]: ./media/application-gateway-create-gateway-cli/figure2.png
[3]: ./media/application-gateway-create-gateway-cli/figure3.png