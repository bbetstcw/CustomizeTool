<properties 
   pageTitle="Specifying DNS Settings in a service configuration file | Microsoft Azure"
   description="specifying custom DNS settings using service configuration file for virtual network"
   services="virtual-network"
   documentationCenter="na"
   authors="joaoma"
   manager="carmonm"
   editor="tysonn" />
<tags
	ms.service="virtual-network"
	ms.date="02/24/2016"
	wacn.date=""/>

# Specifying DNS Settings in a Service Configuration File

## DNS elements

A service configuration file may contain a DnsServers element with a list of IPv4 addresses for the Domain Name System (DNS) servers that the service will use. Settings in the service configuration file take precedence over settings in the network configuration file. For more information, see [Azure Service Configuration Schema (.cscfg File)](https://msdn.microsoft.com/library/azure/ee758710.aspx).

**NetworkConfiguration element**

      <DnsServers>
        <DnsServer name="ID1" IPAddress="IPAddress1" />
        <DnsServer name="ID2" IPAddress="IPAddress2" />
        <DnsServer name="ID3" IPAddress="IPAddress3" />
      </DnsServers>

>[AZURE.WARNING] The **name** attribute in the **DnsServer** element is used only as a reference name. It does not represent the host name for the DNS server. Each **DnsServer** attribute value must be unique across the entire Microsoft Azure subscription.

## See Also

[Azure Service Configuration Schema (.cscfg)](https://msdn.microsoft.com/library/windowsazure/ee758710)

[Azure Virtual Network Configuration Schema](https://msdn.microsoft.com/library/azure/jj157100)

[Configure a Virtual Network Using Network Configuration Files](/documentation/articles/virtual-networks-create-vnet-classic-portal/)

[About Virtual Network settings in the Management Portal](/documentation/articles/virtual-networks-settings/)

