<properties
	pageTitle="Secure a web app in Azure Websites"
	description="Learn how to secure an Azure web app."
	services="app-service"
	documentationCenter=""
	authors="cephalin"
	manager="wpickett"
	editor=""/>

<tags
	ms.service="app-service"
	ms.date="09/16/2015"
	wacn.date=""/>


#Secure a web app in Azure Websites

One of the challenges of developing a web app is how to provide a safe and secure service for your customers. In this article, you will learn about features of [Azure Websites](/documentation/services/web-sites/) that can secure your web app.

> [AZURE.NOTE] A full discussion of security considerations for web-based applications is beyond the scope of this document. As a starting point for further guidance on securing web applications, see the [Open Web Application Security Project (OWASP)]( https://www.owasp.org/index.php/Main_Page), specifically the [top 10 project.](https://www.owasp.org/index.php/Category:OWASP_Top_Ten_Project), which lists the current top 10 critical web application security flaws, as determined by OWASP members.
###Table of contents

* [Secure communications](#https)
* [Secure development](#develop)
* [Next steps](#next)
##<a name="https"></a> Secure communications

If you use the ***.chinacloudsites.cn** domain name created for your web app, you can immediately use HTTPS, as an SSL certificate is provided for all ***.chinacloudsites.cn** domain names. If your site uses a [custom domain name](/documentation/articles/web-sites-custom-domain-name), you can upload an SSL certificate to [enable HTTPS](/documentation/articles/web-sites-configure-ssl-certificate) for the custom domain.

##<a name="develop"></a> Secure development

### Publishing profiles and publish settings

When developing applications, performing management tasks, or automating tasks using utilities such as **Visual Studio**, **Web Matrix**, **Azure PowerShell** or the **Azure Command-Line Interface (Azure CLI)**, you can use either a *publish settings* file or a *publishing profile*. Both authenticate you to Azure, and should be secured to prevent unauthorized access.

* A **publish settings** file contains

	* Your Azure subscription ID

	* A management certificate that allows you to perform management tasks for your subscription *without having to provide an account name or password*.

* A **publishing profile** file contains

	* Information for publishing to your web app

If you use a utility that uses publish settings or publish profile, import the file containing the publish settings or profile into the utility and then **delete** the file. If you must keep the file, to share with others working on the project for example, store it in a secure location such as an **encrypted** directory with restricted permissions.

Additionally, you should make sure the imported credentials are secured. For example, **Azure PowerShell** and the **Azure Command-Line Interface (Azure CLI)** both store imported information in your **home directory** (*~* on Linux or OS X systems and */users/yourusername* on Windows systems.) For extra security, you may wish to **encrypt** these locations using encryption tools available for your operating system.

### Configuration settings, and connection strings
It's common practice to store connection strings, authentication credentials, and other sensitive information in configuration files. Unfortunately, these files may be exposed on your website, or checked into a public repository, exposing this information.

Azure Websites allows you to store configuration information as part of the Web Apps runtime environment as **app settings** and **connection strings**. The values are exposed to your application at runtime through *environment variables* for most programming languages. For .NET applications, these values are injected into your .NET configuration at runtime.

**App settings** and **connection strings** are configurable using the [Azure Management Portal](http://manage.windowsazure.cn) or utilities such as PowerShell or the Azure CLI.

For more information on app settings and connection strings, see [Configuring web apps](/documentation/articles/web-sites-configure).


## Next steps

For more information on the security of the Azure platform, information on reporting a **security incident or abuse**, or to inform Microsoft that you will be performing **penetration testing** of your site, see the security section of the [Windows Azure Trust Center](/support/trust-center/security/).

For more information on **web.config** or **applicationhost.config** files in web apps, see [Configuration options unlocked in Azure Websites web apps](http://azure.microsoft.com/blog/2014/01/28/more-to-explore-configuration-options-unlocked-in-windows-azure-web-sites/).

For information on logging information for web apps, which may be useful in detecting attacks, see [Enable diagnostic logging](/documentation/articles/web-sites-enable-diagnostic-log).
 