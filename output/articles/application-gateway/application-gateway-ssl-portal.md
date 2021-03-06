<properties
    pageTitle="Configure an application gateway for SSL offload by using the portal | Azure"
    description="This page provides instructions to create an application gateway with SSL offload by using the portal"
    documentationcenter="na"
    services="application-gateway"
    author="georgewallace"
    manager="carmonm"
    editor="tysonn" />
<tags
    ms.assetid="8373379a-a26a-45d2-aa62-dd282298eff3"
    ms.service="application-gateway"
    ms.devlang="na"
    ms.topic="article"
    ms.tgt_pltfrm="na"
    ms.workload="infrastructure-services"
    ms.date="11/16/2016"
    wacn.date=""
    ms.author="gwallace" />

# Configure an application gateway for SSL offload by using the portal
> [AZURE.SELECTOR]
- [Azure portal](/documentation/articles/application-gateway-ssl-portal/)
- [Azure Resource Manager PowerShell](/documentation/articles/application-gateway-ssl-arm/)
- [Azure Classic PowerShell](/documentation/articles/application-gateway-ssl/)

Azure Application Gateway can be configured to terminate the Secure Sockets Layer (SSL) session at the gateway to avoid costly SSL decryption tasks to happen at the web farm. SSL offload also simplifies the front-end server setup and management of the web application.

## Scenario

The following scenario goes through configuring SSL offload on an existing application gateway. The scenario assumes that you have already followed the steps to [Create an Application Gateway](/documentation/articles/application-gateway-create-gateway-portal/).

## Before you begin

To configure SSL offload with an application gateway, a certificate is required. This certificate is loaded on the application gateway and used to encrypt and decrypt the traffic sent via SSL. The certificate needs to be in Personal Information Exchange (pfx) format. This file format allows for the private key to be exported which is required by the application gateway to perform the encryption and decryption of traffic.

## Add an HTTPS listener

The HTTPS listener looks for traffic based on its configuration and helps route the traffic to the backend pools.

### Step 1

Navigate to the Azure portal and select an existing application gateway

### Step 2

Click Listeners and click the Add button to add a listener.

![app gateway overview blade][1]

### Step 3

Fill out the required information for the listener and upload the .pfx certificate, when complete click OK.

**Name** - This value is a friendly name of the listener.

**Frontend IP configuration** - This value is the frontend IP configuration that is used for the listener.

**Frontend port (Name/Port)** - A friendly name for the port used on the front end of the application gateway and the actual port used.

**Protocol** - A switch to determine if https or http is used for the front end.

**Certificate (Name/Password)** - If SSL offload is used, a .pfx certificate is required for this setting and a friendly name and password are required.

![add listener blade][2]

## Create a rule and associate it to the listener

The listener has now been created. It is time to create a rule to handle the traffic from the listener.

### Step 1

Click the **Rules** of the application gateway, and then click Add.

![app gateway rules blade][3]

### Step 2

On the **Add basic rule** blade, type in the friendly name for the rule and choose the listener created in the previous step. Choose the appropriate backend pool and http setting and click **OK**

![https settings window][4]

The settings are now saved to the application gateway. The save process for these settings may take a while before they are available to view through the portal or through PowerShell. Once saved the application gateway handles the encryption and decryption of traffic. All traffic between the application gateway and the backend web servers will be handled over http. Any communication back to the client if initiated over https will be returned to the client encrypted.

## Next steps

To learn how to configure a custom health probe with Azure Application Gateway, see [Create a custom health probe](/documentation/articles/application-gateway-create-gateway-portal/).

[1]: ./media/application-gateway-ssl-portal/figure1.png
[2]: ./media/application-gateway-ssl-portal/figure2.png
[3]: ./media/application-gateway-ssl-portal/figure3.png
[4]: ./media/application-gateway-ssl-portal/figure4.png
