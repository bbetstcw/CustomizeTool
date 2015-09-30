始终复制 Windows Azure 存储帐户中的数据以确保持久性和高可用性，并且即使在遇到临时硬件故障时也符合 [Azure 存储空间 SLA](/support/legal/sla/) 要求。创建存储帐户时，必须选择以下复制选项之一：

- **本地冗余存储 (LRS)。** 本地冗余存储保留数据的三个副本。LRS 将在单个区域中的单个设施内复制三次。LRS 可以保护你的数据免受普通的硬件故障损害，但无法保护你的数据免受单个设施故障的损害。  
  
	LRS 以折扣价格提供。为获得最大持久性，我们建议你使用下文所述的地域冗余存储。


- **区域冗余存储 (ZRS)。** 区域冗余存储保留数据的三个副本。ZRS 在两到三个设施之间复制三次（在单个区域内或两个区域之间），提供比 LRS 更高的持久性。ZRS 在单个区域内确保数据持久保存。

	ZRS 提供比 LRS 更高级别的持久性；不过，为获得最大持久性，我们建议你使用下文所述的地域冗余存储。

	> [AZURE.NOTE]ZRS 当前仅可用于块 Blob。
	> 
	> 在创建存储帐户并选择 ZRS 后，你无法将其转换为使用任何其他类型的复制，反之亦然。

- 异地冗余存储 (GRS)。默认情况下，在你创建存储帐户时便为存储帐户启用了地域冗余存储。GRS 维护你的数据的六个副本。使用 GRS 时，你的数据将在主区域内复制三次，并且还在离主区域数百英里的辅助区域中复制三次，从而提供最高级别的持久性。当主区域中发生故障时，Azure 存储空间将故障转移到辅助区域。GRS 在两个不同的区域中确保你的数据持久保存。


- 读取访问异地冗余存储 (RA-GRS)。读取访问地域冗余存储将你的数据复制到一个辅助地理位置，同时提供对你在辅助位置中的数据的读访问权限。读取访问地域冗余存储允许你从主位置或辅助位置访问数据，以防其中一个位置不可用。

	> [AZURE.IMPORTANT]创建存储帐户后，你可以更改复制数据的方式，除非你在创建该帐户时指定了 ZRS。但请注意，如果你从 LRS 切换到 GRS 或 RA-GRS，可能会产生额外的一次性数据传输费用。
 
有关存储复制选项的其他详细信息，请参阅 [Azure 存储空间复制](/documentation/articles/storage-redundancy)。

有关存储帐户复制的定价信息，请参阅[存储定价详细信息](/pricing/details/storage/)。

有关 Azure 存储持久性的体系结构详细信息，请参阅 [Azure 存储空间 SOSP 白皮书](http://blogs.msdn.com/b/windowsazurestorage/archive/2011/11/20/windows-azure-storage-a-highly-available-cloud-storage-service-with-strong-consistency.aspx)。

<!---HONumber=70-->