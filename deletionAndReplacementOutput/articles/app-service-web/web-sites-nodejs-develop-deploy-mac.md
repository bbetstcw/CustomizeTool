deletion:

deleted:

		[activate your MSDN subscriber benefits](/pricing/member-offers/msdn-benefits-details/?WT.mc_id=A261C142F) or

reason: ()

deleted:

		>
		> If you want to get started with Azure Websites before you sign up for an Azure account, go to [Try Azure Websites](https://tryappservice.azure.com/). There, you can immediately create a short-lived starter web app in Azure Websites—no credit card required, and no commitments.

reason: ()

deleted:

		![Browse button](./media/web-sites-nodejs-develop-deploy-mac/browsebutton.png)
		
			![Hello world in Azure](./media/web-sites-nodejs-develop-deploy-mac/helloworldazure.png)

reason: ()

deleted:

		To learn more about how Azure Websites web apps run Node.js applications, see [Azure Websites: Node.js](http://blogs.msdn.com/b/silverlining/archive/2012/06/14/windows-azure-websites-node-js.aspx) and [Specifying a Node.js version in an Azure application](/documentation/articles/nodejs-specify-node-version-azure-apps).

reason: ()

deleted:

		To learn how Web Apps works with modules, see [Using Node.js modules with Azure applications](/documentation/articles/nodejs-use-node-modules-azure-apps).

reason: ()

replacement:

deleted:

		[Git](http://git-scm.com/%20target="_blank) is a distributed version control system that you can use to deploy your Azure Website. You'll store the code you write for your web app in a local Git repository, and you'll deploy your code to Azure by pushing to a remote repository. This method of deployment is a feature of Azure Websites web apps.  
		
		1. Sign in to the [Azure preview portal](https://manage.windowsazure.cn).
		
		2. Click the **+ NEW** icon on the top left of the portal.
		
		3. Click **Web + Mobile**, and then click **Web app**.
		
		    ![][portal-quick-create]
		
		4. Enter a name for the web app in the **Web app** box.
		
			This name must be unique in the chinacloudsites.cn domain because the URL of the web app will be {name}.chinacloudsites.cn. If the name you enter isn't unique, a red exclamation mark appears in the text box.
		
		5. Select a **Resource Group** or create a new one.
		
			For more information about resource groups, see [Using the Azure Preview Portal to manage your Azure resources](/documentation/articles/resource-group-portal).
		
		5. Select an **App Service plan/Location** or create a new one.
		
			For more information about App Service plans, see [App Service plans overview](/documentation/articles/azure-web-sites-web-hosting-plans-in-depth-overview)
		
		6. Click **Create**.
		   
			![][portal-quick-create2]
		
			In a short time, typically less than a minute, Azure finishes creating the new web app.
		
		7. Click **Web apps > {your new web app}**.
		
			![](./media/web-sites-nodejs-develop-deploy-mac/gotowebapp.png)
		
		8. In the **Web app** blade, click the **Deployment** part.
		
			![][deployment-part]
		
		9. In the **Continuous Deployment** blade, click **Choose Source**
		
		14. Click **Local Git Repository**, and then click **OK**.
		
			![][setup-git-publishing]
		
		11. Set up deployment credentials if you haven't already done so.
		
			a. In the Web app blade, click **Settings > Deployment credentials**.
		
			![][deployment-credentials]
		 
			b. Create a user name and password. 
			
			![](./media/web-sites-nodejs-develop-deploy-mac/setdeploycreds.png)
		
		11. In the Web app blade, click **Settings**, and then click **Properties**.
		 
			To publish, you'll push to a remote Git repository. The URL for the repository is listed under **GIT URL**. You'll use this URL later in the tutorial.
		
			![][git-url]

replaced by:

		> If you want to get started with Azure Websites before signing up for an account, go to <a href="https://trywebsites.chinacloudsites.cn/?language=nodejs">https://trywebsites.chinacloudsites.cn</a>, where you can immediately create a short-lived ASP.NET starter site in Azure Websites for free. No credit card required, no commitments.
		
		1. Login to the [Azure Management Portal].
		
		2. Click the **+ NEW** icon on the bottom left of the portal
		
		    ![The Azure Management Portal with the +NEW link highlighted.][portal-new-website]
		
		3. Click **WEBSITE**, then **QUICK CREATE**. Enter a value for **URL** and select the datacenter for your website in the **REGION** dropdown. Click the checkmark at the bottom of the dialog.
		
		    ![The Quick Create dialog][portal-quick-create]
		
		4. Once the website status changes to **Running**, click on the name of the website to access the **Dashboard**
		
			![Open web site dashboard][go-to-dashboard]
		
		6. At the bottom right of the Quickstart page, select **Set up a deployment from source control**.
		
			![Set up Git publishing][setup-git-publishing]
		
		6. When asked "Where is your source code?" select **Local Git repository**, and then click the arrow.
		
			![where is your source code][where-is-code]
		
		7. To enable Git publishing, you must provide a user name and password. If you have previously enabled publishing for an Azure Website, you will not be prompted for the user name or password. Instead, a Git repository will be created using the user name and password you previously specified. Make a note of the user name and password, as they will be used for Git publishing to all Azure Websites you create.
		
			![The dialog prompting for user name and password.][portal-git-username-password]
		
		8. Once the Git repository is ready, you will be presented with instructions on the Git commands to use in order to setup a local repository and then push the files to Azure.
		
			![Git deployment instructions returned after creating a repository for the website.][git-instructions]

reason: ()

deleted:

		## Roll back a deployment
		
		From the **Web app** blade you can click **Settings > Continuous Deployment** to see the deployment history in the **Deployments** blade. If you need to roll back to an earlier deployment, you can select it and then click **Redeploy** in the **Deployment Details** blade.

replaced by:

		4. You can revert to the previous deployment by selecting it in **Deployments**.

reason: ()

