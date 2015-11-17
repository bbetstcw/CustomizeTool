deletion:

deleted:

		> [AZURE.SELECTOR]
		- [Graphical](/documentation/articles/automation-first-runbook-graphical)
		- [PowerShell Workflow](/documentation/articles/automation-first-runbook-textual)

reason: ()

deleted:

		![Save runbook](./media/automation-first-runbook-textual/runbook-edit-toolbar-save.png)

reason: ()

deleted:

		![Start Runbook](./media/automation-first-runbook-textual/start-runbook-input-params.png)

reason: ()

deleted:

		## Related articles
		
		- [My first graphical runbook](/documentation/articles/automation-first-runbook-graphical)

reason: ()

replacement:

deleted:

		[activate your MSDN subscriber benefits](/pricing/member-offers/msdn-benefits-details/) or <a href="/pricing/1rmb-trial/" target="_blank">[sign

replaced by:

		[sign

reason: ()

deleted:

		1. In the Azure Preview Portal, open your Automation account.  
		The Automation account page gives you a quick view of the resources in this account.  You should already have some Assets.  Most of those are the modules that are automatically included in a new Automation account.  You should also have the Credential asset that's mentioned in the [prerequisites](#prerequisites).
		2. Click on the **Runbooks** tile to open the list of runbooks.<br>
		![Runbooks control](./media/automation-first-runbook-textual/runbooks-control.png)
		2. Create a new runbook by clicking on the **Add a runbook** button and then **Create a new runbook**.
		3. Give the runbook the name *MyFirstRunbook-Workflow*.
		4. In this case, we're going to create a [PowerShell Workflow runbook](/documentation/articles/automation-runbook-types#powerShell-workflow-runbooks) so select **Powershell Workflow** for **Runbook type**.<br>
		![New runbook](./media/automation-first-runbook-textual/new-runbook.png)
		5. Click **Create** to create the runbook and open the textual editor.

replaced by:

		1. In the Azure Management Portal, click **NEW** > **APP SERVICES** > **AUTOMATION** > **RUNBOOK** > **QUICK CREATE**
		2. Enter the **RUNBOOK NAME**, **DESCRIPTION**, and choose an **AUTOMATION ACCOUNT**. If you don't have one, you can create one by choosing **Create a new automation account** and entering the **ACCOUNT NAME**
		3. After the runbook being created, you can find it under the **RUNBOOKS** tile of you automation account.
		4. Click the **AUTHOR** tile of you runbook to enable the textual editor.

reason: ()

deleted:

		2. Click **Test pane** to open the Test pane.<br>
		![Test pane](./media/automation-first-runbook-textual/runbook-edit-toolbar-test-pane.png)
		2. Click **Start** to start the test.  This should be the only enabled option.
		3. A [runbook job](/documentation/articles/automation-runbook-execution) is created and its status displayed.  
		The job status will start as *Queued* indicating that it is waiting for a runbook worker in the cloud to come available.  It will then move to *Starting*  when a worker claims the job, and then *Running* when the runbook actually starts running.  
		4. When the runbook job completes, its output is displayed.  In our case, we should see *Hello World*.<br>
		![Hello World](./media/automation-first-runbook-textual/test-output-hello-world.png)
		5. Close the Test pane to return to the canvas.

replaced by:

		2. Click **Test** to open the Test pane. And then, click **Yes** to confirm.
		3. After a few seconds you will see output pane printing the message *"Hello World."*

reason: ()

deleted:

		prompted.<br>

replaced by:

		prompted.

reason: ()

deleted:

		![Publish](./media/automation-first-runbook-textual/runbook-edit-toolbar-publish.png)
		2. If you scroll left to view the runbook in the **Runbooks** pane now, it will show an **Authoring Status** of **Published**.
		3. Scroll back to the right to view the pane for **MyFirstRunbook-Workflow**.  
		The options across the top allow us to start the runbook, schedule it to start at some time in the future, or create a [webhook](/documentation/articles/automation-webhooks) so it can be started through an HTTP call. 
		4. We just want to start the runbook so click **Start** and then **Yes** when prompted.<br>
		![Start runbook](./media/automation-first-runbook-textual/runbook-toolbar-start.png)
		5. A job pane is opened for the runbook job that we just created.  We can close this pane, but in this case we'll leave it open so we can watch the job's progress.
		6.  The job status is shown in **Job Summary** and matches the statuses that we saw when we tested the runbook.<br>
		![Job Summary](./media/automation-first-runbook-textual/job-pane-summary.png)
		7.  Once the runbook status shows *Completed*, click **Output**.  The Output pane is opened, and we can see our *Hello World*.<br>
		![Job Summary](./media/automation-first-runbook-textual/job-pane-output.png)  
		8.  Close the Output pane.
		9.  Click **Streams** to open the Streams pane for the runbook job.  We should only see *Hello World* in the output stream, but this can show other streams for a runbook job such as Verbose and Error if the runbook writes to them.<br>
		![Job Summary](./media/automation-first-runbook-textual/job-pane-streams.png) 
		9. Close the Streams pane and the Job pane to return to the MyFirstRunbook pane.
		9.  Click **Jobs** to open the Jobs pane for this runbook.  This lists all of the jobs created by this runbook.  We should only see one job listed since we only ran the job once.<br>
		![Jobs](./media/automation-first-runbook-textual/runbook-control-jobs.png) 
		9. You can click on this job to open the same Job pane that we viewed when we started the runbook.  This allows you to go back in time and view the details of any job that was created for a particular runbook.

replaced by:

		2. If you view the runbook in the **Runbooks** tile of you automation account now, it will show an **Authoring Status** of **Published**.
		4. Click **Start** to start the runbook and then **Yes** when prompted.
		5. Click **View Job** to see the summary of the job you just started.
		6. The job status is shown in **Job Summary**, and after a few seconds, the same output as the testing you have done earlier will be shown under **output**.
		9. Go back to your runbook. Under the **Jobs** tile, you can seen a list of job created will filter above.
		10. Click a job, you can see the **summary** and **history** of the job.

reason: ()

deleted:

		1.  Open the textual editor by clicking **Edit** on the MyFirstRunbook-Workflow pane.<br>
		![Edit runbook](./media/automation-first-runbook-textual/runbook-toolbar-edit.png)

replaced by:

		1.  Open the textual editor by clicking **Author** tile > **Draft** tag > **Edit runbook**.

reason: ()

deleted:

		3.  In the Library control, expand **Assets** and then **Credentials**.
		4.  Right click your credential and click **Add to canvas**.  This adds a **Get-AutomationPSCredential** activity for your credential.

replaced by:

		3.  Click **Insert** > **Setting** > **Get Windows PowerShell Credential**, choose the credential you want.
		4.  If you don't have a credential, you can add one by clicking **Manage** > **Add Credential** to create one. For more information, see [Azure Active Directory user and Automation Credential asset](/documentation/articles/automation-configuring).

reason: ()

deleted:

		3.  On the next line, type *Add-AzureAccount -Credential $Credential*. <br>
		![Authenticate](./media/automation-first-runbook-textual/authentication.png) 
		3. Click **Test pane** so that we can test the runbook.
		10. Click **Start** to start the test.  Once it completes, you should receive output similar to the following that returns the information for the user in the credential.  This confirms that the credential is valid.<br>
		![Authenticate](./media/automation-first-runbook-textual/authentication-test.png)

replaced by:

		3.  On the next line, type *Add-AzureAccount -Credential $Credential –Environment AzureChinaCloud*.
		
				workflow test
				{
		    		$Credential = Get-AutomationPSCredential -Name "<your credential>"
		    		Add-AzureAccount –Credential $Credential –Environment AzureChinaCloud
				}
		
		3. Click **Test** and then **Yes** when prompted.
		10.  Once it completes, you should receive output similar to the following that returns the information for the user in the credential.  This confirms that the credential is valid.<br>
		![Authenticate](./media/automation-first-runbook-textual/authentication-test.png)

reason: ()

deleted:

		1. After *Add-AzureAccount*, type *Start-AzureVM -Name 'VMName' -ServiceName 'VMServiceName'* providing the name and service name of the virtual machine to start. <br>
		![Authenticate](./media/automation-first-runbook-textual/start-azurevm.png) 
		9. Save the runbook and then click **Test pane** so that we can test it.
		10. Click **Start** to start the test.  Once it completes, check that the virtual machine was started.

replaced by:

		1. After *Add-AzureAccount*, type *Start-AzureVM -Name 'VMName' -ServiceName 'VMServiceName'* providing the name and service name of the virtual machine to start. 
		
				workflow test
				{
		    		$Credential = Get-AutomationPSCredential -Name "<your credential>"
		    		Add-AzureAccount –Credential $Credential –Environment AzureChinaCloud
		    		Start-AzureVM -Name "<your vm>" -ServiceName "<your vm service>"
				}
		
		9. Save the runbook and then click **Test** so that we can test it.

reason: ()

deleted:

		![Authenticate](./media/automation-first-runbook-textual/params.png)

replaced by:

		workflow test
				{
		    		Param (
		        		[string]$VMName,
		        		[String]$VMServiceName
		    		)
		    
		    		$Credential = Get-AutomationPSCredential -Name "<your credential>"
		    		Add-AzureAccount –Credential $Credential –Environment AzureChinaCloud
		    		Start-AzureVM -Name $VMName -ServiceName $VMServiceName
				}

reason: ()

