deletion:

deleted:

		[AZURE.INCLUDE [app-service-web-to-api-and-mobile](../includes/app-service-web-to-api-and-mobile.md)]

reason: ()

deleted:

		* [Azure web apps online tools you should know about](/blog/2014/03/28/windows-azure-websites-online-tools-you-should-know-about-2/). Blog post by Amit Apple.

reason: ()

deleted:

		### Remote debugging in Azure
		
		For more information about remote debugging for Azure web apps and WebJobs, see the following resources:
		
		* [Introduction to Remote Debugging Azure Websites](/blog/2014/05/06/introduction-to-remote-debugging-on-azure-web-sites/).
		* [Introduction to Remote Debugging Azure Websites part 2 - Inside Remote debugging](/blog/2014/05/07/introduction-to-remote-debugging-azure-web-sites-part-2-inside-remote-debugging/)
		* [Introduction to Remote Debugging on Azure Websites part 3 - Multi-Instance environment and GIT](/blog/2014/05/08/introduction-to-remote-debugging-on-azure-web-sites-part-3-multi-instance-environment-and-git/)
		* [WebJobs Debugging (video)](https://www.youtube.com/watch?v=ncQm9q5ZFZs&list=UU_SjTh-ZltPmTYzAybypB-g&index=1)
		
		If your web app uses an Azure Web API or Mobile Services back-end and you need to debug that, see [Debugging .NET Backend in Visual Studio](http://blogs.msdn.com/b/azuremobile/archive/2014/03/14/debugging-net-backend-in-visual-studio.aspx).

reason: ()

deleted:

		## What's changed
		* For a guide to the change from Websites to Azure Websites see: [Azure Websites and Its Impact on Existing Azure Services](/documentation/services/web-sites/)
		* For a guide to the change of the Azure Management Portal to the Azure preview portal see: [Reference for navigating the preview portal](https://manage.windowsazure.cn/)

reason: ()

replacement:

deleted:

		preview portal

replaced by:

		Management Portal

reason: ()

deleted:

		* In the Azure Management Portal, go to **Settings > Application settings** for your web app, and then scroll down to the **Debugging** section.

replaced by:

		* In the Azure Management Portal, go to the **Configure** tab for your website, and then scroll down to the **Site Diagnostics** section.

reason: ()

deleted:

		3. In the Azure preview portal blade for your web app, click **Settings > Deployment credentials**, and then enter a new user name and password.
		
			![New FTP user name and password](./media/web-sites-dotnet-troubleshoot-visual-studio/tws-enterftpcredentials.png)
		
			**When you log in, you have to use the full user name with the web app name prefixed to it. For example, if you enter "myid" as a user name and the site is "myexample", you log in as "myexample\myid".
		
		5. In a new browser window, go to the URL that is shown under **FTP hostname** or **FTPS hostname** in the **Web App** portal blade for your web app.

replaced by:

		3. In the management portal, click **Dashboard**, and then click **Reset your deployment credentials** in the **Quick Glance** section.
		
			![Reset FTP credentials link in Dashboard](./media/web-sites-dotnet-troubleshoot-visual-studio/tws-resetftpcredentials.png)
		
		4. Enter a new user name and password.
		
			![New FTP user name and password](./media/web-sites-dotnet-troubleshoot-visual-studio/tws-enterftpcredentials.png)
		
		5. In the management portal **Dashboard** tab press F5 to refresh the page, and then scroll down to where you see **Deployment / FTP User**. Notice that the user name has the web app name prefixed to it. **When you log in, you have to use this full user name with the web app name prefixed to it as shown here.**
		
		5. In a new browser window, go to the URL that is shown under **FTP Host Name** in the **Dashboard** tab of the management portal page for your web app. **FTP Host Name** is located near **Deployment / FTP User** in the **Quick Glance** section.

reason: ()

