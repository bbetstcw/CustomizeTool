
This section shows how to send notifications from a .NET console app and any other.
If you are using Mobile Services please refer to the [Get Started with <!-- deleted by customization Push](/documentation/articles/mobile-services-dotnet-backend-windows-store-dotnet-get-started-push) --><!-- keep by customization: begin --> Push](/documentation/articles/mobile-services-dotnet-backend-windows-store-dotnet-get-started-push/) <!-- keep by customization: end --> tutorials. If you want to use Java or PHP refer to [How to use Notification Hubs from <!-- deleted by customization Java/PHP](/documentation/articles/notification-hubs-java-backend-how-to) --><!-- keep by customization: begin --> Java/PHP](/documentation/articles/notification-hubs-java-backend-how-to/) <!-- keep by customization: end -->. You can send notifications from any backend using the [Notification <!-- deleted by customization Hub --><!-- keep by customization: begin --> Hubs <!-- keep by customization: end --> REST <!-- deleted by customization interface](http://msdn.microsoft.com/zh-cn/library/azure/dn223264.aspx) --><!-- keep by customization: begin --> interface] <!-- keep by customization: end -->.

The following code sends notifications to Windows Store, Windows Phone, iOS, and Android devices. 

Skip steps 1-3 if you created a console app when you completed [Get started with Notification Hubs][get-started].

1. In Visual Studio create a new Visual C# console application: 

   	![][13]

2. In the Visual Studio main menu, click **Tools**, **Library Package Manager**, and **Package Manager Console**, then in the console window type the following and press **Enter**:

<!-- deleted by customization
        Install-Package Microsoft.Azure.NotificationHubs
 	
	This adds a reference to the Azure Notification Hubs SDK using the <a href="http://www.nuget.org/packages/Microsoft.Azure.NotificationHubs/">Microsoft.Azure.Notification Hubs NuGet package</a>. 
-->
<!-- keep by customization: begin -->
        Install-Package WindowsAzure.ServiceBus
 	
	This adds a reference to the Azure Service Bus SDK by using the <a href="http://nuget.org/packages/WindowsAzure.ServiceBus/">WindowsAzure.ServiceBus NuGet package</a>. 
<!-- keep by customization: end -->

3. Open the file Program.cs and add the following `using` statement:

<!-- deleted by customization
        using Microsoft.Azure.NotificationHubs;
-->
<!-- keep by customization: begin -->
        using Microsoft.ServiceBus.Notifications;
<!-- keep by customization: end -->

4. In the `Program` class, add the following method, or replace it if it already exists:

        private static async void SendNotificationAsync()
        {
			// Define the notification hub.
		    NotificationHubClient hub = 
				NotificationHubClient.CreateClientFromConnectionString(
					"<connection string with full access>", "<hub name>");
		
		    // Create an array of breaking news categories.
		    var categories = new string[] { "World", "Politics", "Business", 
		        "Technology", "Science", "Sports"};
		
            foreach (var category in categories)
            {
                try
                {
                    // Define a Windows Store toast.
                    var wnsToast = "<toast><visual><binding template=\"ToastText01\">" 
                        + "<text id=\"1\">Breaking " + category + " News!" 
                        + "</text></binding></visual></toast>";         
                    await hub.SendWindowsNativeNotificationAsync(wnsToast, category);

                    // Define a Windows Phone toast.
                    var mpnsToast =
                        "<?xml version=\"1.0\" encoding=\"utf-8\"?>" +
                        "<wp:Notification xmlns:wp=\"WPNotification\">" +
                            "<wp:Toast>" +
                                "<wp:Text1>Breaking " + category + " News!</wp:Text1>" +
                            "</wp:Toast> " +
                        "</wp:Notification>";         
                    await hub.SendMpnsNativeNotificationAsync(mpnsToast, category);

                    // Define an iOS alert.
                    var alert = "{\"aps\":{\"alert\":\"Breaking " + category + " News!\"}}";
                    await hub.SendAppleNativeNotificationAsync(alert, category);

					// Define an Android notification.
                    var notification = "{\"data\":{\"msg\":\"Breaking " + category + " News!\"}}";
                    await hub.SendGcmNativeNotificationAsync(notification, category);
                }
                catch (ArgumentException)
                {
                    // An exception is raised when the notification hub hasn't been 
                    // registered for the iOS, Windows Store, or Windows Phone platform. 
                }
            }
		 }

	This code sends notifications for each of the six tags in the string array to Windows Store, Windows Phone and iOS devices. The use of tags makes sure that devices receive notifications only for the registered categories.
	
<!-- deleted by customization
	> [AZURE.NOTE] This<!-- keep by customization: begin --> <p>This <!-- keep by customization: end --> backend code supports Windows Store, Windows Phone, iOS, and Android clients. Send methods return an error response when the notification hub hasn't yet been configured for a particular client platform. <!-- keep by customization: begin --> </p> <!-- keep by customization: end -->
-->
<!-- keep by customization: begin -->
	<div class="dev-callout"><strong>Note</strong> 
	<!-- keep by customization: begin --> <p>This <!-- keep by customization: end --> backend code supports Windows Store, Windows Phone, iOS, and Android clients. Send methods return an error response when the notification hub hasn't yet been configured for a particular client platform. <!-- keep by customization: begin --> </p> <!-- keep by customization: end -->
	</div>
<!-- keep by customization: end -->

6. In the above code, replace the `<hub name>` and `<connection string with full access>` placeholders with your notification hub name and the connection string for *DefaultFullSharedAccessSignature* that you obtained earlier.

7. Add the following lines in the **Main** method:

         SendNotificationAsync();
		 Console.ReadLine();

<!-- Anchors -->
[From a console app]: #console
[From Mobile Services]: #mobile-services
[Run the app and generate notifications]: #test-app

<!-- Images. -->
[13]: ./media/notification-hubs-back-end/notification-hub-create-console-app.png

[15]: ./media/notification-hubs-back-end/notification-hub-scheduler1.png
[16]: ./media/notification-hubs-back-end/notification-hub-scheduler2.png

<!-- URLs. -->
[get-started]: <!-- deleted by customization /documentation/articles/notification-hubs-windows-store-dotnet-get-started --><!-- keep by customization: begin --> /documentation/articles/notification-hubs-windows-store-dotnet-get-started/ <!-- keep by customization: end -->
[Use Notification Hubs to send notifications to users]: /documentation/articles/tutorial-notify-users-mobileservices
[Get started with Mobile Services]: /documentation/articles/mobile-services-javascript-backend-windows-store-dotnet-get-started/#create-new-service
[Azure Management Portal]: https://manage.windowsazure.cn/
[wns object]: https://msdn.microsoft.com/zh-cn/library/azure/jj860484.aspx
[Notification Hubs Guidance]: http://msdn.microsoft.com/zh-cn/library/jj927170.aspx
[Notification Hubs How-To for Windows Store]: http://msdn.microsoft.com/zh-cn/library/jj927172.aspx
[Notification Hubs REST interface]: http://msdn.microsoft.com/zh-cn/library/azure/dn223264.aspx