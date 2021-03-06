<properties
    pageTitle="Troubleshoot Application Gateway Bad Gateway (502) errors | Azure"
    description="Learn how to troubleshoot Application Gateway 502 errors"
    services="application-gateway"
    documentationcenter="na"
    author="amitsriva"
    manager="rossort"
    editor=""
    tags="azure-resource-manager" />
<tags
    ms.assetid="1d431ead-d318-47d8-b3ad-9c69f7e08813"
    ms.service="application-gateway"
    ms.devlang="na"
    ms.topic="article"
    ms.tgt_pltfrm="na"
    ms.workload="infrastructure-services"
    ms.date="11/16/2016"
    wacn.date=""
    ms.author="amitsriva" />

# Troubleshooting bad gateway errors in Application Gateway

## Overview

After configuring an Azure Application Gateway, one of the errors which users may encounter is "Server Error: 502 - Web server received an invalid response while acting as a gateway or proxy server". This may happen due to the following main reasons:

* Azure Application Gateway's back-end pool is not configured or empty.
* None of the VMs or instances in VM Scale Set are healthy.
* Back-end VMs or instances of VM Scale Set are not responding to the default health probe.
* Invalid or improper configuration of custom health probes.
* Request time out or connectivity issues with user requests.

## Empty BackendAddressPool

### Cause

If the Application Gateway has no VMs or VM Scale Set configured in the back-end address pool, it cannot route any customer request and throws a bad gateway error.

### Solution

Ensure that the back-end address pool is not empty. This can be done either via PowerShell, CLI, or portal.

```powershell
Get-AzureRmApplicationGateway -Name "SampleGateway" -ResourceGroupName "ExampleResourceGroup"
```

The output from the preceding cmdlet should contain non-empty back-end address pool. Following is an example where two pools are returned which are configured with FQDN or IP addresses for backend VMs. The provisioning state of the BackendAddressPool must be 'Succeeded'.

BackendAddressPoolsText:

```json
[{
    "BackendAddresses": [{
        "ipAddress": "10.0.0.10",
        "ipAddress": "10.0.0.11"
    }],
    "BackendIpConfigurations": [],
    "ProvisioningState": "Succeeded",
    "Name": "Pool01",
    "Etag": "W/\"00000000-0000-0000-0000-000000000000\"",
    "Id": "/subscriptions/<subscription id>/resourceGroups/<resource group name>/providers/Microsoft.Network/applicationGateways/<application gateway name>/backendAddressPools/pool01"
}, {
    "BackendAddresses": [{
        "Fqdn": "xyx.chinacloudapp.cn",
        "Fqdn": "abc.chinacloudapp.cn"
    }],
    "BackendIpConfigurations": [],
    "ProvisioningState": "Succeeded",
    "Name": "Pool02",
    "Etag": "W/\"00000000-0000-0000-0000-000000000000\"",
    "Id": "/subscriptions/<subscription id>/resourceGroups/<resource group name>/providers/Microsoft.Network/applicationGateways/<application gateway name>/backendAddressPools/pool02"
}]
```

## Unhealthy instances in BackendAddressPool

### Cause

If all the instances of BackendAddressPool are unhealthy, then Application Gateway would not have any back-end to route user request to. This could also be the case when back-end instances are healthy but do not have the required application deployed.

### Solution

Ensure that the instances are healthy and the application is properly configured. Check if the back-end instances are able to respond to a ping from another VM in the same VNet. If configured with a public end point, ensure that a browser request to the web application is serviceable.

## Problems with default health probe

### Cause

502 errors can also be frequent indicators that the default health probe is not able to reach back-end VMs. When an Application Gateway instance is provisioned, it automatically configures a default health probe to each BackendAddressPool using properties of the BackendHttpSetting. No user input is required to set this probe. Specifically, when a load balancing rule is configured, an association is made between a BackendHttpSetting and BackendAddressPool. A default probe is configured for each of these associations and Application Gateway initiates a periodic health check connection to each instance in the BackendAddressPool at the port specified in the BackendHttpSetting element. Following table lists the values associated with the default health probe.

| Probe property | Value | Description |
| --- | --- | --- |
| Probe URL |http://127.0.0.1/ |URL path |
| Interval |30 |Probe interval in seconds |
| Time-out |30 |Probe time-out in seconds |
| Unhealthy threshold |3 |Probe retry count. The back-end server is marked down after the consecutive probe failure count reaches the unhealthy threshold. |

### Solution

* Ensure that a default site is configured and is listening at 127.0.0.1.
* If BackendHttpSetting specifies a port other than 80, the default site should be configured to listen at that port.
* The call to http://127.0.0.1:port should return an HTTP result code of 200. This should be returned within the 30 sec time-out period.
* Ensure that port configured is open and that there are no firewall rules or Azure Network Security Groups, which block incoming or outgoing traffic on the port configured.
* If Azure classic VMs or Cloud Service is used with FQDN or Public IP, ensure that the corresponding [endpoint](/documentation/articles/virtual-machines-windows-classic-setup-endpoints/) is opened.
* If the VM is configured via Azure Resource Manager and is outside the VNet where Application Gateway is deployed, [Network Security Group](/documentation/articles/virtual-networks-nsg/) must be configured to allow access on the desired port.

## Problems with custom health probe

### Cause

Custom health probes allow additional flexibility to the default probing behavior. When using custom probes, users can configure the probe interval, the URL, and path to test, and how many failed responses to accept before marking the back-end pool instance as unhealthy. The following additional properties are added.

| Probe property | Description |
| --- | --- |
| Name |Name of the probe. This name is used to refer to the probe in back-end HTTP settings. |
| Protocol |Protocol used to send the probe. The probe will use the protocol defined in the back-end HTTP settings |
| Host |Host name to send the probe. Applicable only when multi-site is configured on Application Gateway. This is different from VM host name. |
| Path |Relative path of the probe. The valid path starts from '/'. The probe is sent to \<protocol\>://\<host\>:\<port\>\<path\> |
| Interval |Probe interval in seconds. This is the time interval between two consecutive probes. |
| Time-out |Probe time-out in seconds. The probe is marked as failed if a valid response is not received within this time-out period. |
| Unhealthy threshold |Probe retry count. The back-end server is marked down after the consecutive probe failure count reaches the unhealthy threshold. |

### Solution

Validate that the Custom Health Probe is configured correctly as the preceding table. In addition to the preceding troubleshooting steps, also ensure the following:

* Ensure that the probe is correctly specified as per the [guide](/documentation/articles/application-gateway-create-probe-ps/).
* If Application Gateway is configured for a single site, by default the Host name should be specified as '127.0.0.1', unless otherwise configured in custom probe.
* Ensure that a call to http://\<host\>:\<port\>\<path\> returns an HTTP result code of 200.
* Ensure that Interval, Time-out and UnhealtyThreshold are within the acceptable ranges.

## Request time out

### Cause

When a user request is received, Application Gateway applies the configured rules to the request and routes it to a back-end pool instance. It waits for a configurable interval of time for a response from the back-end instance. By default, this interval is **30 seconds**. If Application Gateway does not receive a response from back-end application in this interval, user request would see a 502 error.

### Solution

Application Gateway allows users to configure this setting via BackendHttpSetting, which can be then applied to different pools. Different back-end pools can have different BackendHttpSetting and hence different request time out configured.

    New-AzureRmApplicationGatewayBackendHttpSettings -Name 'Setting01' -Port 80 -Protocol Http -CookieBasedAffinity Enabled -RequestTimeout 60

## Next steps

If the preceding steps do not resolve the issue, open a [support ticket](/support/contact/).

