<properties
	pageTitle="Configure a custom domain name in Azure Websites"
	description="Learn how to use a custom domain name with a web app in Azure Websites."
	services="app-service"
	documentationCenter=""
	authors="MikeWasson"
	manager="wpickett"
	editor="jimbe"/>

<tags
	ms.service="app-service"
	ms.date="09/16/2015"
	wacn.date=""/>

# Configure a custom domain name in Azure Websites

> [AZURE.SELECTOR]

- [Web Apps with External Domains](/documentation/articles/web-sites-custom-domain-name)
- [Web Apps with Traffic Manager](/documentation/articles/web-sites-traffic-manager-custom-domain-name)


When you create a web app, Azure assigns it to a subdomain of chinacloudsites.cn. For example, if your web app is named **contoso**, the URL is **contoso.chinacloudsites.cn**. Azure also assigns a virtual IP address.

For a production web app, you probably want users to see a custom domain name. This article explains how to reserve or configure a custom domain with [Azure Websites](/documentation/services/web-sites/). 

[AZURE.INCLUDE [websites-cloud-services-css-guided-walkthrough](../includes/websites-cloud-services-css-guided-walkthrough.md)]

[AZURE.INCLUDE [introfooter](../includes/custom-dns-web-site-intro-notes.md)]


In this article:

-   [Overview]
-   [DNS record types]
-   [Find the virtual IP address]
-   [Create the DNS records]
-   [Create an 揳wverify?record (A records only)](#awverify)
-   [Enable the domain name on your website]

## Overview

If you have a domain name already, or you want reserve domain from other domain registrars, here are the general steps to bring a custom domain name for web app:

1. Reserve your domain name. This article does not cover that process. There are many domain registrars to choose from. When you sign up, their site will walk you through the process.
1. Create DNS records that map the domain to your Azure web app.
1. Add the domain name inside the [Azure Management Portal](https://manage.windowsazure.cn/).

Within this basic outline, there are specific cases to consider:

- Mapping your root domain. The root domain is the domain that you reserved with the domain registrar. For example, **contoso.com**.
- Mapping a subdomain. For example, **blogs.contoso.com**.  You can map different subdomains to different web apps.
- Mapping a wildcard. For example, **\*.contoso.com**. A wildcard entry applies to all subdomains of your domain.

[AZURE.INCLUDE [modes](../includes/custom-dns-web-site-modes.md)]


## DNS record types

The Domain Name System (DNS) uses data records to map domain names into IP addresses. There are several types of DNS records. For web apps, you’ll create either an *A* record or a *CNAME* record.

- An A **(Address)** record maps a domain name to an IP address.
- A **CNAME (Canonical Name)** record maps a domain name to another domain name. DNS uses the second name to look up the address. Users still see the first domain name in their browser. For example, you could map contoso.com to *&lt;yourwebapp&gt;*.chinacloudsites.cn.

If the IP address changes, a CNAME entry is still valid, whereas an A record must be updated. However, some domain registrars do not allow CNAME records for the root domain or for wildcard domains. In that case, you must use an A record.

> [AZURE.NOTE] The IP address may change if you delete and recreate your web app, or change the web app mode back to free.


## Find the virtual IP address

Skip this step if you are creating a CNAME record. To create an A record, you need the virtual IP address of your web app. To get the IP address:


1.	In your browser, open the [Azure Management Portal](https://manage.windowsazure.cn).
2.	In the **Websites** tab, click the name of your site and select **Dashboard**.
3.	Select **Manage Domains** from the bottom of the page. (If this option is disabled, make sure you are using Shared, Basic, or Standard mode. For more information, see [How to scale websites](http://www.windowsazure.cn/documentation/articles/web-sites-scale/).) 

	![](./media/web-sites-custom-domain-name/dncmntask-cname-6.png)

4.	The IP address is listed toward the bottom of the dialog.

	![](./media/web-sites-custom-domain-name/ipaddress.png)

## Create the DNS records

Log in to your domain registrar and use their tool to add an A record or CNAME record. Every registrar’s web app is slightly different, but here are some general guidelines.

1.	Find the page for managing DNS records. Look for links or areas of the site labeled **Domain Name**, **DNS**, or **Name Server Management**. Often the link can be found be viewing your account information, and then looking for a link such as **My domains**.
2.	When you find the management page, look for a link that lets you add or edit DNS records. This might be listed as a **Zone file**, **DNS Records**, or as an **Advanced** configuration link.

The page might list A records and CNAME records separately, or else provide a drop-down to select the record type. Also, it might use other names for the record types, such as **IP Address record** instead of A record, or **Alias Record** instead of CNAME record.  Usually the registrar creates some records for you, so there may already be records for the root domain or common subdomains, such as **www**.

When you create or edit a record, the fields will let you map your domain name to an IP address (for A records) or another domain (for CNAME records). For a CNAME record, you will map *from* your custom domain *to* your chinacloudsites.cn subdomain.

In many registrar tools, you will just type the subdomain portion of your domain, not the entire domain name. Also, many tools use ‘@’ to mean the root domain. For example:

<table cellspacing="0" border="1">
  <tr>
    <th>Host</th>
    <th>Record type</th>
    <th>IP Address or URL</th>
  </tr>
  <tr>
    <td>@</td>
    <td>A (address)</td>
    <td>127.0.0.1</td>
  </tr>
  <tr>
    <td>www</td>
    <td>CNAME (alias)</td>
    <td>contoso.chinacloudsites.cn</td>
  </tr>
</table>

Assuming the custom domain name is ‘contoso.com’, this would create the following records:

- **contoso.com** mapped to 127.0.0.1.
- **www.contoso.com** mapped to **contoso.chinacloudsites.cn**.


<a name="awverify" />
## Create an awverify record (A records only)

If you create an A record, web app also requires a special CNAME record, which is used to verify that you own the domain you are attempting to use. This CNAME record must have the following form.

- *If the A record maps the root domain or a wildcard domain:* Create a CNAME record that maps from **awverify.&lt;yourdomain&gt;** to **awverify.&lt;yourwebappname&gt;.chinacloudsites.cn**.  For example, if the A record is for **contoso.com**, create a CNAME record for **awverify.contoso.com**.
- *If the A record maps a specific subdomain:* Create a CNAME record that maps from **awverify.&lt;subdomain&gt;** to **awverify.&lt;yourwebappname&gt;.chinacloudsites.cn**. For example, if the A record is for **blogs.contoso.com**, create a CNAME record for **awverify.blogs.contoso.com**.

Visitors to your web app will not see the awverify subdomain; it’s only for Azure to verify your domain.

## Enable the domain name on your web app

[AZURE.INCLUDE [modes](../includes/custom-dns-web-site-enable-on-web-site.md)]






<!-- Anchors. -->
[Overview]: #overview
[DNS record types]: #dns-record-types
[Find the virtual IP address]: #find-the-virtual-ip-address
[Create the DNS records]: #create-the-dns-records
[Enable the domain name on your web app]: #enable-the-domain-name-on-your-web-app

<!-- Images -->
[subdomain]: media/web-sites-custom-domain-name/azurewebsites-subdomain.png
 