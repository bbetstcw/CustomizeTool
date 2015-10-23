﻿


本教程的最后一个阶段是生成和运行您的新应用。

1. 浏览到您保存压缩项目文件的位置，在计算机上展开文件，并使用 Xcode 打开项目文件。

   	![](./media/mobile-services-ios-run-app/mobile-xcode-project.png)

2. 按"运行"按钮以生成项目，并在 iPhone 模拟器中启动应用，这是此项目的默认设置。

3. 在应用中键入有意义的文本（例如 _Complete the tutorial_），然后单击加号 (**+**) 图标。

   	![](./media/mobile-services-ios-run-app/mobile-quickstart-startup-ios.png)

   	这样可向在 Azure 中托管的新移动服务发送 POST 请求。来自请求的数据被插入到 TodoItem 表。移动服务返回存储在表中的项，数据显示在列表中。

	>[WACOM.NOTE]您可以查看访问您的移动服务以查询和插入数据的代码，这些代码在 TodoService.m 文件中。</p> 
 	</div>
