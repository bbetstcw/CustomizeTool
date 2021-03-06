<properties
	pageTitle="Create an ASP.NET web app in Azure Websites | Windows Azure"
	description="This tutorial shows you how to create an ASP.NET web project in Visual Studio 2013 and deploy it to a web app in Azure Websites."
	services="app-service\web"
	documentationCenter=".net"
	authors="tdykstra"
	manager="wpickett"
	editor="jimbe"/>

<tags
	ms.service="app-service-web"
	ms.date="08/10/2015"
	wacn.date=""/>

# Create an ASP.NET web app in Azure Websites

> [AZURE.SELECTOR]
- [.Net](/documentation/articles/web-sites-dotnet-get-started)
- [Node.js](/documentation/articles/web-sites-nodejs-develop-deploy-mac)
- [Java](/documentation/articles/web-sites-java-get-started)
- [PHP - Git](/documentation/articles/web-sites-php-mysql-deploy-use-git)
- [PHP - FTP](/documentation/articles/web-sites-php-mysql-deploy-use-ftp)
- [Python](/documentation/articles/web-sites-python-ptvs-django-mysql)

## Overview

This tutorial shows how to create an ASP.NET web application and deploy it to a [web app in Azure Websites](/home/features/web-site/) by using Visual Studio 2015 or Visual Studio 2013. The tutorial assumes that you have no prior experience with using Azure or ASP.NET. On completing the tutorial, you'll have a simple web application up and running in the cloud.

The following illustration shows the completed application:

![Web app home page](./media/web-sites-dotnet-get-started/deployedandazure.png)

You'll learn:

* How to prepare your machine for Azure development by installing the [Azure SDK for .NET](/documentation/articles/dotnet-sdk).
* How to set up Visual Studio to create a new Azure Websites web app while it creates an ASP.NET MVC 5 web project.
* How to deploy a web project to an Azure Websites web app by using Visual Studio.
* How to use the [Azure Management Portal](https://manage.windowsazure.cn) to monitor and manage your web app.

This is a quick and simple tutorial that doesn't show how to customize the web project that you create. For an introduction to ASP.NET MVC 5 web application development, see [Getting Started with ASP.NET MVC 5](http://www.asp.net/mvc/overview/getting-started/introduction/getting-started) on the [ASP.NET](http://asp.net/) site. For links to other articles that go into more depth about web applications in Azure Websites, see the [Next steps](#next-steps) section.

##<a name="video"></a>Sign up for Windows Azure

You need an Azure account to complete this tutorial. You can:

* [Open an Azure account for free](/pricing/1rmb-trial/?WT.mc_id=A261C142F). You get credits you that can use to try out paid Azure services. Even after the credits are used up, you can keep the account and use free Azure services and features, such as the Web Apps feature in Azure Websites.

[AZURE.INCLUDE [install-sdk-2015-2013](../includes/install-sdk-2015-2013.md)]

## Create a project and a web app

Your first step is to create a web project in Visual Studio and a web app in Azure Websites. When that's done, you'll deploy the project to the web app to make it available on the Internet. 

The diagram illustrates what you're doing in the create and deploy steps.

![Create and deploy](./media/web-sites-dotnet-get-started/Create_App.png)

1. Open Visual Studio 2015 or Visual Studio 2013.

	If you use Visual Studio 2013, the screens will be slightly different from the screenshots, but the procedures are essentially the same.

2. From the **File** menu, click **New > Project**.

3. In the **New Project** dialog box, click **C# > Web > ASP.NET Web Application**. If you prefer, you can choose **Visual Basic**.

3. Make sure that **.NET Framework 4.5.2** is selected as the target framework.

4. Name the application **MyExample**.

5. Click **OK**.

	![New Project dialog box](./media/web-sites-dotnet-get-started/GS13newprojdb.png)

5. In the **New ASP.NET Project** dialog box, select the **MVC** template.

	[MVC](http://www.asp.net/mvc) is an ASP.NET framework for developing web apps.

7. Click **Change Authentication**.

	![New ASP.NET Project dialog box](./media/web-sites-dotnet-get-started/GS13changeauth.png)

6. In the **Change Authentication** dialog box, click **No Authentication**, and then click **OK**.

	![No Authentication](./media/web-sites-dotnet-get-started/GS13noauth.png)

	The sample application that you're creating won't enable users to log in. The [Next steps](#next-steps) section links to a tutorial that implements authentication and authorization.

5. In the **New ASP.NET Project** dialog box, leave the settings under **Windows Azure** unchanged, and then click **OK**.

	![New ASP.NET Project dialog box](./media/web-sites-dotnet-get-started/GS13newaspnetprojdb.png)

	The default settings specify that Visual Studio will create an Azure web app for your web project. In the next section of the tutorial, you'll deploy the web project to the newly created web app.

5. If you haven't already signed in to Azure, Visual Studio prompts you to do so. Sign in with the ID and password of the account that you use to manage your Azure subscription.

	When you're signed in, the **Configure Windows Azure Web App Settings** dialog box asks you what resources you want to create.

	![Signed in to Azure](./media/web-sites-dotnet-get-started/configuresitesettings.png)
6. In the **Sign in to Azure** dialog box, enter the ID and password of the account that you use to manage your Azure subscription.
	
	When you're signed in, the **Configure Azure Site Settings** dialog box asks you what resources you want to create.

	![Signed in to Azure](./media/web-sites-dotnet-get-started/configuresitesettings.png)

3. Visual Studio provides a default **Site name**, which Azure will use as the prefix for your application's URL. If you prefer, enter a different site name.

	The complete URL will consist of what you enter here plus *chinacloudsites.cn* (as shown next to the **Site name** text box). For example, if the site name is `MyExample6442`, the URL will be `MyExample6442.chinacloudsites.cn`. The URL has to be unique. If someone else has already used the one you entered, you'll see a red exclamation mark to the right instead of a green check mark, and you'll need to enter a different site name.
5. In the **Region** drop-down list, choose the location that is closest to you.

	This setting specifies which Azure datacenter your web app will run in. For this tutorial, you can select any region and it won't make a noticeable difference. But for a production web app, you want your web server to be as close as possible to the browsers that are accessing your site in order to minimize [latency](http://www.bing.com/search?q=web%20latency%20introduction&qs=n&form=QBRE&pq=web%20latency%20introduction&sc=1-24&sp=-1&sk=&cvid=eefff99dfc864d25a75a83740f1e0090).

5. Leave the database field unchanged.

	For this tutorial, you aren't using a database. The [Next steps](#next-steps) section links to a tutorial that shows how to use a database.

6. Click **OK**.

	![](./media/web-sites-dotnet-get-started/configuresitesettings2.png)

	In a few seconds, Visual Studio creates the web project in the folder that you specified, and it creates the web app in the Azure region that you specified.  

	The **Solution Explorer** window shows the files and folders in the new project.

	![Solution Explorer](./media/web-sites-dotnet-get-started/solutionexplorer.png)

	The **Azure Websites Activity** window shows that the web app has been created.

	![Web app created](./media/web-sites-dotnet-get-started/GS13sitecreated1.png)

	And you can see the web app in **Server Explorer**.

	![Web app created](./media/web-sites-dotnet-get-started/siteinse.png)

## Deploy the project to the web app

In this section you deploy web project to the web app, as illustrated in step 2 of the diagram.

![Create and deploy](./media/web-sites-dotnet-get-started/Create_App.png)
7. In the **Azure Websites Activity** window, click **Publish MyExample to this Web App now**.

	![Web app created](./media/web-sites-dotnet-get-started/GS13sitecreated.png)

	In a few seconds, the **Publish Web** wizard appears.

	Settings that Visual Studio needs to deploy your project to Azure have been saved in a *publish profile*. You can use the wizard to review and change those settings.
8. On the **Connection** tab of the **Publish Web** wizard, click **Next**.

	![Successfully validated connection](./media/web-sites-dotnet-get-started/GS13ValidateConnection.png)

10. On the **Settings** tab, click **Next**.

	You can accept the default values for **Configuration** and **File Publish Options**.

	You can use the **Configuration** drop-down to deploy a Debug build for remote debugging. The [Next steps](#next-steps) section links to a tutorial that shows how to run Visual Studio in debug mode remotely.

	![Settings tab](./media/web-sites-dotnet-get-started/GS13SettingsTab.png)

11. On the **Preview** tab, click **Publish**.

	If you want to see what files will be copied to Azure, you can click **Start Preview** before clicking **Publish**.

	![](./media/web-sites-dotnet-get-started/GS13previewoutput.png)

	When you click **Publish** Visual Studio begins the process of copying the files to the Azure server.

	The **Output** and **Azure Websites Activity** windows show what deployment actions were taken and report successful completion of the deployment.

	![Output window reporting successful deployment](./media/web-sites-dotnet-get-started/PublishOutput.png)

	Upon successful deployment, the default browser automatically opens to the URL of the deployed web app, and the application that you created is now running in the cloud. The URL in the browser address bar shows that the web app is loaded from the Internet.

	![Web app running in Azure](./media/web-sites-dotnet-get-started/GS13deployedsite.png)

13. Close the browser.

**Tip:** You can enable the **Web One Click Publish** toolbar for quick deployment. Click **View > Toolbars**, and then select **Web One Click Publish**. You can use the toolbar to select a profile, click a button to publish, or click a button to open the **Publish Web** wizard.

![Web One Click Publish Toolbar](./media/web-sites-dotnet-get-started/weboneclickpublish.png)

## Monitor and manage the web app in the Azure Management Portal

The [Azure Management Portal](/home/features/management-portal/) is a web interface that you can use to manage and monitor your Azure services, such as the web app that you just created. In this section of the tutorial, you look at some of what you can do in the portal.

1. In your browser, go to [https://manage.windowsazure.cn](https://manage.windowsazure.cn), and sign in with your Azure credentials.
	The portal displays a list of your Azure services.

2. Click the name of your website.

	![Portal home page with new web site called out](./media/web-sites-dotnet-get-started-vs2013/portalhome.png)
  
3. Click the **Dashboard** tab.

	The **Dashboard** tab displays an overview of usage statistics and link for a number of commonly used site management functions. Under **Quick Glance** you'll also find a link to your application's home page.

	![Portal web site dashboard tab](./media/web-sites-dotnet-get-started-vs2013/portaldashboard.png)
  
	At this point your site hasn't had much traffic and may not show anything in the graph. If you browse to your application, refresh the page a few times, and then refresh the portal **Dashboard** page, you'll see some statistics show up. You can click the **Monitor** tab for more details.

4. Click the **Configure** tab.

	The [Configure](/documentation/articles/web-sites-configure//) tab enables you to control the .NET version used for the site, enable features such as [WebSockets](/blog/2013/11/14/introduction-to-websockets-on-windows-azure-web-sites/) and [diagnostic logging](/documentation/articles/web-sites-enable-diagnostic-log), set [connection string values](http://azure.microsoft.com/blog/2013/07/17/windows-azure-web-sites-how-application-strings-and-connection-strings-work/), and more. 

	![Portal web site configure tab](./media/web-sites-dotnet-get-started-vs2013/portalconfigure.png)
  
5. Click the **Scale** tab.

	For the paid tiers of the  Websites service, the [Scale](/documentation/articles/web-sites-scale) tab enables you to control the size and number of machines that service your web application in order to handle variations in traffic.

	You can scale manually or configure criteria or schedules for automatic scaling.

	![Portal website scale tab](./media/web-sites-dotnet-get-started-vs2013/portalscale.png)
These are just a few of the portal's features. You can create new web apps, delete existing web apps, stop and restart web apps, and manage other kinds of Azure services, such as databases and virtual machines.  

## Next steps

In this tutorial, you've seen how to create a simple web application and deploy it to an Azure web app. Here are some related topics and resources for learning more about web apps in Azure Websites:

* How to add database and authorization functionality

	For a tutorial that shows how to access a database and restrict some application functions to authorized users, see [Deploy a secure ASP.NET MVC app with membership, OAuth, and SQL Database to an Azure web app](/documentation/articles/web-sites-dotnet-deploy-aspnet-mvc-app-membership-oauth-sql-database/). That tutorial assumes some knowledge of MVC 5; if you are new to MVC 5, see [Getting Started with ASP.NET MVC 5](http://www.asp.net/mvc/overview/getting-started/introduction/getting-started).

* Other ways to deploy a web project

	For information about other ways to deploy web projects to web apps, by using Visual Studio or by [automating deployment](http://www.asp.net/aspnet/overview/developing-apps-with-windows-azure/building-real-world-cloud-apps-with-windows-azure/continuous-integration-and-continuous-delivery) from a [source control system](http://www.asp.net/aspnet/overview/developing-apps-with-windows-azure/building-real-world-cloud-apps-with-windows-azure/source-control), see [How to deploy an Azure web app](/documentation/articles/web-sites-deploy).

	Visual Studio can also generate Windows PowerShell scripts that you can use to automate deployment. For more information, see [Automate Everything (Building Real-World Cloud Apps with Azure)](http://www.asp.net/aspnet/overview/developing-apps-with-windows-azure/building-real-world-cloud-apps-with-windows-azure/automate-everything).

* How to troubleshoot a web app

	Visual Studio provides features that make it easy to view Azure logs as they are generated in real time. You can also run in debug mode remotely in Azure. For more information, see [Troubleshooting Azure web apps in Visual Studio](/documentation/articles/web-sites-dotnet-troubleshoot-visual-studio).

* How to add a custom domain name and SSL

	For information about how to use SSL and your own domain (for example, www.contoso.com instead of contoso.chinacloudsites.cn), see the following resources:

	* [Configure a custom domain name in Azure Websites](/documentation/articles/web-sites-custom-domain-name)
	* [Enable HTTPS for an Azure website](/documentation/articles/web-sites-configure-ssl-certificate)

* How to add real-time features such as chat

	If your web app will include real-time features (such as a chat service, a game, or a stock ticker), you can get the best performance by using [ASP.NET SignalR](http://www.asp.net/signalr) with the [WebSockets](/blog/2013/11/14/introduction-to-websockets-on-windows-azure-web-sites/) transport method. For more information, see [Using SignalR with Azure web apps](http://www.asp.net/signalr/overview/signalr-20/getting-started-with-signalr-20/using-signalr-with-windows-azure-web-sites).

* How to choose between Azure Websites, Azure Cloud Services, and Azure Virtual Machines for web applications

	In Azure, you can run web applications in Azure Websites as shown in this tutorial, or in Cloud Services or in Virtual Machines. For more information, see [Azure web apps, cloud services, and VMs: When to use which?](/documentation/articles/choose-web-site-cloud-service-vm/).

* [How to choose or create an App Service plan](/documentation/articles/azure-web-sites-web-hosting-plans-in-depth-overview)
