<properties 
	pageTitle="Download the Azure SDK for Java" 
	description="Learn how to download the Azure SDK for Java, with sample code provided for Maven projects and basic installation steps for the Azure Tookit for Eclipse." 
	services="" 
	documentationCenter="java" 
	authors="rmcmurray" 
	manager="wpickett" 
	editor="jimbe"/>

<tags
	ms.service="multiple"
	ms.date="11/30/2015"
	wacn.date=""/>

# Download the Azure SDK for Java #

This article contains instructions for downloading and installing the Azure Libraries for Java.

**Note:** The Azure Libraries for Java are distributed under the [Apache License, Version 2.0][license].

## Azure Libraries for Java - Manual Download ##

To manually install the Azure Libraries for Java, click <http://go.microsoft.com/fwlink/?LinkId=690320> to download a ZIP file which contains all of the libraries and all dependencies.

Once you have downloaded the zip file to your computer, extract the contents and use one of the following options to add the JAR files to your project:

* Import the JAR files into your Java project in Eclipse.

* Configure the **Build Path** for your Java project in Eclipse to include the path to the JAR files.

For detailed information on setting up build paths in Eclipse, see the [Java Build Path][] article at the Eclipse website.

**Note:** See the license.txt and ThirdPartyNotices.txt file file inside the ZIP for license and other information.

## Azure Libraries for Java - Building with Maven ##

### Step 1 - Set up your project to use Maven for build ###

To create Maven projects in Eclipse which use the Azure libraries for Java, following the instructions in the [Getting Started with Azure Management Libraries for Java][maven-getting-started] article. 

### Step 2 - Configure your Maven settings with the requisite dependencies ###

Once your project has been configured to use Maven for build, you can add the the requisite dependencies to your pom.xml file using syntax like the following example. Note that you do not need to add every dependency that is listed in the following example; you only need to add the specific dependencies which your project requires.

    <dependency>
        <groupId>com.microsoft.azure</groupId>
        <artifactId>azure-management</artifactId>
        <version>0.7.0</version>
    </dependency>
    <dependency>
        <groupId>com.microsoft.azure</groupId>
        <artifactId>azure-management-compute</artifactId>
        <version>0.7.0</version>
    </dependency>
    <dependency>
        <groupId>com.microsoft.azure</groupId>
        <artifactId>azure-management-network</artifactId>
        <version>0.7.0</version>
    </dependency>
    <dependency>
        <groupId>com.microsoft.azure</groupId>
        <artifactId>azure-management-sql</artifactId>
        <version>0.7.0</version>
    </dependency>
    <dependency>
        <groupId>com.microsoft.azure</groupId>
        <artifactId>azure-management-storage</artifactId>
        <version>0.7.0</version>
    </dependency>
    <dependency>
        <groupId>com.microsoft.azure</groupId>
        <artifactId>azure-management-websites</artifactId>
        <version>0.7.0</version>
    </dependency>
    <dependency>
        <groupId>com.microsoft.azure</groupId>
        <artifactId>azure-media</artifactId>
        <version>0.6.0</version>
    </dependency>
    <dependency>
        <groupId>com.microsoft.azure</groupId>
        <artifactId>azure-servicebus</artifactId>
        <version>0.7.0</version>
    </dependency>
    <dependency>
        <groupId>com.microsoft.azure</groupId>
        <artifactId>azure-serviceruntime</artifactId>
        <version>0.6.0</version>
    </dependency>

**Note:** Within each `<version>` element in the preceding example, replace the version numbers in this example with valid version numbers, which can be obtained from the [Azure Libraries Repository on Maven][].

## Installing the Azure Toolkit for Eclipse ##

This section contains basic instructions for installing the Azure Toolkit for Eclipse; for detailed instructions, see [Installing the Azure Toolkit for Eclipse][].

### Prerequisites ###

1. Windows operting systems listed in the [What's New in the Azure Toolkit for Eclipse][] article.
1. Macintosh or Linux operting systems listed in the [What's New in the Azure Toolkit for Eclipse][] article.
1. Eclipse IDE for Java EE Developers, Indigo or later. This can be downloaded from <http://www.eclipse.org/downloads/>.

### Basic Installation steps ###

1. In Eclipse, from the **Help** menu, select **Install New Software**.
1. Enter the site location <http://dl.msopentech.com/eclipse> and press **Enter**.
1. Select the items to be installed and click **Finish**.

The Azure Toolkit for Eclipse uses the latest version of the Azure SDK. This can be downloaded using the Web Platform Installer (WebPI) at <http://go.microsoft.com/fwlink/?LinkID=252838>. However, if you don't have it installed, when you create your first Azure deployment project, the Azure Toolkit for Eclipse will automatically install the appropriate version of the Azure SDK.

## See Also ##

[Azure Toolkit for Eclipse][]

[Installing the Azure Toolkit for Eclipse][] 

[Creating a Hello World Application for Azure in Eclipse][]

For more information about using Azure with Java, see the [Azure Java Developer Center][].

<!-- URL List -->

[Azure Java Developer Center]: http://go.microsoft.com/fwlink/?LinkID=699547
[Azure Libraries Repository on Maven]: http://search.maven.org/#browse%7C1671162511
[Azure Toolkit for Eclipse]: http://go.microsoft.com/fwlink/?LinkID=699529
[Creating a Hello World Application for Azure in Eclipse]: http://go.microsoft.com/fwlink/?LinkID=699533
[Installing the Azure Toolkit for Eclipse]: http://go.microsoft.com/fwlink/?LinkId=699546
[Java Build Path]: http://help.eclipse.org/luna/index.jsp?topic=%2Forg.eclipse.jdt.doc.user%2Freference%2Fref-properties-build-path.htm
[license]: http://www.apache.org/licenses/LICENSE-2.0.html
[maven-getting-started]: http://go.microsoft.com/fwlink/?LinkID=622998
[zip-download]: http://go.microsoft.com/fwlink/?LinkId=690320
[What's New in the Azure Toolkit for Eclipse]: https://msdn.microsoft.com/zh-cn/library/azure/hh694270.aspx
