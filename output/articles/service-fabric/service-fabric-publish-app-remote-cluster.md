<properties
    pageTitle="Publish an app to a remote cluster with VS | Windows Azure"
    description="Learn about the steps needed to publish an application to a remote service fabric cluster by using Visual Studio."
    services="service-fabric"
    documentationCenter="na"
    authors="cawams"
    manager="timlt"
    editor="" />

<tags
	ms.service="multiple"
	ms.date="12/06/2015"
	wacn.date=""/>

# Publish an application to a remote cluster using Visual Studio

The Service Fabric extension for Visual Studio provides an easy, repeatable, and scriptable way to publish an application to a Service Fabric cluster.

## Understanding the artifacts required for publishing

### Deploy-FabricApplication.ps1

This is a PowerShell script that uses a publish profile path as a parameter for publishing Service Fabric applications. Since this script is part of your application, you are welcome to modify it as necessary for your application.

### Publish profiles

A folder in the Service Fabric application project called **PublishProfiles** contains XML files that store essential information for publishing an application, such as:
- Service Fabric cluster connection parameters
- Path to an application parameter file
- Upgrade settings

By default, your application will include two publish profiles: Local.xml and Cloud.xml. You can add more profiles by copying and pasting one of the default files.

### Application parameter files

A folder in the Service Fabric application project called **ApplicationParameters** contains XML files for user-specified application manifest parameter values. Application manifest files can be parameterized so that you can use different values for deployment settings. To learn more about parameterizing your application, see [Manage multiple environments in Service Fabric](/documentation/articles/service-fabric-manage-multiple-environment-app-configuration).

>[AZURE.NOTE] For Actor services, you should build the project first to generate the parameters in the manifest file.

## Publish an application using the Publish Service Fabric Application dialog box

The following steps demonstrate how to publish an application by using use the **Publish Service Fabric Application** dialog provided by the Visual Studio Service Fabric Tools.

1. On the shortcut menu of the Service Fabric Application project, choose **Publish…** to view the **Publish Service Fabric Application** dialog box.

    ![][0]

    The file selected in the **Target profile** dropdown list box is where all of the settings, except **Manifest versions**, are saved. You can either reuse an existing profile or create a new one by choosing **<Manage Profiles…>** in the **Target profile** dropdown list box. When you choose a publish profile, its contents appear in the corresponding fields of the dialog. To save your changes at any time, choose the **Save Profile** link.    

2. The **Connection endpoint** section lets you specify a local or remote Service Fabric cluster's publishing endpoint. To add or change the connection endpoint, choose the **Select…** button. The **Select Service Fabric Cluster** dialog box shows the available Service Fabric cluster connection endpoints to which you can publish based on your Azure subscription(s). Note that if you are not already logged in to Visual Studio, you will be prompted to do so.

    The cluster selection dialog allows you to choose from the set of available subscriptions and clusters.

    ![][1]

    >[AZURE.NOTE] If you would like to publish to an arbitrary endpoint (such as a party cluster), see the **Publishing to an arbitrary cluster endpoint** section below.

    Once you choose an endpoint, Visual Studio validates the connection to the selected Service Fabric cluster. If the cluster isn't secure, Visual Studio can connect to it immediately. However, if the cluster is secure, you'll need to install a certificate on your local machine before proceeding. See [How to configure secure connections](/documentation/articles/service-fabric-visualstudio-configure-secure-connections) for more information. When you're done, choose the **OK** button. The selected cluster appears in the **Publish Service Fabric Application** dialog.

3. The **Application Parameters File** dropdown list box lets you navigate to an application parameters file. An application parameters file holds user-specified values for parameters in the application manifest file. To add or change a parameter, choose the **Edit** button. Enter or change the parameter's value in the **Parameters** grid. When you're done, choose the **Save** button.

    ![][2]

4. The **Upgrade the Application** checkbox lets you specify whether this publish action is an upgrade. Upgrade publish actions differ from normal publish actions. See [Service Fabric Application Upgrade](/documentation/articles/service-fabric-application-upgrade) for a list of differences. To configure upgrade settings, choose the **Configure Upgrade Settings** link. The upgrade parameter editor appears. See [Configure the upgrade of a Service Fabric application](/documentation/articles/service-fabric-visualstudio-configure-upgrade) to learn more about upgrade parameters.

5. Choose the **Manifest Versions…** button to view the **Edit Versions** dialog. You need to update application and service versions for an upgrade to take place. See [Service Fabric Application Upgrade Tutorial](/documentation/articles/service-fabric-application-upgrade-tutorial) to learn how application and service manifest versions impact an upgrade process.

    ![][3]

    If the application and service versions use semantic versioning such as 1.0.0 or numerical values in the format of 1.0.0.0, select the **Automatically update application and service versions** option. When you choose this option, the service and application version numbers are automatically updated whenever a code, config, or data package version is updated. If you prefer to edit the versions manually, clear the checkbox to disable this feature.

    >[AZURE.NOTE] For all package entries to appear for an Actor project, first build the project to generate the entries in the Service Manifest files.

6. When you're done specifying all of the necessary settings, choose the **Publish** button to publish your application to the selected Service Fabric cluster. The settings you specified are applied to the publish process.

## Publishing to an arbitrary cluster endpoint (including party clusters)

The Visual Studio publishing experience is optimized for publishing to remote clusters associated with one of your Azure subscriptions. However, it is possible to publish to an arbitrary endpoint (such as Service Fabric party clusters) by directly editing the publish profile XML. As described above, two publish profiles are provided by default - **Local.xml** and **Cloud.xml** - but you are welcome to create additional profiles for different environments. For instance, you might want to create a profile for publishing to party clusters, perhaps named **Party.xml**.

If you are connecting to a unsecured cluster, all that's required is the cluster connection endpoint, such as `partycluster1.chinaeast.chinacloudapp.cn:19000`. In that case, the connection endpoint in the publish profile would something like this:

```XML
<ClusterConnectionParameters ConnectionEndpoint="partycluster1.chinaeast.chinacloudapp.cn:19000" />
```

  If you are connecting to a secured cluster, you will also need to provide the details of the client certificate from the local store to be used for authentication. For more details, see [configuring secure connections to a Service Fabric cluster](/documentation/articles/service-fabric-visualstudio-configure-secure-connections).

  Once your publish profile is set up, you can reference it in the publish dialog as shown below.

  ![New publish profile in publish dialog][4]

  Note that in this case, the new publish profile points to one of the default application parameters files. This is appropriate if you want to publish the same application configuration to a number of environments. By contrast, in cases where you want to have different configurations for each environment you want to publish to, it would make sense to create a corresponding application parameters file.

## Next steps

To learn how to automate the publishing process in a Continuous Integration environment, see [Service Fabric Set Up Continuous Integration](/documentation/articles/service-fabric-set-up-continuous-integration).


[0]: ./media/service-fabric-publish-app-remote-cluster/PublishDialog.png
[1]: ./media/service-fabric-publish-app-remote-cluster/SelectCluster.png
[2]: ./media/service-fabric-publish-app-remote-cluster/EditParams.png
[3]: ./media/service-fabric-publish-app-remote-cluster/EditVersions.png
[4]: ./media/service-fabric-publish-app-remote-cluster/publish-to-party-cluster.png
