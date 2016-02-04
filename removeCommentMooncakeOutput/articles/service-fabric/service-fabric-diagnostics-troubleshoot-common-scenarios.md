<properties
   pageTitle="Troubleshoot common issues | Windows Azure"
   description="The most common issues encountered while deploying services on Windows Azure Service Fabric."
   services="service-fabric"
   documentationCenter=".net"
   authors="mattrowmsft"
   manager="timlt"
   editor=""/>

<tags
	ms.service="service-fabric"
	ms.date="11/10/2015"
	wacn.date=""/>


# Troubleshoot common issues
When you're running services on your developer computer, it is easy to use [Visual Studio's debugging tools](/documentation/articles/service-fabric-diagnostics-how-to-monitor-and-diagnose-services-locally). For remote clusters, [health reports](/documentation/articles/service-fabric-view-entities-aggregated-health) are always a good place to start. The easiest ways to access these reports are through PowerShell or [SFX](/documentation/articles/service-fabric-visualizing-your-cluster). This article assumes that you are debugging a remote cluster and have a basic understanding of how to use either of these tools.

##Application crash
The "Partition is below target replica or instance count" report is a good indication that your service is crashing. To find out where your service is crashing takes a little more investigation. When your service is running at scale, your best friend will be a set of well-thought-out traces.  We suggest that you try [Azure Diagnostics](/documentation/articles/service-fabric-diagnostics-how-to-setup-wad-operational-insights) for collecting and viewing those traces.

![SFX Partition Health](./media/service-fabric-diagnostics-troubleshoot-common-scenarios/crashNewApp.png)

###During service or actor initialization
Any exceptions before the service type is initialized will cause the process to crash. For these types of crashes, the application event log will show the error from your service.
These are the most common exceptions to see before the service is initialized.

| Error | Description |
| --- | --- |
| System.IO.FileNotFoundException | This error is often due to missing assembly dependencies. Check the CopyLocal property in Visual Studio or the global assembly cache for the node.
| System.Runtime.InteropServices.COMException at System.Fabric.Interop.NativeRuntime+IFabricRuntime.RegisterStatefulServiceFactory(IntPtr, IFabricStatefulServiceFactory)|This indicates that the registered service type name does not match the service manifest. |

[Azure Diagnostics](/documentation/articles/service-fabric-diagnostics-how-to-setup-wad-operational-insights) can be configured to upload the application event log for all your nodes automatically.

###RunAsync() or OnActivateAsync()
If the crash happens during the initialization or running of your registered service type or actor, the exception will be caught by Azure Service Fabric. You can view these from the EventSource providers detailed in the "Next steps" section.

## Next steps

Learn more about existing diagnostics provided by Service Fabric:

* [Reliable Actors diagnostics](/documentation/articles/service-fabric-reliable-actors-diagnostics)
* [Reliable Services diagnostics](/documentation/articles/service-fabric-reliable-services-diagnostics)
