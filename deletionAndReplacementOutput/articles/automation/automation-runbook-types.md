deletion:

deleted:

		| [Graphical](#graphical-runbooks) | Based on Windows PowerShell Workflow and created and edited completely in graphical editor in Azure Management Portal. |

reason: ()

deleted:

		## Graphical runbooks
		
		[Graphical runbooks](/documentation/articles/automation-runbook-types#graphical-runbooks) are created and edited with the graphical editor in the Azure Management Portal.  You can export them to a file and then import them into another automation account, but you cannot create or edit them with another tool.  Graphical runbooks generate PowerShell Workflow code, but you can't directly view or modify the code. Graphical runbooks cannot be converted to one of the [text formats](/documentation/articles/automation-runbook-types), nor can a text runbook be converted to graphical format.
		
		### Advantages
		
		- Create runbooks with minimal knowledge of [PowerShell Workflow](/documentation/articles/automation-powershell-workflow).
		- Visually represent management processes.
		- Use [checkpoints](/documentation/articles/automation-powershell-workflow#checkpoints) to resume runbook in case of error.
		- Use [parallel processing](/documentation/articles/automation-powershell-workflow#parallel-processing) to perform mulitple activities in parallel.
		- Can include other Graphical runbooks and PowerShell Workflow runbooks as child runbooks to create high level workflows.

reason: ()

deleted:

		Graphical runbooks and

reason: ()

deleted:

		Graphical or

reason: ()

deleted:

		- [Graphical authoring in Azure Automation](/documentation/articles/automation-graphical-authoring-intro)

reason: ()

replacement:

deleted:

		runbook](http://msdn.microsoft.com/zh-cn/library/azure/dn643637.aspx)

replaced by:

		runbook](/documentation/articles/automation-creating-importing-runbook)

reason: ()

deleted:

		runbook](http://msdn.microsoft.com/zh-cn/library/azure/dn643637.aspx)

replaced by:

		runbook](/documentation/articles/automation-creating-importing-runbook)

reason: ()

deleted:

		- Can't run runbooks on [Hybrid Runbook Worker](/documentation/articles/automation-hybrid-runbook-worker).
		- PowerShell Workflow runbooks and Graphical runbooks can only be included as child runbooks by using the Start-AzureAutomationRunbook cmdlet which creates a new job.

replaced by:

		- PowerShell Workflow runbooks can only be included as child runbooks by using the Start-AzureAutomationRunbook cmdlet which creates a new job.

reason: ()

deleted:

		- [Creating or Importing a Runbook](http://msdn.microsoft.com/zh-cn/library/azure/dn643637.aspx)

replaced by:

		- [Creating or Importing a Runbook](/documentation/articles/automation-creating-importing-runbook)

reason: ()

