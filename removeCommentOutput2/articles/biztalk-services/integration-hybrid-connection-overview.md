<properties
	pageTitle="Hybrid Connections overview | Windows Azure"
	description="Learn about Hybrid Connections, including security, TCP ports, and supported configurations. MABS, WABS."
	services="biztalk-services"
	documentationCenter=""
	authors="MandiOhlinger"
	manager="dwrede"
	editor="cgronlun"/>

<tags
	ms.service="biztalk-services"
	ms.date="12/01/2015"
	wacn.date=""/>



# Hybrid Connections Overview
This topic introduces Hybrid Connections, lists the supported configurations, and lists the required TCP Ports. Specifically:

- [What is a Hybrid Connection](#HCOverview)
- [Supported Configurations](#KnownIssues)
- [Security and Ports](#HCSecurity)

##<a name="HCOverview"></a>What is a Hybrid Connection

Hybrid Connections provides an easy and convenient way to connect Azure  Websites and Azure Mobile Services to on-premises resources. Hybrid Connections are a feature of Azure BizTalk Services:

![Hybrid Connections][HCImage]

Hybrid Connections benefits include:

-  Websites and Mobile Services can access existing on-premises data and services securely.
- Multiple  Websites or Mobile Services can share a Hybrid Connection to access an on-premises resource. 
- Minimal TCP Ports are required to access your network.
- Applications using Hybrid Connections access only the specific on-premises resource that is published through the Hybrid Connection.
- Can connect to any on-premises resource that uses a static TCP port, such as SQL Server, MySQL, HTTP Web APIs, and most custom Web Services.

	> [WACOM.NOTE] TCP-based services that use dynamic ports (such as FTP Passive Mode or Extended Passive Mode) are currently not supported.

- Can be used with all frameworks supported by Websites (.NET, PHP, Java, Python, Node.js) and Mobile Services (Node.js, .NET).
- web sites and Mobile Apps can access on-premises resources in exactly the same way as if the Web or Mobile App is located on your local network. For example, the same connection string used on-premises can also be used on Azure.


Hybrid Connections also provide Enterprise Administrators control and visibility into the corporate resources accessed by hybrid applications, including:

- Using Group Policy settings, administrators can allow Hybrid Connections on the network and also designate resources that can be accessed by hybrid applications.
- Event and audit logs on the corporate network provide visibility into the resources accessed by Hybrid Connections.


##<a name="KnownIssues"></a>Supported Configurations

Hybrid Connections support the following framework and application combinations:

- .NET framework access to SQL Server
- .NET framework access to HTTP/HTTPS services with WebClient
- PHP access to SQL Server, MySQL
- Java access to SQL Server, MySQL and Oracle
- Java access to HTTP/HTTPS services

When using Hybrid Connections to access on-premises SQL Server, consider the following:

- SQL Express Named Instances must be configured to use static ports. By default, SQL Express named instances use dynamic ports.
- SQL Express Default Instances use a static port, but TCP must be enabled. By default, TCP is not enabled.
- When using Clustering or Availability Groups, the `MultiSubnetFailover=true` mode is currently not supported.
- The `ApplicationIntent=ReadOnly` is currently not supported.
- SQL Authentication may be required as the end-to-end authorization method supported by the Azure application and the on-premises SQL server.


##<a name="HCSecurity"></a>Security and Ports

Hybrid Connections uses Shared Access Signature (SAS) authorization to secure the connections from the Azure applications and the on-premises Hybrid Connection Manager to the Hybrid Connection. Separate connection keys are created for the application and the on-premises Hybrid Connection Manager. These connection keys can be rolled over and revoked independently.

Hybrid Connections provides for seamless and secure distribution of the keys to the applications and the on-premises Hybrid Connection Manager.

See [Create and Manage Hybrid Connections](/documentation/articles/integration-hybrid-connection-create-manage).

**Application authorization is separate from the Hybrid Connection**. Any appropriate authorization method can be used. The authorization method depends on the end-to-end authorization methods supported across the Azure cloud and the on-premises components. For example, your Azure application accesses an on-premises SQL Server. In this scenario, SQL Authorization may be the authorization method that is supported end-to-end.

####TCP Ports
Hybrid Connections requires only outbound TCP or HTTP connectivity from your private network. You do not need to open any firewall ports or change your network perimeter configuration to allow any inbound connectivity into your network.

The following TCP ports are used by Hybrid Connections:

<table border="1">
    <tr>
       <th><strong>Port</strong></th>
        <th>Why</th>
    </tr>
    <tr>
        <td>80</td>
        <td>HTTP port; Used for certificate validation.</td>
    </tr>
    <tr>
        <td>443</td>
        <td>HTTPS port</td>
    </tr>
	<tr>
        <td>5671</td>
        <td>Used to connect to Azure. If TCP port 5671 is unavailable, TCP port 443 is used.</td>
	</tr>
	<tr>
        <td>9352</td>
        <td>Used to push and pull data. If TCP port 9352 is unavailable, TCP port 443 is used.</td>
	</tr>
</table>


## Next

- [Create and Manage Hybrid Connections](/documentation/articles/integration-hybrid-connection-create-manage)
- [Connect an Azure  Website to an On-Premises Resource](http://go.microsoft.com/fwlink/p/?LinkId=397538)
- [Hybrid Connections Step-by-Step: Connect to on-premises SQL Server from an Azure  Website](/documentation/articles/web-sites-hybrid-connection-connect-on-premises-sql-server/)
- [Azure Mobile Services and Hybrid Connections](/documentation/articles/mobile-services-dotnet-backend-hybrid-connections-get-started)


## See Also

- [REST API for Managing BizTalk Services on Windows Azure](http://msdn.microsoft.com/zh-cn/library/azure/dn232347.aspx)
- [BizTalk Services: Editions Chart](http://go.microsoft.com/fwlink/p/?LinkID=302279)<br/>
- [Create a BizTalk Service using Azure Management Portal](http://go.microsoft.com/fwlink/p/?LinkID=302280)<br/>
- [BizTalk Services: Dashboard, Monitor and Scale tabs](http://go.microsoft.com/fwlink/p/?LinkID=302281)<br/>

[HCImage]: ./media/integration-hybrid-connection-overview/WABS_HybridConnectionImage.png
[HybridConnectionTab]: ./media/integration-hybrid-connection-overview/WABS_HybridConnectionTab.png
[HCOnPremSetup]: ./media/integration-hybrid-connection-overview/WABS_HybridConnectionOnPremSetup.png
[HCManageConnection]: ./media/integration-hybrid-connection-overview/WABS_HybridConnectionManageConn.png
