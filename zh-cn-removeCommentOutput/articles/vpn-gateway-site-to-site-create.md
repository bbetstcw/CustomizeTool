<properties
   pageTitle="配置站点到站点虚拟网络连接 | Windows Azure"
   description="使用站点到站点 VPN 连接创建虚拟网络，以便进行跨界的和混合的配置。"
   services="vpn-gateway"
   documentationCenter=""
   authors="cherylmc"
   manager="adinah"
   editor=""/>

<tags
   ms.service="vpn-gateway"
   ms.date="05/12/2015"
   wacn.date="06/26/2015"/>

# 使用站点到站点 VPN 连接配置虚拟网络

通过创建站点到站点 VPN 连接，你可以使用虚拟网络连接本地位置。此过程将引导你创建虚拟网络，以及在新创建的 VNet 与本地位置之间创建站点到站点 VPN 连接。你可以使用此过程来创建跨界的和混合的虚拟网络配置。


## 在开始之前

- 请确保你要使用的 VPN 设备符合创建跨界虚拟网络连接所需达到的要求。有关详细信息，请参阅[关于用于虚拟网络连接的 VPN 设备](https://msdn.microsoft.com/zh-cn/library/azure/jj156075.aspx)。

- 获取 VPN 设备的外向 IPv4 IP。此 IP 地址是进行站点到站点配置所必需的，适用于不能置于 NAT 后面的 VPN 设备。

>[AZURE.IMPORTANT]如果你不熟悉 VPN 设备的配置，或者不熟悉本地网络配置中的 IP 地址范围，则需咨询能够为你提供此类详细信息的人员。

## 创建虚拟网络

1. 登录到**管理门户**。

2. 在屏幕左下角，单击“新建”。在导航窗格中，单击“网络服务”，然后单击“虚拟网络”。单击“自定义创建”以启动配置向导。

3. 在以下页面上填写创建 VNet 所需的信息。

## “虚拟网络详细信息”页

请输入以下信息。有关详细信息页上的设置的详细信息，请参阅[“虚拟网络详细信息”页](https://msdn.microsoft.com/zh-cn/library/azure/09926218-92ab-4f43-aa99-83ab4d355555#BKMK_VNetDetails)。

- **名称**：为虚拟网络命名。例如，*ChinaEastVNet*。在部署你的 VM 和 PaaS 实例时，将使用此虚拟网络名称，因此最好不要让此名称太复杂。
- **位置**：位置直接与你想让资源 (VM) 驻留在的物理位置（区域）有关。例如，如果你希望部署到此虚拟网络的 VM 的物理位置位于*中国东部*，请选择该位置。创建虚拟网络后，将无法更改与虚拟网络关联的区域。

## “DNS 服务器和 VPN 连接”页
请输入以下信息，然后单击右下的“下一步”箭头。有关此页上的设置的详细信息，请参阅[“DNS 服务器和 VPN 连接”页](https://msdn.microsoft.com/zh-cn/library/azure/09926218-92ab-4f43-aa99-83ab4d355555#BKMK_VNETDNS)。

- **DNS 服务器**：输入 DNS 服务器名称和 IP 地址，或从下拉列表中选择一个以前注册的 DNS 服务器。此设置不创建 DNS 服务器，但可以指定要用于对此虚拟网络进行名称解析的 DNS 服务器。
- **配置站点到站点 VPN**：选中“配置站点到站点 VPN”复选框。
- **本地网络**：本地网络代表你的物理本地位置。你可以选择以前创建的本地网络，也可以创建一个新的本地网络。但是，如果你选择使用此前创建的本地网络，则需转到“本地网络”配置页，确保用于此连接的 VPN 设备 IP 地址（面向公众的 IPv4 地址）是准确的。

## “站点到站点连接”页
如果你要创建新的本地网络，则会看到“站点到站点连接”页。如果你要使用此前创建的本地网络，则此页不会显示在向导中，你可以转到下一部分。

请输入以下信息，然后单击“下一步”箭头。有关此页上的设置的详细信息，请参阅[“站点到站点连接”页](https://msdn.microsoft.com/zh-cn/library/azure/09926218-92ab-4f43-aa99-83ab4d355555#BKMK_VNETSITE)。

- 	**名称**：你希望用于称呼本地（内部）网络站点的名称。
- 	**VPN 设备 IP 地址**：这是本地 VPN 设备面向公众的 IPv4 地址，你将使用该地址来连接到 Azure。VPN 设备不能位于 NAT 之后。
- 	**地址空间**：包括“起始 IP”和 CIDR（地址计数）。在此可以指定要通过虚拟网络网关发送到你本地内部位置的地址范围。如果目标 IP 地址处于你在此处指定的范围之内，则将通过虚拟网络网关来路由它。
- 	**添加地址空间**：如果你提供多个要通过虚拟网络网关发送的地址范围，则可在此指定每个额外的地址范围。可以稍后在“本地网络”页中添加或删除范围。

## “虚拟网络地址空间”页
指定要用于虚拟网络的地址范围。这些都是动态 IP 地址 (DIPS)，将分配给你部署到此虚拟网络的 VM 和其他角色实例。有相当多的规则与虚拟网络地址空间有关，因此请参阅[“虚拟网络地址空间”页](https://msdn.microsoft.com/zh-cn/library/azure/09926218-92ab-4f43-aa99-83ab4d355555#BKMK_VNET_ADDRESS)以了解详细信息。

所选范围不要与本地网络所用范围重叠，这一点尤其重要。你需要与网络管理员协调。网络管理员可能需要从本地网络地址空间为你划分一个 IP 地址范围，以供你的虚拟网络使用。

输入以下信息，然后单击右下角的复选标记以配置你的网络。

- **地址空间**：包括起始 IP 和地址计数。请验证你指定的地址空间不与本地网络的任一个地址空间相重叠。
- **添加子网**：包括起始 IP 和地址计数。附加的子网不是必需的，但你可能需要为具有静态 DIP 的 VM 创建一个单独的子网。或者，你可能需要在子网中拥有与其他角色实例分开的 VM。
- **添加网关子网**：单击此项可添加网关子网。网关子网仅用于此虚拟网络网关，并且是此配置必需的。

单击页面底部的复选标记，此时将开始创建虚拟网络。创建完成时，将在“管理门户”的“网络”页上看到“状态”下列出的“已创建”。在创建 VNet 之后，你就可以配置虚拟网络网关。

## 配置虚拟网络网关

接下来，你将配置**虚拟网络网关**，以便创建安全的站点到站点连接。请参阅[在管理门户中配置虚拟网络网关](https://msdn.microsoft.com/zh-cn/library/azure/jj156210.aspx)。

## 后续步骤

你可以在下文中了解有关虚拟网络跨界连接的更多信息：[关于虚拟网络安全跨界连接](https://msdn.microsoft.com/zh-cn/library/azure/dn133798.aspx)

如果你想要配置点到站点 VPN 连接，请参阅[配置点到站点 VPN 连接](/documentation/articles/vpn-gateway-point-to-site-create)

你可以将虚拟机添加到虚拟网络。请参阅[如何创建自定义虚拟机](/documentation/articles/virtual-machines-create-custom)

如果你想要使用 RRAS 配置 VNet 连接，请参阅[使用 Windows Server 2012 路由和远程访问服务 (RRAS) 配置站点到站点 VPN](https://msdn.microsoft.com/zh-cn/library/dn636917.aspx)

<!---HONumber=61-->