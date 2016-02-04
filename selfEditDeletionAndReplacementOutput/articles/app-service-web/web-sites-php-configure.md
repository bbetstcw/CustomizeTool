deletion:

deleted:

		[AZURE.INCLUDE [app-service-web-to-api-and-mobile](../includes/app-service-web-to-api-and-mobile.md)]

reason: (terminology: Azure App Service Web)

deleted:

		>[AZURE.NOTE] If you want to get started with Azure Websites before signing up for an Azure account, go to [Try Azure Websites](https://tryappservice.azure.com/), where you can immediately create a short-lived starter web app in Azure Websites. No credit cards required; no commitments.

reason: (“Try it now”)

deleted:

		## What's changed
		* For a guide to the change from Websites to Azure Websites see: [Azure Websites and Its Impact on Existing Azure Services](/documentation/services/web-sites/)
		* For a guide to the change of the Management Portal to the new portal see: [Reference for navigating the preview portal](https://manage.windowsazure.cn/)

reason: (terminology: Azure App Service Web, the new Ibiza portal)

replacement:

deleted:

		1. Browse to your web app in the [Azure Management Portal](https://manage.windowsazure.cn/) and click on the **Settings** button.
		
			![Web App Settings][settings-button]
		
		2. From the **Settings** blade select **Application Settings** and choose the new PHP version.
		
		    ![Application Settings][application-settings]
		
		3. Click the **Save** button at the top of the **Web app settings** blade.
		
			![Save configuration settings][save-button]

replaced by:

		1. Browse to your  Website's dashboard in the Azure Management Portal, click on **Configure**.
		
			![Configure tab on  Websites dashboard][configure]
		
		1. Click PHP 5.5.
		
			![Select PHP version][select-php-version]
		
		1. Click **Save** at the bottom of the page.
		
			![Save configuration settings][save-button]

reason: (the new Ibiza portal)

deleted:

		4. Browse to your web app in the Azure Management Portal and click on the **Settings** button.
		
			![Web App Settings][settings-button]
		
		5. From the **Settings** blade select **Application Settings** and scroll to the **App settings** section.
		6. In the **App settings** section, create a **PHP_EXTENSIONS** key. The value for this key would be a path relative to website root: **bin\your-ext-file**.
		
			![Enable extension in app settings][php-extensions]
		
		7. Click the **Save** button at the top of the **Web app settings** blade.
		
			![Save configuration settings][save-button]

replaced by:

		1. Navigate to your site's dashboard in the Azure Management Portal, and click on **Configure**.
		
			![Configure tab on  Websites dashboard][configure]
		
		1. In the **app settings** section, create a key **PHP_EXTENSIONS** and a value **bin\your-ext-file**. To enable multiple extensions, incude a comma-separated list of `.dll` files.
		
			![Enable extension in app settings][app-settings]
		
		1. Click **Save** at the bottom of the page.
		
			![Save configuration settings][save-button]

reason: (the new Ibiza portal)

deleted:

		4. Browse to your web app in the Azure Management Portal and click on the **Settings** button.
		
			![Web App Settings][settings-button]
		
		7. From the **Settings** blade select **Application Settings** and scroll to the **Handler mappings** section. Add `*.php` to the Extension field and add the path to the `php-cgi.exe` executable. If you put your PHP runtime in the `bin` directory in the root of you application, the path will be `D:\home\site\wwwroot\bin\php\php-cgi.exe`.
		
			![Specify handler in hander mappings][handler-mappings]
		
		8. Click the **Save** button at the top of the **Web app settings** blade.
		
			![Save configuration settings][save-button]

replaced by:

		1. Navigate to your site's dashboard in the Azure Management Portal, and click on **Configure**.
		
			![Configure tab on  Websites dashboard][configure]
		
		1. In the **handler mappings** section, add `*.php` to EXTENSION and add the path to the `php-cgi.exe` executable. If your put your PHP runtime in the `bin` directory in the root of you application, the path will be `D:\home\site\wwwroot\bin\php\php-cgi.exe`.
		
			![Specify handler in hander mappings][handler-mappings]
		
		1. Click **Save** at the bottom of the page.
		
			![Save configuration settings][save-button]

reason: (the new Ibiza portal)

