<properties 
   pageTitle="什么是网络访问控制列表 (ACL)？"
   description="了解 ACL"
   services="virtual-network"
   documentationCenter="na"
   authors="telmosampaio"
   manager="carolz"
   editor="tysonn"/>
<tags 
   ms.service="virtual-network"
   ms.date="09/04/2015"
   wacn.date="10/17/2015"/>

# 什么是网络访问控制列表 (ACL)？

网络访问控制列表 (ACL) 是可用于 Azure 部署的安全增强。利用 ACL，你可以选择允许还是拒绝虚拟机终结点的流量。此数据包筛选功能额外提供了一层安全性。你只能为终结点指定网络 ACL，而无法为虚拟网络或虚拟网络中包含的特定子网指定 ACL。

> [AZURE.IMPORTANT]建议你尽可能地使用网络安全组 (NSG) 而不是 ACL。若要了解有关 NSG 的详细信息，请参阅[什么是网络安全组](/documentation/articles/virtual-network-nsg)。

可以使用 PowerShell 或在管理门户中配置 ACL。若要使用 PowerShell 配置网络 ACL，请参阅[使用 PowerShell 管理终结点的访问控制列表 (ACL)](/documentation/articles/virtual-networks-acl-powershell)。若要使用管理门户配置网络 ACL，请参阅[如何设置虚拟机的终结点](/documentation/articles/virtual-machines-set-up-endpoints)。

使用网络 ACL 可以实现以下目的：

- 根据远程子网 IPv4 地址范围选择允许还是拒绝传入流量流向虚拟机输入终结点。

- 方块列表 IP 地址

- 为每个虚拟机终结点创建多个规则

- 为每个虚拟机终结点指定最多 50 个 ACL 规则

- 使用规则排序可确保将一组正确的规则应用于给定的虚拟机终结点（最低到最高）

- 为特定远程子网 IPv4 地址指定 ACL。

## ACL 的工作原理

ACL 是包含规则列表的对象。当你创建 ACL 并将其应用于虚拟机终结点时，数据包筛选将在 VM 的主机节点上发生。这意味着，来自远程 IP 地址的流量将通过主机节点筛选以匹配 ACL 规则，而不是在你的 VM 上进行筛选。这可以防止 VM 将宝贵的 CPU 周期耗费在数据包筛选上。

创建虚拟机时，将会设置一个默认 ACL 来阻止所有传入流量。但是，如果创建了一个终结点（针对端口 3389），则会修改默认 ACL 以允许该终结点的所有入站流量。随后，将允许来自任何远程子网的传入流量流向该终结点，而且不需要配置防火墙。除非为这些端口创建了终结点，否则将阻止所有其他端口的传入流量。默认情况下允许出站流量。

**默认 ACL 表示例**

| **规则编号** | **远程子网** | **终结点** | **允许/拒绝** |
|--------|---------------|----------|-------------|
| 100 | 0.0.0.0/0 | 3389 | 允许 |

## 允许和拒绝

你可以通过创建指定“允许”或“拒绝”的规则来选择允许还是拒绝虚拟机输入终结点的网络流量。请务必注意，默认情况下，在创建一个终结点后，将拒绝所有流量流向该终结点。因此，如果你希望精确地控制你选择允许访问虚拟机终结点的网络流量，则了解如何创建允许/拒绝规则并按正确的优先顺序放置它们非常重要。

考虑的要点：

1. **无 ACL** - 默认情况下，在创建一个终结点后，我们将允许终结点的所有流量。

1. **允许** - 默认情况，在添加一个或多个“允许”范围后，你将拒绝所有其他范围。只有来自允许的 IP 范围的数据包才能与虚拟机终结点进行通信。

1. **拒绝** - 默认情况，在添加一个或多个“拒绝”范围后，你将允许所有其他范围的流量。

1. **允许和拒绝的组合** - 当你要指定允许或拒绝的特定 IP 范围时，可结合使用“允许”和“拒绝”。

## 规则和规则优先顺序

可对特定虚拟机终结点设置网络 ACL。例如，你可以为在虚拟机上创建的 RDP 终结点指定一个网络 ACL，它将锁定对某些 IP 地址的访问。下表显示了如何为某个范围的公共虚拟 IP (VIP) 授予访问权限以允许访问 RDP。将拒绝所有其他远程 IP。我们将采用*越小越优先*的规则顺序。

### 多个规则

在以下示例中，如果你希望仅允许两个公共 IPv4 地址范围（65.0.0.0/8 和 159.0.0.0/8）内的 IP 访问 RDP 终结点，则可以通过指定两个“允许”规则来实现。在这种情况下，由于系统会默认为虚拟机创建 RDP，你可能希望基于远程子网锁定对 RDP 端口的访问。下面的示例显示了如何为某个范围的公共虚拟 IP (VIP) 授予访问权限以允许访问 RDP。将拒绝所有其他远程 IP。由于可为特定虚拟机终结点设置网络 ACL 访问且在默认情况下访问会被拒绝，因此这种方法很有用。

**示例 - 多个规则**

| **规则编号** | **远程子网** | **终结点** | **允许/拒绝** |
|--------|---------------|----------|-------------|
| 100 | 65.0.0.0/8 | 3389 | 允许 |
| 200 | 159.0.0.0/8 | 3389 | 允许 |

### 规则顺序

由于可为一个终结点指定多个规则，因此必须有一种方法来组织规则以确定哪个规则优先。规则顺序决定了优先顺序。网络 ACL 将采用*越小越优先*的规则顺序。在以下示例中，选择性地为端口 80 上的终结点授予了对某些 IP 地址范围的访问权限。为了进行此配置，我们制定了针对 175.1.0.1/24 范围内的地址的拒绝规则（规则号 100）。然后，我们指定了优先顺序为 200 的第二个规则，该规则允许访问 175.0.0.0/8 下的所有其他地址。

**示例 - 规则优先顺序**

| **规则编号** | **远程子网** | **终结点** | **允许/拒绝** |
|--------|---------------|----------|-------------|
| 100 | 175.1.0.1/24 | 80 | 拒绝 |
| 200 | 175.0.0.0/8 | 80 | 允许 |

## 网络 ACL 和负载平衡的集

网络 ACL 可在负载平衡集（LB 集）终结点上指定。如果为 LB 集指定了 ACL，则网络 ACL 将应用于该 LB 集中的所有虚拟机。例如，如果使用“端口 80”创建了 LB 集，并且 LB 集包含 3 个 VM，则在一台 VM 的终结点“端口 80”上创建的网络 ACL 将自动应用于其他 VM。

![网络 ACL 和负载平衡的集](./media/virtual-networks-acl/IC674733.png)

## 后续步骤

[如何使用 PowerShell 管理终结点的访问控制列表 (ACL)](/documentation/articles/virtual-networks-acl-powershell)

<!---HONumber=74-->