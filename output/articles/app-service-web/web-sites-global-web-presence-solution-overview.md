<properties 
	pageTitle="Create a global web presence on Azure Web Apps" 
	description="This guide provides a technical overview of how to host your organization's (.COM) site on Azure Web Apps. This includes deployment, custom domains, SSL, and monitoring." 
	editor="jimbe" 
	manager="wpickett" 
	authors="cephalin" 
	services="app-service\web" 
	documentationCenter=""/>

<tags
	ms.service="app-service-web"
	ms.date="02/26/2016"
	wacn.date=""/>


# Create a global web presence on Azure Web Apps

[Azure Web App](/documentation/services/web-sites/) Web Apps has all the capabilities you need to establish a global web presence for your .COM site. Regardless of the size of your organization, you need a robust, secure, and scalable platform to drive your business , your brand awareness, and your customer communications. Azure Web Apps can help maintain your corporate brand and identity with Microsoft backed business continuity.


Below is an example of a .COM website running on Azure Web Apps. It demonstrates what you can do simply by composing Web Apps together with other services with minimal technical investments. **Click on an element in the topography to read more about it.** 

<div style="display:none">
![](./media/web-sites-global-web-presence-solution-overview/corp-website-visio.svg)
</div>

<object type="image/svg+xml" data="./media/web-sites-global-web-presence-solution-overview/corp-website-visio.svg" width="100%" height="100%"></object>


> [AZURE.NOTE]
> This guide presents some of the most common areas and tasks that are aligned with running a public-facing .COM site in Azure Web Apps. However, there are other common solutions that you can implement in Azure Web Apps. To review these solutions, see the other guides on [Digital Marketing Campaigns](/documentation/articles/web-sites-digital-marketing-application-solution-overview/) and [Business Applications](/documentation/articles/web-sites-business-application-solution-overview/).

## Create from scratch or bring existing assets

Quickly create new sites from a popular CMS in the gallery or bring your existing web assets to Azure Web Apps from a variety of languages and frameworks.

You can create a web app using your favorite CMS flavor. You can choose from various database backends to meet your needs, including [Azure SQL Database] and [MySQL].

Your existing web assets can run on Azure Web Apps, whether they are .NET, PHP, Java, Node.js, or Python. You can move them to Web Apps using your familiar [FTP] tools or your source control management system. Web Apps supports direct publishing from popular source control options, such as [Visual Studio] and [Git] - local, GitHub, BitBucket, DropBox, Mercurial, etc..

## Publish reliably

Publish your website reliably by continuously publishing directly from your existing source control system and live-testing your content. 

During the planning, prototyping, and early development of a site, you can look at real working versions of the website before it goes live by [deploying to a staging slot] of your site on Azure Web Apps. By integrating source control with Web Apps, you can [continuously publish] to a staging slot, and swap it into production with no downtime when you are ready to do so. If anything goes wrong on the production site, you can also swap it out for a previous version of your site immediately. 

Also, when planning changes to a live website, you can easily [run A/B tests] on the proposed updates using the Test in Production feature in and analyze real user behavior to help you make informed decisions on site design.

## Brand and secure

Use the Azure Web Apps domain for free or map to your registered domain name, then secure your brand with your CA-signed SSL certificate.

The **\*.chinacloudsites.cn** domain is complimentary when you run your website on Web Apps. Or, you can map your website to a [custom domain] - e.g. contoso.com - which you obtained from any DNS registry, such as GoDaddy.

If you collect any user information, perform ecommerce, or manage any other sensitive data, you can protect your brand reputation and your customers with [HTTPS]. The **\*.chinacloudsites.cn** domain name already comes with an SSL certificate, and if you use your custom domain, you can bring your SSL certificate for it to Web Apps. There is a monthly charge (prorated hourly) associated with each SSL certificate. For more information, see [Azure Web App Pricing Details].

## Go global

Go global by serving regional sites with Azure Traffic Manager and delivering content lightning fast with Azure CDN.

To serve global customers in their respective regions, use [Azure Traffic Manager] to route site visitors to a regional site that provides the best performance. Alternatively, you can spread the site load evenly across multiple copies of your website hosted in multiple regions.

Deliver your static content lightning fast to users globally by [integrating your web app with Azure CDN]. Azure CDN caches static content in the [CDN node] closest to the user, which minimizes latency and connections to your website.

## Optimize

Optimize your .COM site by scaling automatically with Autoscale, caching with Azure Redis Cache, running background tasks with WebJobs, and maintaining high availability with Azure Traffic Manager.

The ability of Azure Web Apps to [scale up and out] meets the need of your .COM site, regardless of the size of your workload. Scale out your website manually through the [Azure Classic Management Portal](https://manage.windowsazure.cn), programmatically through the [Service Management API] or [PowerShell scripting], or automatically through the Autoscale feature. In **Standard** hosting plan, Autoscale enables you to scale out a website automatically based on CPU utilization. For best practices, see [Troy Hunt]'s [10 things I learned about rapidly scaling web apps with Azure].

Make your website more responsive with the [Azure Redis Cache]. Use it to cache data from backend databases and other things such as the [ASP.NET session state] and [output cache].

Maintain high availability of your website using [Azure Traffic Manager]. Using the **Failover** method, Traffic Manager automatically routes traffic to a secondary site if there is a problem on the primary site.

## Monitor and analyze

Stay up-to-date on your website's performance with Azure or third-party tools. Receive alerts on critical website events. Gain user insight easily with Application Insight or with web log analytics from HDInsight. 

Get a [quick glance] of the website's current performance metrics and resource quotas in the web app's blade in the [Azure Classic Management Portal](https://manage.windowsazure.cn). For a 360° view of your application across availability, performance and usage, use [Azure Application Insights] to give you fast & powerful troubleshooting, diagnostics and usage insights. Or, use a third-party tool to provide advanced monitoring data for your websites.

In the **Standard** hosting plan, monitor site responsiveness receive email notifications whenever your site becomes unresponsive. For more information, see [How to: Receive Alert Notifications and Manage Alert Rules in Azure].

## Use rich media and reach all devices

Make your .COM site attractive with rich media, such as:

-  Upload and stream videos globally with [Azure Media Services]

## More Resources

- [Azure Web Apps Documentation](/home/features/web-site/)
- [Azure Web Blog](/blog/tags/网站/)

[AZURE.INCLUDE [app-service-web-whats-changed](../includes/app-service-web-whats-changed.md)]


[Azure Web App]: /home/features/web-site/

[MySQL]: /documentation/articles/web-sites-php-mysql-deploy-use-git/
[Azure SQL Database]: /documentation/articles/web-sites-dotnet-deploy-aspnet-mvc-app-membership-oauth-sql-database/
[FTP]: /documentation/articles/web-sites-deploy/#ftp
[Visual Studio]: /documentation/articles/web-sites-dotnet-get-started/
[Git]: /documentation/articles/web-sites-publish-source-control/

[deploying to a staging slot]: /documentation/articles/web-sites-staged-publishing/
[continuously publish]:http://rickrainey.com/2014/01/21/continuous-deployment-github-with-azure-web-sites-and-staged-publishing/
[run A/B tests]:http://blogs.msdn.com/b/tomholl/archive/2014/11/10/a-b-testing-with-azure-websites.aspx

[custom domain]: /documentation/articles/web-sites-custom-domain-name/
[HTTPS]: /documentation/articles/web-sites-configure-ssl-certificate/
[Azure Web App Pricing Details]: /home/features/web-site/pricing/

[Azure Traffic Manager]:http://www.hanselman.com/blog/CloudPowerHowToScaleAzureWebsitesGloballyWithTrafficManager.aspx
[integrating your web app with Azure CDN]: /documentation/articles/cdn-websites-with-cdn/
[CDN node]:https://msdn.microsoft.com/zh-cn/library/azure/gg680302.aspx

[scale up and out]: /documentation/articles/web-sites-scale/
[Azure Classic Management Portal]:http://manage.windowsazure.cn/
[Service Management API]:https://msdn.microsoft.com/zh-cn/library/azure/ee460799.aspx
[PowerShell scripting]:https://msdn.microsoft.com/zh-cn/library/azure/jj152841.aspx
[Troy Hunt]:https://twitter.com/troyhunt
[10 things I learned about rapidly scaling web apps with Azure]:http://www.troyhunt.com/2014/09/10-things-i-learned-about-rapidly.html
[Azure Redis Cache]:/blog/2014/06/05/mvc-movie-app-with-azure-redis-cache-in-15-minutes/
[ASP.NET session state]:https://msdn.microsoft.com/zh-cn/library/azure/dn690522.aspx
[output cache]:https://msdn.microsoft.com/zh-cn/library/azure/dn798898.aspx

[quick glance]: /documentation/articles/web-sites-monitor/
[Azure Application Insights]:http://blogs.msdn.com/b/visualstudioalm/archive/2015/01/07/application-insights-and-azure-websites.aspx
[How to: Receive Alert Notifications and Manage Alert Rules in Azure]:http://msdn.microsoft.com/zh-cn/library/azure/dn306638.aspx

[Azure Media Services]:http://blogs.technet.com/b/cbernier/archive/2013/09/03/windows-azure-media-services-and-web-sites.aspx

 
