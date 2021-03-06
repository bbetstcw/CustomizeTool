<properties
    pageTitle="Continuous Deployment to Azure App Service | Azure"
    description="Learn how to enable continuous deployment to Azure App Service."
    services="app-service"
    documentationcenter=""
    author="dariagrigoriu"
    manager="wpickett"
    editor="mollybos" />
<tags
    ms.assetid="6adb5c84-6cf3-424e-a336-c554f23b4000"
    ms.service="app-service"
    ms.workload="na"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="10/28/2016"
    wacn.date=""
    ms.author="dariagrigoriu" />

# Continuous Deployment to Azure App Service
This tutorial shows you how to configure a continuous deployment workflow for your [Azure App Service] app. App Service integration with BitBucket, GitHub, and Visual Studio Team Services (VSTS) enables a continuous deployment workflow where Azure pulls in the most recent updates from your project published to one of these services. Continuous deployment is a great option for projects where multiple and frequent contributions are being integrated.

## <a name="overview"></a>Enable continuous deployment
To enable continuous deployment,

Publish your app content to the repository that will be used for continuous deployment.

For more information on publishing your project to these services, see [Create a repo (GitHub)], [Create a repo (BitBucket)], and [Get started with VSTS]. The folloing steps are only for GitHub Repo. For Repo other than GitHub's, see [Manually triggering a deployment](https://github.com/projectkudu/kudu/wiki/Manually-triggering-a-deployment) to setup the webhook.

1. Log in to the [Azure Classic Management Portal]. In the **Web App** page, select the web app you want to setup continous deployment for, and select the **DASHBOARD** tab.
2. In the **quick glance** section, select **Set up deployment from source control**. In **SET UP DEPLOYMENT** dialog, select **External Repository**.
3. Enter the url of you GitHub Repo, and enter the branch you want to track. Leave the **Repository Type** to be Git, and click **Complete**

    > [AZURE.NOTE] Azure China currently only support public repository, because there is no way we can enter the git credential for the web app.

    Azure creates an association with the selected repository, pulls in the files from the specified branch, and maintains a clone of your repository for your Azure Web App.
4. In the **quick glance** section, click **Download the publish profile** to download the publish profile of you web app.
5. Open the publish profile with a text editor, and get the following values.

        userName="$<YOUR_WEB_APP>" userPWD="<SOME_BIG_RANDOM_TOKEN>"
6. In the page of you GitHub Repo, click **Settings**, choose **Webhooks & services**, and then click **Add webhook**
7. In **Payload URL**, enter `https://$<YOUR_WEB_APP>:<SOME_BIG_RANDOM_TOKEN>@<YOUR_WEB_APP>.scm.chinacloudsites.cn/deploy`, leave everything else as what it was, and click **Add webhook** to save the new webhook.
8. To verify the app is successfully deployed, click the **URL** of the app in the Azure Classic Management Portal.
9. To verify that continuous deployment is occurring from the repository of your choice, push a change to the repository. Your app should update to reflect the changes shortly after the push to the repository completes. You can verify that it has pulled in the update in the **Deployments** page of your app.

## <a name="VSsolution"></a>Continuous deployment of a Visual Studio solution
Pushing a Visual Studio solution to Azure App Service is just as easy as pushing a simple index.html file. The App Service deployment process streamlines all the details, including restoring NuGet dependencies and building the application binaries. You can follow the source control best practices of maintaining code only in your Git repository, and let App Service deployment take care of the rest.

The steps for pushing your Visual Studio solution to App Service are the same as in the [previous section](#overview), provided that you configure your solution and repository as follows:

* Use the Visual Studio source control option to generate a `.gitignore` file such as the image below or manually add a `.gitignore` file in your repository root with content similar to this [.gitignore sample](https://github.com/github/gitignore/blob/master/VisualStudio.gitignore).
  
  ![](./media/app-service-continuous-deployment/VS_source_control.png)
* Add the entire solution's directory tree to your repository, with the .sln file in the repository root.

Once you have set up your repository as described, and configured your app in Azure for continuous publishing from one of the online Git repositories, you can develop your ASP.NET application locally in Visual Studio and continuously deploy your code simply by pushing your changes to your online Git repository.

## <a name="disableCD"></a>Disable continuous deployment
To disable continuous deployment,

1. Log in to the [Azure Classic Management Portal](https://manage.windowsazure.cn). In you web app page, select the **DASHBOARD** tab.

2. In the **quick glance** section, select **Disconnect from external repository**.

## Additional Resources
* [How to investigate common issues with continuous deployment](https://github.com/projectkudu/kudu/wiki/Investigating-continuous-deployment)
* [How to use PowerShell for Azure]
* [How to use the Azure Command-Line Tools for Mac and Linux]
* [Git documentation]
* [Project Kudu](https://github.com/projectkudu/kudu/wiki)

[Azure App Service]: /documentation/articles/app-service-changes-existing-services/
[Azure portal Preview]: https://portal.azure.cn
[VSTS Portal]: https://www.visualstudio.com/products/visual-studio-team-services-vs.aspx
[Installing Git]: http://git-scm.com/book/zh/v2/%E8%B5%B7%E6%AD%A5-%E5%AE%89%E8%A3%85-Git
[How to use PowerShell for Azure]: /documentation/articles/powershell-install-configure/
[How to use the Azure Command-Line Tools for Mac and Linux]: /documentation/articles/xplat-cli-install/
[Git Documentation]: http://git-scm.com/documentation

[Create a repo (GitHub)]: https://help.github.com/articles/create-a-repo
[Create a repo (BitBucket)]: https://confluence.atlassian.com/display/BITBUCKET/Create+an+Account+and+a+Git+Repo
[Get started with VSTS]: https://www.visualstudio.com/get-started/overview-of-get-started-tasks-vs
[Continuous delivery to Azure using Visual Studio Team Services]: /documentation/articles/cloud-services-continuous-delivery-use-vso/
