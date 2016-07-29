<properties
	pageTitle="Deploy your app to Azure Web App"
	description="Learn how to deploy your app to Azure Web App."
	services="app-service"
	documentationCenter=""
	authors="cephalin"
	manager="wpickett"
	editor="mollybos"/>

<tags
	ms.service="app-service"
	ms.date="04/21/2016"
	wacn.date=""/>
    
# Deploy your app to Azure

This article helps you determine the best option to deploy the files for your web app, mobile app backend, or API app to 
[Azure Web App](/documentation/services/web-sites/), and then guides you to appropriate resources with instructions specific to your preferred option.

## <a name="overview"></a>Azure Web App deployment overview

Azure Web App maintains the application framework for you (ASP.NET, PHP, Node.js, etc). Some frameworks are enabled by default
while others, like Java and Python, may need a simple checkmark configuration to enable it. In addition, you can customize your
application framework, such as the PHP version or the bitness of your runtime. For more information, see 
[Configure your app in Azure Web App](/documentation/articles/web-sites-configure/).

Since you don't have to worry about the web server or application framework, deploying your app to Azure Web App is 
a matter of deploying your code, binaries, content files, and their respective directory structure, to the 
**/site/wwwroot** directory in Azure (or the **/site/wwwroot/App_Data/Jobs/** directory 
for WebJobs). Azure Web App supports the following deployment options: 

- [FTP or FTPS](https://en.wikipedia.org/wiki/File_Transfer_Protocol): Use your favorite FTP or FTPS enabled tool to move your 
files to Azure, from [FileZilla](https://filezilla-project.org) to full-featured IDEs like [NetBeans](https://netbeans.org). This is strictly
a file upload process. No additional services are provided by Azure Web App, such as version control, file structure management, etc. 

- [Web Deploy](http://www.iis.net/learn/publish/using-web-deploy/introduction-to-web-deploy): Deploy code to Azure directly from your favorite Microsoft tools
such as Visual Studio using the same tooling that automates deployment to IIS servers. This tool supports diff-only deployment, database creation, transforms of 
connection strings, etc. For Web Deploy , application binaries are built before they are deployed to Azure.
Similar to FTP, no additional services are provided by Azure Web App.

Popular web development tools support one or more of these deployment processes. While the tool you choose determines the deployment 
processes you can leverage, the actual DevOps functionality at your disposal depends on the combination of the deployment process and the 
specific tools you choose. For example, if you perform Web Deploy from [Visual Studio with Azure SDK](#vspros), you get package restore and MSBuild automation in Visual Studio. Azure SDK also provides an easy-to-use wizard to 

>[AZURE.NOTE] These deployment processes don't actually [provision the Azure resources](/documentation/articles/resource-group-template-deploy-portal/) that your app may need. However, most of the linked how-to articles show you how to provision the app AND deploy
>[AZURE.NOTE] These deployment processes don't actually provision the Azure resources that your app may need, such as 
your code to it end-to-end. You can also find additional options for provisioning Azure resources in the 
[Automate deployment by using command-line tools](#automate) section.
     
## <a name="ftp"></a>Deploy via FTP by copying files to Azure manually
If you are used to manually copying your web content to a web server, you can use an [FTP](http://en.wikipedia.org/wiki/File_Transfer_Protocol) utility to copy files, such as Windows Explorer or 
[FileZilla](https://filezilla-project.org/).

The pros of copying files manually are:

- Familiarity and minimal complexity for FTP tooling. 
- Knowing exactly where your files are going.
- Added security with FTPS.

The cons of copying files manually are:

- Having to know how to deploy files to the correct directories in Azure Web App. 
- No version control for rollback when failures occur.
- No built-in deployment history for troubleshooting deployment issues.
- Potential long deployment times because many FTP tools don't provide diff-only copying and simply copy all the files.  

### <a name="howtoftp"></a>How to deploy by copying files to Azure manually
Copying files to Azure involves a few simple steps:

1. Assuming you already established deployment credentials, obtain the FTP connection information by going to **Settings** > **Properties**, and then
copying the values for **FTP/Deployment User**, **FTP Host Name**, and **FTPS Host Name**. Please copy the **FTP/Deployment User** user value as displayed by the Azure Classic Management Portal including the app name in order to provide proper context for the FTP server.
2. From your FTP client, use the connection information you gathered to connect to your app.
3. Copy your files and their respective directory structure to the 
**/site/wwwroot** directory in Azure (or the **/site/wwwroot/App_Data/Jobs/** directory 
for WebJobs).
4. Browse to your app's URL to verify the app is running properly. 

For more information, see the following resource:

* [Create a PHP-MySQL web app and deploy using FTP](/documentation/articles/web-sites-php-mysql-deploy-use-ftp/).

## <a name="localgitdeployment"></a>Deploy from local Git
If your development team uses an on-premises local source code management (SCM) service based on Git, you can configure this as a deployment source to Azure Web App. 

Pros of deploying from local Git are:

- Version control to enable rollback.
- Branch-specific deployment, can deploy different branches to different [slots](/documentation/articles/web-sites-staged-publishing/).

Con of deploying from local Git is:

- Some knowledge of the respective SCM system required.
- No turn-key solutions for continuous deployment. 

###<a name="vsts"></a>How to deploy from local Git
In the [Azure Classic Management Portal](https://manage.windowsazure.cn/), you can configure local Git deployment.

* [Local Git Deployment to Azure Web App](/documentation/articles/app-service-deploy-local-git/). 
* [Publishing to Web Apps from any git/hg repo](http://blog.davidebbo.com/2013/04/publishing-to-azure-web-sites-from-any.html).  

## Deploy using an IDE
If you are already using [Visual Studio](https://www.visualstudio.com/products/visual-studio-community-vs.aspx) 
with an [Azure SDK](/downloads/), or other IDE suites like [Xcode](https://developer.apple.com/xcode/), [Eclipse](https://www.eclipse.org), and [IntelliJ IDEA](https://www.jetbrains.com/idea/), you can deploy to Azure directly from within your IDE. This option is ideal for an individual developer.

Visual Studio supports all three deployment processes (FTP, Git, and Web Deploy), depending on your preference, while other IDEs can 
deploy to Azure if they have FTP or Git integration (see [Overview of deployment processes](#overview)).

The pros of deploying using an IDE are:

- Potentially minimize the tooling for your end-to-end application life-cycle. Develop, debug, track, and deploy your app to Azure all 
without moving outside of your IDE. 

The cons of deploying using an IDE are:

- Added complexity in tooling.
- Still requires a source control system for a team project.

<a name="vspros"></a>
Additional pros of deploying using Visual Studio with Azure SDK are:

- Azure SDK makes Azure resources first-class citizens in Visual Studio. Create, delete, edit, start, and stop apps, 
query the backend SQL database, live-debug the Azure app, and much more. 
- Live editing of code files on Azure.
- Live debugging of apps on Azure.
- Integrated Azure explorer.
- Diff-only deployment. 

###<a name="vs"></a>How to deploy from Visual Studio directly

* [Get started with Azure and ASP.NET](/documentation/articles/web-sites-dotnet-get-started/). How to create and deploy a simple ASP.NET MVC web project by 
using Visual Studio and Web Deploy.
* [How to Deploy Azure WebJobs using Visual Studio](/documentation/articles/websites-dotnet-deploy-webjobs/). How to configure Console Application projects 
so that they deploy as WebJobs.  
* [Deploy a Secure ASP.NET MVC 5 app with Membership, OAuth, and SQL Database to Web Apps](/documentation/articles/web-sites-dotnet-deploy-aspnet-mvc-app-membership-oauth-sql-database/). 
How to create and deploy an ASP.NET MVC web project with a SQL database, by using Visual Studio, Web Deploy, and Entity Framework Code 
First Migrations.
* [ASP.NET Web Deployment using Visual Studio](http://www.asp.net/mvc/tutorials/deployment/visual-studio-web-deployment/introduction). 
A 12-part tutorial series that covers a more complete range of deployment tasks than the others in this list. Some Azure deployment 
features have been added since the tutorial was written, but notes added later explain what's missing.
* [Deploying an ASP.NET Website to Azure in Visual Studio 2012 from a Git Repository directly](http://www.dotnetcurry.com/ShowArticle.aspx?ID=881). 
Explains how to deploy an ASP.NET web project in Visual Studio, using the Git plug-in to commit the code to Git and connecting Azure to 
the Git repository. Starting in Visual Studio 2013, Git support is built-in and doesn't require installation of a plug-in.

## <a name="automate"></a>Automate deployment by using command-line tools

* [Automate deployment with MSBuild](#msbuild)
* [Copy files with FTP tools and scripts](#ftp)
* [Automate deployment with Windows PowerShell](#powershell)
* [Automate deployment with .NET management API](#api)
* [Deploy from Azure Command-Line Interface (Azure CLI)](#cli)
* [Deploy from Web Deploy command line](#webdeploy)
* [Using FTP Batch Scripts](http://support.microsoft.com/kb/96269).
 
Another deployment option is to use a cloud-based service such as [Octopus Deploy](http://en.wikipedia.org/wiki/Octopus_Deploy). For more information, see [Deploy ASP.NET applications to Azure Web Sites](https://octopusdeploy.com/blog/deploy-aspnet-applications-to-azure-websites).

###<a name="msbuild"></a>Automate deployment with MSBuild

If you use the [Visual Studio IDE](#vs) for development, you can use [MSBuild](http://msbuildbook.com/) to automate anything you can do in your IDE. You can configure MSBuild to use either [Web Deploy](#webdeploy) or [FTP/FTPS](#ftp) to copy files. Web Deploy can also automate many other deployment-related tasks, such as deploying databases.

For more information about command-line deployment using MSBuild, see the following resources:

* [ASP.NET Web Deployment using Visual Studio: Command Line Deployment](http://www.asp.net/mvc/tutorials/deployment/visual-studio-web-deployment/command-line-deployment). Tenth in a series of tutorials about deployment to Azure using Visual Studio. Shows how to use the command line to deploy after setting up publish profiles in Visual Studio.
* [Inside the Microsoft Build Engine: Using MSBuild and Team Foundation Build](http://msbuildbook.com/). Hard-copy book that includes chapters on how to use MSBuild for deployment.

###<a name="powershell"></a>Automate deployment with Windows PowerShell

You can perform MSBuild or FTP deployment functions from [Windows PowerShell](http://msdn.microsoft.com/zh-cn/library/dd835506.aspx). If you do that, you can also use a collection of Windows PowerShell cmdlets that make the Azure REST management API easy to call.

For more information, see the following resources:

* [Building Real-World Cloud Apps with Azure - Automate Everything](http://asp.net/aspnet/overview/developing-apps-with-windows-azure/building-real-world-cloud-apps-with-windows-azure/automate-everything). E-book chapter that explains how the sample application shown in the e-book uses Windows PowerShell scripts to create an Azure test environment and deploy to it. See the [Resources](http://asp.net/aspnet/overview/developing-apps-with-windows-azure/building-real-world-cloud-apps-with-windows-azure/automate-everything#resources) section for links to additional Azure PowerShell documentation.

###<a name="api"></a>Automate deployment with .NET management API

You can write C# code to perform MSBuild or FTP functions for deployment. If you do that, you can access the Azure management REST API to perform site management functions.

For more information, see the following resource:

* [Automating everything with the Azure Management Libraries and .NET](http://www.hanselman.com/blog/PennyPinchingInTheCloudAutomatingEverythingWithTheWindowsAzureManagementLibrariesAndNET.aspx). Introduction to the .NET management API and links to more documentation.

###<a name="cli"></a>Deploy from Azure Command-Line Interface (Azure CLI)

You can use the command line in Windows, Mac or Linux machines to deploy by using FTP. If you do that, you can also access the Azure REST management API using the Azure CLI.

For more information, see the following resource:

* [Azure Command line tools](/downloads/#cmd-line-tools). Portal page in Azure.cn for command line tool information.

###<a name="webdeploy"></a>Deploy from Web Deploy command line

[Web Deploy](http://www.iis.net/downloads/microsoft/web-deploy) is Microsoft software for deployment to IIS that not only provides intelligent file sync features but also can perform or coordinate many other deployment-related tasks that can't be automated when you use FTP. For example, Web Deploy can deploy a new database or database updates along with your web app. Web Deploy can also minimize the time required to update an existing site since it can intelligently copy only changed files. Microsoft Visual Studio and Team Foundation Server have support for Web Deploy built-in, but you can also use Web Deploy directly from the command line to automate deployment. Web Deploy commands are very powerful but the learning curve can be steep.

For more information, see the following resource:

* [Simple Web Apps: Deployment](https://azure.microsoft.com/blog/2014/07/28/simple-azure-websites-deployment/). Blog by David Ebbo about a tool he wrote to make it easier to use Web Deploy.
* [Web Deployment Tool](http://technet.microsoft.com/zh-cn/library/dd568996). Official documentation on the Microsoft TechNet site. Dated but still a good place to start.
* [Using Web Deploy](http://www.iis.net/learn/publish/using-web-deploy). Official documentation on the Microsoft IIS.NET site. Also dated but a good place to start.
* [ASP.NET Web Deployment using Visual Studio: Command Line Deployment](http://www.asp.net/mvc/tutorials/deployment/visual-studio-web-deployment/command-line-deployment). MSBuild is the build engine used by Visual Studio, and it can also be used from the command line to deploy web applications to Web Apps. This tutorial is part of a series that is mainly about Visual Studio deployment.

##<a name="nextsteps"></a>Next Steps

In some scenarios you might want to be able to easily switch back and forth between a staging and a production version of your app. For more information, see [Staged Deployment on Web Apps](/documentation/articles/web-sites-staged-publishing/).

Having a backup and restore plan in place is an important part of any deployment workflow. For information about the Azure backup and restore feature, see [Web Apps Backups](/documentation/articles/web-sites-backup/).  


 
