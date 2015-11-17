<properties
	pageTitle="IPython Notebook | Windows Azure"
	description="本教程演示如何使用 Linux 或 Windows 虚拟机 (VM) 在 Azure 上部署 IPython Notebook。"
	services="virtual-machines"
	documentationCenter="python"
	authors="huguesv"
	manager="wpickett"
	editor=""/>

<tags
	ms.service="virtual-machines"
	ms.date="05/20/2015"
	wacn.date="09/18/2015"/>


# Azure 上的 IPython Notebook

[IPython 项目](http://ipython.org)提供了用于科学计算的工具集合，其中包括强大的交互式 shell、高性能且易于使用的并行库以及称为 IPython Notebook 的基于 Web 的环境。Notebook 为将代码执行与实时计算文档的创建组合起来的交互式计算提供了工作环境。这些 Notebook 文件可以包含任意文本、数学公式、输入代码、结果、图形、视频以及新型 Web 浏览器能够显示的任何其他种类的媒体。

无论您是第一次使用 Python 并希望在有趣的交互式环境中了解它，还是执行一些严格的并行/技术计算，IPython Notebook 都是一个很好的选择。为说明其各项功能，下面的屏幕截图显示了与 SciPy 和 Matplotlib 包结合使用来分析录音结构的 IPython Notebook：

![屏幕快照](./media/virtual-machines-python-ipython-notebook/ipy-notebook-spectral.png)

本文将演示如何使用 Linux 或 Windows 虚拟机 (VM) 在 Microsoft Azure 上部署 IPython Notebook。通过在 Azure 上使用 IPython Notebook，你可以轻松地为具有 Python 的所有功能及其许多库的可缩放计算资源提供可通过 Web 访问的接口。由于所有安装都是在云中进行的，因此用户可以访问这些资源，而无需除最新 Web 浏览器以外的任何其他本地配置。

[AZURE.INCLUDE [create-account-and-vms-note](../includes/create-account-and-vms-note.md)]

## 在 Azure 上创建并配置 VM

第一步是创建在 Azure 上运行的虚拟机 (VM)。此 VM 是云中的完整操作系统，它将用于运行 IPython Notebook。Azure 能够同时运行 Linux 和 Windows 虚拟机，我们将介绍如何在这两种类型的虚拟机上设置 IPython。

### Linux VM

按照[此处][portal-vm-linux]提供的说明可创建 *OpenSUSE* 或 *Ubuntu* 分发的虚拟机。本教程使用 OpenSUSE 13.2 和 Ubuntu Server 14.04 LTS。我们将假定默认用户名为 *azureuser*。

### Windows VM

按照[此处][portal-vm-windows]提供的说明可创建 *Windows Server 2012 R2 Datacenter* 分发的虚拟机。在本教程中，我们将假定用户名为 *azureuser*。

## 为 IPython Notebook 创建终结点

此步骤同时适用于 Linux 和 Windows VM。稍后我们将在端口 9999 上配置 IPython 以运行其 notebook 服务器。要使此端口公开可用，必须在 Azure 管理门户中创建终结点。此终结点将在 Azure 防火墙中打开一个端口并将公用端口（HTTPS，443）映射到 VM 上的专用端口 (9999)。

若要创建终结点，请转到 VM 仪表板，单击“终结点”，再单击“添加终结点”，然后创建新终结点（在此示例中称为 `ipython_nb`）。为协议选取 **TCP**，为公用端口选取 **443**，并为专用端口选取 **9999**。

![屏幕快照](./media/virtual-machines-python-ipython-notebook/ipy-azure-linux-005.png)

在此步骤后，“终结点”仪表板选项卡将如下一个屏幕截图所示。

![屏幕快照](./media/virtual-machines-python-ipython-notebook/ipy-azure-linux-006.png)

## 在 VM 上安装必需软件

若要在我们的 VM 上运行 IPython Notebook，必须首先安装 IPython 及其依赖项。

### Linux (OpenSUSE)

若要安装 IPython 及其依赖项，请通过 SSH 进入 Linux VM 并完成以下步骤。

使用以下命令安装 [NumPy][NumPy]、[Matplotlib][Matplotlib]、[Tornado][Tornado] 和 IPython 的其他依赖项。

    sudo zypper install python-matplotlib
    sudo zypper install python-tornado
    sudo zypper install python-jinja2
    sudo zypper install ipython

### Linux (Ubuntu)

若要安装 IPython 及其依赖项，请通过 SSH 进入 Linux VM 并执行以下步骤。

首先，使用以下命令检索包的新列表。

    sudo apt-get update

使用以下命令安装 [NumPy][NumPy]、[Matplotlib][Matplotlib]、[Tornado][Tornado] 和 IPython 的其他依赖项。

    sudo apt-get install python-matplotlib
    sudo apt-get install python-tornado
    sudo apt-get install ipython
    sudo apt-get install ipython-notebook

### Windows

若要在 Windows VM 上安装 IPython 及其依赖项，请使用远程桌面连接到 VM。然后执行以下步骤，使用 Windows PowerShell 运行所有命令行操作。

**注意**：若要使用 Internet Explorer 下载任何内容，你将需要更改某些安全设置。从“服务器管理器”中，单击“本地服务器”，然后在“IE 增强的安全配置”上，对于管理员关闭它。在安装 IPython 后，你可以重新启用它。

1.  下载并安装最新 32 位版本的 [Python 2.7][]。你需要将 `C:\Python27` 和 `C:\Python27\Scripts` 添加到 `PATH` 环境变量中。

1.  使用以下命令安装 [Tornado][Tornado]、[PyZMQ][PyZMQ] 和 IPython 的其他依赖项。

        easy_install tornado
        easy_install pyzmq
        easy_install jinja2
        easy_install six
        easy_install python-dateutil
        easy_install pyparsing

1.  使用各自网站上提供的 `.exe` 二进制安装程序，下载并安装 [NumPy][NumPy]。截至撰写本文之际，最新版本为 numpy-1.9.1-win32-superpack-python2.7.exe。

1.  使用以下命令安装 [Matplotlib][Matplotlib]。

        pip install matplotlib==1.4.2

1.  下载并安装 [OpenSSL][]。

	* 你可以在同一下载页中找到必需的 Visual C++ 2008 Redistributable。

	* 你需要将 `C:\OpenSSL-Win32\bin` 添加到 `PATH` 环境变量中。

	> [AZURE.NOTE]在安装 OpenSSL 时，请使用版本 1.0.1g 或更高版本，因为这些版本包含 Heartbleed 安全漏洞的修补程序。

1.  使用以下命令安装 IPython。

        pip install ipython==2.4

1.  在 Windows 防火墙中打开一个端口。在 Windows Server 2012 上，防火墙默认将阻止传入连接。若要打开端口 9999，请执行下列步骤：

    - 从“开始”屏幕启动“高级安全 Windows 防火墙”。

    - 在左面板中，单击“入站规则”。

	- 在“操作”面板中单击“新建规则”。

	- 在“新建入站规则向导”中，选择“端口”。

	- 在下一个屏幕中，选择 **TCP**，然后在“特定本地端口”中输入 **9999**。

	- 接受默认设置，为规则提供一个名称，然后单击“完成”。

### 配置 IPython Notebook

接下来，我们配置 IPython Notebook。第一步是使用以下命令创建自定义 IPython 配置文件以封装配置信息。

    ipython profile create nbserver

然后，通过 `cd` 进入配置文件目录来创建 SSL 证书并编辑配置文件。

在 Linux 上使用以下命令。

    cd ~/.ipython/profile_nbserver/

在 Windows 上使用以下命令。

    cd \users\azureuser\.ipython\profile_nbserver

使用以下命令创建 SSL 证书（Linux 和 Windows）。

    openssl req -x509 -nodes -days 365 -newkey rsa:1024 -keyout mycert.pem -out mycert.pem

请注意，由于我们创建的是自签名 SSL 证书，因此在连接到 Notebook 时，您的浏览器将显示安全警告。若要进行长期生产使用，您将需要使用与您的组织关联的正确签名的证书。由于证书管理超出了本演示的范围，我们目前将只讨论自签名证书。

除了使用证书外，您还必须提供密码以确保您的 Notebook 免遭未授权使用。出于安全原因，IPython 在其配置文件中使用加密密码，因此您将首先需要加密您的密码。IPython 提供了一个实用工具来执行此操作；在命令提示符下运行以下命令。

    python -c "import IPython;print IPython.lib.passwd()"

这会提示你提供密码并确认，然后将输出密码，如下所示。

    Enter password:
    Verify password:
    sha1:b86e933199ad:a02e9592e59723da722.. (elided the rest for security)

接下来，我们将编辑配置文件，它是你所在的配置文件目录中的 `ipython_notebook_config.py` 文件。请注意此文件可能不存在 - 只需创建它。此文件包含多个字段，默认情况下会全部注释掉。你可以使用你喜欢的任何文本编辑器打开此文件，并应确保它至少具有以下内容。

    c = get_config()

    # This starts plotting support always with matplotlib
    c.IPKernelApp.pylab = 'inline'

    # You must give the path to the certificate file.

    # If using a Linux VM:
    c.NotebookApp.certfile = u'/home/azureuser/.ipython/profile_nbserver/mycert.pem'

    # And if using a Windows VM:
    c.NotebookApp.certfile = r'C:\Users\azureuser\.ipython\profile_nbserver\mycert.pem'

    # Create your own password as indicated above
    c.NotebookApp.password = u'sha1:b86e933199ad:a02e9592e5 etc... '

    # Network and browser details. We use a fixed port (9999) so it matches
    # our Azure setup, where we've allowed traffic on that port

    c.NotebookApp.ip = '*'
    c.NotebookApp.port = 9999
    c.NotebookApp.open_browser = False

### 运行 IPython Notebook

此时，我们已准备好启动 IPython Notebook。为此，请导航到要在其中存储 Notebook 的目录并使用以下命令启动 IPython Notebook 服务器。

    ipython notebook --profile=nbserver

你现在应能够通过地址 `https://[Your Chosen Name Here].cloudapp.net` 访问你的 IPython Notebook。

在你首次访问 Notebook 时，登录页会询问你的密码。

![屏幕快照](./media/virtual-machines-python-ipython-notebook/ipy-notebook-001.png)

在登录后，您会看到“IPython Notebook 仪表板”，它是所有 Notebook 操作的控制中心。从此页中，你可以创建新 Notebook，打开现有 Notebook。

![屏幕快照](./media/virtual-machines-python-ipython-notebook/ipy-notebook-002.png)

如果单击“新建 Notebook”按钮，你将看到以下打开页。

![屏幕快照](./media/virtual-machines-python-ipython-notebook/ipy-notebook-003.png)

标记有 `In []:` 提示的区域是输入区域，在这里，你可以键入任何有效的 Python 代码，并且它会在你按 `Shift-Enter` 或单击“播放”图标（工具栏中的右指三角形）时执行。

由于我们已将 Notebook 配置为使用 NumPy 和 Matplotlib 支持自动启动，因此你甚至可以生成图形，如下一个屏幕截图所示：

![屏幕快照](./media/virtual-machines-python-ipython-notebook/ipy-notebook-004.png)

## 强大的范例：具有丰富媒体的实时计算文档

Notebook 本身对使用过 Python 和字处理器的任何人来说应感觉非常自然，因为它在某些方面是这二者的融合：你可以执行 Python 代码块，但也可以通过使用工具栏中的下拉菜单将单元格的样式从“代码”更改为“标记”来保留笔记及其他文本。

![屏幕快照](./media/virtual-machines-python-ipython-notebook/ipy-notebook-005.png)


但 IPython Notebook 不仅仅是字处理器，因为它允许混合计算和丰富媒体（文本、图形、视频以及新型 Web 浏览器可以显示的几乎任何内容）。例如，你可以出于教育目的混合说明性视频和计算。

![屏幕快照](./media/virtual-machines-python-ipython-notebook/ipy-notebook-006.png)

或者，如下一个屏幕截图所示，在 Notebook 文件中嵌入始终有效且可用的外部网站。

![屏幕快照](./media/virtual-machines-python-ipython-notebook/ipy-notebook-007.png)

在以下屏幕截图中，利用 Python 的许多用于科技计算的优秀库的功能，可以与进行复杂网络分析一样轻松地执行简单计算，且全部操作都在一个环境中完成。

![屏幕快照](./media/virtual-machines-python-ipython-notebook/ipy-notebook-008.png)

将最新 Web 的功能与实时计算混合的此范例提供了许多可能性，并且非常适合云；Notebook 可以：

* 用作计算暂存器以记录对问题进行的探索工作。

* 用于以“实时”计算形式或以硬拷贝格式（HTML、PDF）与同事共享结果。

* 用于分发并显示涉及计算的实时教学材料，以便学生可以立即使用真实代码进行试验，对其进行修改，然后以交互方式重新执行它。

* 用于提供可由他人以立即重现、验证和扩展的方式显示研究结果的“可执行论文”。

* 用作协作计算的平台：多个用户可以登录到同一 notebook 服务器来共享实时计算会话。



如果你转到 IPython 源代码[存储库][]，你将找到具有 notebook 示例的整个目录，你可以下载这些示例，然后在自己的 Azure IPython VM 上进行试用。只需从网站中下载 `.ipynb` 文件并将它们上载到你的 notebook Azure VM 的仪表板上（或将它们直接下载到 VM 中）。

## 结束语

IPython Notebook 为交互访问 Azure 上的 Python 生态系统的功能提供了强大接口。它涵盖范围广泛的用例，包括简单的探索和学习 Python、数据分析和可视化、模拟和并行计算。生成的 Notebook 文档包含所执行的可与其他 IPython 用户共享的计算的完整记录。IPython Notebook 可用作本地应用程序，但它非常适合 Azure 上的云部署

还可通过 [Python Tools for Visual Studio][](/documentation/articles/PTVS) 在 Visual Studio 中使用 IPython 的核心功能。PTVS 是 Microsoft 提供的免费开放源代码插件，它可将 Visual Studio 转变为高级 Python 开发环境，其中包括具有 IntelliSense、调试、分析和并行计算集成功能的高级编辑器。




[Tornado]: http://www.tornadoweb.org/ "Tornado"
[PyZMQ]: https://github.com/zeromq/pyzmq "PyZMQ"
[NumPy]: http://www.numpy.org/ "NumPy"
[Matplotlib]: http://matplotlib.sourceforge.net/ "Matplotlib"

[portal-vm-windows]: /documentation/articles/virtual-machines-windows-tutorial
[portal-vm-linux]: /documentation/articles/virtual-machines-linux-tutorial
[存储库]: https://github.com/ipython/ipython
[python Tools for visual studio]: http://aka.ms/ptvs
[Python 2.7]: http://www.python.org/download
[OpenSSL]: http://slproweb.com/products/Win32OpenSSL.html

<!---HONumber=70-->