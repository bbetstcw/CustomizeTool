<properties
    pageTitle="Azure Resource Manager support for Traffic Manager | Azure"
    description="Using PowerShell for Traffic Manager with Azure Resource Manager"
    services="traffic-manager"
    documentationcenter="na"
    author="sdwheeler"
    manager="carmonm" />
<tags
    ms.assetid="bc247448-1d2e-4104-ac03-42b59ebde065"
    ms.service="traffic-manager"
    ms.devlang="na"
    ms.topic="article"
    ms.tgt_pltfrm="na"
    ms.workload="infrastructure-services"
    ms.date="10/11/2016"
    wacn.date=""
    ms.author="sewhee" />

# Azure Resource Manager support for Azure Traffic Manager

Azure Resource Manager is the preferred management interface for services in Azure. Azure Traffic Manager profiles can be managed using Azure Resource Manager-based APIs and tools.

## Resource model

Azure Traffic Manager is configured using a collection of settings called a Traffic Manager profile. This profile contains DNS settings, traffic routing settings, endpoint monitoring settings, and a list of service endpoints to which traffic is routed.

Each Traffic Manager profile is represented by a resource of type 'TrafficManagerProfiles'. At the REST API level, the URI for each profile is as follows:

    https://management.chinacloudapi.cn/subscriptions/{subscription-id}/resourceGroups/{resource-group-name}/providers/Microsoft.Network/trafficManagerProfiles/{profile-name}?api-version={api-version}

## Comparison with the Azure Traffic Manager classic API

The Azure Resource Manager support for Traffic Manager uses different terminology than the classic deployment model. The following table shows the differences between the Resource Manager and Classic terms:

| Resource Manager term | Classic term |
| --- | --- |
| Traffic-routing method |Load-balancing method |
| Priority method |Failover method |
| Weighted method |Round-robin method |
| Performance method |Performance method |

Based on customer feedback, we changed the terminology to improve clarity and reduce common misunderstandings. There is no difference in functionality.

## Limitations

When referencing an endpoint of type 'AzureEndpoints' for a Web App, Traffic Manager endpoints can only reference the default (production) [Web App slot](/documentation/articles/web-sites-staged-publishing/). Custom slots are not supported. As a workaround, custom slots can be configured using the 'ExternalEndpoints' type.

## Setting up Azure PowerShell

These instructions use Azure PowerShell. The following article explains how to install and configure Azure PowerShell.

* [How to install and configure Azure PowerShell](/documentation/articles/powershell-install-configure/)

The examples in this article assume that you have an existing resource group. You can create a resource group using the following command:

    New-AzureRmResourceGroup -Name MyRG -Location "China North"

> [AZURE.NOTE]
> Azure Resource Manager requires that all resource groups have a location. This location is used as the default for resources created in that resource group. However, since Traffic Manager profile resources are global, not regional, the choice of resource group location has no impact on Azure Traffic Manager.

## Create a Traffic Manager Profile

To create a Traffic Manager profile, use the `New-AzureRmTrafficManagerProfile` cmdlet:

    $profile = New-AzureRmTrafficManagerProfile -Name MyProfile -ResourceGroupName MyRG -TrafficRoutingMethod Performance -RelativeDnsName contoso -Ttl 30 -MonitorProtocol HTTP -MonitorPort 80 -MonitorPath "/"

The following table describes the parameters:

| Parameter | Description |
| --- | --- |
| Name |The resource name for the Traffic Manager profile resource. Profiles in the same resource group must have unique names. This name is separate from the DNS name used for DNS queries. |
| ResourceGroupName |The name of the resource group containing the profile resource. |
| TrafficRoutingMethod |Specifies the traffic-routing method used to determine which endpoint is returned in response a DNS query. Possible values are 'Performance', 'Weighted' or 'Priority'. |
| RelativeDnsName |Specifies the hostname portion of the DNS name provided by this Traffic Manager profile. This value is combined with the DNS domain name used by Azure Traffic Manager to form the fully qualified domain name (FQDN) of the profile. For example, setting the value of 'contoso' becomes 'contoso.trafficmanager.cn.' |
| TTL |Specifies the DNS Time-to-Live (TTL), in seconds. This TTL informs the Local DNS resolvers and DNS clients how long to cache DNS responses for this Traffic Manager profile. |
| MonitorProtocol |Specifies the protocol to use to monitor endpoint health. Possible values are 'HTTP' and 'HTTPS'. |
| MonitorPort |Specifies the TCP port used to monitor endpoint health. |
| MonitorPath |Specifies the path relative to the endpoint domain name used to probe for endpoint health. |

The cmdlet creates a Traffic Manager profile in Azure and returns a corresponding profile object to PowerShell. At this point, the profile does not contain any endpoints. For more information about adding endpoints to a Traffic Manager profile, see [Adding Traffic Manager Endpoints](#adding-traffic-manager-endpoints).

## Get a Traffic Manager Profile

To retrieve an existing Traffic Manager profile object, use the `Get-AzureRmTrafficManagerProfle` cmdlet:

    $profile = Get-AzureRmTrafficManagerProfile -Name MyProfile -ResourceGroupName MyRG

This cmdlet returns a Traffic Manager profile object.

## Update a Traffic Manager Profile

Modifying Traffic Manager profiles follows a 3-step process:

1. Retrieve the profile using `Get-AzureRmTrafficManagerProfile` or use the profile returned by `New-AzureRmTrafficManagerProfile`.
2. Modify the profile. You can add and remove endpoints or change endpoint or profile parameters. These changes are off-line operations. You are only changing the local object in memory that represents the profile.
3. Commit your changes using the `Set-AzureRmTrafficManagerProfile` cmdlet.

All profile properties can be changed except the profile's RelativeDnsName. To change the RelativeDnsName, you must delete profile and a new profile with a new name.

The following example demonstrates how to change the profile's TTL:

    $profile = Get-AzureRmTrafficManagerProfile -Name MyProfile -ResourceGroupName MyRG
    $profile.Ttl = 300
    Set-AzureRmTrafficManagerProfile -TrafficManagerProfile $profile

There are three types of Traffic Manager endpoints:

1. **Azure endpoints** are services hosted in Azure
2. **External endpoints** are services hosted outside of Azure
3. **Nested endpoints** are used to construct nested hierarchies of Traffic Manager profiles. Nested endpoints enable advanced traffic-routing configurations for complex applications.

In all three cases, endpoints can be added in two ways:

1. Using a 3-step process described previously. The advantage of this method is that several endpoint changes can be made in a single update.
2. Using the New-AzureRmTrafficManagerEndpoint cmdlet. This cmdlet adds an endpoint to an existing Traffic Manager profile in a single operation.

## <a name="adding-traffic-manager-endpoints"></a> Adding Azure Endpoints

Azure endpoints reference services hosted in Azure. Three types of Azure endpoints are supported:

1. Azure Web Apps
2. 'Classic' cloud services (which can contain either a PaaS service or IaaS virtual machines)
3. Azure PublicIpAddress resources (which can be attached to a load-balancer or a virtual machine NIC). The PublicIpAddress must have a DNS name assigned to be used in Traffic Manager.

In each case:

* The service is specified using the 'targetResourceId' parameter of `Add-AzureRmTrafficManagerEndpointConfig` or `New-AzureRmTrafficManagerEndpoint`.
* The 'Target' and 'EndpointLocation' are implied by the TargetResourceId.
* Specifying the 'Weight' is optional. Weights are only used if the profile is configured to use the 'Weighted' traffic-routing method. Otherwise, they are ignored. If specified, the value must be a number between 1 and 1000. The default value is '1'.
* Specifying the 'Priority' is optional. Priorities are only used if the profile is configured to use the 'Priority' traffic-routing method. Otherwise, they are ignored. Valid values are from 1 to 1000 with lower values indicating a higher priority. If specified for one endpoint, they must be specified for all endpoints. If omitted, default values starting from '1' are applied in the order that the endpoints are listed.

### Example 1: Adding Web App endpoints using `Add-AzureRmTrafficManagerEndpointConfig`

In this example, we create a Traffic Manager profile and add two Web App endpoints using the `Add-AzureRmTrafficManagerEndpointConfig` cmdlet.

    $profile = New-AzureRmTrafficManagerProfile -Name myprofile -ResourceGroupName MyRG -TrafficRoutingMethod Performance -RelativeDnsName myapp -Ttl 30 -MonitorProtocol HTTP -MonitorPort 80 -MonitorPath "/"
    $webapp1 = Get-AzureRMWebApp -Name webapp1
    Add-AzureRmTrafficManagerEndpointConfig -EndpointName webapp1ep -TrafficManagerProfile $profile -Type AzureEndpoints -TargetResourceId $webapp1.Id -EndpointStatus Enabled
    $webapp2 = Get-AzureRMWebApp -Name webapp2
    Add-AzureRmTrafficManagerEndpointConfig -EndpointName webapp2ep -TrafficManagerProfile $profile -Type AzureEndpoints -TargetResourceId $webapp2.Id -EndpointStatus Enabled
    Set-AzureRmTrafficManagerProfile -TrafficManagerProfile $profile

### Example 2: Adding a 'classic' cloud service endpoint using `New-AzureRmTrafficManagerEndpoint`

In this example, a 'classic' Cloud Service endpoint is added to a Traffic Manager profile. In this example, we specified the profile using the profile and resource group names, rather than passing a profile object. Both approaches are supported.

    $cloudService = Get-AzureRmResource -ResourceName MyCloudService -ResourceType "Microsoft.ClassicCompute/domainNames" -ResourceGroupName MyCloudService
    New-AzureRmTrafficManagerEndpoint -Name MyCloudServiceEndpoint -ProfileName MyProfile -ResourceGroupName MyRG -Type AzureEndpoints -TargetResourceId $cloudService.Id -EndpointStatus Enabled

### Example 3: Adding a publicIpAddress endpoint using `New-AzureRmTrafficManagerEndpoint`

In this example, a public IP address resource is added to the Traffic Manager profile. The public IP address must have a DNS name configured, and can be bound either to the NIC of a VM or to a load balancer.

    $ip = Get-AzureRmPublicIpAddress -Name MyPublicIP -ResourceGroupName MyRG
    New-AzureRmTrafficManagerEndpoint -Name MyIpEndpoint -ProfileName MyProfile -ResourceGroupName MyRG -Type AzureEndpoints -TargetResourceId $ip.Id -EndpointStatus Enabled

## Adding External Endpoints

Traffic Manager uses external endpoints to direct traffic to services hosted outside of Azure. As with Azure endpoints, external endpoints can be added either using `Add-AzureRmTrafficManagerEndpointConfig` followed by `Set-AzureRmTrafficManagerProfile`, or `New-AzureRMTrafficManagerEndpoint`.

When specifying external endpoints:

* The endpoint domain name must be specified using the 'Target' parameter
* If the 'Performance' traffic-routing method is used, the 'EndpointLocation' is required. Otherwise it is optional. The value must be a [valid Azure region name](https://azure.microsoft.com/regions/).
* The 'Weight' and 'Priority' are optional.

### Example 1: Adding external endpoints using `Add-AzureRmTrafficManagerEndpointConfig` and `Set-AzureRmTrafficManagerProfile`

In this example, we create a Traffic Manager profile, add two external endpoints, and commit the changes.

    $profile = New-AzureRmTrafficManagerProfile -Name myprofile -ResourceGroupName MyRG -TrafficRoutingMethod Performance -RelativeDnsName myapp -Ttl 30 -MonitorProtocol HTTP -MonitorPort 80 -MonitorPath "/"
    Add-AzureRmTrafficManagerEndpointConfig -EndpointName eu-endpoint -TrafficManagerProfile $profile -Type ExternalEndpoints -Target app-eu.contoso.com -EndpointStatus Enabled
    Add-AzureRmTrafficManagerEndpointConfig -EndpointName us-endpoint -TrafficManagerProfile $profile -Type ExternalEndpoints -Target app-us.contoso.com -EndpointStatus Enabled
    Set-AzureRmTrafficManagerProfile -TrafficManagerProfile $profile

### Example 2: Adding external endpoints using `New-AzureRmTrafficManagerEndpoint`

In this example, we add an external endpoint to an existing profile. The profile is specified using the profile and resource group names.

    New-AzureRmTrafficManagerEndpoint -Name eu-endpoint -ProfileName MyProfile -ResourceGroupName MyRG -Type ExternalEndpoints -Target app-eu.contoso.com -EndpointStatus Enabled

## Adding 'Nested' endpoints

Each Traffic Manager profile specifies a single traffic-routing method. However, there are scenarios that require more sophisticated traffic routing than the routing provided by a single Traffic Manager profile. You can nest Traffic Manager profiles to combine the benefits of more than one traffic-routing method. Nested profiles allow you to override the default Traffic Manager behavior to support larger and more complex application deployments. For more detailed examples, see [Nested Traffic Manager profiles](/documentation/articles/traffic-manager-nested-profiles/).

Nested endpoints are configured at the parent profile, using a specific endpoint type, 'NestedEndpoints'. When specifying nested endpoints:

* The endpoint must be specified using the 'targetResourceId' parameter
* If the 'Performance' traffic-routing method is used, the 'EndpointLocation' is required. Otherwise it is optional. The value must be a [valid Azure region name](http://azure.microsoft.com/regions/).
* The 'Weight' and 'Priority' are optional, as for Azure endpoints.
* The 'MinChildEndpoints' parameter is optional. The default value is '1'. If the number of available endpoints falls below this threshold, the parent profile considers the child profile 'degraded' and diverts traffic to the other endpoints in the parent profile.

### Example 1: Adding nested endpoints using `Add-AzureRmTrafficManagerEndpointConfig` and `Set-AzureRmTrafficManagerProfile`

In this example, we create new Traffic Manager child and parent profiles, add the child as a nested endpoint to the parent, and commit the changes.

    $child = New-AzureRmTrafficManagerProfile -Name child -ResourceGroupName MyRG -TrafficRoutingMethod Priority -RelativeDnsName child -Ttl 30 -MonitorProtocol HTTP -MonitorPort 80 -MonitorPath "/"
    $parent = New-AzureRmTrafficManagerProfile -Name parent -ResourceGroupName MyRG -TrafficRoutingMethod Performance -RelativeDnsName parent -Ttl 30 -MonitorProtocol HTTP -MonitorPort 80 -MonitorPath "/"
    Add-AzureRmTrafficManagerEndpointConfig -EndpointName child-endpoint -TrafficManagerProfile $parent -Type NestedEndpoints -TargetResourceId $child.Id -EndpointStatus Enabled -EndpointLocation "China North" -MinChildEndpoints 2
    Set-AzureRmTrafficManagerProfile -TrafficManagerProfile $profile

For brevity in this example, we did not add any other endpoints to the child or parent profiles.

### Example 2: Adding nested endpoints using `New-AzureRmTrafficManagerEndpoint`

In this example, we add an existing child profile as a nested endpoint to an existing parent profile. The profile is specified using the profile and resource group names.

    $child = Get-AzureRmTrafficManagerEndpoint -Name child -ResourceGroupName MyRG
    New-AzureRmTrafficManagerEndpoint -Name child-endpoint -ProfileName parent -ResourceGroupName MyRG -Type NestedEndpoints -TargetResourceId $child.Id -EndpointStatus Enabled -EndpointLocation "China North" -MinChildEndpoints 2

## Update a Traffic Manager Endpoint

There are two ways to update an existing Traffic Manager endpoint:

1. Get the Traffic Manager profile using `Get-AzureRmTrafficManagerProfile`, update the endpoint properties within the profile, and commit the changes using `Set-AzureRmTrafficManagerProfile`. This method has the advantage of being able to update more than one endpoint in a single operation.
2. Get the Traffic Manager endpoint using `Get-AzureRmTrafficManagerEndpoint`, update the endpoint properties, and commit the changes using `Set-AzureRmTrafficManagerEndpoint`. This method is simpler, since it does not require indexing into the Endpoints array in the profile.

### Example 1: Updating endpoints using `Get-AzureRmTrafficManagerProfile` and `Set-AzureRmTrafficManagerProfile`

In this example, we modify the priority on two endpoints within an existing profile.

    $profile = Get-AzureRmTrafficManagerProfile -Name myprofile -ResourceGroupName MyRG
    $profile.Endpoints[0].Priority = 2
    $profile.Endpoints[1].Priority = 1
    Set-AzureRmTrafficManagerProfile -TrafficManagerProfile $profile

### Example 2: Updating an endpoint using `Get-AzureRmTrafficManagerEndpoint` and `Set-AzureRmTrafficManagerEndpoint`

In this example, we modify the weight of a single endpoint in an existing profile.

    $endpoint = Get-AzureRmTrafficManagerEndpoint -Name myendpoint -ProfileName myprofile -ResourceGroupName MyRG -Type ExternalEndpoints
    $endpoint.Weight = 20
    Set-AzureRmTrafficManagerEndpoint -TrafficManagerEndpoint $endpoint

## Enabling and Disabling Endpoints and Profiles

Traffic Manager allows individual endpoints to be enabled and disabled, as well as allowing enabling and disabling of entire profiles.
These changes can be made by getting/updating/setting the endpoint or profile resources. To streamline these common operations, they are also supported via dedicated cmdlets.

### Example 1: Enabling and disabling a Traffic Manager profile

To enable a Traffic Manager profile, use `Enable-AzureRmTrafficManagerProfile`. The profile can be specified using a profile object. The profile object can be passed via the pipeline or by using the '-TrafficManagerProfile' parameter. In this example, we specify the profile by the profile and resource group name.

    Enable-AzureRmTrafficManagerProfile -Name MyProfile -ResourceGroupName MyResourceGroup

To disable a Traffic Manager profile:

    Disable-AzureRmTrafficManagerProfile -Name MyProfile -ResourceGroupName MyResourceGroup

The Disable-AzureRmTrafficManagerProfile cmdlet prompts for confirmation. This prompt can be suppressed using the '-Force' parameter.

### Example 2: Enabling and disabling a Traffic Manager endpoint

To enable a Traffic Manager endpoint, use `Enable-AzureRmTrafficManagerEndpoint`. There are two ways to specify the endpoint

1. Using a TrafficManagerEndpoint object passed via the pipeline or using the '-TrafficManagerEndpoint' parameter
2. Using the endpoint name, endpoint type, profile name, and resource group name:

    Enable-AzureRmTrafficManagerEndpoint -Name MyEndpoint -Type AzureEndpoints -ProfileName MyProfile -ResourceGroupName MyRG

Similarly, to disable a Traffic Manager endpoint:

    Disable-AzureRmTrafficManagerEndpoint -Name MyEndpoint -Type AzureEndpoints -ProfileName MyProfile -ResourceGroupName MyRG -Force

As with `Disable-AzureRmTrafficManagerProfile`, the `Disable-AzureRmTrafficManagerEndpoint` cmdlet prompts for confirmation. This prompt can be suppressed using the '-Force' parameter.

## Delete a Traffic Manager Endpoint

To remove individual endpoints, use the `Remove-AzureRmTrafficManagerEndpoint` cmdlet:

    Remove-AzureRmTrafficManagerEndpoint -Name MyEndpoint -Type AzureEndpoints -ProfileName MyProfile -ResourceGroupName MyRG

This cmdlet prompts for confirmation. This prompt can be suppressed using the '-Force' parameter.

## Delete a Traffic Manager Profile

To delete a Traffic Manager profile, use the `Remove-AzureRmTrafficManagerProfile` cmdlet, specifying the profile and resource group names:

    Remove-AzureRmTrafficManagerProfile -Name MyProfile -ResourceGroupName MyRG [-Force]

This cmdlet prompts for confirmation. This prompt can be suppressed using the '-Force' parameter.

The profile to be deleted can also be specified using a profile object:

    $profile = Get-AzureRmTrafficManagerProfile -Name MyProfile -ResourceGroupName MyRG
    Remove-AzureRmTrafficManagerProfile -TrafficManagerProfile $profile [-Force]

This sequence can also be piped:

    Get-AzureRmTrafficManagerProfile -Name MyProfile -ResourceGroupName MyRG | Remove-AzureRmTrafficManagerProfile [-Force]

## Next steps

[Traffic Manager monitoring](/documentation/articles/traffic-manager-monitoring/)

[Traffic Manager performance considerations](/documentation/articles/traffic-manager-performance-considerations/)
