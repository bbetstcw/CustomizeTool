<properties 
	pageTitle="五分钟内开始使用 Azure 存储空间 | Windows Azure" 
	description="使用 Azure 快速入门、Visual Studio 和 Azure 存储模拟器快速掌握 Windows Azure Blob、表和队列。在五分钟内运行你的第一个 Azure 存储空间应用程序。" 
	services="storage" 
	documentationCenter=".net" 
	authors="Selcin" 
	manager="adinah" 
	editor=""/>

<tags 
	ms.service="storage" 
	ms.date="05/28/2015" 
	wacn.date="11/12/2015"/>

# 五分钟内开始使用 Azure 存储空间 

针对 Azure 存储空间开始进行开发非常容易。本教程向你说明如何构建一个 Azure 存储空间应用程序并快速运行。我们将演示轻松掌握 Azure 存储空间的两种方案：

- [使用 Azure 存储模拟器在本地运行你的第一个 Azure 存储空间应用程序](#run-your-first-azure-storage-application-locally-against-the-azure-storage-emulator)
- [使用 Azure 存储模拟器在云中运行你的第一个 Azure 存储空间应用程序](#run-your-first-azure-storage-application-against-azure-storage-in-the-cloud)

如果在深入分析代码之前你想要详细了解 Azure 存储空间，请参阅[后续步骤](#next-steps)。

## 先决条件

在开始之前，请确保符合以下先决条件：

1. 若要编译和生成应用程序，你需要在你的计算机上安装 [Visual Studio](https://www.visualstudio.com/)。 
2. 安装最新版的 [Azure SDK for .NET](/downloads/)。SDK 包括 Azure 快速入门示例项目、Azure 存储模拟器和[用于 .NET 的 Azure 存储空间客户端库](https://msdn.microsoft.com/zh-cn/library/azure/wa_storage_30_reference_home.aspx)。
3. 确保在你的计算机上安装了 [.NET Framework 4.5](http://www.microsoft.com/download/details.aspx?id=30653)，因为我们将在本教程中使用的 Azure 快速入门示例项目需要它。如果你不确定计算机上安装了哪个版本的 .NET Framework，请参阅[如何：确定安装的 .NET Framework 版本](https://msdn.microsoft.com/zh-cn/vstudio/hh925568.aspx)。或者，按“开始”按钮或 Windows 键，并键入“控制面板”。然后，单击“程序” > “程序和功能”，然后在已安装程序中确定是否列出 .NET Framework 4.5。

可在 [NuGet](https://www.nuget.org/packages/WindowsAzure.Storage/) 中获得最新版的 Azure 存储空间客户端库二进制文件 。


## 使用 Azure 存储模拟器在本地运行你的第一个 Azure 存储空间应用程序

在开发使用 Azure 存储空间的应用程序时，你可以使用 [Azure 存储模拟器](/documentation/articles/storage-use-emulator) 运行该应用程序。存储模拟器提供了一个针对开发目的模拟 Azure Blob、队列和表服务的本地环境。你可以使用存储模拟器在本地测试你的应用程序，而无需创建 Azure 订阅或存储帐户，并且不会产生任何费用。

若要尝试，让我们在 Visual Studio 中使用 Azure 快速入门示例项目之一创建一个简单的 Azure 存储空间应用程序。本教程重点介绍 **Azure Blob 存储**、**Azure 表存储**和 **Azure 队列存储**示例项目：

1. 启动 Visual Studio。
2. 在“文件”菜单中，单击“新建项目”。
3. 在“新建项目”对话框中，单击“已安装” > “模板” > “Visual C#” > “云” > “快速启动” >“数据服务”。
	- 3.a.选择以下模板之一：“Azure Blob 存储”、“Azure 表存储”或“Azure 存储队列”。 
	- 3.b.确保选择 **.NET Framework 4.5** 作为目标框架。	
	- 3.c.为你的项目指定一个名称并创建新的 Visual Studio 解决方案，如下所示：
	
	![Azure 快速入门][Image1]

你可能想要在运行应用程序之前检查源代码。若要查看代码，请在 Visual Studio 中的“查看”菜单上选择“解决方案资源管理器”。然后双击 Program.cs 文件。

接下来，在 Azure 存储模拟器中运行示例应用程序:

1.	按“开始”按钮或 Windows 键，然后搜索 *Azure 存储模拟器*并启动该程序。
2.	在 Visual Studio 中，单击“生成”菜单上的“生成解决方案”。 
3.	在“调试”**菜单**上，按 **F11** 逐步运行该解决方案，或按 **F5** 从头到尾运行该解决方案。

## 使用 Azure 存储模拟器在云中运行你的第一个 Azure 存储空间应用程序

若要使用 Azure 存储空间在云中运行，你需要 Azure 订阅和存储帐户，如果你尚未拥有的话：

- 若要获得 Azure 订阅，请参阅[试用](/pricing/1rmb-trial/)、[购买选项](/pricing/purchase-options/)和[成员优惠](/pricing/member-offers/)（适用于 MSDN、Microsoft 合作伙伴网络和 BizSpark 以及其他 Microsoft 计划的成员）。
- 若要在 Azure 中创建一个存储帐户，请参阅[如何创建、管理或删除存储帐户](/documentation/articles/storage-create-storage-account)。

拥有帐户之后，即可以在 Visual Studio 中使用 Azure 快速入门示例项目之一创建一个简单的 Azure 存储空间应用程序。本教程重点介绍 **Azure Blob 存储**、**Azure 表存储**和 **Azure 队列存储**示例项目：

1. 启动 Visual Studio。
2. 在“文件”菜单中，单击“新建项目”。
3. 在“新建项目”对话框中，单击“已安装” > “模板” > “Visual C#” > “云” > “快速启动” >“数据服务”。
	- 3\.a.选择以下模板之一：“Azure Blob 存储”、“Azure 表存储”或“Azure 存储队列”。
	- 3\.b.确保选择 **.NET Framework 4.5** 作为目标框架。
	- 3\.c.为你的项目指定一个名称并创建新的 Visual Studio 解决方案。 

你可能想要在运行应用程序之前检查源代码。若要查看代码，请在 Visual Studio 中的“查看”菜单上选择“解决方案资源管理器”。然后双击 Program.cs 文件。

接下来，运行示例应用程序：

1.	在 Visual Studio 中的“查看”菜单上，选择“解决方案资源管理器”。双击 App.config 文件并注释掉 Azure SDK 存储模拟器的连接字符串：

	`<!--<add key="StorageConnectionString" value = "UseDevelopmentStorage=true;"/>-->`

2.	取消 Azure 存储服务的连接字符串的注释，并在 App.config 文件中提供存储帐户名称和访问密钥：`<add key="StorageConnectionString" value="DefaultEndpointsProtocol=https;AccountName=[AccountName];AccountKey=[AccountKey];EndpointSuffix=core.Chinacloudapi.cn"`

	若要查找存储帐户名称和访问密钥，请参阅[什么是存储帐户](/documentation/articles/storage-whatis-account)。 

3.	在 App.config 文件中提供存储帐户名称和访问密钥后,在“文件” 菜单中，单击“全部保存”以保存所有项目文件。
4.	在“生成”菜单中，单击“生成解决方案”。
5.	在“调试”菜单中，按 **F11** 逐步运行该解决方案，或按 **F5** 运行该解决方案。


## 后续步骤

若要了解有关 Azure 存储空间的详细信息，请参阅以下资源：

* [Windows Azure 存储空间简介](/documentation/articles/storage-introduction)
* [如何通过 .NET 使用 Blob 存储](/documentation/articles/storage-dotnet-how-to-use-blobs)
* [如何通过 .NET 使用表存储](/documentation/articles/storage-dotnet-how-to-use-tables)
* [如何通过 .NET 使用队列存储](/documentation/articles/storage-dotnet-how-to-use-queues)
* [Azure 存档文档](/documentation/services/storage/)
* [Azure 存储客户端库](https://msdn.microsoft.com/zh-cn/library/azure/wa_storage_30_reference_home.aspx)
* [Azure 存储 REST API](https://msdn.microsoft.com/zh-cn/library/azure/dd179355.aspx)

[Image1]: ./media/storage-getting-started-guide/QuickStart.png


<!---HONumber=70-->