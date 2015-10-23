

Push notifications are normally sent in a back-end service like Mobile Services or ASP.NET using a compatible library. You can also use the REST API directly to send notification messages if a library is not available for your back-end. 

Here is a list of some other tutorials you may want to review for sending notifications:

- Azure Mobile Services : For an example of how to send notifications from an Azure Mobile Services backend integrated with Notification Hubs, see [Get started with push notifications in Mobile Services].  
- ASP.NET : [Use Notification Hubs to push notifications to users].
- Azure Notification Hub Java SDK: See [How to use Notification Hubs from Java](notification-hubs-java-backend-how-to) for sending notifications from Java. This has been tested in Eclipse for Android Development.
- PHP: [How to use Notification Hubs from PHP](notification-hubs-php-backend-how-to).


In the next section of the tutorial, you will learn how to use the [Notification Hub REST interface](http://msdn.microsoft.com/zh-cn/library/windowsazure/dn223264.aspx) to send the notification message directly in your app. All registered devices receive the notification sent by any device.  

