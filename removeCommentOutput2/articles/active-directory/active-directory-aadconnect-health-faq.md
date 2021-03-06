<properties
	pageTitle="Azure AD Connect Health FAQ"
	description="This FAQ answers questions about Azure AD Connect Health. This FAQ covers questions about using the service, including the billing model, capabilities, limitations, and support."
	services="active-directory"
	documentationCenter=""
	authors="billmath"
	manager="stevenpo"
	editor="curtand"/>

<tags
	ms.service="active-directory"
	ms.date="10/15/2015"
	wacn.date=""/>


# Azure AD Connect Health Frequently Asked Questions (FAQ)

This FAQ answers questions about Azure AD Connect Health. This FAQ covers questions about using the service, including the billing model, capabilities, limitations, and support.

## General Questions



**Q: I manage multiple Azure AD directories. How do I switch to the one with Azure Active Directory Premium?**

You can switch between different Azure AD directories by selecting the currently signed in User Name on the top right corner and choosing the appropriate account. If the account is not listed here, select Sign out and then use the global admin credentials of the Directory that has Azure Active Directory Premium enabled to sign in.

## Installation Questions



**Q: What is the impact of installing the Azure AD Connect Health Agent on individual servers?**

The impact of installing the Microsoft Identity Health Agent on the ADFS servers is minimal with respect to the CPU, Memory consumption network bandwidth and storage.

The numbers below are an approximation.

- CPU consumption: ~1% increase
- Memory consumption: Up to 10 % of the total system memory
- Network Bandwidth Usage: ~1 MB / 1000 ADFS requests

>[AZURE.NOTE]In the event of the agent being unable to communicate to Azure, the agent will store the data locally, up to a defined maximum limit. Once the agent reaches the limit, if the agent has not been able to upload the data to the service, the new ADFS transactions will overwrite any “cached” transactions on a “least recently serviced” basis.

- Local buffer storage for AD Health Agent: ~20 MB
- Data Storage required for Audit Channel


It is recommended that you provision a disk space of 1024 MB (1 GB) for the AD FS Audit Channel for AD Health Agents to process all the data.

**Q: Will I have to reboot my servers during the installation of the Azure AD Connect Health Agents?**

No. The installation of the agents will not require you to reboot the server. However, installation of some of the prerequisite steps may require a reboot of the server.

For example, on Windows Server 2008 R2 the installation of .Net 4.5 Framework requires a server reboot.


**Q: Does Azure AD Connect Health Services work through a pass-through http proxy?**

Yes.  For on going operations, you can configure the Health Agent to forward outbound http requests using an HTTP Proxy. See [Configure Azure AD Connect Health Agents to use HTTP Proxy](/documentation/articles/active-directory-aadconnect-health-agent-install-adfs#configure-azure-ad-connect-health-agent-to-use-http-proxy) for more information.

If you need to configure a proxy during Agent registration, you need to modify your Internet Explorer Proxy settings. <br>
Open Internet Explorer -> Settings -> Internet Options -> Connections -> LAN Settings.<br>
Select Use a Proxy Server for you LAN.<br>
Select Advanced IF you have different proxy ports for HTTP and HTTPS/Secure.<br>


**Q: Does Azure AD Connect Health Services support basic authentication when connecting to Http Proxies?**

No. A mechanism for specifying arbitrary username/password for Basic Authentication is not currently supported.



## Operations Questions



**Q: Do I need to enable auditing on my AD FS Application Proxy Servers or my Web Application Proxy Servers?**

No, auditing does not need to be enabled on AD FS Application Proxy Servers or Web Application Proxy Servers. It only needs to be enabled on the AD FS federated servers.



**Q: How do Azure AD Connect Health Alerts get resolved?**

Azure AD Connect Health Alerts get resolved on a success condition. Azure AD Connect Health Agents detect and report the success conditions to the service on a periodic basis. For a few alerts, the suppression is time based. That is if the same error condition is not observed within 48 hours from alert generation, the alert is automatically resolved.




**Q: What firewall ports do I need to open for the Azure AD Connect Health Agent to work?**

You will need to have TCP/UDP ports 80, 443 and 5671 open for the Azure AD Connect Health Agent to be able to communicate with the Azure AD Health service endpoints.

## Related links

* [Azure AD Connect Health](/documentation/articles/active-directory-aadconnect-health)
* [Azure AD Connect Health Agent Installation](/documentation/articles/active-directory-aadconnect-health-agent-install)
* [Azure AD Connect Health Operations](/documentation/articles/active-directory-aadconnect-health-operations)
* [Using Azure AD Connect Health with AD FS](/documentation/articles/active-directory-aadconnect-health-adfs)
* [Using Azure AD Connect Health for sync](/documentation/articles/active-directory-aadconnect-health-sync)
