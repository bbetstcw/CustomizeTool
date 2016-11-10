<properties 
	pageTitle="Deploy your first web app to Azure in five minutes | Azure" 
	description="Learn how easy it is to run web apps in App Service by deploying a sample app. Start doing real development quickly and see results immediately." 
	services="app-service\web"
	documentationCenter=""
	authors="cephalin"
	manager="wpickett"
	editor=""
/>

<tags
	ms.service="app-service-web"
	ms.workload="web"
	ms.tgt_pltfrm="na"
	ms.devlang="na"
	ms.topic="hero-article"
	ms.date="09/09/2016"
	wacn.date="" 
	ms.author="cephalin"
/>
	
# Deploy your first web app to Azure in five minutes

This tutorial helps you deploy your first web app to [Azure App Service](/documentation/articles/app-service-value-prop-what-is/).
You can use App Service to create web apps, [mobile app back  ends](/documentation/learning-paths/appservice-mobileapps/)  ends](/documentation/services/app-service/mobile/) ,
and [API apps](/documentation/articles/app-service-api-apps-why-best-platform/).

You will: 

- Create a web app in Azure App Service.
- Deploy sample code (choose between ASP.NET, PHP, Node.js, Java, or Python).
- See your code running live in production.
- Update your web app the same way you would [push Git commits](https://git-scm.com/docs/git-push).

## Prerequisites

- [Install Git](http://www.git-scm.com/downloads). Verify that your installation is successful by running `git --version` from a new Windows command prompt, 
PowerShell window, Linux shell, or OS X terminal.
- Get a Azure account. If you don't have an account, you can 

[sign up for a trial](/pricing/1rmb-trial/?WT.mc_id=A261C142F) or 
[activate your Visual Studio subscriber benefits](/pricing/member-offers/msdn-benefits-details/?WT.mc_id=A261C142F).

>[AZURE.NOTE] You can [Try App Service](https://tryappservice.azure.com/) without an Azure account. Create a starter app and play with
it for up to an hour--no credit card required, no commitments.


[sign up for a trial](/pricing/1rmb-trial/?WT.mc_id=A261C142F).


## <a name="create"></a> Create a web app

1. Sign in to the [Azure  portal](https://portal.azure.cn)  Portal Preview](https://portal.azure.cn)  with your Azure account.

2. From the left menu, click **New** > **Web + Mobile** > **Web App**.

    ![start creating your first web app in Azure](./media/app-service-web-get-started/create-web-app-portal.png)

3. In the app creation blade, use the following settings for your new app:

    - **App name**: Type a unique name.
    - **Resource group**: Select **Create new** and give the resource group a name.
    - **App Service plan/Location**: Click it to configure, then click **Create New** to set the name, location, and 
    pricing tier of the App Service plan. Feel free to use the **Free** pricing tier.

    When you're done, your app creation blade should look like this:

    ![configure your first web app in Azure](./media/app-service-web-get-started/create-web-app-settings.png)

3. Click **Create** at the bottom. You can click the **Notification** icon at the top to see the progress.

    ![app creation notification for your first web app in Azure](./media/app-service-web-get-started/create-web-app-started.png)

4. When deployment is finished, you should see this notification message. Click the message to open your deployment's blade.

    ![deployment finished message for your first web app in Azure](./media/app-service-web-get-started/create-web-app-finished.png)

5. In the **Deployment succeeded** blade, click the **Resource** link to open your new web app's blade.

    ![resource link for your first web app in Azure](./media/app-service-web-get-started/create-web-app-resource.png)

## Deploy code to your web app

Now, let's deploy some code to Azure using Git.


5. In the web app blade, scroll down to **Deployment options** or search for it, then click it. 

    ![deployment options for your first web app in Azure](./media/app-service-web-get-started/deploy-web-app-deployment-options.png)

6. Click **Choose Source** > **Local Git Repository** > **OK**.


1. Use the following PowerShell Commands to enable local git repo for your app.

        $a = Get-AzureRmResource -ResourceId /subscriptions/<subscription id>/resourcegroups/<resource group name>/providers/Microsoft.Web/sites/<web app name>/Config/web -ApiVersion 2015-08-01

        $a.Properties.scmType = "LocalGit"

        Set-AzureRmResource -PropertyObject $a.Properties -ResourceId /subscriptions/<subscription id>/resourcegroups/<resource group name>/providers/Microsoft.Web/sites/<web app name>/Config/web -ApiVersion 2015-08-01


7. Back in the web app blade, click **Deployment credentials**.

8. Set your deployment credentials and click **Save**.

7. Back in the web app blade, scroll down to **Properties** or search for it, then click it. Next to **Git URL**, click the **Copy** button.

    ![properties blade for your first web app in Azure](./media/app-service-web-get-started/deploy-web-app-properties.png)

    You're now ready to deploy your code with Git.

1. In your command-line terminal, change to a working directory (`CD`) and clone the sample app like this:

        git clone <github_sample_url>

    ![Clone the app sample code for your first web app in Azure](./media/app-service-web-get-started/html-git-clone.png)

    For *&lt;github_sample_url>*, use one of the following URLs, depending on the framework that you like:

    - HTML+CSS+JS: [https://github.com/Azure-Samples/app-service-web-html-get-started.git](https://github.com/Azure-Samples/app-service-web-html-get-started.git)
    - ASP.NET: [https://github.com/Azure-Samples/app-service-web-dotnet-get-started.git](https://github.com/Azure-Samples/app-service-web-dotnet-get-started.git)
    - PHP (CodeIgniter): [https://github.com/Azure-Samples/app-service-web-php-get-started.git](https://github.com/Azure-Samples/app-service-web-php-get-started.git)
    - Node.js (Express): [https://github.com/Azure-Samples/app-service-web-nodejs-get-started.git](https://github.com/Azure-Samples/app-service-web-nodejs-get-started.git)
    - Java: [https://github.com/Azure-Samples/app-service-web-java-get-started.git](https://github.com/Azure-Samples/app-service-web-java-get-started.git)
    - Python (Django): [https://github.com/Azure-Samples/app-service-web-python-get-started.git](https://github.com/Azure-Samples/app-service-web-python-get-started.git)

2. Change to the repository of your sample app. For example, 

        cd app-service-web-html-get-started

3. Configure the Git remote for your Azure app its Git URL, which you copied from the Portal a few steps ago.

        git remote add azure <giturlfromportal>

4. Deploy your sample code to your Azure app like you would push any code with Git:

        git push azure master

    ![Push code to your first web app in Azure](./media/app-service-web-get-started/html-git-push.png)    

    If you used one of the language frameworks, you'll see different output. This is because `git push` not only puts code in Azure, but also triggers deployment tasks
    in the deployment engine. If you have any package.json
    (Node.js) or requirements.txt (Python) files in your project (repository) root, or if you have a packages.config file in your ASP.NET project, the deployment
    script restores the required packages for you. You can also [enable the Composer extension](/documentation/articles/web-sites-php-mysql-deploy-use-git/#composer) to automatically process composer.json files
    in your PHP app.

That's it! Your code is now running live in Azure. In your browser, navigate to http://*&lt;appname>*.chinacloudsites.cn to see it in action. 

## Make updates to your app

You can now use Git to push from your project (repository) root anytime to make an update to the live site. You do it the same way as when you deployed your code
the first time. For example, every time you want to push a new change that you've tested locally, just run the following commands from your project 
(repository) root:

    git add .
    git commit -m "<your_message>"
    git push azure master

## Next steps

Find the preferred development and deployment steps for your language framework:

> [AZURE.SELECTOR]
- [.NET](/documentation/articles/web-sites-dotnet-get-started/)
- [PHP](/documentation/articles/app-service-web-php-get-started/)
- [Node.js](/documentation/articles/app-service-web-nodejs-get-started/)
- [Python](/documentation/articles/web-sites-python-ptvs-django-mysql/)
- [Java](/documentation/articles/web-sites-java-get-started/)

Or, do more with your first web app. For example:


- Try out [other ways to deploy your code to Azure](/documentation/articles/web-sites-deploy/). For example, to deploy from one of your GitHub repositories, simply select
**GitHub** instead of **Local Git Repository** in **Deployment options**.


- Try out [other ways to deploy your code to Azure](/documentation/articles/web-sites-deploy/).

- Take your Azure app to the next level. Authenticate your users. Scale it based on demand. Set up some performance alerts. All with a few clicks. See 
[Add functionality to your first web app](/documentation/articles/app-service-web-get-started-2/).

