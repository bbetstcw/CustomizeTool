<properties
	pageTitle="如何从 iOS 客户端调用自定义 API"
	description="了解如何定义自定义 API，然后从使用 Azure 移动服务的 iOS 应用程序调用它。"
	services="mobile-services"
	documentationCenter="ios"
	authors="krisragh"
	writer="krisragh"
	manager="dwrede"
	editor=""/>

<tags
	ms.service="mobile-services"
	ms.date="06/03/2015"
	wacn.date="10/03/2015"/>


# 如何从 iOS 客户端调用自定义 API（.NET 后端）

[AZURE.INCLUDE [mobile-services-selector-call-custom-api](../includes/mobile-services-selector-call-custom-api.md)]

本主题说明如何从 iOS 应用程序调用自定义 API。自定义 API 可让你定义具有服务器功能的自定义终结点，但此功能不会映射到插入、更新、删除或读取操作。使用自定义 API 能够以更大的力度控制消息，包括 HTTP 标头和正文格式。

## <a name="define-custom-api"></a>定义自定义 API

[AZURE.INCLUDE [mobile-services-dotnet-backend-create-custom-api](../includes/mobile-services-dotnet-backend-create-custom-api.md)]

[AZURE.INCLUDE [mobile-services-ios-call-custom-api](../includes/mobile-services-ios-call-custom-api.md)]

本主题说明了如何使用 **invokeApi** 方法从 iOS 应用程序调用一个相当简单的自定义 API。若要了解有关使用 **invokeApi** 方法的详细信息，请参阅文章 [Azure 移动服务中的自定义 API](http://blogs.msdn.com/b/carlosfigueira/archive/2013/06/19/custom-api-in-azure-mobile-services-client-sdks.aspx)。

<!-- Anchors. -->

[Define the custom API]: #define-custom-api
[Update the app to call the custom API]: #update-app
[Test the app]: #test-app
[Next Steps]: #next-steps

<!-- Images. -->

<!-- URLs. -->

[Windows Push Notifications & Live Connect]: http://go.microsoft.com/fwlink/?LinkID=257677
[Mobile Services server script reference]: /documentation/articles/mobile-services-how-to-use-server-scripts
[My Apps dashboard]: http://go.microsoft.com/fwlink/?LinkId=262039
[Get started with Mobile Services]: /documentation/articles/mobile-services-dotnet-backend-ios-get-started
[Mobile Services Quick Start]: /documentation/articles/mobile-services-dotnet-backend-ios-get-started
[Get started with data]: /documentation/articles/mobile-services-dotnet-backend-ios-get-started-data
[Get started with authentication]: /documentation/articles/mobile-services-dotnet-backend-ios-get-started-users
[Get started with push notifications]: /documentation/articles/mobile-services-dotnet-backend-ios-get-started-push
[Store server scripts in source control]: /documentation/articles/mobile-services-store-scripts-source-control
<!---HONumber=71-->