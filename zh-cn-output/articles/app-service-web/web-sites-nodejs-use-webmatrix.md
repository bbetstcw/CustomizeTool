<properties 
	pageTitle="使用 WebMatrix 构建 Node.js Web 应用并将其部署到 Azure" 
	description="本教程演示如何使用 WebMatrix 开发 Node.js 应用程序并将其部署到 Azure 网站 Web Apps。" 
	services="app-service\web" 
	documentationCenter="nodejs" 
	authors="MikeWasson" 
	manager="wpickett" 
	editor="mollybos"/>

<tags 
	ms.service="app-service-web" 
	ms.date="08/03/2015"
	wacn.date="10/03/2015"/>


# 使用 WebMatrix 构建 Node.js Web 应用并将其部署到 Azure

本教程将介绍如何使用 WebMatrix 开发 Node.js 应用程序并将其部署到 Azure 网站。WebMatrix 是 Microsoft 提供的一类免费的 Web 开发工具，此工具包含进行网站开发所需的一切。WebMatrix 包含一些使您能够轻松使用 Node.js 的功能，包括代码完成、预构建模板以及对 Jade、LESS 和 CoffeeScript 的编辑器支持。了解有关 [WebMatrix for Azure](http://go.microsoft.com/fwlink/?LinkID=253622&clcid=0x409) 的详细信息。

完成本指南之后，你将拥有一个在 Azure 中运行的 Node.js Web 应用。
 
以下是已完成应用程序的屏幕快照：

![Azure Node 网站][webmatrix-node-completed]

[WACOM.INCLUDE [create-account-and-websites-note](../includes/create-account-and-websites-note.md)]

## 登录 Azure

请按照以下步骤创建 Azure 网站。

> [AZURE.NOTE]若要完成本教程，你需要一个启用了 Azure 网站功能的 Azure 帐户。<br />如果你没有帐户，只需花费几分钟就能创建一个免费试用帐户。有关详细信息，请参阅 [Azure 免费试用](http://www.windowsazure.cn/zh-cn/pricing/1rmb-trial/?WT.mc_id=A7171371E"%20target="_blank")。<br />

1. 启动 WebMatrix
2. 如果这是你首次使用 WebMatrix 3，系统将会提示你登录 Azure。您也可以单击“登录”按钮，并选择“添加帐户”。选择使用你的 Microsoft 帐户**登录**。

	![添加帐户][addaccount]

3. 如果你已注册某一 Azure 帐户，则可以使用你的 Microsoft 帐户登录：

	![登录 Azure][signin]


## 使用 Azure 的内置模板创建网站

1. 在开始屏幕上，单击“新建”按钮，然后选择“模板库”，从模板库创建新站点：

	![从模板库创建新网站][sitefromtemplate]

2. 在“从模板创建站点”对话框中，选择“节点”，然后选择“Express 站点”。最后单击“下一步”。如果你缺少“Express 站点”模板的任何必备组件，系统将提示你进行安装。

	![选择 Express 模板][webmatrix-templates]

3. 如果你已登录到 Azure 中，现在将具有该选项以便为你的本地网站创建一个 Azure 网站。选择一个独有名称，然后选择你希望创建网站的数据中心：

	![在 Azure 中创建网站][nodesitefromtemplateazure]
	
4. 在 WebMatrix 生成网站完毕后，将显示 WebMatrix IDE。

	![WebMatrix IDE][webmatrix-ide]

##将应用程序发布到 Azure

1. 在 WebMatrix 中，从“主页”功能区单击“发布”，为网站显示“发布预览”对话框。

	![发布预览][webmatrix-node-publishpreview]

2. 单击“继续”。发布完成后，网站在 Azure 上的 URL 将显示在 WebMatrix IDE 底部

	![发布完成][webmatrix-publish-complete]

3. 单击链接在您的浏览器中打开该网站。

	![Express 网站][webmatrix-node-express-site]

##修改并重新发布应用程序

您可轻松修改并重新发布应用程序。此处，你将简单更改 **index.jade** 文件的标题，然后重新发布应用程序。

1. 在 WebMatrix 中，选择“文件”，然后展开“views”文件夹。双击打开 **index.jade** 文件。

	![显示 index.jade 的 WebMatrix][webmatrix-modify-index]

2. 将第二行更改为以下内容：

		p Welcome to #{title} with WebMatrix on Azure!

3. 保存所做更改，然后单击发布图标。最后，在“发布预览”对话框中单击“继续”，并等待发布更新。

	![发布预览][webmatrix-republish]

4. 发布完成后，使用发布过程完成时返回的链接查看更新的网站。

	![Azure Node Web 应用][webmatrix-node-completed]

##后续步骤

若要了解有关随 Azure 提供的 Node.js 版本的更多信息以及如何指定要与你的应用程序结合使用的版本，请参阅[在 Azure 应用程序中指定 Node.js 版本](/documentation/articles/nodejs-specify-node-version-azure-apps/)。

如果你将应用程序部署到 Azure 后遇到问题，有关如何诊断问题的信息，请参阅[如何在 Azure 网站中调试 Node.js 应用程序](/documentation/articles/web-sites-nodejs-debug/)。


[Azure Management Portal]: http://manage.windowsazure.cn
[WebMatrix  Website]: http://www.microsoft.com/click/services/Redirect2.ashx?CR_CC=200106398
[WebMatrix for Azure]: http://go.microsoft.com/fwlink/?LinkID=253622&clcid=0x409

[Publishing an Azure  Website using Git]: /documentation/articles/web-sites-publish-source-control/
[for free]: /zh-cn/pricing/free-trial
[webmatrix-node-completed]: ./media/web-sites-nodejs-use-webmatrix/webmatrix-node-complete.png



[webmatrix-templates]: ./media/web-sites-nodejs-use-webmatrix/webmatrix-templates.png







[webmatrix-node-publishpreview]: ./media/web-sites-nodejs-use-webmatrix/webmatrix-publishpreview.png



[webmatrix-ide]: ./media/web-sites-nodejs-use-webmatrix/webmatrix-ide.png
[webmatrix-publish-complete]: ./media/web-sites-nodejs-use-webmatrix/webmatrix-publish-complete.png
[webmatrix-node-express-site]: ./media/web-sites-nodejs-use-webmatrix/webmatrix-express-webiste.png
[webmatrix-modify-index]: ./media/web-sites-nodejs-use-webmatrix/webmatrix-node-edit.png
[webmatrix-republish]: ./media/web-sites-nodejs-use-webmatrix/webmatrix-republish.png
[addaccount]: ./media/web-sites-nodejs-use-webmatrix/webmatrix-add-account.png
[signin]: ./media/web-sites-nodejs-use-webmatrix/webmatrix-sign-in.png
[sitefromtemplate]: ./media/web-sites-nodejs-use-webmatrix/webmatrix-site-from-template.png
[nodesitefromtemplateazure]: ./media/web-sites-nodejs-use-webmatrix/webmatrix-node-site-azure.png

<!---HONumber=71-->